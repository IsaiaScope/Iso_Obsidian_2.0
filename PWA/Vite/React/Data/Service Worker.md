# Service Worker

## Theory

Service worker are on a different thread they dont have access to the DOM as other java-script file, are background services listening for events
Service worker must be in root folder to access all server resources
[[SW - Thread.png]]

## Lifecycle

[service-worker-lifecycle - Articles](https://web.dev/articles/service-worker-lifecycle)

We need to register our service worker by another java-script file as in follow `app.js`
When a service worker is registered an `install event is fired` just once
After being installed service worker becomes active and `active event is fired` and it access all different pages and files and intercept events and messages or HTTP requests
[[SW Lifecycle 1.png]]

---

This lifecycle fire first time and retrigger if files into service worker changes after a deploy
Refreshing the page doesn't delete service worker witch stay on our browser
So if nothing change `istall event` and `active event` dont fire anymore

[[SW Lifecycle 2.png]]

---

If something change the `install event` is fired but not the `active event`, and the service worker become in `waiting status`
The browser still use old version on service worker until the new version become active, this could happen on page refresh or when user close and reopen the site tab
In other word the new versione become active after being installed on site refresh not before

[[SW Lifecycle 3.png]]

---

## Register a service worker

remember to add this file to HTML or export that function into an another script already imported

```js
if ("serviceWorker" in navigator) {
	window.addEventListener("load", () => {
		navigator.serviceWorker
			.register("/sw.js", { scope: "/" })
			.then((req) => console.log(`[PWA] sw register successfully `, req))
			.catch((e) => console.log(`[PWA] sw register error`, e));
	});
}
```

---

# Service Worker Events

[Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)

## Fetch Event

Service worker can active as proxy and intercept HTTPS request. Later on is possible to cache data and decide to serve that instead on API call avoid an extra HTTPS request

[[Fetch Event.png]]

---

## Offline Mode

Without connection service worker could use resources from cache storage that is a different on from the network one, to serve data on HTTPS request or other situation where internet is needed

[[Offline Mode.png]]

Good files to cache are file dont change too much, like asset. We dont want to cache every time maybe just when files in service worker change so `install event` is a good place where add some logic because that event fire when a new version of service worker is deployed

## Cache

### Precache Request in Assets

`assets` contains the request urls that also are keys to serve the cache data. In other words contains a list of file urls and when our application calls that specific file, with a fetch request that request are not made and instead cache data is served; this happen just if we are in offline mode.

`waitUntil()` is necessary because the install event is very fast so caching could be skipped or not complete in time

_the data in cache store is just a HTTPS Path used as key and the response data of that specific API call_

[[Cache Storage.png]]

Note: check the response that we are caching because maybe some sub resources are called and also that API must be added to the assets

```js
const staticCacheName = "site-static";
const assets = [
	"/",
	"/index.html",
	"/js/app.js",
	"/js/ui.js",
	"/js/materialize.min.js",
	"/css/styles.css",
	"/css/materialize.min.css",
	"/img/dish.png",
	"https://fonts.googleapis.com/icon?family=Material+Icons",
	"https://fonts.gstatic.com/s/materialicons/v47/flUhRq6tzZclQEJ-Vdg-IuiaDsNcIhQ8tQ.woff2",
	"/pages/fallback.html",
];

// install event
self.addEventListener("install", (evt) => {
	evt.waitUntil(
		caches.open(staticCacheName).then((cache) => {
			cache.addAll(assets);
		})
	);
});
```

### Getting Cached Assets as Response

in fetch event can manage the response and serve our cache data,
If data is present in the cache serve that or do the fetch request `cacheRes || fetch(evt.request)`

#### Dynamic Caching

If we want to update automatically cache every time API is called we can do as follow.
Those API are calls _not included_ in `assets array`
`cache.put()` do the update

```js
const staticCacheName = "site-static-v2";
const dynamicCacheName = "site-dynamic-v1";

// fetch event
self.addEventListener("fetch", (evt) => {
	//console.log('fetch event', evt);
	evt.respondWith(
		caches
			.match(evt.request)
			.then((cacheRes) => {
				return (
					cacheRes ||
					fetch(evt.request).then((fetchRes) => {
						return caches.open(dynamicCacheName).then((cache) => {
							cache.put(evt.request.url, fetchRes.clone());
							// check cached items size
							limitCacheSize(dynamicCacheName, 15);
							return fetchRes;
						});
					})
				);
			})
			.catch(() => {
				if (evt.request.url.indexOf(".html") > -1) {
					return caches.match("/pages/fallback.html");
				}
			})
	);
});
```

### Cache Versioning

Doing a change to a file, we need to resave that into the cache so we can create a new version and delete the old one, because cache storage keep old versions inside browser memory

If we dont delete old cache the browser doesn't know where take the correct data and could response with the old one

`caches.keys()` return and array of key names off different caches stored

```js
const staticCacheName = "site-static-v2";
const dynamicCacheName = "site-dynamic-v1";

// activate event
self.addEventListener("activate", (evt) => {
	//console.log('service worker activated');
	evt.waitUntil(
		caches.keys().then((keys) => {
			//console.log(keys);
			return Promise.all(
				keys
					.filter((key) => key !== staticCacheName && key !== dynamicCacheName)
					.map((key) => caches.delete(key))
			);
		})
	);
});
```

### Limiting Cache Size

useful for prevent large amount of stored data

```js
// cache size limit function
const limitCacheSize = (name, size) => {
	caches.open(name).then((cache) => {
		cache.keys().then((keys) => {
			if (keys.length > size) {
				cache.delete(keys[0]).then(limitCacheSize(name, size));
			}
		});
	});
};
```

---
