# GLSS Framework — Scénarios de Kill Chain Multi-Segments

**Version 1.1 — Février 2026**
**Licence : CC BY-SA 4.0**

---

## Introduction

Ce document présente des scénarios d'attaque modélisés avec le framework GLSS. Chaque scénario illustre une kill chain traversant un ou plusieurs segments (Ground, Link, Space) et démontre l'utilité du modèle tri-segmentaire pour tracer des attaques de bout en bout.

Les scénarios sont basés sur des incidents réels documentés, des recherches publiées et des exercices Red Team plausibles.

---

## Scénario 1 : Attaque Viasat KA-SAT (Février 2022)

**Type :** Kill chain tri-segment (Ground → Link → Ground Impact)
**Classification :** Attaque étatique (APT)
**Attribution :** GRU (renseignement militaire russe)
**Impact :** ~30 000 modems rendus inopérants en Europe

### Contexte

Le 24 février 2022, simultanément à l'invasion de l'Ukraine, une cyberattaque a visé le réseau satellite KA-SAT opéré par Viasat. L'objectif était de neutraliser les communications militaires ukrainiennes utilisant ce réseau. Les dommages collatéraux ont affecté des dizaines de milliers d'utilisateurs civils en Europe, dont 5 800 éoliennes Enercon en Allemagne.

### Kill Chain GLSS

```
GROUND                    LINK                 GROUND (Impact)
━━━━━━━━━━━━━━━━━━━━     ━━━━━━━━━━━━━━━     ━━━━━━━━━━━━━━━━
[1] GRD-TA-0001          [4] LNK-TA-0003     [5] GRD-TA-0009
Reconnaissance VPN       Injection commande   Wiper AcidRain
                         firmware via          ~30K modems
[2] GRD-TA-0002          management system     brickés
Exploitation VPN
(accès initial)

[3] GRD-TA-0006
Pivot vers réseau
de management des
modems SurfBeam2
```

### Étapes détaillées

| # | Segment | Tactique | Technique GLSS | Description | Preuves |
|---|---|---|---|---|---|
| 1 | Ground | GRD-TA-0001 | GRD-TE-0001 | Reconnaissance du réseau de management VPN du système KA-SAT. | Identification du point d'entrée VPN mal configuré. |
| 2 | Ground | GRD-TA-0002 | GRD-TE-0010 | Exploitation d'une vulnérabilité dans le dispositif VPN pour obtenir l'accès initial au réseau de management. | Compromission du VPN — accès au réseau interne de gestion. |
| 3 | Ground | GRD-TA-0006 | GRD-TE-0050 | Mouvement latéral depuis le réseau IT compromis vers le segment de gestion des terminaux SurfBeam2. | Pivot IT → réseau de management des modems. |
| 4 | Link | LNK-TA-0003 | LNK-TE-0021 | Envoi massif de commandes de mise à jour firmware malveillantes via le système de gestion aux modems. | Commande de flash firmware envoyée aux terminaux via le satellite. |
| 5 | Ground | GRD-TA-0009 | GRD-TE-0081 | Le wiper AcidRain écrase la mémoire flash des modems, les rendant définitivement inopérants (bricking). | ~30 000 modems détruits, 5 800 éoliennes Enercon déconnectées. |

### Enseignements GLSS

Ce scénario démontre pourquoi un framework mono-segment est insuffisant : l'attaque commence par une compromission IT classique (couvert par ATT&CK), transite par une injection via le canal de communication (couvert par SPACE-SHIELD/SPARTA), et produit un impact sur les équipements sol (modems). Seul GLSS permet de tracer cette chaîne complète.

### Contre-mesures recommandées par segment

- **Ground :** MFA sur le VPN, segmentation IT/management, audit des accès fournisseurs.
- **Link :** Signature cryptographique des mises à jour firmware, vérification d'intégrité avant flash.
- **Impact :** Sauvegardes firmware, capacité de recovery à distance, redondance des terminaux.

---

## Scénario 2 : Spoofing GPS et désorientation d'un satellite LEO

**Type :** Kill chain bi-segment (Link → Space)
**Classification :** Attaque technique plausible
**Base :** Recherches publiées sur le GPS spoofing spatial

### Contexte

Un adversaire utilise un émetteur GPS/GNSS puissant pour tromper le récepteur de navigation d'un satellite LEO (Low Earth Orbit). En faussant sa position perçue, l'attaquant peut provoquer des manœuvres d'évitement de collision erronées, voire une modification d'orbite non souhaitée.

### Kill Chain GLSS

```
LINK                           SPACE
━━━━━━━━━━━━━━━━━━━━━━━━━━    ━━━━━━━━━━━━━━━━━━━━━━━━
[1] LNK-TA-0001               [4] SPC-TA-0006
Signal GNSS reconnaissance     AOCS réagit au faux signal
Caractérisation du signal       → manœuvre erronée

[2] LNK-TA-0005               [5] SPC-TA-0005
GPS Spoofing                    Dégradation du service
Faux signal GNSS                (pointage incorrect)

[3] LNK-TA-0003
Injection de données
de navigation falsifiées
```

### Étapes détaillées

| # | Segment | Tactique | Technique GLSS | Description |
|---|---|---|---|---|
| 1 | Link | LNK-TA-0001 | LNK-TE-0001 | Caractérisation du signal GNSS reçu par le satellite cible (fréquences, puissance, constellation utilisée). |
| 2 | Link | LNK-TA-0005 | LNK-TE-0040 | Envoi de faux signaux GNSS avec une puissance supérieure au signal légitime pour capturer le récepteur du satellite. |
| 3 | Link | LNK-TA-0003 | LNK-TE-0023 | Injection progressive de données de position falsifiées pour déplacer la position perçue du satellite. |
| 4 | Space | SPC-TA-0006 | SPC-TE-0050 | Le système AOCS réagit aux données de navigation falsifiées et effectue une correction d'attitude/orbite erronée. |
| 5 | Space | SPC-TA-0005 | SPC-TE-0043 | Dégradation du service due au mauvais pointage (perte de lien avec la station sol, couverture incorrecte). |

### Contre-mesures recommandées

- **Link :** Récepteurs GNSS anti-spoofing, authentification Galileo OSNMA, multi-constellation.
- **Space :** Validation croisée GNSS vs modèle orbital embarqué, limites de sécurité sur les corrections AOCS.

---

## Scénario 3 : Compromission supply chain et exfiltration de données d'observation

**Type :** Kill chain tri-segment (Ground → Space → Link → Ground)
**Classification :** Espionnage industriel / étatique
**Base :** Basé sur des patterns documentés d'attaques APT supply chain

### Contexte

Un acteur étatique compromet un sous-traitant logiciel impliqué dans le développement du logiciel de traitement d'image embarqué d'un satellite d'observation. Le code malveillant, dormant pendant la phase AIT et le lancement, s'active une fois en orbite pour exfiltrer les images haute résolution vers une station sol hostile.

### Kill Chain GLSS

```
GROUND (Supply Chain)     SPACE                    LINK                  GROUND (Exfil)
━━━━━━━━━━━━━━━━━━━━     ━━━━━━━━━━━━━━━━━━━━     ━━━━━━━━━━━━━━━━━━   ━━━━━━━━━━━━━━━
[1] GRD-TA-0001          [3] SPC-TA-0001          [5] LNK (downlink)   [6] GRD-TA-0007
Recon sous-traitant      Activation du backdoor    Données exfiltrées   Réception par
                         embarqué                  via downlink          station hostile
[2] GRD-TA-0002                                    détourné
Supply chain attack      [4] SPC-TA-0004
Code malveillant         Collecte d'images
injecté pré-lancement    sensibles
```

### Étapes détaillées

| # | Segment | Tactique | Technique GLSS | Description |
|---|---|---|---|---|
| 1 | Ground | GRD-TA-0001 | GRD-TE-0003 | Identification du sous-traitant développant le logiciel de traitement d'image embarqué. |
| 2 | Ground | GRD-TA-0002 | GRD-TE-0012 | Compromission du sous-traitant et injection d'un backdoor dans le code source du logiciel embarqué. Le code passe les tests AIT car il est dormant. |
| 3 | Space | SPC-TA-0001 | SPC-TE-0002 | Activation du backdoor après le lancement lors d'une fenêtre de communication spécifique (trigger conditionnel). |
| 4 | Space | SPC-TA-0004 | SPC-TE-0030 | Le backdoor sélectionne les images haute résolution de zones d'intérêt et les prépare pour exfiltration. |
| 5 | Link | LNK-TA-0003 | LNK-TE-0023 | Les données sont encodées dans le flux de télémétrie normal ou transmises lors de passes au-dessus d'une station sol contrôlée par l'adversaire. |
| 6 | Ground | GRD-TA-0007 | GRD-TE-0060 | Réception et décodage des images exfiltrées par la station sol hostile. |

### Enseignements GLSS

Ce scénario est le cas d'usage le plus complexe de GLSS : la kill chain traverse les 4 directions (Ground→Space, Space→Link, Link→Ground) et implique une phase dormante de plusieurs mois. Les frameworks existants ne peuvent pas modéliser ce type de chaîne d'attaque complète.

### Contre-mesures recommandées

- **Ground (Supply Chain) :** Audit de code tiers, vérification d'intégrité SBOM, environnements de build reproductibles.
- **Space :** Attestation de démarrage, monitoring comportemental du logiciel embarqué, sandboxing.
- **Link :** Chiffrement end-to-end du downlink, monitoring des passes non planifiées.

---

## Scénario 4 : Ransomware sur centre de contrôle mission

**Type :** Kill chain mono-segment (Ground only)
**Classification :** Cybercriminalité
**Base :** Patterns observés dans les attaques ransomware sur infrastructures critiques

### Contexte

Un groupe ransomware (type Qilin/LockBit) cible le centre de contrôle mission d'un opérateur satellite commercial. L'objectif est de chiffrer les systèmes de contrôle pour exiger une rançon, sachant que la perte de contact avec les satellites crée une pression temporelle extrême.

### Kill Chain GLSS

```
GROUND
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[1] GRD-TA-0001  Reconnaissance (scan réseau, OSINT)
[2] GRD-TA-0002  Accès initial (phishing opérateur)
[3] GRD-TA-0003  Exécution (Cobalt Strike)
[4] GRD-TA-0004  Persistance (backdoor serveur)
[5] GRD-TA-0005  Élévation de privilèges (IT → OT)
[6] GRD-TA-0006  Mouvement latéral (MCC → backup site)
[7] GRD-TA-0007  Collection (clés crypto TT&C)
[8] GRD-TA-0008  Exfiltration (données clients, TM)
[9] GRD-TA-0009  Impact (ransomware déployé sur MCC)
```

### Étapes détaillées

| # | Tactique | Technique GLSS | Description |
|---|---|---|---|
| 1 | GRD-TA-0001 | GRD-TE-0006 | Identification du personnel via LinkedIn (opérateurs mission, admins). |
| 2 | GRD-TA-0002 | GRD-TE-0011 | Spearphishing ciblant un ingénieur réseau avec un document piégé. |
| 3 | GRD-TA-0003 | GRD-TE-0020 | Déploiement de Cobalt Strike via PowerShell encodé. |
| 4 | GRD-TA-0004 | GRD-TE-0030 | Installation d'un service persistant sur le serveur de monitoring. |
| 5 | GRD-TA-0005 | GRD-TE-0040 | Exploitation de la segmentation faible pour pivoter de l'IT vers le réseau de contrôle mission. |
| 6 | GRD-TA-0006 | GRD-TE-0051 | Découverte et compromission du site de backup via le réseau de management partagé. |
| 7 | GRD-TA-0007 | GRD-TE-0062 | Extraction des clés de chiffrement TT&C (pour maximiser la pression). |
| 8 | GRD-TA-0008 | GRD-TE-0070 | Exfiltration des données clients et de la télémétrie avant chiffrement (double extorsion). |
| 9 | GRD-TA-0009 | GRD-TE-0082 | Déploiement du ransomware sur les systèmes MCC principal et backup simultanément. |

### Pression temporelle spécifique au spatial

Contrairement à un ransomware classique, l'attaque sur un MCC crée une urgence vitale : sans capacité de contrôle, les satellites en orbite ne peuvent pas être commandés. Les batteries se déchargent, les orbites dérivent, les collisions deviennent possibles. Cette pression fait des opérateurs spatiaux des cibles premium pour les ransomwares.

### Contre-mesures recommandées

- Segmentation stricte IT/OT avec DMZ industrielle
- Sauvegardes offline testées régulièrement
- Site de backup avec réseau physiquement séparé
- Protection des clés cryptographiques dans HSM
- Exercices de crise incluant scénario perte de MCC

---

## Scénario 5 : Empoisonnement de modèle IA embarqué

**Type :** Kill chain bi-segment (Ground → Space)
**Classification :** Attaque adversariale IA
**Base :** Recherches MITRE ATLAS sur les attaques adversariales ML

### Contexte

Un satellite de nouvelle génération utilise un modèle de deep learning embarqué pour la détection automatique d'objets dans les images d'observation. L'adversaire empoisonne le modèle pendant la phase d'entraînement au sol pour qu'il ignore systématiquement certains types d'installations militaires.

### Kill Chain GLSS

```
GROUND (MLOps)               SPACE
━━━━━━━━━━━━━━━━━━━━━━━     ━━━━━━━━━━━━━━━━━━━━━━━━
[1] GRD-TA-0001             [4] SPC-TA-0003
Recon du pipeline ML         Modèle empoisonné actif
(données, tools, processus)  en orbite → décisions
                             biaisées
[2] GRD-TA-0002
Compromission du pipeline    [5] SPC-TA-0002
d'entraînement               Impact sur le service
                             d'observation
[3] GRD-TA-0003
Injection de données
empoisonnées dans le
training set
```

### Étapes détaillées

| # | Segment | Tactique | Technique GLSS | Description |
|---|---|---|---|---|
| 1 | Ground | GRD-TA-0001 | GRD-TE-0005 | Identification du pipeline MLOps (outils d'entraînement, registre de modèles, données d'entraînement). |
| 2 | Ground | GRD-TA-0002 | GRD-TE-0012 | Compromission du pipeline via un package Python piégé dans les dépendances (supply chain ML). |
| 3 | Ground | GRD-TA-0003 | GRD-TE-0022 | Injection de données d'entraînement empoisonnées contenant des labels erronés pour certaines classes d'objets militaires. |
| 4 | Space | SPC-TA-0003 | SPC-TE-0022 | Le modèle embarqué, déployé via OTA, produit des classifications erronées en orbite. |
| 5 | Space | SPC-TA-0002 | SPC-TE-0013 | Les images d'observation sont retransmises avec des détections manquantes, dégradant la valeur du service. |

### Particularité

Cette attaque est extrêmement difficile à détecter car le modèle fonctionne normalement sur la majorité des images — il ne montre un comportement erroné que sur des cas spécifiques choisis par l'attaquant (targeted poisoning). C'est l'intersection entre MITRE ATLAS et le domaine spatial, une zone que seul GLSS couvre de manière structurée.

### Contre-mesures recommandées

- **Ground (MLOps) :** Vérification d'intégrité du dataset, pipeline d'entraînement en environnement air-gapped, SBOM des dépendances ML.
- **Space :** Validation croisée des détections IA avec des algorithmes classiques, monitoring de la dérive du modèle, mécanisme de rollback.

---

## Tableau récapitulatif des scénarios

| # | Scénario | Type | Segments | Tactiques impliquées | Techniques |
|---|---|---|---|---|---|
| 1 | Viasat KA-SAT | Tri-segment | G→L→G | 5 | 5 |
| 2 | GPS Spoofing LEO | Bi-segment | L→S | 5 | 5 |
| 3 | Supply Chain + Exfil | Tri-segment | G→S→L→G | 6 | 6 |
| 4 | Ransomware MCC | Mono-segment | G | 9 | 9 |
| 5 | AI Model Poisoning | Bi-segment | G→S | 5 | 5 |

---

## Utilisation des scénarios

### Pour les SOC
Utilisez ces scénarios pour créer des règles de détection corrélées entre segments. Chaque étape de la kill chain correspond à des indicateurs de détection spécifiques documentés dans la matrice détaillée.

### Pour les Red Teams
Les kill chains servent de playbooks d'exercice. Adaptez les techniques au contexte de l'organisation cible en sélectionnant les variantes pertinentes dans la matrice.

### Pour les auditeurs NIS2/CRA
Chaque scénario permet de vérifier que l'organisation audite dispose de contre-mesures sur l'ensemble des segments traversés par la kill chain. Un audit segment par segment révèle les angles morts.

### Pour la formation
Les scénarios sont conçus pour être utilisés comme études de cas dans les formations GLSS Practitioner et GLSS Expert. Le scénario 1 (Viasat) est particulièrement adapté car il est documenté publiquement.

---

*GLSS Framework — Scénarios de Kill Chain v1.1 — © 2026 Badaoui Salah, Hajer Boudhir, Achille Lele — CC BY-SA 4.0*
