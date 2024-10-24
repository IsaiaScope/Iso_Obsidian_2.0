---
tags:
  - Fincons/Filmbank/backend
  - Java/v8
---

# Generic view of a repository

## For example `dds-repository-servicelayer-login`

I progetti Filmbank sono progetti `java8 Maven` per la gestione delle dipendenze e della build , quindi c'è un file di configuration `pom.xml`, la parte più importante di questo file sono le `<dependecy>` che gestiscono come vengono prese le librerie dipendenti sia interne che esterne con la versione annessa

- Directories Structure - divisa in package (like `ServiceLogin`)
- `src/main/java/*` where `*` is like filmbank
  - `extra`: contiene utilies per aiutare lo sviluppatore per esempio nella cartella run
  - `ServiceLogin/src` contene
    - `main` è la dirctory che viene poi usata per portare il codice in prod e produrre un `jar` che contiene tutte le classi java
    - `test` ➜ non viene portata in prod
  - `ServiceLogin/src/main/java/filmbank/service`
    - _classi sempre presenti_
    - `ServiceApplication` ➜ classe di avvio che deve essere indicato all'inizio dell'app e specifica tutto quello che serve in generale
    - `paths` ➜ contiene tutti i paths utile per vedere a quali srvizi si aggancia in maniera veloce
    - `ServiceConfiguration` ➜ configurazioni per il servizio corrente `dds-repository-servicelayer-login` contiene alcune classi che possono essere solo per questo servizio oppure condivise con altre con `extend`
    - `resource` ➜ contiene gli endpoint e come vengono gestiti (in un buon pattern MVC questo pacchetto deve essere ignorante e definire solo le route) \
    - `business` ➜ contiene la logica di business con cui calcolare le risposte alle route
    - `buildspec.yml` ➜ gestisce il deploy e la pipeline

---

[Dropwizard](https://www.dropwizard.io/en/stable/) è un `framework` installato, che rispetto a una libreria installa a codice che richiamiamo e usiamo dove volgiamo, si istanzia da sola all'avvio dell'applicativo e gestisce tutto lui per noi ma ti obbliga a lavorare in una certa maniera impostando un processo all'interno del quale sei costretto a al vorare. Ha una sua chain di sviluppo ➜ quando avviene una richiesta Dropwizard permette di avere delle validazioni prima che arrivi alla resources (routes) consentendo di inserirsi in punti e momenti specifici.

---

## Going in details

service application parte da li
config yml contiene la configurazione 
![[run debugconfiguration.png]]
![[local run.png]]

fare richiesta con post man di test e vedere i log su open serach