+++
title = 'Deploiement Cluster'
date = 2022-03-28T14:26:26+02:00
draft = false
+++
Déploiement d\'un cluster

lundi, 28 mars 2022

14:26

 

 

 

Kubespray (déploiement de cluster)

<https://github.com/kubernetes-sigs/kubespray>

 

Sur single-host-docker:

- sudo mkdir /k8s-control

- sudo addgroup k8s-control

- sudo chown :k8s-control /k8s-control

- sudo vi /etc/group\
   

> sudo:x:27:uinstaller,itmvp,olivier,xavier,antoine
>
> docker:x:999:itmvp,olivier,xavier,antoine
>
> k8s-control:x:1003:itmvp,olivier,xavier,antoine
>
>  
>
>  
>
> **Deploiement Cluster**
>
> **Table des matières**

- [Table des
  matières](https://jop.xovidb.stream/shares/l0BzZ3Vp3ELM8tn32SVY6O#table-des-matières)

- [1. Déploiement du
  cluster](https://jop.xovidb.stream/shares/l0BzZ3Vp3ELM8tn32SVY6O#1-déploiement-du-cluster)

  - [1.1. Création de répertoire
    partagé](https://jop.xovidb.stream/shares/l0BzZ3Vp3ELM8tn32SVY6O#11-création-de-répertoire-partagé)

  - [1.2. Installation
    Ansible](https://jop.xovidb.stream/shares/l0BzZ3Vp3ELM8tn32SVY6O#12-installation-ansible)

  - [1.3. Configuration
    SSH](https://jop.xovidb.stream/shares/l0BzZ3Vp3ELM8tn32SVY6O#13-configuration-ssh)

  - [1.4. Installation
    Kubespray](https://jop.xovidb.stream/shares/l0BzZ3Vp3ELM8tn32SVY6O#14-installation-kubespray)

  - [1.5. Configurer
    Kubectl](https://jop.xovidb.stream/shares/l0BzZ3Vp3ELM8tn32SVY6O#15-configurer-kubectl)

  - [1.6. Setup bash
    autocompletion](https://jop.xovidb.stream/shares/l0BzZ3Vp3ELM8tn32SVY6O#16-setup-bash-autocompletion)

- [2. Historique de commandes
  17/10/2023](https://jop.xovidb.stream/shares/l0BzZ3Vp3ELM8tn32SVY6O#2-historique-de-commandes-17102023)

> **1. Déploiement du cluster**
>
> Documentation de [Kubespray
> (Github)](https://github.com/kubernetes-sigs/kubespray)
>
> **1.1. Création de répertoire partagé**
>
> sudo mkdir/k8s-control\
> sudo addgroup k8s-control\
> sudo chown:k8s-control /k8s-control\
> sudo vi /etc/group
>
> Ajouter le texte suivant dans le fichier
>
> sudo:x:27:uinstaller,itmvp,olivier,xavier,antoine\
> docker:x:999:itmvp,olivier,xavier,antoine\
> k8s-control:x:1003:itmvp,olivier,xavier,antoine
>
> **1.2. Installation Ansible**
>
> sudo apt install python3.10-venv\
> VENVDIR=kubespray-venv\
> python3 -m venv \$VENVDIR
>
> source \$VENVDIR/bin/activate\
> pip install -U -r requirements.txt
>
> **1.3. Configuration SSH**
>
> mkdir-p \~/.ssh\
> vim \~/.ssh/flatcar_ssh.pem
>
> Coller le contenu de la clé privée dans le fichier.
>
> chmod 600\~/.ssh/flatcar_ssh.pem
>
> **1.4. Installation Kubespray**
>
> cd /k8s-control/\
> git clone <https://github.com/kubernetes-sigs/kubespray.git>\
> cd kubespray\
> cp -rfp inventory/sample inventory/mvp-cluster\
> sed -i \'s@bin_dir: /usr/local/bin@bin_dir: /opt/bin@\'
> inventory/mvp-cluster/group_vars/all/all.yml
>
>  
>
> \# Configure external DNS resolution
>
> \$ vim inventory/mvp-cluster/group_vars/k8s_cluster/k8s-cluster.yml
>
> \# Enable k8s_external plugin for CoreDNS
>
> enable_coredns_k8s_external: true
>
> coredns_k8s_external_zone: k8s.intranet-mvp.ch
>
>  
>
> declare -a IPS=(192.168.79.63 192.168.79.82 192.168.79.86)\
> CONFIG_FILE=inventory/mvp-cluster/hosts.yaml python3
> contrib/inventory_builder/inventory.py \${IPS\[@\]}\
> ansible-playbook -i inventory/mvp-cluster/hosts.yaml -u core
> \--key-file \~/.ssh/flatcar_ssh.pem \--become \--become-user=root
> cluster.yml
>
> **1.5. Configurer Kubectl**
>
> \# Install kubectl as per documentation
>
> curl -LO \"[https://dl.k8s.io/release/\$(curl -L -s
> https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl](https://dl.k8s.io/release/$(curl%20-L%20-s%20https:/dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl)\"
>
> chmod +x kubectl
>
> ./kubectl version
>
> sudo mv kubectl /usr/local/bin/kubectl
>
> sudo chown root:root /usr/local/bin/kubectl
>
>  
>
> \# logon to core01\
> ssh -i \~/.ssh/flatcar_ssh.pem core@flatcar01
>
> \# change owner so it can be downloaded\
> sudo chown core:core /etc/kubernetes/admin.conf
>
> \# exit core01 continue on single-host-kubernetes
>
> \# download config file
>
> scp -i \~/.ssh/flatcar_ssh.pem
> core@flatcar01:/etc/kubernetes/admin.conf kubespray-do.conf
>
> vim kubespray-do.conf
>
> \# change 127.0.0.1 to flatcar01 IP address
>
> mkdir \~/.kube
>
> cp kubespray-do.conf \~/.kube/config\
> kubectl getnodes
>
> **1.6. Setup bash autocompletion**
>
> kubectl completion bash \| sudo tee /etc/bash_completion.d/kubectl \>
> /dev/null\
> echo \'alias k=kubectl\' \>\> \~/.bashrc\
> echo \'complete -o default -F \_\_start_kubectl k\' \>\> \~/.bashrc
>
>  

# Deploy Metallb

> \# see what changes would be made, returns nonzero returncode if
> different
>
> kubectl get configmap kube-proxy -n kube-system -o yaml \| \\\
> sed -e\"s/strictARP: false/strictARP: true/\"\| \\\
> kubectl diff -f- -n kube-system
>
> \# actually apply the changes, returns nonzero returncode on errors
> only
>
> kubectl get configmap kube-proxy -n kube-system -o yaml \| \\\
> sed -e\"s/strictARP: false/strictARP: true/\"\| \\\
> kubectl apply -f- -n kube-system
>
>  
>
> kubectl apply -f
> <https://raw.githubusercontent.com/metallb/metallb/v0.13.12/config/manifests/metallb-native.yaml>
>
>  
>
>  

*From \<<https://metallb.universe.tf/installation/>\>*

>  
>
>  
>
> apiVersion: metallb.io/v1beta1
>
> kind: IPAddressPool
>
> metadata:
>
> name: first-pool
>
> namespace: metallb-system
>
> spec:
>
> addresses:
>
> \- 192.168.79.190-192.168.79.199
>
>  
>
> apiVersion: metallb.io/v1beta1
>
> kind: L2Advertisement
>
> metadata:
>
> name: default
>
> namespace: metallb-system
>
>  
>
>  

*From \<<https://metallb.universe.tf/configuration/>\>*

>  
>
>  

# Setup NFS on Single-Host-Docker

> sudo mkdir /k8s-nfs/
>
> sudo chown -R nobody:nogroup /k8s-nfs
>
> sudo chmod 777 /k8s-nfs
>
>  
>
> sudo nvim /etc/exports
>
> \# /etc/exports
>
> /k8s-nfs 192.168.79.63(rw,sync,no_subtree_check,no_root_squash)
> 192.168.79.82(rw,sync,no_subtree_check,no_root_squash)
> 192.168.79.86(rw,sync,no_subtree_check,no_root_squash)
>
>  
>
> sudo exportfs -a
>
>  
>
> showmount -e 192.168.77.201
>
>  
>
>  

# Setup NFS subdir provisionner (creates PersitentVolumes automatically)

> \> curl -L
> <https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3>
> \| bash
>
> \> helm version
>
> \> helm repo add nfs-subdir-external-provisioner
> <https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/>
>
> \> kubectl create ns nfs-provisioner
>
> \> export NFS_SERVER=192.168.56.1
>
> \> export NFS_EXPORT_PATH=/mnt/nfs_share
>
> \> helm -n nfs-provisioner install nfs-provisioner-01
> nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \--set
> nfs.server=\$NFS_SERVER \--set nfs.path=\$NFS_EXPORT_PATH \--set
> storageClass.defaultClass=true \--set replicaCount=1 \--set
> storageClass.name=nfs-01 \--set
> storageClass.provisionerName=nfs-provisioner-01
>
> \> kubectl get pods -n nfs-provisioner
>
> \> kubectl get sc -n nfs-provisioner
>
> \> nvim nfs-claim.yaml
>
> kind: PersistentVolumeClaim
>
> apiVersion: v1
>
> metadata:
>
> name: nfs-test-claim
>
> spec:
>
> storageClassName: nfs-01
>
> accessModes:
>
> \- ReadWriteMany
>
> resources:
>
> requests:
>
> storage: 10Mi
>
> \> nvim nfs-claim-pod.yaml
>
> kind: Pod
>
> apiVersion: v1
>
> metadata:
>
> name: nfs-test-pod
>
> spec:
>
> containers:
>
> \- name: nfs-test
>
> image: busybox:stable
>
> command:
>
> \- \"/bin/sh\"
>
> args:
>
> \- \"-c\"
>
> \- \"touch /mnt/SUCCESS && exit 0 \|\| exit 1\"
>
> volumeMounts:
>
> \- name: nfs-pvc
>
> mountPath: \"/mnt\"
>
> restartPolicy: \"Never\"
>
> volumes:
>
> \- name: nfs-pvc
>
> persistentVolumeClaim:
>
> claimName: nfs-test-claim
>
> \> kubectl apply -f nfs-claim.yaml
>
> \> kubectl apply -f nfs-claim-pod.yaml
>
> \> kubectl get pods
>
> \> ls
> /mnt/nfs_share/default-nfs-test-claim-pvc-afb2ea1b-33b8-4662-b49d-1a8e69e781d1/
>
> \> kubectl delete -f nfs-claim-pod.yaml
>
> \> kubectl delete -f nfs-claim.yaml
>
>  
>
> <https://computingforgeeks.com/configure-nfs-as-kubernetes-persistent-volume-storage/>
>
>  
>
>  

# Prépa cluster (avant DNS)

kubectl edit cm -n kube-system coredns

ajout k8s-external \<domain\> après la section \"kubernetes
cluster.local\"

![](/images/infra-as-code/deploiement-cluster/media/image1.png){width="6.4375in"
height="6.03125in"}

 

Créer le fichier dns-service.yaml et l\'appliquer (kubectl apply -f
dns-service.yaml)

\# dns-service.yaml

apiVersion: v1

kind: Service

metadata:

  name: ext-dns-udp

  namespace: kube-system

  annotations:

    metallb.universe.tf/allow-shared-ip: \"DNS\"

spec:

  type: LoadBalancer

  loadBalancerIP: 192.168.79.199

  ports:

  - port: 53

    targetPort: 53

    protocol: UDP

  selector:

    k8s-app: kube-dns

\-\--

apiVersion: v1

kind: Service

metadata:

  name: ext-dns-tcp

  namespace: kube-system

  annotations:

    metallb.universe.tf/allow-shared-ip: \"DNS\"

spec:

  type: LoadBalancer

  loadBalancerIP: 192.168.79.199

  ports:

  - port: 53

    targetPort: 53

    protocol: TCP

  selector:

    k8s-app: kube-dns

>  

 

# Config DNS

Add-DnsServerResourceRecordA -IPv4Address 192.168.79.199 -Name k8s
-ZoneName intranet-mvp.ch -ComputerName 192.168.77.13

Add-DnsServerZoneDelegation -name \"intranet-mvp.ch\" -ChildZoneName
\"k8s\" -NameServer \"k8s.intranet-mvp.ch\" -IPAddress
\"192.168.77.199\" -PassThru -Verbose -ComputerName 192.168.77.13

Get-DnsServerZoneDelegation -name intranet-mvp.ch -ComputerName
192.168.77.13

 

 

 

# Setup Rook Ceph (Storage)

<https://rook.io/docs/rook/latest/Getting-Started/quickstart/>

<https://rook.io/docs/rook/latest/Troubleshooting/ceph-toolbox/>

 

lsblk (vérifier que le disque non utilisé est bien /dev/sdb)

sudo wipefs \--all \--force /dev/sdb

Dans le BIOS mettre les disques SATA en mode IDE au lieu de AHCI sans
quoi les disques seront vus comme \"removable\" et pas pris en compte
par Ceph, puis redémarrer.

 

git clone \--single-branch \--branch master
<https://github.com/rook/rook.git>

cd rook/deploy/examples

\# Edit operator.yaml set \'ROOK_ENABLE_DISCOVERY_DAEMON: \"true\"\'

vim operator.yaml

![](/images/infra-as-code/deploiement-cluster/media/image2.png){width="9.65625in"
height="1.7291666666666667in"}

 

kubectl create -f crds.yaml -f common.yaml -f operator.yaml

kubectl create -f cluster.yaml

 

\# troubleshoot

kubectl create -f toolbox.yaml

kubectl -n rook-ceph exec -it deploy/rook-ceph-tools \-- bash

ceph status

 

## Utilisation de Ceph / création d\'une classe de stockage

<https://rook.io/docs/rook/latest/Storage-Configuration/Block-Storage-RBD/block-storage/>

 

\# Storage.yaml

apiVersion: ceph.rook.io/v1

kind: CephBlockPool

metadata:

name: replicapool

namespace: rook-ceph

spec:

failureDomain: host

replicated:

size: 3

\-\--

apiVersion: storage.k8s.io/v1

kind: StorageClass

metadata:

name: rook-ceph-block

\# Change \"rook-ceph\" provisioner prefix to match the operator
namespace if needed

provisioner: rook-ceph.rbd.csi.ceph.com

parameters:

\# clusterID is the namespace where the rook cluster is running

clusterID: rook-ceph

\# Ceph pool into which the RBD image shall be created

pool: replicapool

 

\# (optional) mapOptions is a comma-separated list of map options.

\# For krbd options refer

\#
<https://docs.ceph.com/docs/master/man/8/rbd/#kernel-rbd-krbd-options>

\# For nbd options refer

\# <https://docs.ceph.com/docs/master/man/8/rbd-nbd/#options>

\# mapOptions: lock_on_read,queue_depth=1024

 

\# (optional) unmapOptions is a comma-separated list of unmap options.

\# For krbd options refer

\#
<https://docs.ceph.com/docs/master/man/8/rbd/#kernel-rbd-krbd-options>

\# For nbd options refer

\# <https://docs.ceph.com/docs/master/man/8/rbd-nbd/#options>

\# unmapOptions: force

 

\# RBD image format. Defaults to \"2\".

imageFormat: \"2\"

 

\# RBD image features

\# Available for imageFormat: \"2\". Older releases of CSI RBD

\# support only the \`layering\` feature. The Linux kernel (KRBD)
supports the

\# full complement of features as of 5.4

\# \`layering\` alone corresponds to Ceph\'s bitfield value of \"2\" ;

\# \`layering\` + \`fast-diff\` + \`object-map\` + \`deep-flatten\` +
\`exclusive-lock\` together

\# correspond to Ceph\'s OR\'d bitfield value of \"63\". Here we use

\# a symbolic, comma-separated format:

\# For 5.4 or later kernels:

#imageFeatures:
layering,fast-diff,object-map,deep-flatten,exclusive-lock

\# For 5.3 or earlier kernels:

imageFeatures: layering

 

\# The secrets contain Ceph admin credentials.

csi.storage.k8s.io/provisioner-secret-name: rook-csi-rbd-provisioner

csi.storage.k8s.io/provisioner-secret-namespace: rook-ceph

csi.storage.k8s.io/controller-expand-secret-name:
rook-csi-rbd-provisioner

csi.storage.k8s.io/controller-expand-secret-namespace: rook-ceph

csi.storage.k8s.io/node-stage-secret-name: rook-csi-rbd-node

csi.storage.k8s.io/node-stage-secret-namespace: rook-ceph

 

\# Specify the filesystem type of the volume. If not specified,
csi-provisioner

\# will set default as \`ext4\`. Note that \`xfs\` is not recommended
due to potential deadlock

\# in hyperconverged settings where the volume is mounted on the same
node as the osds.

csi.storage.k8s.io/fstype: ext4

 

\# Delete the rbd volume when a PVC is deleted

reclaimPolicy: Delete

 

\# Optional, if you want to add dynamic resize for PVC.

\# For now only ext3, ext4, xfs resize support provided, like in
Kubernetes itself.

allowVolumeExpansion: true

 

 

- kubectl apply -f Storage.yaml

 

## En faire la default storageclass

kubectl patch storageclasses.storage.k8s.io rook-ceph-block -p
\'{\"metadata\":
{\"annotations\":{\"storageclass.kubernetes.io/is-default-class\":\"true\"}}}\'

 

## Utilisation

Référencer la classe de stockage \"rook-ceph-block\" dans les fichiers
Yaml servant à créer des volumes persistents (PersistentVolumeClaim).

 

# Flatcar Linux Reboot Operator (FLUO)

Déploiement flatcar fichier cl.yaml sur github mis à jour (config)

systemd:

units:

\- name: locksmithd.service

mask: true

\- name: update-engine.service

enabled: true

 

Ou fait à la main sur chaque nœud:

- sudo systemctl mask locksmithd.service

- sudo systemctl unmask update-engine.service

 

git clone <https://github.com/flatcar/flatcar-linux-update-operator.git>

cd flatcar-linux-update-operator

definir la fenêtre de maintenance dans le fichier
examples/deploy/update-operator.yaml

![](/images/infra-as-code/deploiement-cluster/media/image3.png){width="6.25in"
height="2.1770833333333335in"}

kubectl kustomize examples/deploy \| kubectl apply -f-

 

 

 

## Installation Prometheus

\###

itmvp@SINGLE-HOST-DOCKER:/k8s-control/flatcar-linux-update-operator/examples/deploy\$
helm install prometheus prometheus-community/prometheus

NAME: prometheus

LAST DEPLOYED: Tue Oct 31 15:31:41 2023

NAMESPACE: default

STATUS: deployed

REVISION: 1

TEST SUITE: None

NOTES:

The Prometheus server can be accessed via port 80 on the following DNS
name from within your cluster:

prometheus-server.default.svc.cluster.local

 

 

Get the Prometheus server URL by running these commands in the same
shell:

export POD_NAME=\$(kubectl get pods \--namespace default -l
\"app.kubernetes.io/name=prometheus,app.kubernetes.io/instance=prometheus\"
-o jsonpath=\"{.items\[0\].metadata.name}\")

kubectl \--namespace default port-forward \$POD_NAME 9090

 

 

The Prometheus alertmanager can be accessed via port 9093 on the
following DNS name from within your cluster:

prometheus-alertmanager.default.svc.cluster.local

 

 

Get the Alertmanager URL by running these commands in the same shell:

export POD_NAME=\$(kubectl get pods \--namespace default -l
\"app.kubernetes.io/name=alertmanager,app.kubernetes.io/instance=prometheus\"
-o jsonpath=\"{.items\[0\].metadata.name}\")

kubectl \--namespace default port-forward \$POD_NAME 9093

\#################################################################################

\###### WARNING: Pod Security Policy has been disabled by default since
\#####

\###### it deprecated after k8s 1.25+. use \#####

\###### (index .Values \"prometheus-node-exporter\" \"rbac\" \#####

\###### . \"pspEnabled\") with (index .Values \#####

\###### \"prometheus-node-exporter\" \"rbac\" \"pspAnnotations\") \#####

\###### in case you still need it. \#####

\#################################################################################

 

 

The Prometheus PushGateway can be accessed via port 9091 on the
following DNS name from within your cluster:

prometheus-prometheus-pushgateway.default.svc.cluster.local

 

 

Get the PushGateway URL by running these commands in the same shell:

export POD_NAME=\$(kubectl get pods \--namespace default -l
\"app=prometheus-pushgateway,component=pushgateway\" -o
jsonpath=\"{.items\[0\].metadata.name}\")

kubectl \--namespace default port-forward \$POD_NAME 9091

 

For more information on running Prometheus, visit:

<https://prometheus.io/>

 

\###

 

# Déploiement d\'un serveur S3 (Minio) pour les backups Velero

\# Installation

wget
<https://dl.min.io/server/minio/release/linux-amd64/archive/minio_20231101183725.0.0_amd64.deb>
-O minio.deb

sudo dpkg -i minio.deb

 

sudo vi /etc/default/minio

\####

\# MINIO_ROOT_USER and MINIO_ROOT_PASSWORD sets the root account for the
MinIO server.

\# This user has unrestricted permissions to perform S3 and
administrative API operations on any resource in the deployment.

\# Omit to use the default values \'minioadmin:minioadmin\'.

\# MinIO recommends setting non-default values as a best practice,
regardless of environment

 

MINIO_ROOT_USER=myminioadmin

MINIO_ROOT_PASSWORD=XXX

 

\# MINIO_VOLUMES sets the storage volume or path to use for the MinIO
server.

 

MINIO_VOLUMES=\"**/mnt/minio**\"

 

\# MINIO_SERVER_URL sets the hostname of the local machine for use with
the MinIO Server

\# MinIO assumes your network control plane can correctly resolve this
hostname to the local machine

 

\# Uncomment the following line and replace the value with the correct
hostname for the local machine and port for the MinIO server (9000 by
default).

 

MINIO_SERVER_URL=\"**http://192.168.77.201:9000**\"

\####

 

\# Création du répertoire data

sudo mkdir /mnt/minio

sudo chown minio-user:minio-user /mnt/minio

 

\# Activation et démarrage du service

sudo systemctl enable minio.service

sudo systemctl start minio.service

sudo systemctl status minio.service

 

\# Log (utile pour URLs et troubleshooting)\
sudo journalctl -xeu minio.service

Nov 03 14:18:56 SINGLE-HOST-DOCKER minio\[118840\]: MinIO Object Storage
Server

Nov 03 14:18:56 SINGLE-HOST-DOCKER minio\[118840\]: Copyright: 2015-2023
MinIO, Inc.

Nov 03 14:18:56 SINGLE-HOST-DOCKER minio\[118840\]: License: GNU AGPLv3
\<<https://www.gnu.org/licenses/agpl-3.0.html>\>

Nov 03 14:18:56 SINGLE-HOST-DOCKER minio\[118840\]: Version:
RELEASE.2023-11-01T18-37-25Z (go1.21.3 linux/amd64)

Nov 03 14:18:56 SINGLE-HOST-DOCKER minio\[118840\]: Status: 1 Online, 0
Offline.

Nov 03 14:18:56 SINGLE-HOST-DOCKER minio\[118840\]: S3-API:
<http://192.168.77.201:9000>

Nov 03 14:18:56 SINGLE-HOST-DOCKER minio\[118840\]: Console:
<http://192.168.77.201:44579> <http://172.17.0.1:44579>
<http://127.0.0.1:44579>

Nov 03 14:18:56 SINGLE-HOST-DOCKER minio\[118840\]: Documentation:
<https://min.io/docs/minio/linux/index.html>

Nov 03 14:18:56 SINGLE-HOST-DOCKER minio\[118840\]: Warning: The
standard parity is set to 0. This can lead to data loss.

 

 

# Déploiement de Velero (backup)

\# Liens sur <https://github.com/heptio/velero/releases>

\# Installation de la commande velero

cd /k8s-control

wget
<https://github.com/vmware-tanzu/velero/releases/download/v1.12.1/velero-v1.12.1-linux-amd64.tar.gz>

tar xvzf velero-v1.12.1-linux-amd64.tar.gz

cd velero-v1.12.1-linux-amd64

sudo mv velero /usr/local/bin/

 

# Installation de velero server

<https://velero.io/docs/v1.12/contributions/minio/>

\# Create velero bucket on Minio

\# Create AccessKey on Minio to put in credentials-velero file below

git clone <https://github.com/vmware-tanzu/velero.git>

sudo chown -R :k8s-control .

vim credentials-velero

kubectl apply -f examples/minio/00-minio-deployment.yaml

\# <https://velero.io/docs/main/csi/>

velero install \--provider aws \--plugins
velero/velero-plugin-for-aws:v1.8.0,velero/velero-plugin-for-csi:v0.6.1
\--bucket velero \--secret-file ./credentials-velero
\--use-volume-snapshots=true \--backup-location-config
region=minio,s3ForcePathStyle=\"true\",s3Url=http://192.168.77.201:9000
\--use-node-agent \--snapshot-location-config
region=minio,s3ForcePathStyle=\"true\",s3Url=http://192.168.77.201:9000
\--features=EnableCSI

\# add EnableCSI config on client side so that snapshots will show when
describing backup

velero client config set features=EnableCSI

\# In case of issue, delete veleor namespace before recreating

kubectl logs deployment/velero -n velero \| less

velero backup create corteza-backup \--selector app=corteza

velero backup describe corteza-backup

velero backup logs corteza-backup

 

# Activation des snapshots (pour backup volumes)

<https://github.com/kubernetes-csi/external-snapshotter/tree/v6.0.1#usage>

 

git clone <https://github.com/kubernetes-csi/external-snapshotter.git>

cd external-snapshotter/

kubectl kustomize client/config/crd \| kubectl create -f -

kubectl -n kube-system kustomize deploy/kubernetes/snapshot-controller
\| kubectl create -f -

\# Je pense que la ligne suivante n\'est pas nécessaire, remplacée par
EnableCSI sur le serveur

~~kubectl kustomize deploy/kubernetes/csi-snapshotter \| kubectl create
-f -~~

 

cd ../rook

kubectl create -f deploy/examples/csi/rbd/storageclass.yaml

kubectl create -f deploy/examples/csi/rbd/snapshotclass.yaml

 

\# Les snapshots ne fonctionnent pas si ce label manque

kubectl label volumesnapshotclass csi-rbdplugin-snapclass
velero.io/csi-volumesnapshot-class=\"true\"

 

\# La doc à suivre en mettant à jour les versions de plugins

<https://velero.io/docs/main/csi-snapshot-data-movement/>

 

## Création d\'un backup planifié

Ignore les namespaces kube-system et autre namespaces système.

\$ velero schedule create daily-backup-all-but-kube
\--exclude-namespaces kube-node-lease,kube-public,kube-system
\--schedule \"0 4 \* \* \*\" \--snapshot-move-data

 

## Restore d\'un seul namespace (ne pas oublier le filtre include-namespace!!!)

\$ velero restore create prometheus-restore-17112023-1425 \--from-backup
full-backup-17112023-0956 \--namespace-mappings default:prometheus
**\--include-namespaces default**

 

# RBAC

<https://www.weave.works/blog/kubernetes-rbac-101>

 

\# Test

openssl genrsa -out magalix.key 2048

openssl req -new -key magalix.key -out magalix.csr -subj \"/CN=magalix\"

openssl x509 -req -in magalix.csr -CA ca.crt -CAkey ca.key
-CAcreateserial -out magalix.crt -days 365

kubectl config set-credentials magalix \--client-certificate=magalix.crt
\--client-key=magalix.key

cat ../.kube/config

kubectl config set-context magalix-context \--cluster=cluster.local
\--user=magalix

kubectl get pods

kubectl get pods \--user=magalix

cat ../.kube/config

vim ReadPodsRole.yaml

kubectl apply -f ReadPodsRole.yaml

vim ReadPodsRole.yaml

kubectl apply -f ReadPodsRole.yaml

vim ReadPodsRole.yaml

kubectl apply -f ReadPodsRole.yaml

kubectl get pods \--user=magalix

 

 

# Mise à jour du cluster

## Mise à jour client kubectl

\$ curl -LO \"[https://dl.k8s.io/release/\$(curl -L -s
https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl](https://dl.k8s.io/release/$(curl%20-L%20-s%20https:/dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl)\"

\$ chmod +x kubectl

\$ ./kubectl version

\$ sudo mv kubectl /usr/local/bin/kubectl

\$ sudo chown root:root /usr/local/bin/kubectl

 

## Mise à jour serveur

\$ cd /k8s-control/kubespray

\$ git pull

\$ source kubespray-venv/bin/activate

\$ pip install -U -r requirements.txt

\$ ansible-playbook -i inventory/mvp-cluster/hosts.yaml -u core
\--key-file \~/.ssh/flatcar_ssh.pem -b -e kube_version=v1.28.3
upgrade-cluster.yml

 

## Vérification

\$ kubectl version

Client Version: [v1.28.3]{.mark}

Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3

Server Version: [v1.28.3]{.mark}

 

 

# Encryption

\# Créer un secret tls avec la clé privée et le bundle certificat de
\*.marvinpac.com

kubectl create secret tls star-marvinpac-com-tls \--key
star_marvinpac_com.key \--cert star_marvinpac_com.crt

 

# Déploiement d\'ArgoCD

Ajouter le search domain intranet-mvp.ch à la config nodelocaldns via le
fichier inventaire Kubespray:
/k8s-control/kubespray/inventory/mvp-cluster/group_vars/k8s_cluster/k8s-cluster.yml

## Mettre à jour le cluster avec:

\$ cd /k8s-control/kubespray/

\$ source kubespray-venv/bin/activate

\$ ansible-playbook -i inventory/mvp-cluster/hosts.yaml -u core
\--key-file \~/.ssh/flatcar_ssh.pem -b -e kube_version=v1.28.3
upgrade-cluster.yml

 

![](/images/infra-as-code/deploiement-cluster/media/image4.png){width="4.270833333333333in"
height="2.1041666666666665in"}

 

Puis suivre la doc getting-started officielle:

<https://argo-cd.readthedocs.io/en/stable/getting_started/>

 

## Installer ArgoCD Server

kubectl create namespace argocd\
kubectl apply -n argocd -f
<https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml>

 

## Installer ArgoCD Client

Trouver l\'URL dans:

<https://github.com/argoproj/argo-cd/releases/latest>

\$ wget
<https://github.com/argoproj/argo-cd/releases/download/v2.9.1/argocd-linux-amd64>

\$ chmod +x argocd-linux-amd64

\$ sudo mv argocd-linux-amd64 /usr/local/bin

 

## Rendre le serveur ArgoCD accessible via une IP LoadBalancer

\$ kubectl patch svc argocd-server -n argocd -p \'{\"spec\": {\"type\":
\"LoadBalancer\"}}\'

 

## Login

Obtenir le mot de passe initial

\$ argocd admin initial-password -n argocd

Obtenir l\'IP externe du serveur

\$ kubectl get svc argocd-server -n argocd

NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE

argocd-server LoadBalancer 10.233.47.159 192.168.77.110
80:32498/TCP,443:31390/TCP 16h

Logon (à essayer, utiliser le nom DNS:
argocd-server.argocd.k8s.intranet-mvp.ch

\$ argocd login 192.168.77.110

Changer le mot de passe

\$ argocd account update-password

 

## Ajout du cluster à ArgoCD

Lister les contextes existants (magalix-context est un test de droits
RBAC)

\$ kubectl config get-contexts -o name

kubernetes-admin@cluster.local

magalix-context

Ajouter le context admin

\$ argocd cluster add kubernetes-admin@cluster.local

Vérifier que l\'ajout du cluster soit un succès

![](/images/infra-as-code/deploiement-cluster/media/image5.png){width="6.21875in"
height="1.03125in"}

##  

## Configurer la completion bash pour la command argocd

Générer le fichier de completion et en mettre le contenu au bon endroit.
Permet après logon d\'avoir la completion des commandes argocd

\$ argocd completion bash \| sudo tee /etc/bash_completion.d/argocd \>
/dev/null

 

## Création d\'un file système partagé Ceph

Nécessaire pour Vault, les volumes RBD ne pouvant pas être partagé.

 

\# FileSystem.yaml

apiVersion: ceph.rook.io/v1

kind: CephFilesystem

metadata:

name: cephfs

namespace: rook-ceph

spec:

metadataPool:

replicated:

size: 3

dataPools:

\- name: replicated

replicated:

size: 3

preserveFilesystemOnDelete: true

metadataServer:

activeCount: 1

activeStandby: true

 

\# FileSystemClass.yaml

apiVersion: storage.k8s.io/v1

kind: StorageClass

metadata:

name: rook-cephfs

\# Change \"rook-ceph\" provisioner prefix to match the operator
namespace if needed

provisioner: rook-ceph.cephfs.csi.ceph.com

parameters:

\# clusterID is the namespace where the rook cluster is running

\# If you change this namespace, also change the namespace below where
the secret namespaces are defined

clusterID: rook-ceph

 

\# CephFS filesystem name into which the volume shall be created

fsName: cephfs

 

\# Ceph pool into which the volume shall be created

\# Required for provisionVolume: \"true\"

pool: cephfs-replicated

 

\# The secrets contain Ceph admin credentials. These are generated
automatically by the operator

\# in the same namespace as the cluster.

csi.storage.k8s.io/provisioner-secret-name: rook-csi-cephfs-provisioner

csi.storage.k8s.io/provisioner-secret-namespace: rook-ceph

csi.storage.k8s.io/controller-expand-secret-name:
rook-csi-cephfs-provisioner

csi.storage.k8s.io/controller-expand-secret-namespace: rook-ceph

csi.storage.k8s.io/node-stage-secret-name: rook-csi-cephfs-node

csi.storage.k8s.io/node-stage-secret-namespace: rook-ceph

 

reclaimPolicy: Delete

 

# Install sealed-secrets

 

## Install server

\$ helm repo add sealed-secrets
<https://bitnami-labs.github.io/sealed-secrets>

\$ help repo update

\$ helm install sealed-secrets -n kube-system \--set-string
fullnameOverride=sealed-secrets-controller sealed-secrets/sealed-secrets

 

> NAME: sealed-secrets
>
> LAST DEPLOYED: Thu Nov 16 12:49:03 2023
>
> NAMESPACE: kube-system
>
> STATUS: deployed
>
> REVISION: 1
>
> TEST SUITE: None
>
> NOTES:
>
> \*\* Please be patient while the chart is being deployed \*\*
>
>  
>
> You should now be able to create sealed secrets.
>
>  
>
> 1\. Install the client-side tool (kubeseal) as explained in the docs
> below:
>
>  
>
> <https://github.com/bitnami-labs/sealed-secrets#installation-from-source>
>
>  
>
> 2\. Create a sealed secret file running the command below:
>
>  
>
> kubectl create secret generic secret-name \--dry-run=client
> \--from-literal=foo=bar -o \[json\|yaml\] \| \\
>
> kubeseal \\
>
> \--controller-name=sealed-secrets-controller \\
>
> \--controller-namespace=kube-system \\
>
> \--format yaml \> mysealedsecret.\[json\|yaml\]
>
>  
>
> The file mysealedsecret.\[json\|yaml\] is a commitable file.
>
>  
>
> If you would rather not need access to the cluster to generate the
> sealed secret you can run:
>
>  
>
> kubeseal \\
>
> \--controller-name=sealed-secrets-controller \\
>
> \--controller-namespace=kube-system \\
>
> \--fetch-cert \> mycert.pem
>
>  
>
> to retrieve the public cert used for encryption and store it locally.
> You can then run \'kubeseal \--cert mycert.pem\' instead to use the
> local cert e.g.
>
>  
>
> kubectl create secret generic secret-name \--dry-run=client
> \--from-literal=foo=bar -o \[json\|yaml\] \| \\
>
> kubeseal \\
>
> \--controller-name=sealed-secrets-controller \\
>
> \--controller-namespace=kube-system \\
>
> \--format \[json\|yaml\] \--cert mycert.pem \>
> mysealedsecret.\[json\|yaml\]
>
>  
>
> 3\. Apply the sealed secret
>
>  
>
> kubectl create -f mysealedsecret.\[json\|yaml\]
>
>  
>
> Running \'kubectl get secret secret-name -o \[json\|yaml\]\' will show
> the decrypted secret that was generated from the sealed secret.
>
>  
>
> Both the SealedSecret and generated Secret must have the same name and
> namespace.

 

## Install client

\$ wget
<https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.24.4/kubeseal-0.24.4-linux-amd64.tar.gz>

 

## Utilisation

\$ kubeseal -f postgres-pass-secret.yaml -w
postgres-pass-secret-sealed.yaml

Mettre le secret dans .gitignore pour qu\'il ne soit pas versionné. Le
sealed secret par contre est encrypté et peut être versionné.

Dans le fichier kustomize.yaml mettre le sealed secret. A
l\'instanciation dans le cluster il sera transformé localement en secret
utilisable dans les déploiements.

Attention un secret n\'est valable que pour un namespace.

 

# Installation Nginx-Ingress (TLS)

\# Création du namespace

\$ k create ns nginx-ingress

 

\# Création du certificat TLS (en dry run pour garder le fichier)

\$ kubectl create secret tls star-marvinpac-com
\--key=star_marvinpac_com.key \--cert=star_marvinpac_com.crt
\--dry-run=client -o yaml \> mvp-tls-secret.yaml

 

\# Importation du certificat

\$ k apply -n nginx-ingress -f mvp-tls-secret.yaml

 

\# Supprimer le helm chart s\'il existe déjà

\$ helm uninstall ingress-nginx ingress-nginx/ingress-nginx \--namespace
nginx-ingress

 

\# Installer le helm chart

\$ helm install ingress-nginx ingress-nginx/ingress-nginx \--namespace
nginx-ingress \--set
controller.wildcardTLS.cert=nginx-ingress/star-marvinpac-com \--set
controller.service.loadBalancerIP=192.168.77.148 \--set
controller.config.force-ssl-redirect=\"true\"

 

\# Vérifier que la config force-ssl-redirect

\$ k get cm -n nginx-ingress ingress-nginx-controller -o yaml

 

 

# Installation de Vault =\> Ignore this section too complex

 

## Déploiement de Vault avec Helm 

## \$ k create namespace vault

\$ helm install vault hashicorp/vault \--namespace vault \--set
\'server.dataStorage.storageClass=rook-cephfs\'

 

## Initialisation et déblocage du vault

\$ kubectl -n vault exec \--stdin=true \--tty=true vault-0 \-- vault
operator init

Unseal Key 1: XXX

Unseal Key 2: XXX

Unseal Key 3: XXX

Unseal Key 4: XXX

Unseal Key 5: XXX

 

Initial Root Token: XXX

 

Vault initialized with 5 key shares and a key threshold of 3. Please
securely

distribute the key shares printed above. When the Vault is re-sealed,

restarted, or stopped, you must supply at least 3 of these keys to
unseal it

before it can start servicing requests.

 

Vault does not store the generated root key. Without at least 3 keys to

reconstruct the root key, Vault will remain permanently sealed!

 

It is possible to generate new unseal keys, provided you have a quorum
of

existing unseal keys shares. See \"vault operator rekey\" for more
information.

 

## Déblocage du vault

\## Unseal the first vault server until it reaches the key threshold

\$ kubectl exec -n vault \--stdin=true \--tty=true vault-0 \-- vault
operator unseal XXX

\$ kubectl exec -n vault \--stdin=true \--tty=true vault-0 \-- vault
operator unseal XXX

\$ kubectl exec -n vault \--stdin=true \--tty=true vault-0 \-- vault
operator unseal XXX

 

![](/images/infra-as-code/deploiement-cluster/media/image6.png){width="14.385416666666666in"
height="7.03125in"}

 
