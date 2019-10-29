# Prima Local Stack (pls)

## Prima installazione

* Installa `minikube` seguendo [questa guida](https://github.com/primait/board/wiki/Kubernetes#installazione-minikube-per-linux)
* Clona questo repository 
  * `git clone git@github.com:primait/pls.git`
* Installa `pls` in locale con
  * `cd pls`
  * `./bin/pls install`
  * **--> Segui le istruzioni <--**
* Aggiungi Prima allo stack seguendo [questa guida](#avviare-il-progetto-prima)

## Start/stop

Gestisci lo stack Kubernetes e i servizi associati con:

* `pls start`
* `pls stop`

## Gestione servizi

* Aggiungi con `pls add *nome_servizio*` 
 * Se non hai giá clonato il repo in locale verrá fatto `git clone` automaticamente
* Rimuovi con `pls rm *nome_servizio*`
* Riavvia con `pls restart *nome_servizio*`

In caso tu abbia modificato il container Docker, `pls rebuild *nome_servizio*`

🔎 _FYI_: Puoi passare multipli servizi (ad es. `pls add prima borat hal9000`)

⚠️ **NB**: Se avevi l'ambiente in locale giá configurato, assicurati di aggiornare i seguenti file con i valori presenti nell'omonimo file `*.dist`
  * Prima
    * `app/config/parameters.yml`

## Accedere a un servizio

* Apri il servizio sul browser (se disponibile)
  * `pls open *nome_servizio*`
* Ottieni l'url del servizio 
  * `pls url *nome_servizio*`
* Accedi alla shell del servizio
  * `pls bash *nome_servizio*` 
  * 🔎 _FYI_: Se disponibili piú container, puoi entrare in un container specifico con `pls bash *nome_servizio* *nome_container*` (ex. `pls bash prima nginx`)

⚠️ **NB**: A parte alcune eccezioni (Prima e Borat), Kubernetes assegna a ogni servizio una porta random in range 30000~3xxxx che potrebbe variare tra un riavvio e l'altro. Puoi settare una porta specifica settando `kubernetes.yml -> spec -> ports -> nodePort` (guarda come esempio di Prima)

## Debugging 

* Per accedere in _bash_
  * `pls bash *nome_servizio*` 
* Per leggere i log
  * `pls log *nome_servizio*`
* Per visionare lo status dettagliato di un container
  * `pls inspect *nome_servizio*`
* Per visionare lo status generale di Kubernetes
  * `pls status`
  
## Servizi supportati

* Prima
* Borat
* Peano
* Hal9000
* Roger
* Bburago

## Avviare il progetto Prima 

* `pls add prima`
* `pls dump` 
  * Per ottenere la password del db
    * configura AWS in locale
    * clona il repo di Artemide
    * `biscuit get -f artemide/configs/secrets/common.yml common_staging_db_mysql_prima`
* `pls restore`
* Copia il file `app/config/parameters.yml.dist` in `app/config/parameters.yml`
  * Se utilizzavi Docker, rinomina il file `parameters.yml` in `parameters.yml.backup`

## Aggiornamento

É possibile aggiornare `pls` all'ultima versione con `pls update`

## 🆘 Troubleshooting
* Kubernetes mi da problemi di permessi!
  * `pls fix`
* Prima non funziona!
  * Assicurati che il file `parameters.yml` sia aggiornato con i valori presenti in `parameters.yml.dist`
* `cURL error 7: Failed to connect to *servizio*-service port XXXX: Connection refused (see http://curl.haxx.se/libcurl/c/libcurl-errors.html)`!
  * `pls add *servizio*`
* `pls add *servizio*` da errori strani!
  * Assicurati che `*servizio*` sia su `master` e di aver fatto `git pull`. In caso dia ancora errori, assicurati che sia presente un `kubernetes.yml.dist`. Se non é presente significa che ancora non é stato configurato per funzionare con `kubernetes`.
* `pls dump` esplode con `mysqldump: command: not found`!
  * `sudo apt-get install mysql-client`
* É tutto rotto e la vita fa schifo.
  * `pls reset` (⚠️ **NB**: Resetta _tutto_)
