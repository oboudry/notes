+++
title = 'Troubleshooting Odata'
date = 2022-04-08T11:23:19+02:00
draft = false
+++
Troubleshooting ODATA

vendredi, 8 avril 2022

11:23

 

Log d\'erreur frontend: /n/iwfnd/error_log

Log d\'erreur backend: /n/iwbep/error_log

 

Utilisation du client SAP GW: transaction /IWFND/GW_CLIENT

 

URL du style:

> /sap/opu/odata/sap/ZCOCKPITPROD2_SRV/POHeaderSet(\'1013751\')?\$format=json&sap-statistics=true
>
> /sap/opu/odata/sap/ZCOCKPITPROD2_SRV/POBulkSet?\$filter=OrderNumber%20eq%20%271013751%27

 

![](/images/sap/troubleshooting-odata/media/image1.png){width="17.71875in"
height="9.166666666666666in"}

 

Pour info, une transaction qui permet de tester les ODATA SAP. C'est via
cette transaction que j'ai été pointé sur l'autre transaction
/nIFWND/ERROR_LOG

 

Seule galère, trouver la syntaxe d'appel du service.

 

![](/images/sap/troubleshooting-odata/media/image2.jpg){width="19.28125in"
height="12.5in"}

 

SEGW permet d'afficher les services ODATA (utiliser le bouton folder
pour l'ouvrir), ici le service « POBulkSet » référencé dans l'URL
ci-dessus :

![](/images/sap/troubleshooting-odata/media/image3.jpg){width="5.21875in"
height="7.270833333333333in"}

 

La syntax pour le filtre je l'ai trouvé en débug près des messages
d'erreur (je n'ai pas pris la copie d'écran au bon moment, plus d'erreur
ici dessous).

![](/images/sap/troubleshooting-odata/media/image4.jpg){width="19.166666666666668in"
height="11.291666666666666in"}

 

Dernière option pour trouver l'URL :

 

1.  Click "Ctrl-Alt-Shift + S" pour ouvrir les Diagnostiques Fiori

2.  « Ctrl-Alt-Shift + Click » sur le composant sur lequel mettre le
    focus

3.  Regarder les bindings utilisés

![](/images/sap/troubleshooting-odata/media/image5.jpg){width="19.125in"
height="9.46875in"}

 

Activer les traces détaillées:

 

1.  Lancer /n/iwfnd/traces

2.  Sélectionner l\'utilisateur (ou le créer) puis mettre le niveau F et
    cocher les deux cases

3.  Lancer le journal des erreurs

 

![Outils Trater Saut /n/lwfnd/traces Système SAP Gateway : outils de tr
\'g des erreurs Lltilis. et préfixe LIRI requête • Cl BOUDRYO Aide Nom
utilis. Trace perform. (V2 unqt) BOUDRYO Trace don.ut. Préf. LIRI
requête Journal et traces disponibles Ntv. jrn. err. Trace perform. (V2
unqt) Trace don.ut. Fin validité trace acttve F Intég 24.0E.2022 Dern.
mod. par BOUDRYO
](/images/sap/troubleshooting-odata/media/image6.png){width="10.666666666666666in"
height="5.927083333333333in"}

 

1.  Relancer le processus qui génère une erreur (pour avoir une trace
    incluant les détails)

2.  Visualiser le corps de la requête

![](/images/sap/troubleshooting-odata/media/image7.png){width="17.25in"
height="8.104166666666666in"}

 

Récupérer les appels au backend dans le fichier Corps de requête (1)

![](/images/sap/troubleshooting-odata/media/image8.png){width="12.572916666666666in"
height="5.71875in"}

Les rejouer dans la transaction /n/iwfnd/gw_client

1.  Mettre la requête après /sap/opu/odata/sap/ZCOCKPITPROD2_SRV/

2.  Exécuter

 

![](/images/sap/troubleshooting-odata/media/image9.png){width="18.03125in"
height="7.989583333333333in"}

 

Eventuellement lancer le backend pour test

1.  Lancer SE24
