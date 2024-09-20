---
tags:
  - Fincons/develop
---

# Develop

## IFramePlayer

- [ ] `npm run dev:app`
- [ ] `npm run dev:parent`
- [ ] `quality-check` test e avvii un contenuto del player ➜ network ➜ preserve log ➜ chiamata di login ➜ payload ➜ copy `cognitoId`
- [ ] `node scripts/filmbank-player-configuration.js` nel prompt e inserisci il `cognitoid` direttamente in console dove è chiesto e viene copiato in automatico sul tasto dx il json di configurazione
- [ ] nel browser ➜ http://localhost:3001 il parent per capirci ➜ fare `init` nel config copiare il json recuperato nel punto precedente
- [ ] _NOTE_ se l'oggetto di configurazione non dovesse funzionare vai pure in test su QCheck e fai partire un contenuto; nella console filtra per: [dds:init] e così ottieni il config che ti serve

---

## WordPress

### Podman

#### Installation

- [ ] `~/.docker/config.json`➜ cambiare `credStore` senza la s ➜ `credsStore`
- [ ] `podman compose up` nella dir progetto
- [ ] prima pagina di login per scaricare il db di word press completare il form grossolanamete
- [ ] aprire il wordpress di test e sotto 'my site' scegliere quale vuoi copiare
- [ ] copiare settings ➜ permalinks ➜ Day and name,
- [ ] da user ➜ utente creato prima ➜ eliminare barra in alto che da fastidio ➜ togliere spunta Show Toolbar when viewing site
- [ ] filmbank ➜ copiare tutto
- [ ] tools ➜ export all (dal sito di test) ➜ poi import nel locale
- [ ] problema di spazio per l'import dei video ➜ podman container web-1 ➜ terminal ➜ root ➜ file htaccess ➜ aggiungere alla fine

```
php_value upload_max_filesize 256M
php_value post_max_size 256M
php_value max_execution_time 300
php_value max_input_time 300
```

- [ ] appearance ➜ theme ➜ selezionare quella di filmbank

#### Develop

- [ ] avviare i container quando si sviluppa
- [ ] poi andare su localhost:8080
- [ ] _NOTE_ se non funziona CSS spuntare disable cache in modo che smette di tenere in memoria il vecchi file e lavora con quello nuovo aggiornato
- [ ] _NOTE_ remember to build CSSs with package.json script

---

## Downloadmanager (ngpwa)

### PWA

- [ ] fai la build e la servi con `npm http-server` dalla cartella di dist ➜ per testare la `pwa`
- [ ] _NOTE_ non usare MODALITA AEREO o togliere WiFi ➜ usare devtool e mettersi offline

---
