---
tags:
  - Fincons/develop
---

# Local Develop

## Chromecast

- _NOTE_ preview & standard ➜ cambia il config che vado a prendere ma sono gli stessi praticamente è indifferente quello che selezioni
- _NOTE_ test & prod ➜ cambia al database a cui vado a puntare (occhio perchè magari nella applicazione vedi i contenuti ma l'api se non è buildata con gli endpoint di prod non trova contenuti perchè va a cercare nel db di test)
- _NOTE_ `rte news` ➜ canale live che non ha bisogno della vpn

---

## WEB

- script for test environment ➜ start:preview_cr
- _NOTE_ se Chrome non ti fa proseguire col proxy perchè dice che la pagina non è sicura ➜ metti il focus sulla pagina e digita `thisisunsafe` ➜ sembra una cosa molto strana ma funziona fidati dell'isaia del passato
- VPN aws per vedere i contenuti geobloccati
- VPN forticlient per accedere alle macchine per deployare

---

## Sdp

### Installation & Start Local Development

- [ ] `yarn version` ➜ `v1.22.19`
- [ ] .NET dotnet-runtime-5.0.17-win-x64 must be installed
- [ ] intellij ➜ ..rte-kmp\app-core ➜ `.\gradlew clean BuildJs`
- [ ] VS ➜ ...rte-kmp\app-web ➜ `yarn install` ➜ `yarn upgrade-deps` ➜ `yarn start:bigscreen:rte`
- [ ] URl ➜ `http://localhost:4200/rpng-static/smarttv/home` (Puoi fare un check in env.js sotto BASE_PATH per vedere dove è servita)
- [ ] Nel developer tool selezionare la dimensione responsive 1920x1080 Big Sceen ➜ perchè utilizza questo specifico user agent per simulare di essere una TV e di conseguanza evitare problematiche con i CORS(`Mozilla/5.0 (Linux; x86_64 GNU/Linux) AppleWebKit/601.1 (KHTML, like Gecko) Version/8.0 Safari/601.1 WPE Sky_OTT_RTD1319_2020/1.0.0 (Sky, XiOneUK, Wired)`)
- [ ] _NOTE_ adding `res_1280x720` as a class to body set resoltion to 1280pxx720px

---

### Useful Interaction

- [ ] _NOTE_ `$$('video')[0].currentTime = $$('video')[0].duration` ➜ lanciato in console di chrome permette di schippare la pubblicità corrent (da rilanciare per ogni pubblicità)
- [ ] _Disattivare la pubblicità_ ➜ ..sdp\sdp\app-web\apps\bigscreen-client\pages\[firstLevel]\[secondLevel]\[guidOrId]\play\index.tsx ➜ nel jsx nel config che passiamo al player mettere {adv: {enabled: false}}
- [ ] _NOTE_ Ogni volta che modifichi il codice devi uscire dal contenuto e farlo ripartire
- [ ] _NOTE_ per cambiare la selezione iniziale in modo da passare da test a prod ➜ devi fare clear site data dal tab application ➜ i primi due tab nella selezione sono per test e contengono i puntamenti a test con i contenuti parsati da elemental, i secondi due sono di prod (ma gli analytic puntano cmq a test)

---

## LLamapannel

- [ ] check `KeeWeb` ➜ `LLamapannel` ➜ login skyqtools.uk/ da Firefox
- [ ] {"url": "https://test.rte.ie/sky_ctv_test"} ➜ copiare il json di url
- [ ] prima conf ➜ inserire ip della box nell'elenco generico dove puoi dargli anche un nome e aggiungere alla fine dell'ip la porta ➜ 192.168.5.198:9005
- [ ] quando entri premere connect e poi l'H in alto a destra, copiare il json di url nella label 'App Launch Parameters', poi selezionare com.rte.ie e cliccare L (per avviare il processo) o K per killarlo

---
