# Prima Local Stack (pls)

## Prima installazione

* Clona questo repository 
  * `git clone git@github.com:primait/pls.git`
* Installa `pls` in locale con
  * `cd pls`
  * `./bin/pls install`
* Segui le istruzioni

## Start/stop

Avvia/ferma lo stack Kubernetes e i servizi associati con

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

* Per accedere in _bash_, `pls bash *nome_servizio*` 
* Per leggere i log, `pls log *nome_servizio*`
* Per visionare lo status dettagliato dei container, `pls inspect *nome_servizio*`
* Per visionare lo status generale di Kubernetes, `pls status`

## Aggiornamento

É possibile aggiornare `pls` all'ultima versione con `pls update`

## 🔥 Oh no é tutto rotto 🔥
* Prima non funziona!
  * Assicurati che `parameters.yml` sia configurato con i valori presenti in `parameters.yml.dist`
* `cURL error 7: Failed to connect to test-*servizio*-service port XXXX: Connection refused (see http://curl.haxx.se/libcurl/c/libcurl-errors.html)`!
  * `pls add *servizio*`
* Kubernetes mi da problemi di permessi!
  * `pls fix`
* Opzione nucleare
  * `pls reset` (⚠️ **NB**: Resetta tutto)

## Problemi da risolvere
* Rabbit viene tirato su senza venir configurato. Questo vuol dire che i servizi che utilizzano exchange si potrebbero rompere (`NOT_FOUND - no exchange 'entity' in vhost '/'`)
  * Soluzione: Puó venire risolto creando manualmente le exchanges/queues richieste (`pls open rabbit` e datti da fare 👏👏)
* Le migration potrebbero non corrispondere se si fa il dump da DB e non si é allineati. 
  * Soluzione: Apri il tuo client DB e vedi sopra.