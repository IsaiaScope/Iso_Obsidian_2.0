---
tags:
- players
---

> sorry for italian hard topic needs hard time

# Theory

| Concept            | Description/Link |
| ------------------ | ---------------- |
| DRM                |   [Go](./DRM)               |
| manifest           |       [Go](./DRM)           |
| captions/subtitles |        [Go](./DRM)          |

##

goog.provide('shaka.cast.CastProxy'); export
goog.require('shaka.Player'); import
quando i moduli non era supportato

@privare vuole dire veramente che è privato e inaccessibile

---

sottotitoli wvtt verificare DDSTextDisplayer sono supportati e se i colori si vedono bene i colori sono una specie di sottitoli che usano hmtl
locals.js è un import di shaka che è una lista enorme di latino e english per i sottotitoli e se non è una di queste lingue non sivedono bene i sottotitoli quindi in +../../dds/language_mapping.js che permette di aggiungere alla lista queste label
vedere come aggiungere queste voci da documentazione se è possibile

Configuring text displayer -> wvtt perchè colori e html devono essere rispettati
e aggiungere lingue in più

----------------

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