+++
title = 'Liste des processus sans ps'
date = 2025-05-15T15:03:13+02:00
draft = false
tags = ["linux", "ps", "container"]
+++

Pour lister les processus lorsque la commande ps n'est pas présente, par exemple en exécution dans un conteneur.

```sh
for prc in /proc/*/cmdline; { (printf "$prc "; cat -A "$prc") | sed 's/\^@/ /g;s|/proc/||;s|/cmdline||'; echo; }
```
