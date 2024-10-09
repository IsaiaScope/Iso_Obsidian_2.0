dds-repository-servicelayer-login
progetti Maven per la gestione delle dipendenze, quindi ce un file di configuration pom.xml, la parte più importante del file sono <dependecy> per usere librerie 
Progetti maven 
- direcotries 
	- package
		- src/main/java/* and * is like filmbank
	- extra: contiene utilies per aiutare lo sviluppatore  per esempio nella cartella run 
	- src contene
		- main è la dirctory che viene poi usata per portare il codice in prod e produrre un jar che contiene tutte le classi java
		- test - non viene portata in prod
	-  src/main/java/filmbank/service - 
		- classi sempre presenti
			- service application - classe di avvio che deve essere indicato all'inizio dell'app
			- paths - contiente tutti i paths utile per vedere a quali srvizi si aggangia in maniera veloce
			- service configuration - configurazioni per il servizio corrente contine dds-repository-servicelayer-login alcune possono essere solo per questo servizio oppure condivise con altre con extend
		- resource - sono gli endpoint
			- 
proprie o di terzi con la versione annessa
pogetti java8