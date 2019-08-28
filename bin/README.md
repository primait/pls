# pls (Prima Local Stack)

## Prima installazione

### Linux
- installare `kubectl` e `minikube` seguendo la [guida](https://github.com/primait/board/wiki/Kubernetes-experiments)
- installare `pls` in locale aggiungendo la directory `bin` di questo repository al vostro PATH (ad es. aggiungendo al file `~/.bashrc` o `~/.bash_profile` la seguente riga:
```
export PATH=$PATH:~/**PRIMA PROJECTS DIRECTORY**/artemide/kubernetes-experiments/pls/bin
```
- eseguire `pls setup` per effettuare le operazioni preliminari di build dei container
- eseguire `pls dump && pls restore` per allineare i db in locale con staging
- abilitare i servizi con `pls add prima hal9000 borat peano`

Ora lo stack dovrebbe essere funzionante. Prova a entrare su prima con `pls open prima`!
- in caso non funzionasse, puoi provare con `pls log prima` 
- in caso ci fossero ulteriori problemi, puoi investigare il servizio con `pls inspect prima` o lo status generale con `pls status`

## Avvio e utilizzo

- Per avviare kubernetes, `pls start`
- Per monitorare lo status di kubernetes e dei servizi, `pls status`
- Per ottenere informazioni sullo status k8s di un servizio, `pls inspect *servizio*`
- Per abilitare un servizio, `pls add *servizio*` (Dovrá avere il file `kubernetes.yml` configurato)
- Per disabilitare un servizio, `pls remove *servizio*` 
- Per leggere i log di un servizio, `pls log *servizio*`
- Per entrare nel terminale di un servizio, `pls bash *servizio*`
- Per aprire il servizio nel browser, `pls open *servizio*`
- Per riavviare un servizio, `pls restart *servizio*`
- Per fermare kubernetes, `pls stop`

## Servizi e infrastruttura

- Servizi 
	- Prima 
		-> Borat K8S
		-> Hal9000 K8S
		-> Bburago PROD
		-> Urania Staging
	- Borat
		-> Peano K8S
		-> Prima K8S
	- Hal9000

- Infrastruttura
	- PostgreSQL
	- MySQL
	- Rabbit
	- Redis

`->` = parla con 

## Troubleshooting

* Prima
	* Qualcosa non funziona!
		* Assicurati che hai effettuato effettuato il dump dei dati da staging (`pls dump`) e caricato i dati in locale (`pls restore`)
		* Prima: Controlla che in `parameters.yml`, `api_key_frontend` e `api_key_backoffice` siano settate correttamente rispetto alla tabella `security_apikey`
		* Controlla che le migration siano state eseguite correttamente (`pls bash prima php` > `php bin/console doctrine:migrations:migrate`)
		* Assicurati che hal9000 sia abilitato (`pls add hal9000`)
		* Assicurati che hal9000 abbia importato con successo gli engine (Entra su DB postgres e controlla che le tabelle siano popolate. Nel caso non lo siano, `pls restart hal9000 && pls log hal9000` e controlla che non ci siano errori durante l'esecuzione di `mix import.engines`)
* Borat
	* `** (Mix) The database for Borat.Repo couldn't be dropped: ERROR 55006 (object_in_use) database "borat" is being accessed by other users`
		* `pls restart postgres borat`
	* La ricerca non funziona
		* Assicurati che non siano stati inseriti i seed della tabella `ricerca`. In caso, svuota la tabella e riprova a creare un save da Prima.
		* Assicurati che `Application.get_env(:borat, :prima)[:apikey]` sia popolato con una apikey di un operatore dotato dei permessi corretti. (ProTip: usa la apikey di Arianna)
