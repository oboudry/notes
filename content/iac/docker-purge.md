+++
title = "Nettoyage complet des données inutilisées de Docker"
date = 2025-05-19T13:48:29+02:00
draft = false
tags = ['docker', 'nettoyage']
+++


# Libération de l'espace disque `docker` inutilisé

Lorsque vous travaillez avec Docker, les conteneurs, images, volumes et réseaux peuvent s'accumuler avec le temps, et occuper beaucoup d'espace disque. La commande :

```sh
docker system prune -a --volumes
```

permet de nettoyer rapidement et totalement les ressources Docker inutilisées.

## Que fait cette commande ?

L’exécution de cette commande va :

- Supprimer **tous les conteneurs arrêtés**
- Supprimer **tous les réseaux** non utilisés par au moins un conteneur
- Supprimer **tous les volumes anonymes** non utilisés par au moins un conteneur
- Supprimer **toutes les images** sans conteneur associé
- Supprimer **tout le cache de build**

Un **avertissement** s’affichera :

```
WARNING! This will remove:
        - all stopped containers
        - all networks not used by at least one container
        - all anonymous volumes not used by at least one container
        - all images without at least one container associated to them
        - all build cache
Are you sure you want to continue? [y/N] y
```

L’opération ne commence que si vous confirmez avec `y`.

## Quand utiliser cette commande ?

- Lorsque vous souhaitez libérer de l’espace disque en supprimant toutes les données inutilisées.
- Avant ou après de nombreux tests, constructions d’images ou de conteneurs Docker.
- Sur des machines de développement où vous n’avez pas besoin de ressources persistantes dans Docker.

## ⚠️ **Attention**

- **Perte de données :** Toutes les données non utilisées par un conteneur en cours d’exécution seront supprimées, y compris les volumes anonymes.
- **À utiliser avec précaution :** Exécutez cette commande seulement si vous comprenez ses conséquences. Elle peut supprimer des images, des conteneurs ou des volumes importants que vous souhaitez conserver.
- **Ce qui n’est PAS supprimé :** Les volumes nommés utilisés, les conteneurs en cours d’exécution, et les réseaux utilisés.

## Conclusion

`docker system prune -a --volumes` est une solution puissante pour libérer de l’espace disque sur votre machine, mais utilisez-la avec prudence !

---

Pour plus d’informations, consultez la [documentation officielle Docker sur le nettoyage](https://docs.docker.com/engine/reference/commandline/system_prune/).
