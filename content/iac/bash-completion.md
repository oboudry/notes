+++
title = 'Bash Completion'
date = 2025-05-15T12:43:00+02:00
draft = false
tags = ["bash", "auto-complete", "kubernetes"]
+++

## Configuration d'un alias et de l'auto-completion

Exemple de configuration de l'alias k pour kubectl, avec auto-completion pour la
commande complète ainsi que pour l'alias.

```sh
kubectl completion bash | sudo tee /etc/bash_completion.d/kubectl > /dev/null
echo 'alias k=kubectl' >> ~/.bashrc
echo 'complete -o default -F __start_kubectl k' >> ~/.bashrc
```
