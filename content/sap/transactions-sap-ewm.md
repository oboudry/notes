+++
title = 'Transactions Sap Ewm'
date = 2025-05-15T15:44:43+02:00
draft = false
tags = ["sap", "ewm", "transaction"]
+++

## EWM & ERP: Stock and Communication

- `/SCWM/STOCK_CHECK` — Comparaison stocks ERP & EWM
- `SMQ2` — Messages entre EWM & ERP
- `/SCWM/MON` — Outils / File d'attente des messages : messages entre EWM & ERP

## Staging Webdynpro

Ajouter Favori, Autre Objet, choisir WebDynpro application, et mettre `/SCWM/MFG_STAGING`

## OF, Bin, and Article Management

- `/scwm/pmr` — Mise à disposition des articles pour OF
- `/scwm/binmat` — Articles qui vont dans le frigo (gestion par emplacement fixe)
- `/scwm/bindst` — Lien groupe de groupage → bin

## Simulation et formulaires

- `/scwm/sbst` — Simulation du tri des emplacements (résolution de problème, OM qui ne s'imprime pas, pas d'aire d'activité dans l'OM)
- `/scwm/prwo6` — Définition des formulaires pour impression d'ordre magasin

## PPF

- `SPPFP` — Relancer actions PPF

## Inventory comptage et différences

- `/scwm/pi_user_diff` — Assign user to tolerance groups for difference analysis
- `/scwm/pi_user` — Assign user to tolerance groups for recount/clearing

## Annuler une livraison sortante

- `/SCWM/FD` — Gérer livraison

## Consistency Protocol

- `/n/scwm/reeccheck` — Consistency protocol for ERP-EWM configuration check report (validation EWM ERP)

