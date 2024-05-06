---
tags:
- players
---

> sorry for italian hard topic needs hard time

# Theory

| Concept     | Description/Link |
| ----------- | ---------------- |
| DRM         | [Go](./DRM)      |
| manifest    | [Go](./DRM)      |
| CDN         |                  |
| Text Tracks | [Go](./DRM)      |

## Introduction - What is [CDN](https://www.gumlet.com/learn/what-is-video-cdn/?

A Content Delivery Network (`CDN`) is a geographically distributed network of servers that works to speed up the delivery of internet content. Imagine a CDN like a giant storage network for websites, with outposts all over the world.

For example, think about a popular video streaming service like Netflix. When you hit play on a movie, the data for that movie isn't necessarily coming from Netflix's headquarters. Instead, a CDN might store a copy of the movie on a server closer to you. This way, the data doesn't have to travel as far, resulting in a faster and smoother playback experience.

##

goog.provide('shaka.cast.CastProxy'); export
goog.require('shaka.Player'); import
quando i moduli non era supportato

@privare vuole dire veramente che è privato e inaccessibile

---

Shaka player web => player events state =>  
3.110.0_shaka-player  
player service 615  
player component => add subscription => player events state.subscribe passano tutti gli eventi  
tramite api di shaka aggiungo i bottoni o li metto custom  
objectdetail => contiene i dati del media  
if rte$recapStar => show recap button  
app next chiamata per thumbnail  
if there is timeupdate => siamo sicuri che non ci sia la pub

> initClientSide method nativo di shaka per gestire le ads  
> per client side uso initClientSide che gestisce la pubblicità sotto il culo invece per il server side mando il manifest e mi ritorna con la pubblicità aggiunta e poi passo il nuovo manifest al player senza aver passato prima quello  
> ima => adv google => sdk di google che si legge la url delle chiamate e ritorna tutti i manifest delle pubblicità no seek  
> ima sdk integration => shaka player + theo hai la possibilità di usare client id insertion lato client o farlo lato server
