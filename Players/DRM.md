# DRM - digital right magement

_gestisce i diritti e crittografia dei chunk_ e la crittografazione avviene grazie ad aziende come `irdeto`, `comecast`, `castLab`, che in base alla configurazione che ti danno devi settare il player per avere la gestione del DRM.

## Gli standard di DRM

| DRM                                                       | From      | [DRM Support](https://developer.bitmovin.com/playback/docs/drm-content-protection) |
| --------------------------------------------------------- | --------- | ---------------------------------------------------------------------------------- |
| [Widevine DRM](https://pallycon.com/google-widevine-drm/) | Google    | All, not Safari                                                                    |
| Playready                                                 | Microsoft | Edge                                                                               |
| Fairplay                                                  | Apple     | Safari                                                                             |

### [Widevine](https://www.gumlet.com/learn/widevine-drm/)

`Widevine` is Google’s content protection system for premium media. Widevine is used by major content services around the world, including Google Play, YouTube, Netflix, Hulu, Amazon, and more.

Widevine is embedded in web browsers such as Chrome and Firefox, and devices with Android OS and other various OTT devices.

### [Playready](https://www.gumlet.com/learn/playready-drm/)

Microsoft's `PlayReady` serves as both a DRM solution and a platform dedicated to content protection and dissemination. Similar to `FairPlay` and `Widevine`, it offers a secure client-side SDK for content decryption and rendering. PlayReady includes a license server that manages licenses and keys during distribution between clients and servers. It supports streaming formats like MPEG-DASH, HLS and (MSS) and offers monetization models like Pay-per-view, Ad-based, Subscription, and Purchase/Rentals while implementing security measures against piracy.

### [FairPlay](https://www.gumlet.com/learn/fairplay-drm/)

FairPlay Streaming is a Digital Rights Management solution released by Apple. The purpose of this DRM is to promote secure delivery of digital media in the form of streaming. For that, it uses the `HLS protocol`. The streaming DRM is by default supported by iOS, tvOS, and macOS.

## Player Behaviour

il player va a tentativi usando tutti i DRM configurati;
perchè il player se va in errore con un drm ne prova un altro ma puoi anche digli di usare direttamente un specifico DRM inserendo una logica sia lato cliente che lato server sfruttando informazioni contenute nell'useragent

## Simple Implementation Example (Fairplay in Shaka-player)

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
