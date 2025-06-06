+++
title = "Tronquer une variable d'environnement Windows"
date = 2025-06-06T14:33:33+02:00
draft = false
tags = ["ms-dos", "environment", "variable"]
+++

# Troncation d'une variable d'environnement

Lorsqu'il faut tronquer une variable d'environnement dans MS-DOS on peut utiliser la syntaxe suivante:

* Premiers N caractères: `%Variable:~0,N%`
* Derniers N caractères: `%Variable:~-N%`

Dans le cas MDT, racourcir le numéro de série pour qu'il ne dépasse pas les 15 caractères une fois ajouté au préfixe `MVP-`

* `MDT-%SerialNumber:~0,12%` for the first 12 characters.
