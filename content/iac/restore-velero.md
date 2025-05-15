+++
title = 'Restore Velero'
date = 2025-05-15T12:54:32+02:00
draft = false
+++

## Restore d'un seul namespace

Ne pas oublier le filtre include-namespace!!!

```sh
velero restore create <RESTORE-NAME> --from-backup <BACKUP-NAME> --namespace-mappings <SRC-NAMESPACE>:<DEST-NAMESPACE> --include-namespaces <NAMESPACE>
```
