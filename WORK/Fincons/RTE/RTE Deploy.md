---
tags:
  - Fincons/deploy
---

# Deploy

## Chromecast

- [ ] FortiClient per collegarsi alle macchine RTE ➜ user: `ex-fincons`
- [ ] `WinScp` ➜ mi collego alle 2 MACCHINE di test contenute nella cartella test RTE-Test
- [ ] script di build ➜ di test o prod ➜ creo la dist e copio il contenuto della dist nel path `/var/www/html/chromecast_test` o `chromecast_prod` o `chromecast_dev` sulle 2 macchine di test di rte
- [ ] _NOTE_ ci sono due ambienti di test (test e prod-test) e uno di prod (prod si trova su ina macchina diversa `ftp.rte.ie` e il chromecast è nella cartella `caf`)

---

## RTE WordPress

- [ ] macchine virtuali copiare file per file nelle due macchine di test

---

## Smart TV

- [ ] _NOTE_ `Base_Path`: quello che definisce l'ambiente - Nextjs build sempre con basepath quindi bisogna cambiarla in fase di build

### Box Description

- Soip ➜ è il box più piccolo più performante con un browser più moderno
- Skyq ➜ è un tipo di box (ambiente più vecchio) => non supporta ES6 e quindi bisogna eliminare la liberia firbolt che build in es6
- Skyglass ➜ sky integrato sulla tv ma all'ufficio abbiamo un box con un modello 3D per simularlo (funziona col culo)

### cmd

- Build Soip ➜ build:bigscreen:rte (TEST) ➜ release:bigscreen:rte (PROD)
- Skyq ➜ build:bigscreen:rte:skyq:dev ➜ build:bigscreen:rte:skyq:prod
- Skyglass ➜ build:bigscreen:rte (TEST) ➜ release:bigscreen:rte (PROD) ➜ usa gli stessi comandi di Soip

---
