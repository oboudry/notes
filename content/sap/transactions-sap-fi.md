+++
title = 'Transactions Sap Fi'
date = 2025-05-15T15:33:37+02:00
draft = false
tags = ["sap", "fi", "transaction"]
+++

## Posting Periods

- `MMPV` — Close period for Material Master records (set today as a date, and eventually allow negative stock in previous period)
- `OB52` — Cloture période
- `OKP1` — Clôture période analytique
- `OMSY` — Affichage de la période en cours

## Consignment Settlement

- `MRKO` — Consignment settlement

## G/L Accounts

- `FS10N` — G/L Account Balance Display

#### Demande d'acompte

- `FI-17` — Demande d'acompte client

#### Postes

- `FBL5N` — Liste des postes individuels clients

#### Périodes

- `MMPV` — Période comptable logistique

#### Accomptes

- `F-37`
- `F-29`

## Cost Centre Determination

- `OKB9`

## Activity Types / Budget

- `KP26`

#### Imputation réelle des ordres de process

- `CO88`

#### Schémas de calcul

- `CPT2` — Schémas de calcul (utilisé dans projet)
- `KZS2` — Schémas de calcul (proposé par Guy) ?

#### Calcul du coût de revient CCR

- `CK11N` — Calcul du coût de revient

#### Processus mensuel imputation MTO

- `CO88` — Imputation analytic et controlling
- `VA88` — Analytic

#### CCR

- `CKR1` — Annuler le CCR

## Moving Average Price History

- `S_P00_07000139`

## Annulation de factures

- `VF11`

## Configuration de la longueur des champs pour les comptes bancaires

- `OY17`

## Analyse des échéances de postes non soldés (à payer par mois)

- `/nS_ALR_87012078`

## Currency Conversion de Devises

- `OB08` — (table TCURR)
- `OB09` — Réévaluations de change / exchange rate re-evaluation
- `OBA1` — Détermination automatique de compte pour pertes/gains de taux de changes

## Détermination automatique de compte

- `OBYC` — Détermination de compte pour les articles

## Cost Centre

- `KS01` — Creation of a cost centre
