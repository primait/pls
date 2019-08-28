# Creazione di un nuovo stack con kubernetes

## Operazioni preliminari

Fare il checkout del branch experiment/k8s nei vari microservizi disponibili (hal9000, prima, peano, borat).
Portarsi sulla cartella dove sono presenti i suddetti microservizi e compilare i relativi containers:

- ngnix
```
docker build prima/app/docker/web/dev/ -t prima-nginx:dev
```

- php
```
docker build prima/app/docker/php/ -t prima-php:dev
```

- peano
```
docker build peano/ -t peano:dev
```

- borat
```
docker build borat/ -t borat:dev
```

- hal9000
```
docker build hal9000/ -t hal9000:dev
```

## Kubernetes cluster

Per l'installazione seguire la [guida](https://github.com/primait/board/wiki/Kubernetes-experiments)
All'interno di ogni progetto é presente un file `kubernetes.yml.dist`; copiateli in `kubernetes.yml` e, all'interno di ognuno, sostituite i placeholder {HOME} con la HOME del proprio utente, e {PROJECTS_HOME} con il path della root dei vari progetti (in modo che {PROJECTS_HOME}/prima, {PROJECTS_HOME}/hal9000, {PROJECTS_HOME}/etc... puntino alle directory dei progetti)

Fate partire i comandi:
```
kubectl apply -f infrastructure.yml
kubectl apply -f kubernetes.yml
```

## Operazioni preliminari sui dati

- dump dei dati di prima/peano
```
mysqldump prima --single-transaction -h {host} -u {user} -p \
    bo_operator \
    user_configuration \
    security_apikey \
    company \
    criteria_data \
    criteria_group \
    criteria_value \
    document_type \
    document_type_category \
    document_type_type_category \
    guarantee \
    guarantee_deductible \
    guarantee_limit \
    occupation \
    place_city \
    place_city_zip_code \
    place_country \
    place_province \
    place_region \
    place_zip_code \
    place_zone \
    pricer_config \
    pricer_discount \
    pricer_discount_code \
    pricer_guarantee_formula \
    migration_versions > prima_data.sql
```

- modificare borat per l'apikey (aggiungere il proprio utente qualora non ci fosse)

- importare i dati in hal9000
```
mix import.engines
```

## Utility commands

```
kubectl get services
```

```
kubectl get pod
```

```
kubectl logs --tail=200 -f -l app.kubernetes.io/name=hal9000
```

```
kubectl describe pod {pod-id}
```

```
kubectl exec -it {pod-id}  -- bash
```
