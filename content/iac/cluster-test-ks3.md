+++
title = 'Cluster Test Ks3'
date = 2025-05-15T16:01:55+02:00
draft = false
tags = ["kubernetes", "lab", "k3s"]
+++

# Installation d'un cluster K3S un noeud et déploiement de WordPress

## Installer kubectl (le client Kubernetes)
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
./kubectl version
sudo mv kubectl /usr/local/bin/kubectl
sudo chown root:root /usr/local/bin/kubectl
```

## Installer Helm (le package manager Kubernetes)
```
curl -L https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
```

## Installer un cluster K3S 1 noeud local
```
curl -sfL https://get.k3s.io | sh -     
```
### Vérifier qu'il est up
```
sudo k3s kubectl get node
```

### Simplifier l'accès à kubectl (taper `kubectl` au lieu de `sudo k3s kubectl`)
```
mkdir ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $(whoami):$(id -gn) ~/.kube/config
```

### Remplacer kubectl par k et activer l'autocompletion (tab)
```
kubectl completion bash | sudo tee /etc/bash_completion.d/kubectl > /dev/null
echo 'alias k=kubectl' >> ~/.bashrc
echo 'complete -o default -F __start_kubectl k' >> ~/.bashrc
source ~/.bashrc
```

## Installer Longhorn comme storage provider (permet de provisioner des volumes persistents PVC, remplace Ceph)
```
sudo apt-get install -y open-iscsi
k apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.8.1/deploy/longhorn.yaml
```

## Installer WordPress
### Récupérer le nom de la classe Ingress (traefik)
```
k get ingressclasses.networking.k8s.io    

helm install wordpress --set ingress.enabled=true --set ingressClassName=nginx  oci://registry-1.docker.io/bitnamicharts/wordpress
```

### Les variables disponibles peuvent être obtenues comme ceci:
```
helm show values oci://registry-1.docker.io/bitnamicharts/wordpress > values.yaml
```

Au lieu de passer les variables par `--set` on peut également modifier le fichier `values.yaml` et le passer en argument avec `--values`.

## Recherche de l'IP
```
k get ingress
```

## Ajout de l'IP et du hostname dans la table hosts
```
sudo vi /etc/hosts
```

## Récup du mot de passe (username: user):
```
k get secrets wordpress -o jsonpath='{.data.wordpress-password}' | base64 -d
```

## Afficher le contenu du secret (permet de comprendre la commande ci-dessus)
```
k get secrets wordpress -o yaml
```

