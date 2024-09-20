---

tags:

- Fincons/RTE
- Fincons/business_logic

---

# Chromecast

chiamata al selector => chiamo un link che ritorna un smilfile (xml) che contiene per esempio l'url pubblicità e il manifest che contiene info per il player cioè gli dice dove scraricare o prendere la roba che gli serve
quando viene fatta una load request viene fatta una chiamata al selector => il campo contentid dice il contenuto da recuperare lo smil con queryparam l'authtoken, policy e iu sono le informazioni per la pub e li recuperioamo da un config.json è un file su wordpress che viene aggiornato da rte; poi abbiamo assettype per il linguaggio dei segni
mpx = comecast
TEORIA:

- avvio progetto => np http-server => npm run dev => poi su altro terminale => ngrok http 8080
- console di chrome mettere ngrok => ip sotto application
- il token lo prendi andando su rte/player => application => cookie => mpx_token
- per i live oltre al token bisogna prendere anche il contenuto quindi vai su rte/player nella sezione live e => network => filtri le chiamate con 'link' => in headers il queryparams dopo media/ è l'id del live esempio: d3e0f15e4f1a6e676877d9848de84cb8
- anche per i vod prendi l'id lo prendi da li (IN SOSTANZA l'ID è L'ELEMENTO CHE VUOI STREAMMARE)
- per avere il listingId per le live devi adnare sulla live di rte news tra le chimate che vongo fatte è la terza => filtrate per all-listing e devi prendere il primo numero sul queryParam
- chiamate at internet => filter: logw309

- preview => punta chromecast id nostro => quello creato da lorenzo
- standard => 24i

- rte news => canale live che non ha bisogno della vpn
- id 24i => 67EE6062 basta aggiungerlo nell cactool e passsare il load message

- tipi di vod => crono, seasonal, movie, clip e extra
- fair city, marty in the shed => FhQS8sfY5sWy => serie senza stagioni si dice cronologica
- the late late show => clip
- extra => cera per a => selezia extra

- tipi di vod => crono, seasonal, movie, clip e extra
- fair city, marty in the shed => FhQS8sfY5sWy => serie senza stagioni si dice cronologica
- movie => becoming irish
- the late late show => clip
- extra => cera per a => selezia extra
- clip => boxing world champion
- extra => the panti monologues

test crono ati event:
from start, seek to end=> play, start, start(ads), resume, pause, stop, stop(ads), play, start(ads), pause, stop, stop(ads), play, resume, buffer.start, start, start(ads), resume, pause, stop, stop(ads), play, start, start(ads), pause, stop, stop(ads), play, resume, pause, stop

tansizione evento live =>

- non ha fatto rebuffer
- ati => evento stop (av_content_id precedente) , evento play e evento resume con nuovo av_content_id
- chimate =>
  - 3 watchlive => quindi il timeout è sincrono alla reale durata della live
  - 3 all-listing => le ultime due chimate con id uguale
  - 2 station (uguali) => una iniziale poi un'altra nella transizione e nella load non viene più fatta perchè abbiamo la description
  - 2 chimate selector => media/diverso

COSE UTILI:
contenuto solo irlandere vod => Home and Away

---
