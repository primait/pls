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
🔎 ProTip: Prova a passare multipli servizi! (ad es. `pls add prima borat hal9000`)

## Accedere a un servizio

A parte alcune eccezioni (Borat), Kubernetes assegna a ogni servizio una porta random in range 30000~3xxxx.
Per poter aprire direttamente il servizio nel browser , `pls open *nome_servizio*`.
In caso tu sia interessato soltanto agli indirizzi a cui é possibile accedere, `pls url *nome_servizio*`

*NB*: Alcuni servizi (Borat, Rabbit) espongono multiple porte; in questo caso ti verrá presentato un prompt per scegliere quale aprire.

## Debugging 

* Per accedere in _bash_, `pls bash *nome_servizio*` 
* Per leggere i log, `pls log *nome_servizio*`
* Per visionare lo status dettagliato dei container, `pls inspect *nome_servizio*`
* Per visionare lo status generale di Kubernetes, `pls status`

## Aggiornamento

É possibile aggiornare `pls` all'ultima versione con `pls update`

## 🔥 Oh no é tutto rotto 🔥

* Kubernetes mi da problemi di permessi!
  * `pls fix`
* Opzione nucleare
  * `pls reset` (*NB*: Resetta tutto)