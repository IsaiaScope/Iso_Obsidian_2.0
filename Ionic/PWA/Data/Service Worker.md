# Service Worker

## Theory

Service worker are on a different thread they dont have access to the DOM as other java-script file, are background services listening for events
Service worker must be in root folder to access all server resources
[[SW - Thread.png]]

## Lifecycle

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

https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API

## Fetch Event

Service worker can active as proxy and intercept HTTPS request. Later on is possible to cache data and decide to serve that instead on API call avoid an extra HTTPS request

[[Fetch Event.png]]

---

## Offline Mode

Without connection service worker could use resources from cache storage that is a different on from the network one, to serve data on HTTPS request or other situation where internet is needed

[[Offline Mode.png]]

Good files to cache are file dont change too much, like asset. We dont want to cache every time maybe just when files in service worker change so `install event` is a good place where add some logic because that event fire when a new version of service worker is deployed 

### Cache

assets contains the request urls that also are keys to serve the cache data. In other words contains a list of file urls and when our application calls that specific file, with a fetch request that request are not made and instead cache data is served; this happen just if we are in offline mode.



---

## to order

https://www.youtube.com/watch?v=hxiggHZOGlQ&list=PL4cUxeGkcC9gTxqJBcDmoi5Q2pzDusSL7&index=6

https://web.dev/articles/service-worker-lifecycle

remember  "include": ["src", "public/pwa/snippets/sw.ts"], ts config and ", "WebWorker"], in lib

### Cleanup Outdated Caches[​](https://vite-pwa-org.netlify.app/guide/inject-manifest#cleanup-outdated-caches)

The service worker will store all your application assets in a browser cache (or set of caches). Every time you make changes to your application and rebuild it, the `service worker` will also be rebuilt, including in its precache manifest all new modified assets, which will have their revision changed (all assets that have been modified will have a new version). Assets that have not been modified will also be included in the service worker precache manifest, but their revision will not change from the previous one.

Precache Manifest Entry Revision

The precache manifest entry revision is just a `MD5` hash of the asset content, if an asset is not modified, the calculated hash will be always the same.

When the user installs the new version of the application, we will have on the service worker cache all new assets and also the old ones. To delete old assets (from previous versions that are no longer necessary), and since you are building your own service worker, you will need to add the following code to your custom service worker:
https://vite-pwa-org.netlify.app/guide/inject-manifest
