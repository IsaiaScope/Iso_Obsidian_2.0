# Service Worker

Service Worker are on a different thread they dont have access to the DOM, are background services 
![[Pasted image 20231124103317.png]]
https://www.youtube.com/watch?v=hxiggHZOGlQ&list=PL4cUxeGkcC9gTxqJBcDmoi5Q2pzDusSL7&index=6

Service Worker Lifecycle
![[Pasted image 20231124103547.png]]

https://web.dev/articles/service-worker-lifecycle



remember  "include": ["src", "public/pwa/snippets/sw.ts"], ts config and  ", "WebWorker"], in lib

### Cleanup Outdated Caches[​](https://vite-pwa-org.netlify.app/guide/inject-manifest#cleanup-outdated-caches)

The service worker will store all your application assets in a browser cache (or set of caches). Every time you make changes to your application and rebuild it, the `service worker` will also be rebuilt, including in its precache manifest all new modified assets, which will have their revision changed (all assets that have been modified will have a new version). Assets that have not been modified will also be included in the service worker precache manifest, but their revision will not change from the previous one.

Precache Manifest Entry Revision

The precache manifest entry revision is just a `MD5` hash of the asset content, if an asset is not modified, the calculated hash will be always the same.

When the user installs the new version of the application, we will have on the service worker cache all new assets and also the old ones. To delete old assets (from previous versions that are no longer necessary), and since you are building your own service worker, you will need to add the following code to your custom service worker:
https://vite-pwa-org.netlify.app/guide/inject-manifest