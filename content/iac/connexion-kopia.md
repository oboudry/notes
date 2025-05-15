+++
title = 'Connexion Kopia'
date = 2025-05-15T12:59:27+02:00
draft = false
+++

Connexion à un repository Kopia hébergé sur Minio
```sh
ACCESS_KEY=$(grep aws_access_key_id ~/credentials-velero | awk -F' = ' '{print $2}')
SECRET_KEY=$(grep aws_secret_access_key ~/credentials-velero | awk -F' = ' '{print $2}')
kopia repository connect s3   --bucket=velero   --access-key=${ACCESS_KEY}   --secret-access-key=${SECRET_KEY}   --endpoint=192.168.77.201:9000   --region=minio --disable-tls --prefix=kopia/monitoring/
```
