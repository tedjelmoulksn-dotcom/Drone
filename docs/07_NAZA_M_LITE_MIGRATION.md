# DJI NAZA-M Lite — Migration de l'architecture et journal d'intégration

> État au 13 octobre 2026 — documentation des essais sur table, **pas une validation de vol**.

## 1. Pourquoi changer de contrôleur ?

L'architecture initiale utilisait la **NXP RDDRONE-FMUK66**, avec **PX4** et **QGroundControl**. Pour poursuivre l'assemblage et les premiers essais radio, le contrôleur effectivement utilisé sur le banc a été remplacé par un **DJI NAZA-M Lite**.

Il s'agit d'un changement de **contrôleur de vol et d'écosystème logiciel**, et non d'une simple substitution de carte :

| Fonction | Architecture initiale | Architecture testée |
|---|---|---|
| Contrôleur | NXP RDDRONE-FMUK66 | DJI NAZA-M Lite |
| Configuration | PX4 / QGroundControl | NAZA-M Lite Assistant V1.00 |
| Alimentation contrôleur | Module compatible FMUK66 | Module DJI VU relié à X3 |
| Radio | Récepteur RC à intégrer | FlySky FS-iA6B |
| Propulsion | PDB, ESC, moteurs brushless | Matériel réutilisé, validation complète à faire |
| Télémétrie et autonomie | MAVLink, futur Raspberry Pi | **Non intégrés / non validés** sur NAZA-M Lite |

Les anciens documents FMUK66 restent utiles comme **historique de conception**, mais ne doivent pas être suivis tels quels pour câbler la version NAZA.

## 2. Matériel présent sur le banc

Éléments identifiés lors des échanges et photographies :
- contrôleur **DJI NAZA-M Lite** : entrées **A, E, T, R, U, X1, X2, X3** et sorties moteurs **M1 à M6** ;
- module d'alimentation **DJI VU** (marquage **2S–6S**) ;
- module **DJI LED/USB**, utilisé pour relier le contrôleur à l'ordinateur ;
- récepteur **FlySky FS-iA6B** et sa radiocommande ;
- carte de distribution **Matek PDB-XT60** ;
- ESC, moteur(s) brushless et hélice(s) ;
- alimentation 12 V essayée et batterie LiPo disponible.

**À confirmer sur le montage physique :** nombre exact d'ESC raccordés, références et caractéristiques de la LiPo, polarités réellement câblées, port exact du récepteur et type de signal RC sélectionné.

## 3. Schéma de câblage visé (à vérifier avant mise sous tension)

```text
                     +--> ESC 1 --> moteur 1
LiPo --> PDB-XT60 ---+--> ESC 2 --> moteur 2
          |          +--> ESC 3 --> moteur 3
          |          +--> ESC 4 --> moteur 4
          |
          +--> DJI VU (entrée batterie + / -)
                     |
                     +--> NAZA-M Lite : X3

NAZA-M Lite -- port LED --> DJI LED/USB -- USB --> PC Windows
NAZA-M Lite -- sorties M1..M4 --> signaux des ESC
FlySky FS-iA6B -- liaisons RC --> entrées A/E/T/R/U du NAZA
```

Ce schéma est **fonctionnel et indicatif** : il ne certifie pas que toutes ces liaisons sont déjà présentes ni que leur orientation est correcte.

### Points électriques critiques

- **Ne jamais** connecter directement la LiPo ou 12 V sur les entrées A/E/T/R/U/X3 du NAZA, ni sur le récepteur.
- Le **DJI VU** est destiné à alimenter le contrôleur via **X3** ; ses gros fils +/− se raccordent au bus batterie avec polarité vérifiée.
- Les fils de puissance des ESC sont raccordés à la PDB ; les fils de commande ESC vont aux sorties moteurs du NAZA selon l'ordre et l'orientation prescrits.
- En réception **PWM traditionnelle**, chaque canal FlySky nécessaire doit être câblé séparément (CH1→A, CH2→E, CH3→T, CH4→R, CH5→U, sous réserve de vérification des voies et polarités). **Un seul câble PWM sur CH4 ne transporte pas les cinq canaux.**
- Ne pas sélectionner PPM sans confirmer que le récepteur, son firmware et son câblage délivrent effectivement un signal PPM compatible.
- **Retirer toutes les hélices** pendant les essais électriques, USB, radio et moteurs sur banc.

## 4. Journal des essais — 12–13 octobre 2026

| Essai / observation | Résultat rapporté | Interprétation prudente |
|---|---|---|
| Montage physique des modules DJI, FlySky et PDB | Modules présents et connexions tentées ; l'utilisateur indique qu'un branchement essayé « a marché » | Le **sous-ensemble exact validé reste à préciser** : ne pas assimiler à une validation du drone complet |
| Module DJI branché en USB sur Windows 11 | Un voyant vert a été observé | Indique une activité/alimentation, **pas nécessairement une liaison de données opérationnelle** |
| Détection Windows | « Périphérique série USB (COM3) », statut OK | Périphérique série détecté ; identité DJI et pilote exact non confirmés |
| Test débranchement/rebranchement USB | L'utilisateur a confirmé que le test de disparition/réapparition de COM3 fonctionnait | COM3 est associé au périphérique USB du montage |
| NAZA-M Lite Assistant V1.00 | Affichage initial : `Mixer Type NONE`, `Receiver T NONE`, `MODE: N/A`, barres radio immobiles, `MC OUTPUT: ON` | Communication/configuration non validée ; ne pas conclure uniquement à partir de ces champs |
| Avertissement récepteur | Alerte évoquant le RX / son alimentation, libellé exact non confirmé | Alimentation du récepteur, câblage et protocole RC à vérifier |
| Essai d'alimentation | Alimentation 12 V essayée ; LiPo non utilisée à ce stade | Le point de branchement et la capacité de l'alimentation restent à confirmer |
| Test de démarrage du logiciel | Ouverture en administrateur, compatibilité Windows envisagée | Aucune résolution confirmée |
| Commandes radio et moteurs | Aucun mouvement de barres RC ni test moteur réussi confirmé | **Non validés** |

### État réel du projet

- **Confirmé :** le PC détecte un périphérique série lié au branchement USB du montage.
- **Rapporté, sans détail technique suffisant :** un branchement matériel essayé a fonctionné.
- **Non confirmé :** alimentation correcte du FS-iA6B, réception des cinq canaux, communication NAZA Assistant, calibration RC, ordre des moteurs, sens de rotation, armement, failsafe et vol.

## 5. Prochain protocole de vérification

1. **Hélices retirées**, alimentation coupée : photographier le câblage complet et relever les références batterie/ESC.
2. Vérifier à l'aide d'un multimètre les polarités et les tensions du bus batterie, du module VU et du récepteur, en respectant les spécifications constructeur.
3. Vérifier l'identité USB de COM3 (VID/PID, fournisseur du pilote) et la compatibilité du pilote DJI avec Windows.
4. Confirmer la communication effective entre NAZA Assistant et le contrôleur **avant** toute modification de configuration.
5. Identifier le protocole de sortie du FlySky FS-iA6B ; raccorder les canaux appropriés et vérifier le mouvement des barres A/E/T/R dans l'Assistant.
6. Configurer et tester la perte de liaison radio (failsafe) en conditions sûres.
7. Vérifier l'ordre des sorties moteurs et le sens de rotation **sans hélices**.
8. Ne préparer un essai de vol qu'après validation documentée de toutes les étapes précédentes.

## 6. Conséquences pour la suite du projet

Le NAZA-M Lite permet d'explorer une architecture de pilotage différente, mais **ne reprend pas automatiquement** les capacités PX4/MAVLink, les journaux ou l'interface envisagée avec un Raspberry Pi. Les fonctionnalités de navigation avancée, de télémétrie et d'autonomie doivent être réévaluées avant de les annoncer comme disponibles.

**Références de documentation :** [DJI — téléchargements NAZA-M Lite](https://www.dji.com/downloads/products/naza-m-lite) ; notices des ESC, de la PDB Matek et du récepteur FlySky à consulter selon les références exactes.
