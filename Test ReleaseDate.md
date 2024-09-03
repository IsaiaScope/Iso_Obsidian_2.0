- [ ] [Bucket S3](https://eu-west-1.console.aws.amazon.com/s3/buckets?region=eu-west-1&bucketType=general)
- [ ] _dds-test-ingest_ ogni directory rappresenta un contenuto e dentro ogni contenuto ci posso essere più versioni del medesimo contenuto
- [ ] ogni versione di un contenuto contiene i metadati (metadata\_`id`\_json). Poi contiene la parte video e la parte dei sottotitoli (media folder) e poi gli assets e le immagini (assets folder). Poi abbiamo il file _execute-innest.txt_ che è il file che serve ad AWS per far partire la pipeline; quindi solo modificando questo file (inserendo testo completamente arbitrario) e salvando farò triggerare la pipeline del contenuto nel bucket facendo upload dopo aver modificato il file txt
- [ ] Sull'Adminportal nel title manager posso vedere la pipeline partire cercando il contenuto con l'id sul bucket - l'icona col grafico permette di vedere le pipeline in corso su quel contenuto e seleziona il toggle per l'auto refresh per fare in modo che l'elenco si aggiorni in live (deploy is completed if process goes from step 1 to 11)
- [ ] Accertarsi dell'aggiornamento su Open Search
  - [ ] [Open Search CONTENTS](https://vpc-dds-test-elk-feed-opensearch-nne2ldmqzp6twcchbbesi6vb3e.eu-west-1.es.amazonaws.com/_dashboards/app/home#) - discover - indice content - filter filmbankId: id
- [ ] Accertarsi dell'aggiornamento anche a DB(DBeaver - SQL)
  - [ ] _dds-utils-configuration_ - _ms-config.yml_ di test - sotto DB ha le credenziali da inserire per configurare DBeaver
  - [ ] DBeaver - database - backend - dds_contents - con query (menù in altro selezioni sql e new script): SELECT \* FROM DDS_CONTENTS dc WHERE dc.FILMBANK_ID = 00099999; - questo esempio recupera tutti i dati del contenuto e posso verificare e modificare i metadati
- [ ] Altra risorsa importante è un altro Open Serach differente dal precedente che contiene tutti [Open Search LOGS](https://vpc-dds-test-elk-monitoring-p7ar64ikjyd6rhz5gy3jmeorty.eu-west-1.es.amazonaws.com/_dashboards "https://vpc-dds-test-elk-monitoring-p7ar64ikjyd6rhz5gy3jmeorty.eu-west-1.es.amazonaws.com/_dashboards") - discover - filter: _service: login and level: error_ è un esempio, _service: workflow and message: "INGEST WORKFLOW"_ è un altro esempio che filtra l'inizio dei vari step di ingest sul bucket s3 quando vado a modificare i _execute-innest.txt_.
- [ ] _NOTE_ l'Adminportal cerca su Open Search e quindi può capitare che trova contenuti che non ci sono su s3 cioè i contenuti non esistono più effettivamente. Open Search offre un ecosistema di fare query complesse di ricerca e modifica agganciandoci ad un db s3 economico e ma poco performante per calcoli di query. La cancellazione di contenuti su Open Search non avviene in maniera automatica quando un contenuto viene cancellato su s3, cancellando un contenuto sull'Adminportal vado a cancellarlo su s3 e per una mancanza di implementazione non avviene lo stesso su Open Search.
- [ ] _NOTE_ Per le Series aggiungere theatricalReleaseDate a "movie", "program" , "season"

Movies

- [ ] [00010597](https://eu-west-1.console.aws.amazon.com/s3/buckets/dds-test-ingest?prefix=00010597/) - 001 - "theatricalReleaseDate": "2024-05-31",
- [ ] [00026283](https://eu-west-1.console.aws.amazon.com/s3/buckets/dds-test-ingest?prefix=00026283/) - 001 - "theatricalReleaseDate": "2023-05-25",
- [ ] [00029006](https://eu-west-1.console.aws.amazon.com/s3/buckets/dds-test-ingest?prefix=00029006/) - 001 - "theatricalReleaseDate": "2021-01-16",
- [ ] [00031740](https://eu-west-1.console.aws.amazon.com/s3/buckets/dds-test-ingest?prefix=00031740/) - 001 - "theatricalReleaseDate": "2022-04-22",
- [ ] [00018615](https://eu-west-1.console.aws.amazon.com/s3/buckets/dds-test-ingest?prefix=00018615/) - 001 - "theatricalReleaseDate": "2023-03-23",

Series

- [ ] [61236192](https://eu-west-1.console.aws.amazon.com/s3/buckets/dds-test-ingest?prefix=61236192/) - 001 - "theatricalReleaseDate": "2019-07-12", title: 3% - episode title: Chapter 01: Cubes - change program, season and episode
- [ ] [60973401](https://eu-west-1.console.aws.amazon.com/s3/buckets/dds-test-ingest?prefix=60973401/) - 001 - "theatricalReleaseDate": "2018-07-12", title: The Good Doctor - episode title: Hello - change program, season
- [ ] [60062226](https://eu-west-1.console.aws.amazon.com/s3/buckets/dds-test-ingest?prefix=60062226/) - 001 - "theatricalReleaseDate": "2017-07-12", title: The Simpsons - episode title: Hello - change program
- [ ] [61536324](https://eu-west-1.console.aws.amazon.com/s3/buckets/dds-test-ingest?prefix=61536324/) - 001 - "theatricalReleaseDate": "2019-07-12", title: The Good Doctor - episode title: Hello - change program, season and episode
