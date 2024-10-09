---
tags:
  - Fincons/Filmbank/back-end
  - Java
---

# Generic view of a repository

## For example `dds-repository-servicelayer-login`

I progetti Filmbank sono progetti `Maven` per la gestione delle dipendenze, quindi ce un file di configuration pom.xml, la parte più importante del file sono <dependecy> per usere librerie
Progetti maven

- direcotries - package - src/main/java/_ and _ is like filmbank - extra: contiene utilies per aiutare lo sviluppatore per esempio nella cartella run - servicelogin/src contene - main è la dirctory che viene poi usata per portare il codice in prod e produrre un jar che contiene tutte le classi java - test - non viene portata in prod - servicelogin/src/main/java/filmbank/service - - classi sempre presenti - service application - classe di avvio che deve essere indicato all'inizio dell'app - paths - contiente tutti i paths utile per vedere a quali srvizi si aggangia in maniera veloce - service configuration - configurazioni per il servizio corrente contine dds-repository-servicelayer-login alcune possono essere solo per questo servizio oppure condivise con altre con extend - resource - sono gli endpoint e vengono gestirli (in un buon pattern mvc questo pacchetto deve essere ignorante e definire solo le route) - business - contiene la logica di business con cui calcolare le risposte alle route - buildspec.yml gestisce il deploy e la pipeline
  proprie o di terzi con la versione annessa
  pogetti java8
  drop wizard è una libreria installata che rispetto ad averla installa a codice che richiamiamo e usiamo dove volgiamo esiste il concetto di framework si istanzia da sola all'avvio dell'applicativo e gestisce tutto lui per noi e ti obbliga a lavorare in una certa maniera impostando un processo all'interno del quale sei costretto a alvorare ha una sua chain di sviluppo - quando viene una richiesta drop wizard permette di avere delle validazioni prima che arrivi alla resouces (routes) inserendoci in punti specifici
