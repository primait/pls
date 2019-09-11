# Prima Local Stack (pls)

## Prima installazione

* Installa minikube seguendo [questa guida](https://github.com/primait/board/wiki/Kubernetes#installazione-minikube-per-linux)
* Clona questo repository 
  * `git clone git@github.com:primait/pls.git`
* Installa `pls` in locale con
  * `cd pls`
  * `./bin/pls install`
* Segui le istruzioni

## Start/stop

Avvia/ferma lo stack Kubernetes e i servizi associati con:

* `pls start`
* `pls stop`

## Gestione servizi

* Aggiungi con `pls add *nome_servizio*`
* Rimuovi con `pls rm *nome_servizio*`
* Riavvia con `pls restart *nome_servizio*`

In caso tu abbia modificato il container Docker, `pls rebuild *nome_servizio*`

🔎 _ProTip_: Prova a passare multipli servizi! (ad es. `pls add prima borat hal9000`)

⚠️ **NB**: Se avevi l'ambiente in locale giá configurato, assicurati di aggiornare i seguenti file con i valori presenti nell'omonimo file `*.dist`
  * Prima
    * `app/config/parameters.yml`

## Accedere a un servizio

A parte alcune eccezioni (Borat), Kubernetes assegna a ogni servizio una porta random in range 30000~3xxxx.
Per poter aprire direttamente il servizio nel browser , `pls open *nome_servizio*`.
In caso tu sia interessato soltanto agli indirizzi a cui é possibile accedere, `pls url *nome_servizio*`

⚠️ **NB**: Alcuni servizi (Borat, Rabbit) espongono multiple porte; in questo caso ti verrá presentato un prompt per scegliere quale aprire.

## Debugging 

* Per accedere in _bash_
  * `pls bash *nome_servizio*` 
  * 🔎 _ProTip_: Se disponibili piú container, verrá aperto il primo che capita. Puoi entrare in un container specifico con `pls bash *nome_servizio* *nome_container*` (ex. `pls bash prima nginx`)
* Per leggere i log
  * `pls log *nome_servizio*`
* Per visionare lo status dettagliato di un container
  * `pls inspect *nome_servizio*`
* Per visionare lo status generale di Kubernetes
  * `pls status`

## Aggiornamento

É possibile aggiornare `pls` all'ultima versione con `pls update`

## Avviare il progetto Prima 

* `pls add prima`
* `pls dump` 
  * Per ottenere la password 
    * configura AWS in locale
    * clona il repo di Artemide
    * `biscuit get -f artemide/configs/secrets/common.yml common_staging_db_mysql_prima`
* `pls restore`
* Copia il file `app/config/parameters.yml.dist` in `app/config/parameters.yml`
  * É necessario comunque modificare alcuni valori in `parameters.yml`. Troverai istruzioni al suo interno.

## 🆘 Troubleshooting
 
* `cURL error 7: Failed to connect to test-*servizio*-service port XXXX: Connection refused (see http://curl.haxx.se/libcurl/c/libcurl-errors.html)`!
  * `pls add *servizio*`
* `pls dump` mi ci manda con `mysqldump: command: not found`!
  * `sudo apt-get install mysql-client`
* Kubernetes mi da problemi di permessi!
  * `pls fix`
* ☢️ Opzione nucleare ☢️
  * `pls reset` (⚠️ **NB**: Resetta tutto)