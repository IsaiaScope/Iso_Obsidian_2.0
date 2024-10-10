---
tags:
  - Fincons/Filmbank
  - Fincons/business_logic
---

# IFramePlayer

## Config

comunica tramite `post message` ➜ il parent istanza il player e invia la configurazione tramite post message ➜ quando viene inviato 'config' ➜ lancia la funzione di `initconfig` e vienme passatto come detail dell'evento il json che contiene tutta la configurazione ➜ viene chiamata la configurazione esterna cioè quella dell'istanza (education, qc ....) recuperandone il nome ➜ con axios va a chiamare la configurazione in base all'stanza ➜ e mergi le informazioni che arrivano dalla cdn e dal parent
`setconfiguration` ➜ setta `env`

---
