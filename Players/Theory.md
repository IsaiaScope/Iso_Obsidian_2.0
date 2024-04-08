---
tags:
- players
---

# Theory

| Concept  | Description/Link |
| -------- | ---------------- |
| DRM      |                  |
| manifest |                  |
| captions/subtitles         |                  |

##

DRM - digital right magement - gestisce i diritti e crittografa i chunk e la crittografazione avviene tipo con irdeto, comecast, castlab (azienda cje ti permette di fare la crittografia dei chunk) e in base alla configurazione che ti danno devi configurare il player per avere la gestione del drm
drm per shaka e antri player sono:
widewine - per tutti non va su safari
playready - edge
fairplay- safari
il player va a tentativi usando tutti i drm configurati o per esempio con useragent - perchè il player va in errore con un drm ne prova un altro ma puoi digli di fare direttamente provare uno specifico lato client o server ritornasndo l'user agent

manifest hls - formato file m3u8 (tipo file di testo) che descrive come recuperare i chunck - hls è un file di testo che descrive a quale minutaggio corrisponde un determinato chunk, questo è per safari e configura fairplay come drm sviluppato da apple
manifest dash formato mpd (se lo apri sembra un xml) usa xml file di testo per i chunk (che sono file fisici storati sulla macchia server e quindi possono essere storati e salvati localmente) e si usa su quello che non è apple
chrome può usare sia m3u8 che dash verificare che il player usa muxjs per usare m3u8 su windows perchè m3u8 è solo player

```js
  /**

   * @param {string} _certificateUrl

   * @param {string} _licenseUrl

   * @param {string} _manifest

   * @return {Promise<*>}

   */

  fairplay(_certificateUrl, _licenseUrl, _manifest) {

    return fetch(_certificateUrl)

      .then((response) => response.arrayBuffer())

      .then((data) => {

        const cert = data;

        this.player.configure(

          'drm.advanced.com\\.apple\\.fps\\.1_0.serverCertificate',

          new Uint8Array(cert)

        );



        this.player.configure('drm.initDataTransform', (initData) => {

          const contentId =

            this.shaka.util.FairPlayUtils.defaultGetContentId(initData);

          const _cert = this.player.drmInfo().serverCertificate;

          return this.shaka.util.FairPlayUtils.initDataTransform(

            initData,

            contentId,

            _cert

          );

        });



        this.player

          .getNetworkingEngine()

          .registerRequestFilter((type, request) => {

            if (type !== this.shaka.net.NetworkingEngine.RequestType.LICENSE) {

              return;

            }

            request.headers['Content-Type'] = 'application/octet-stream';

          });



        this.player

          .getNetworkingEngine()

          .registerResponseFilter((type, request) => {

            if (type !== this.shaka.net.NetworkingEngine.RequestType.LICENSE) {

              return;

            }

            request.data = new Uint8Array(request.data);

          });



        this.player.configure(

          'drm.servers.com\\.apple\\.fps\\.1_0',

          _licenseUrl

        );

        return this.player.load(_manifest);

      });

  }
```

---


sottotitoli wvtt verificare DDSTextDisplayer sono supportati e se i colori si vedono bene i colori sono una specie di sottitoli che usano hmtl
locals.js è un import di shaka che è una lista enorme di latino e english per i sottotitoli e se non è una di queste lingue non sivedono bene i sottotitoli quindi in +../../dds/language_mapping.js che permette di aggiungere alla lista queste label
vedere come aggiungere queste voci da documentazione se è possibile

Configuring text displayer -> wvtt perchè colori e html devono essere rispettati
e aggiungere lingue in più