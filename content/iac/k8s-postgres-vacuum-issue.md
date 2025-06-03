+++
title = 'K8s Postgres Vacuum Issue'
date = 2025-06-03T11:13:23+02:00
draft = false
tags = ["kubernetes", "postgresql", "vacuum"]
+++

## Erreur lors du `vacuum` d'une base de donnée PostgreSQL hébergée sur Kubernetes

Lors d'une tentative de récupération de stockage à l'aide de la commande `vacuum`, le message suivant
apparaît:

```psql
awx=# vacuum ;
ERROR:  could not resize shared memory segment "/PostgreSQL.3547573408" to 46825344 bytes: No space left on device
```

Par défaut le volume /dev/shm des conteneurs docker fait 64M cd qui peut être insuffisant pour PostgreSQL. Pour 
corriger le problème, on peut monter un volume mémoire sur `/dev/shm`.

Dans le cas d'AWX, le statefulset étant piloté par l'opérateur, j'ai édité directement le déploiement en cours
d'exécution, juste le temps de faire le `vacuum` de la base de données.

```yaml
apiVersion: apps/v1
kind: StatefulSet
...
spec:
  ...
  template:
    ...
    spec:
      containers:
        ...
        image: quay.io/sclorg/postgresql-15-c9s:latest
        ...
        volumeMounts:
        - mountPath: /var/lib/pgsql/data
          name: postgres-15
          subPath: data
        - mountPath: /dev/shm
          name: dshm
      volumes:
      - emptyDir:
          medium: Memory
          sizeLimit: 1Gi
        name: dshm
        ...
```
