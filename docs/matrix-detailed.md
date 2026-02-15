# GLSS Framework — Matrice détaillée des tactiques et techniques

**Version 1.1 — Février 2026**
**Licence : CC BY-SA 4.0**

---

## Table des matières

1. [Segment Ground (Sol) — 9 tactiques](#1-segment-ground-sol)
2. [Segment Link (Liaison) — 6 tactiques](#2-segment-link-liaison)
3. [Segment Space (Spatial) — 6 tactiques](#3-segment-space-spatial)

---

## 1. Segment Ground (Sol)

### GRD-TA-0001 — Ground Reconnaissance

> Collecte d'informations sur l'infrastructure du segment sol : centres de contrôle, stations sol, systèmes IT/OT de support.

| ID | Technique | Description | Détection | Contre-mesure | Mapping ATT&CK | Mapping SPARTA |
|---|---|---|---|---|---|---|
| GRD-TE-0001 | Ground Station Network Scanning | Scan réseau des sous-réseaux des stations sol pour identifier services actifs, ports ouverts et topologie. | Surveillance du trafic réseau anormal sur les périmètres des stations sol. | Segmentation réseau et IDS/IPS en périmètre. | T1046 | — |
| GRD-TE-0002 | MCC Infrastructure Enumeration | Énumération des systèmes du centre de contrôle mission (serveurs SCADA, bases de données télémétrie, interfaces opérateur). | Logs d'authentification, requêtes LDAP/AD inhabituelles. | Principe du moindre privilège, MFA sur les consoles d'administration. | T1018, T1083 | — |
| GRD-TE-0003 | Supply Chain Intelligence Gathering | Collecte d'informations sur la chaîne d'approvisionnement : fournisseurs de composants, sous-traitants logiciels, intégrateurs. | Surveillance OSINT, alertes sur fuites d'informations. | Programme de gestion des risques fournisseurs, NDAs. | T1591, T1593 | — |
| GRD-TE-0004 | Ground Antenna Characterization | Identification des caractéristiques des antennes sol (bandes de fréquence, azimut, élévation, fenêtres de visibilité). | Surveillance de l'activité SIGINT autour des installations. | Sécurité physique, classification des paramètres d'antenne. | T1592 | — |
| GRD-TE-0005 | Cloud Ground Segment Discovery | Découverte des instances cloud utilisées pour le segment sol (AWS Ground Station, Azure Orbital, etc.). | Monitoring des requêtes API cloud suspectes. | Inventaire cloud, politiques IAM restrictives. | T1580, T1526 | — |
| GRD-TE-0006 | Personnel Reconnaissance | Identification du personnel clé : opérateurs mission, ingénieurs système, administrateurs réseau. | Surveillance des réseaux sociaux et alertes OSINT. | Sensibilisation du personnel, limitation des informations publiques. | T1589, T1594 | — |

---

### GRD-TA-0002 — Ground Initial Access

> Obtention d'un accès initial aux systèmes du segment sol.

| ID | Technique | Description | Détection | Contre-mesure | Mapping ATT&CK | Mapping SPARTA |
|---|---|---|---|---|---|---|
| GRD-TE-0010 | VPN/Remote Access Exploitation | Exploitation de vulnérabilités dans les solutions d'accès distant (VPN, bastion, Citrix) des stations sol. | Surveillance des connexions VPN inhabituelles, CVE monitoring. | Patch management, MFA obligatoire, Zero Trust Architecture. | T1133, T1190 | — |
| GRD-TE-0011 | Spearphishing Ground Operators | Campagne de phishing ciblant les opérateurs mission et ingénieurs du segment sol. | Formation au phishing, filtrage email, sandbox d'analyse. | Sensibilisation, filtrage des pièces jointes, DMARC/DKIM/SPF. | T1566.001, T1566.002 | — |
| GRD-TE-0012 | Supply Chain Compromise | Compromission d'un fournisseur ou sous-traitant logiciel pour injecter du code malveillant dans les systèmes du segment sol. | Audit de code, vérification d'intégrité des binaires. | Vérification des signatures, SBOM, audits fournisseurs réguliers. | T1195.001, T1195.002 | — |
| GRD-TE-0013 | Physical Access to Ground Station | Accès physique non autorisé à une station sol pour connexion directe aux systèmes. | Vidéosurveillance, contrôle d'accès biométrique. | Périmètre sécurisé, escorte visiteurs, logs d'accès physique. | T1200 | — |
| GRD-TE-0014 | Trusted Contractor Abuse | Exploitation des accès légitimes d'un sous-traitant pour pénétrer le réseau mission. | Surveillance des sessions sous-traitants, journalisation renforcée. | Accès just-in-time, PAM (Privileged Access Management), audits. | T1199 | — |

---

### GRD-TA-0003 — Ground Execution

> Exécution de code malveillant sur l'infrastructure du segment sol.

| ID | Technique | Description | Détection | Contre-mesure | Mapping ATT&CK | Mapping SPARTA |
|---|---|---|---|---|---|---|
| GRD-TE-0020 | Malicious Script on MCC Systems | Exécution de scripts PowerShell/Python/Bash sur les systèmes du centre de contrôle mission. | Surveillance des processus, AMSI, logging script. | Application whitelisting, désactivation des interpréteurs non nécessaires. | T1059 | — |
| GRD-TE-0021 | SCADA/ICS Command Injection | Injection de commandes dans les systèmes SCADA/ICS de contrôle des antennes et équipements sol. | Monitoring des commandes SCADA anormales, IDS industriel. | Segmentation OT/IT, validation des commandes, liste blanche. | T0807, T0855 | — |
| GRD-TE-0022 | Trojanized Ground Software Update | Mise à jour logicielle piégée déployée sur les systèmes opérationnels du segment sol. | Vérification de signature, comparaison de hash. | Supply chain sécurisée, tests en environnement sandbox. | T1195.002 | — |
| GRD-TE-0023 | Cloud API Exploitation | Exploitation d'APIs cloud mal sécurisées (AWS Ground Station API, Azure Orbital) pour exécuter des commandes. | Monitoring des appels API cloud, alertes sur anomalies. | IAM restrictif, rotation des clés, API Gateway. | T1059.009 | — |

---

### GRD-TA-0004 — Ground Persistence

> Maintien de l'accès aux systèmes du segment sol malgré les redémarrages et changements de credentials.

| ID | Technique | Description | Détection | Contre-mesure | Mapping ATT&CK | Mapping SPARTA |
|---|---|---|---|---|---|---|
| GRD-TE-0030 | Backdoor on Ground Station Server | Installation d'une porte dérobée sur un serveur critique de station sol. | EDR, hunting des processus persistants, analyse mémoire. | Contrôle d'intégrité des fichiers, EDR avancé. | T1543, T1547 | — |
| GRD-TE-0031 | Compromised MCC Credentials | Vol et réutilisation de credentials d'opérateurs mission pour maintenir l'accès. | Détection de connexions anormales, corrélation géo-IP. | MFA, rotation fréquente, PAM, surveillance des comptes à privilèges. | T1078 | — |
| GRD-TE-0032 | Cloud IAM Persistence | Création de rôles ou clés API persistants dans l'infrastructure cloud du segment sol. | Audit des politiques IAM, alertes sur création de clés. | Revue IAM régulière, principe du moindre privilège. | T1098 | — |

---

### GRD-TA-0005 — Ground Privilege Escalation

> Élévation de privilèges pour accéder aux interfaces de contrôle mission critiques.

| ID | Technique | Description | Détection | Contre-mesure | Mapping ATT&CK | Mapping SPARTA |
|---|---|---|---|---|---|---|
| GRD-TE-0040 | IT-to-OT Privilege Pivot | Exploitation d'un accès IT standard pour obtenir des privilèges sur le réseau OT de contrôle mission. | Surveillance des flux IT→OT, alertes de segmentation. | Segmentation stricte IT/OT, DMZ industrielle. | T0891 | — |
| GRD-TE-0041 | Mission Control Console Takeover | Prise de contrôle d'une console opérateur mission via escalade de privilèges locale ou distante. | Logs d'accès aux consoles critiques, anomalie de session. | Authentification renforcée, principe des 4 yeux. | T1068 | — |
| GRD-TE-0042 | Key Management System Compromise | Compromission du système de gestion de clés cryptographiques (utilisées pour chiffrer les commandes TT&C). | Audit des accès au KMS, alertes sur exportation de clés. | HSM (Hardware Security Module), séparation des rôles. | T1552 | — |

---

### GRD-TA-0006 — Ground Lateral Movement

> Déplacement latéral dans les réseaux du segment sol, pivot de l'IT vers l'OT et les systèmes de contrôle mission.

| ID | Technique | Description | Détection | Contre-mesure | Mapping ATT&CK | Mapping SPARTA |
|---|---|---|---|---|---|---|
| GRD-TE-0050 | Corporate IT to Mission Network Pivot | Passage du réseau IT corporate au réseau mission via des failles de segmentation. | Monitoring des flux inter-VLAN, détection de tunnels. | Segmentation réseau, firewalls de nouvelle génération. | T1021 | — |
| GRD-TE-0051 | Ground Station to Ground Station Hop | Déplacement d'une station sol à une autre via le réseau de management partagé. | Corrélation des connexions entre stations, anomalies. | Isolation réseau par station, Zero Trust inter-stations. | T1021.002 | — |
| GRD-TE-0052 | Cloud Segment Lateral Movement | Mouvement latéral entre instances cloud du segment sol (ex: d'un bucket S3 de télémétrie vers l'instance EC2 de contrôle). | CloudTrail, alertes sur mouvements inhabituels. | Micro-segmentation cloud, IAM granulaire. | T1550 | — |

---

### GRD-TA-0007 — Ground Collection

> Collecte de données de mission, de télémétrie et d'informations opérationnelles depuis les systèmes du segment sol.

| ID | Technique | Description | Détection | Contre-mesure | Mapping ATT&CK | Mapping SPARTA |
|---|---|---|---|---|---|---|
| GRD-TE-0060 | Telemetry Data Harvesting | Collecte des données de télémétrie satellite stockées dans les bases de données du segment sol. | Surveillance des accès aux bases télémétrie, DLP. | Chiffrement au repos, contrôle d'accès granulaire. | T1005 | — |
| GRD-TE-0061 | Mission Planning Data Theft | Vol des données de planification de mission (orbites, manœuvres, fenêtres de communication). | Audit des accès aux fichiers de planification. | Classification des données, DRM. | T1083, T1119 | — |
| GRD-TE-0062 | Cryptographic Key Extraction | Extraction des clés de chiffrement TT&C depuis le segment sol. | Alertes HSM, audit des accès au KMS. | HSM certifié, double contrôle, journalisation. | T1552.004 | — |
| GRD-TE-0063 | AI/ML Model Exfiltration | Vol des modèles IA/ML opérationnels avant leur embarquement (MLOps pipeline). | Monitoring du pipeline MLOps, fingerprinting des modèles. | Watermarking des modèles, accès restreint au registre. | (ATLAS) AML.T0024 | — |

---

### GRD-TA-0008 — Ground Exfiltration

> Vol de données depuis l'infrastructure du segment sol.

| ID | Technique | Description | Détection | Contre-mesure | Mapping ATT&CK | Mapping SPARTA |
|---|---|---|---|---|---|---|
| GRD-TE-0070 | Exfiltration Over Internet | Exfiltration de données mission vers un serveur C2 externe via Internet. | DLP, analyse des flux sortants, proxy inspection. | Filtrage sortant, DLP, proxy SSL. | T1041 | — |
| GRD-TE-0071 | Exfiltration via Cloud Storage | Transfert de données vers un stockage cloud non autorisé (Dropbox, GDrive, S3 attaquant). | CASB, monitoring des uploads cloud non autorisés. | CASB, blocage des cloud non approuvés. | T1567 | — |
| GRD-TE-0072 | Physical Media Exfiltration | Copie de données sur support physique (USB, disque dur) depuis les systèmes du segment sol. | Endpoint DLP, contrôle des ports USB. | Désactivation des ports USB, politique de support amovible. | T1052 | — |

---

### GRD-TA-0009 — Ground Impact

> Disruption, dégradation ou destruction des opérations du segment sol et des capacités de contrôle mission.

| ID | Technique | Description | Détection | Contre-mesure | Mapping ATT&CK | Mapping SPARTA |
|---|---|---|---|---|---|---|
| GRD-TE-0080 | Mission Control System Disruption | Disruption des systèmes de contrôle mission rendant impossible le pilotage du satellite. | Monitoring de la disponibilité des systèmes critiques. | Redondance, basculement sur site de backup. | T1489 | — |
| GRD-TE-0081 | Wiper Deployment on Ground Systems | Déploiement d'un wiper (type AcidRain) pour détruire les systèmes du segment sol. | EDR, surveillance de la MBR/GPT, alertes de suppression massive. | Sauvegardes offline, segmentation, EDR avancé. | T1485, T1561 | — |
| GRD-TE-0082 | Ransomware on Ground Infrastructure | Chiffrement des systèmes du segment sol par ransomware, bloquant les opérations mission. | EDR, détection de chiffrement massif, honeypots. | Sauvegardes 3-2-1, segmentation, plan de continuité. | T1486 | — |
| GRD-TE-0083 | Sabotage of Ground Antenna Systems | Sabotage logique ou physique des systèmes de contrôle d'antenne empêchant la communication avec le satellite. | Monitoring SCADA, surveillance physique. | Redondance d'antennes, sites de backup, détection d'intrusion physique. | T0831 | — |
| GRD-TE-0084 | Falsification of Telemetry Display | Altération de l'affichage de télémétrie pour présenter des données fausses aux opérateurs. | Comparaison multi-source de télémétrie, checksum. | Validation croisée des données, affichage redondant. | T0832 | — |

---

## 2. Segment Link (Liaison)

### LNK-TA-0001 — Link Signal Reconnaissance

> Identification et caractérisation des liaisons de communication entre segments sol et spatial.

| ID | Technique | Description | Détection | Contre-mesure | Mapping SPARTA | Mapping SPACE-SHIELD |
|---|---|---|---|---|---|---|
| LNK-TE-0001 | RF Signal Characterization | Analyse des signaux RF pour identifier fréquences, bandes passantes, modulations et protocoles utilisés. | Détection de stations d'écoute non autorisées, SIGINT défensif. | Étalement de spectre, saut de fréquence, chiffrement. | REC-0001 | SS-REC-01 |
| LNK-TE-0002 | TT&C Protocol Fingerprinting | Identification des protocoles TT&C utilisés (CCSDS, propriétaires) par analyse du trafic. | Surveillance des signaux de sondage sur les fréquences TT&C. | Minimisation des métadonnées protocolaires, chiffrement du protocole. | REC-0002 | SS-REC-02 |
| LNK-TE-0003 | Communication Window Mapping | Cartographie des fenêtres de communication (passes) entre stations sol et satellites. | Surveillance de la corrélation temporelle des observations. | Classification des éphémérides, variation des passes. | REC-0003 | — |
| LNK-TE-0004 | ISL Link Discovery | Découverte des liaisons inter-satellites dans une constellation. | Surveillance des émissions ISL non planifiées. | Chiffrement ISL, authentification mutuelle. | — | SS-REC-03 |

---

### LNK-TA-0002 — Link Signal Interception

> Interception et écoute des communications entre segments sol et spatial.

| ID | Technique | Description | Détection | Contre-mesure | Mapping SPARTA | Mapping SPACE-SHIELD |
|---|---|---|---|---|---|---|
| LNK-TE-0010 | Downlink Eavesdropping | Interception passive du signal descendant pour capturer la télémétrie ou les données de charge utile. | Difficile à détecter (écoute passive). | Chiffrement du downlink (CCSDS SLS), bandes étroites. | EAV-0001 | SS-INT-01 |
| LNK-TE-0011 | Uplink Command Interception | Interception des commandes TT&C montantes pour comprendre le protocole et les commandes légitimes. | Détection de stations d'écoute proches des stations sol. | Chiffrement des commandes, authentification TC. | EAV-0002 | SS-INT-02 |
| LNK-TE-0012 | ISL Traffic Interception | Interception du trafic entre satellites d'une même constellation. | Surveillance de la qualité du lien ISL. | Chiffrement ISL end-to-end, quantum key distribution (futur). | — | SS-INT-03 |

---

### LNK-TA-0003 — Link Signal Injection

> Injection de commandes ou données malveillantes dans les liaisons de communication.

| ID | Technique | Description | Détection | Contre-mesure | Mapping SPARTA | Mapping SPACE-SHIELD |
|---|---|---|---|---|---|---|
| LNK-TE-0020 | Malicious Uplink Command Injection | Envoi de commandes TT&C forgées depuis une station non autorisée. | Détection d'uplinks non planifiés, vérification de l'horodatage. | Authentification TC (CCSDS 355.0), géolocalisation de l'émetteur. | INJ-0001 | SS-INJ-01 |
| LNK-TE-0021 | Firmware Update Injection via Uplink | Injection d'une mise à jour firmware malveillante via le canal de mise à jour OTA. | Vérification de signature du firmware, comparaison de version. | Signature cryptographique obligatoire, boot sécurisé. | INJ-0002 | SS-INJ-02 |
| LNK-TE-0022 | Replay Attack on TT&C | Rejeu de commandes TT&C légitimes capturées précédemment. | Détection de commandes en doublon, vérification des nonces. | Nonces, horodatage cryptographique, compteurs de séquence. | INJ-0003 | SS-INJ-03 |
| LNK-TE-0023 | Data Injection in Telemetry Stream | Injection de données falsifiées dans le flux de télémétrie descendant. | Validation d'intégrité du flux TM, anomalies statistiques. | HMAC sur les trames TM, redondance de mesure. | — | SS-INJ-04 |

---

### LNK-TA-0004 — Link Signal Jamming

> Disruption ou déni de communication entre segments sol et spatial par interférence électromagnétique.

| ID | Technique | Description | Détection | Contre-mesure | Mapping SPARTA | Mapping SPACE-SHIELD |
|---|---|---|---|---|---|---|
| LNK-TE-0030 | Broadband Uplink Jamming | Brouillage large bande du signal montant pour empêcher l'envoi de commandes au satellite. | Monitoring du rapport signal/bruit, détection de puissance. | Étalement de spectre, augmentation de puissance, antennes directionnelles. | JAM-0001 | SS-JAM-01 |
| LNK-TE-0031 | Targeted Downlink Jamming | Brouillage ciblé du signal descendant vers une station sol spécifique. | Dégradation du C/N0, perte de lock sur le signal. | Diversité de stations sol, antennes adaptatives. | JAM-0002 | SS-JAM-02 |
| LNK-TE-0032 | ISL Disruption | Perturbation des liaisons inter-satellites par interférence laser ou RF. | Monitoring de la qualité du lien ISL, BER anormal. | Redondance de chemins ISL, rerouting automatique. | — | SS-JAM-03 |
| LNK-TE-0033 | GPS/GNSS Jamming of Ground Segment | Brouillage du signal GPS/GNSS utilisé pour la synchronisation temporelle des stations sol. | Monitoring de la qualité du signal GNSS, détection de dérive. | Sources de temps alternatives (atomic clocks), anti-jamming. | JAM-0003 | — |

---

### LNK-TA-0005 — Link Signal Spoofing

> Falsification ou rejeu de signaux légitimes pour tromper les systèmes sol ou spatiaux.

| ID | Technique | Description | Détection | Contre-mesure | Mapping SPARTA | Mapping SPACE-SHIELD |
|---|---|---|---|---|---|---|
| LNK-TE-0040 | GPS/GNSS Spoofing | Envoi de faux signaux GPS/GNSS pour tromper la navigation du satellite ou la synchronisation sol. | Comparaison multi-constellation, détection d'anomalies de position. | Authentification GNSS (Galileo OSNMA), récepteurs anti-spoofing. | SPF-0001 | SS-SPF-01 |
| LNK-TE-0041 | TT&C Spoofing | Imitation d'une station sol légitime pour envoyer de fausses commandes au satellite. | Vérification de l'identité de l'émetteur, géolocalisation RF. | Authentification mutuelle, certificats numériques. | SPF-0002 | SS-SPF-02 |
| LNK-TE-0042 | Telemetry Spoofing | Envoi de fausse télémétrie pour tromper les opérateurs au sol sur l'état du satellite. | Comparaison avec les modèles orbitaux, corrélation multi-source. | Signature cryptographique de la TM, validation croisée. | SPF-0003 | SS-SPF-03 |

---

### LNK-TA-0006 — Link Protocol Exploitation

> Exploitation de vulnérabilités dans les protocoles de communication spatiale (CCSDS, TT&C, ISL).

| ID | Technique | Description | Détection | Contre-mesure | Mapping SPARTA | Mapping SPACE-SHIELD |
|---|---|---|---|---|---|---|
| LNK-TE-0050 | CCSDS Protocol Vulnerability Exploitation | Exploitation de failles dans l'implémentation des protocoles CCSDS (parsing, overflow, injection). | Fuzzing des parseurs CCSDS, monitoring des erreurs de protocole. | Tests de robustesse, implémentations certifiées. | EXP-0001 | SS-EXP-01 |
| LNK-TE-0051 | TT&C Authentication Bypass | Contournement du mécanisme d'authentification des commandes TT&C. | Monitoring des commandes TC non authentifiées ou mal formées. | CCSDS 355.0 (Space Data Link Security), MFA sur les canaux TC. | EXP-0002 | SS-EXP-02 |
| LNK-TE-0052 | ISL Protocol Exploitation | Exploitation de vulnérabilités dans les protocoles de liaison inter-satellites. | Monitoring des échanges ISL anormaux. | Protocoles ISL sécurisés, authentification mutuelle. | — | SS-EXP-03 |

---

## 3. Segment Space (Spatial)

### SPC-TA-0001 — Space Initial Compromise

> Obtention d'un accès initial aux systèmes du segment spatial (bus satellite, charge utile).

| ID | Technique | Description | Détection | Contre-mesure | Mapping SPARTA | Mapping SPACE-SHIELD |
|---|---|---|---|---|---|---|
| SPC-TE-0001 | Malicious Command via Compromised Ground | Envoi de commandes malveillantes au satellite depuis un segment sol compromis. | Validation des commandes par séquenceur embarqué, commandes inattendues. | Validation embarquée des commandes, double autorisation. | IC-0001 | SS-IC-01 |
| SPC-TE-0002 | Pre-Launch Software Supply Chain Attack | Injection de code malveillant dans le logiciel embarqué avant le lancement (phase AIT). | Audit de code, vérification d'intégrité pré-lancement. | Revue de code formelle, environnements de build sécurisés. | IC-0002 | SS-IC-02 |
| SPC-TE-0003 | Exploitation of Satellite Debug Interface | Exploitation d'interfaces de debug restées actives après le lancement (JTAG, UART, ports de test). | Audit matériel pré-lancement, test des interfaces. | Désactivation des interfaces debug avant lancement. | IC-0003 | — |
| SPC-TE-0004 | Rogue OTA Update Acceptance | Acceptation d'une mise à jour OTA non légitime par le satellite. | Monitoring des séquences de mise à jour, comparaison de version. | Signature cryptographique, boot sécurisé, rollback automatique. | IC-0004 | SS-IC-03 |

---

### SPC-TA-0002 — Space Payload Manipulation

> Altération, désactivation ou détournement des opérations de charge utile satellite.

| ID | Technique | Description | Détection | Contre-mesure | Mapping SPARTA | Mapping SPACE-SHIELD |
|---|---|---|---|---|---|---|
| SPC-TE-0010 | Payload Reconfiguration | Modification non autorisée des paramètres de charge utile (fréquences, puissance, couverture). | Monitoring de la configuration payload, alertes sur changements. | Authentification des commandes payload, logs embarqués. | PM-0001 | SS-PM-01 |
| SPC-TE-0011 | Payload Service Denial | Désactivation de la charge utile pour interrompre le service (communications, observation, navigation). | Monitoring de la disponibilité du service, alertes opérateur. | Redondance de charge utile, procédures de recovery automatiques. | PM-0002 | SS-PM-02 |
| SPC-TE-0012 | Payload Hijacking | Détournement de la charge utile pour un usage non autorisé (ex: relais de communication pour attaquant). | Analyse du trafic payload, détection de services non autorisés. | Contrôle d'accès à la charge utile, monitoring du spectre. | PM-0003 | SS-PM-03 |
| SPC-TE-0013 | Sensor Data Manipulation | Altération des données capteurs (imagerie, mesures) avant retransmission. | Validation croisée avec d'autres sources, checksums embarqués. | Signature des données capteur à la source, watermarking. | PM-0004 | — |

---

### SPC-TA-0003 — Space Firmware Tampering

> Ciblage du firmware embarqué incluant mises à jour OTA malveillantes et manipulation de l'IA embarquée.

| ID | Technique | Description | Détection | Contre-mesure | Mapping SPARTA | Mapping SPACE-SHIELD |
|---|---|---|---|---|---|---|
| SPC-TE-0020 | Malicious Firmware Flash | Remplacement du firmware embarqué par une version malveillante via commande OTA. | Vérification d'intégrité au boot, mesure du firmware. | Secure boot, TPM embarqué, signature cryptographique. | FW-0001 | SS-FW-01 |
| SPC-TE-0021 | RTOS Exploitation | Exploitation de vulnérabilités dans le système d'exploitation temps réel (VxWorks, RTEMS, FreeRTOS). | Monitoring des exceptions runtime, watchdog. | Patches RTOS, analyse statique, fuzzing pré-lancement. | FW-0002 | SS-FW-02 |
| SPC-TE-0022 | Adversarial ML Model Poisoning | Empoisonnement d'un modèle IA embarqué pour altérer ses décisions (navigation autonome, traitement d'image). | Monitoring de la dérive du modèle, validation de cohérence. | Validation du modèle, federated learning sécurisé, enclaves de confiance. | (ATLAS) AML.T0020 | — |
| SPC-TE-0023 | Boot Sequence Manipulation | Altération de la séquence de démarrage pour charger un firmware modifié. | Attestation de démarrage, mesure de la chaîne de boot. | Root of Trust matériel, secure boot chain. | FW-0003 | SS-FW-03 |

---

### SPC-TA-0004 — Space Data Exfiltration

> Extraction de données sensibles depuis les systèmes du segment spatial.

| ID | Technique | Description | Détection | Contre-mesure | Mapping SPARTA | Mapping SPACE-SHIELD |
|---|---|---|---|---|---|---|
| SPC-TE-0030 | Unauthorized Downlink of Mission Data | Utilisation du downlink pour transmettre des données vers une station sol non autorisée. | Monitoring des fenêtres de downlink, stations réceptrices inattendues. | Chiffrement end-to-end, authentification de station. | DE-0001 | SS-DE-01 |
| SPC-TE-0031 | Covert Channel via Telemetry | Utilisation de la télémétrie pour exfiltrer des données en les encodant dans les paramètres normaux. | Analyse statistique de la télémétrie, détection d'anomalies. | Monitoring de la bande passante TM, validation de format. | DE-0002 | SS-DE-02 |
| SPC-TE-0032 | ISL Data Rerouting | Détournement de données via les liaisons inter-satellites vers un satellite compromis tiers. | Monitoring du routage ISL, détection de chemins anormaux. | Chiffrement ISL, tables de routage signées. | — | SS-DE-03 |

---

### SPC-TA-0005 — Space Denial of Service

> Disruption ou dégradation des opérations et services satellitaires.

| ID | Technique | Description | Détection | Contre-mesure | Mapping SPARTA | Mapping SPACE-SHIELD |
|---|---|---|---|---|---|---|
| SPC-TE-0040 | Satellite Bus Overload | Surcharge du processeur ou de la mémoire du bus satellite par des commandes massives ou malformées. | Monitoring des ressources embarquées, watchdog. | Rate limiting des commandes, watchdog hardware. | DoS-0001 | SS-DoS-01 |
| SPC-TE-0041 | Power System Manipulation | Manipulation du système de gestion d'énergie pour provoquer un déficit énergétique. | Monitoring des niveaux de puissance, alertes de seuil. | Protection matérielle du power bus, modes dégradés automatiques. | DoS-0002 | SS-DoS-02 |
| SPC-TE-0042 | Thermal Control Sabotage | Altération des paramètres de contrôle thermique pour provoquer une surchauffe ou un refroidissement excessif. | Monitoring thermique, alertes de seuil. | Redondance thermique, modes autonomes de protection. | DoS-0003 | SS-DoS-03 |
| SPC-TE-0043 | Service Degradation via Configuration Change | Modification de la configuration satellite pour dégrader progressivement le service sans interruption immédiate. | Monitoring des performances de service, tendances. | Baseline de configuration, alertes sur déviations. | DoS-0004 | — |

---

### SPC-TA-0006 — Space Orbital Impact

> Conséquences physiques en orbite : manipulation du contrôle d'attitude, modification des paramètres orbitaux.

| ID | Technique | Description | Détection | Contre-mesure | Mapping SPARTA | Mapping SPACE-SHIELD |
|---|---|---|---|---|---|---|
| SPC-TE-0050 | AOCS Manipulation | Modification des paramètres du système de contrôle d'attitude et d'orbite (AOCS) pour désorienter le satellite. | Monitoring des paramètres d'attitude, anomalie de pointage. | Validation autonome des commandes AOCS, limites de sécurité. | OI-0001 | SS-OI-01 |
| SPC-TE-0051 | Orbital Parameter Modification | Modification de l'orbite du satellite (altitude, inclinaison) pour le rendre inutilisable ou le faire réentrer. | Suivi orbital indépendant (radar sol), anomalie d'éphémérides. | Limites de manœuvre embarquées, validation multi-source. | OI-0002 | SS-OI-02 |
| SPC-TE-0052 | Collision Course Setting | Programmation d'une trajectoire de collision avec un autre objet spatial (satellite ou débris). | Space Situational Awareness (SSA), alertes de proximité. | Systèmes SSA, manœuvre d'évitement automatique. | OI-0003 | SS-OI-03 |
| SPC-TE-0053 | Propulsion System Sabotage | Activation non autorisée de la propulsion pour épuiser le carburant ou modifier l'orbite de façon irréversible. | Monitoring de la consommation propulsive, alertes de seuil. | Protection matérielle de la propulsion, double autorisation. | OI-0004 | SS-OI-04 |
| SPC-TE-0054 | De-orbiting Command | Envoi d'une commande de désorbitation forcée pour détruire le satellite par rentrée atmosphérique. | Détection de commande de désorbitation non planifiée. | Triple autorisation pour désorbitation, timeout de sécurité. | OI-0005 | SS-OI-05 |

---

## Statistiques de la matrice

| Métrique | Valeur |
|---|---|
| **Tactiques totales** | 21 |
| **Techniques totales** | 86 |
| **Segment Ground** | 9 tactiques, 38 techniques |
| **Segment Link** | 6 tactiques, 23 techniques |
| **Segment Space** | 6 tactiques, 25 techniques |
| **Format** | STIX 2.1 |
| **Version** | 1.1 |

---

*GLSS Framework — Matrice détaillée v1.1 — © 2026 Badaoui Salah, Hajer Boudhir, Achille Lele — CC BY-SA 4.0*
