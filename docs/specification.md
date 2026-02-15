# GLSS Framework — Spécification Technique

**Version 1.1 — Février 2026**
**Statut : Draft**
**Licence : CC BY-SA 4.0**
**Auteurs : Badaoui Salah, Hajer Boudhir , Achille Lele**

---

## 1. Introduction

### 1.1 Objet du document

Ce document constitue la spécification technique du framework GLSS (Ground-Link-Space Security). Il définit le modèle conceptuel, la taxonomie, les conventions de nommage, le format de données et les règles d'extension du framework.

Ce document est destiné aux implémenteurs, contributeurs et intégrateurs du framework GLSS dans des environnements opérationnels (SOC, SIEM/SOAR, audit, Red Team).

### 1.2 Contexte et motivation

Les systèmes spatiaux sont devenus des infrastructures critiques assurant des services essentiels (télécommunications, navigation, observation de la Terre, défense). Leur surface d'attaque s'étend sur trois domaines physiques distincts — le sol, les liaisons de communication et l'espace orbital — chacun présentant des vecteurs de menace spécifiques.

Les référentiels de cybersécurité existants ne couvrent qu'une partie de cette surface d'attaque :

| Référentiel | Segment couvert | Limitation principale |
|---|---|---|
| MITRE ATT&CK | IT/OT terrestre | Ne modélise pas le spatial |
| SPARTA | Segment spatial | Ignore le segment sol |
| SPACE-SHIELD | Espace + liaisons | Institutionnel ESA, pas de segment sol |
| TREKS | Véhicules spatiaux | Scope limité au bus satellite |
| MITRE ATLAS | IA/ML adversarial | Transversal, pas spécifique spatial |

GLSS résout ce problème en proposant un modèle tri-segmentaire unifié capable de tracer une kill chain complète depuis la compromission initiale d'un centre de contrôle au sol jusqu'à l'impact orbital sur un satellite.

### 1.3 Portée

Ce document couvre :

- Le modèle conceptuel tri-segmentaire (Section 2)
- La taxonomie des tactiques et techniques (Section 3)
- Les conventions de nommage et d'identification (Section 4)
- Le format de données STIX 2.1 (Section 5)
- Les règles de mapping vers les référentiels existants (Section 6)
- Le modèle de kill chain multi-segments (Section 7)
- Les règles de conformité réglementaire (Section 8)
- Les règles d'extension et de contribution (Section 9)

### 1.4 Documents de référence

| Référence | Document |
|---|---|
| [ATT&CK] | MITRE ATT&CK Framework v15 |
| [SPARTA] | Space Attack Research and Tactic Analysis v3.1 |
| [SPACE-SHIELD] | ESA Space Attacks and Countermeasures Engineering Shield |
| [TREKS] | Targeting, Reconnaissance, & Exploitation Kill-Chain for Space Vehicles |
| [ATLAS] | MITRE Adversarial Threat Landscape for AI Systems |
| [STIX] | OASIS STIX 2.1 Specification |
| [NIS2] | Directive (UE) 2022/2555 |
| [CRA] | Règlement Cyber Resilience Act (UE) 2024/2847 |
| [ECSS] | ECSS-Q-ST-80C — Software Product Assurance |
| [NIST-IR-8401] | NIST IR 8401 — Satellite Ground Segment |
| [IEC-62443] | IEC 62443 — Industrial Automation Security |
| [CCSDS-350.1] | CCSDS 350.1-G-3 — Space Data Link Security Protocol |

### 1.5 Terminologie

| Terme | Définition |
|---|---|
| **Segment** | Domaine physique d'un système spatial (Ground, Link, Space) |
| **Tactique** | Objectif tactique de l'adversaire dans un segment donné |
| **Technique** | Méthode spécifique employée pour atteindre un objectif tactique |
| **Sous-technique** | Variante spécifique d'une technique |
| **Kill Chain** | Séquence ordonnée de tactiques/techniques traversant un ou plusieurs segments |
| **Mapping** | Correspondance structurée entre une technique GLSS et une technique d'un autre référentiel |
| **TTP** | Tactics, Techniques and Procedures |
| **C2** | Command and Control |
| **TT&C** | Telemetry, Tracking and Command |
| **ISL** | Inter-Satellite Link |
| **OTA** | Over-The-Air (mise à jour) |
| **AOCS** | Attitude and Orbit Control System |

---

## 2. Modèle conceptuel

### 2.1 Architecture tri-segmentaire

GLSS décompose tout système spatial en trois segments fondamentaux. Chaque segment possède ses propres tactiques, techniques et contre-mesures. Les attaques peuvent traverser un, deux ou trois segments (kill chains multi-segments).

```
┌─────────────────────────────────────────────────┐
│                  🛰️ SPACE                        │
│  Satellite bus • Payload • Firmware • OTA • AI   │
│  ← SPARTA, SPACE-SHIELD, TREKS, ATLAS           │
│                                                   │
│  Tactiques : 6 (SPC-TA-0001 → SPC-TA-0006)      │
├─────────────────────────────────────────────────┤
│                  📡 LINK                         │
│  RF Up/Downlink • Optical • ISL • TT&C (CCSDS)  │
│  ← NIST IR 8401, SPACE-SHIELD                   │
│                                                   │
│  Tactiques : 6 (LNK-TA-0001 → LNK-TA-0006)     │
├─────────────────────────────────────────────────┤
│                  🏢 GROUND                       │
│  Control Centers • Ground Stations • IT/OT •     │
│  Cloud • MLOps • Supply Chain                    │
│  ← ATT&CK, IEC 62443, ATLAS                     │
│                                                   │
│  Tactiques : 9 (GRD-TA-0001 → GRD-TA-0009)      │
└─────────────────────────────────────────────────┘
```

### 2.2 Segment Ground (Sol)

Le segment sol englobe toute l'infrastructure terrestre nécessaire à l'exploitation des systèmes spatiaux.

**Périmètre :**

- Centres de contrôle mission (Mission Control Centers — MCC)
- Stations sol (Ground Stations) et antennes
- Réseaux IT corporate et réseaux OT industriels
- Infrastructure cloud (AWS Ground Station, Azure Orbital, etc.)
- Chaîne d'approvisionnement logicielle (supply chain)
- Systèmes MLOps pour modèles embarqués IA
- Systèmes de gestion de clés cryptographiques

**Référentiels primaires :** MITRE ATT&CK (Enterprise + ICS), IEC 62443, MITRE ATLAS

### 2.3 Segment Link (Liaison)

Le segment liaison couvre tous les canaux de communication entre le sol et l'espace, ainsi qu'entre satellites.

**Périmètre :**

- Liaisons montantes (uplink) et descendantes (downlink) en bande S/C/X/Ka/Ku
- Liaisons optiques (laser communication)
- Liaisons inter-satellites (ISL — Inter-Satellite Links)
- Protocoles TT&C (Telemetry, Tracking and Command)
- Protocoles CCSDS (Consultative Committee for Space Data Systems)
- Signaux de navigation (GPS, Galileo, GLONASS)

**Référentiels primaires :** NIST IR 8401, SPACE-SHIELD, CCSDS 350.1-G-3

### 2.4 Segment Space (Spatial)

Le segment spatial couvre les systèmes en orbite et leurs composants logiciels/matériels.

**Périmètre :**

- Bus satellite (plateforme)
- Charges utiles (payloads) — télécommunications, observation, navigation
- Firmware embarqué et systèmes temps réel (RTOS)
- Mécanismes de mise à jour OTA (Over-The-Air)
- Modèles IA embarqués (edge AI)
- Système AOCS (Attitude and Orbit Control System)
- Systèmes de propulsion et gestion d'énergie

**Référentiels primaires :** SPARTA, SPACE-SHIELD, TREKS, MITRE ATLAS

### 2.5 Relations inter-segments

Les segments ne sont pas isolés. GLSS modélise explicitement les transitions entre segments, ce qui constitue sa différenciation fondamentale.

**Transitions modélisées :**

| Transition | Direction | Exemple |
|---|---|---|
| Ground → Link | Montante | Injection de commande malveillante via uplink |
| Link → Space | Montante | Commande TT&C forgée atteignant le satellite |
| Space → Link | Descendante | Exfiltration de données via downlink détourné |
| Link → Ground | Descendante | Interception de télémétrie en station sol |
| Ground → Space | Directe (via supply chain) | Firmware compromis lors de l'intégration pré-lancement |
| Space → Space | Latérale | Propagation via ISL entre satellites d'une constellation |

---

## 3. Taxonomie des tactiques

### 3.1 Vue d'ensemble

La matrice GLSS v1.1 contient **21 tactiques** réparties sur 3 segments.

### 3.2 Segment Ground — 9 tactiques

| ID | Nom | Description |
|---|---|---|
| GRD-TA-0001 | Ground Reconnaissance | Collecte d'informations sur l'infrastructure du segment sol : centres de contrôle, stations sol, systèmes IT/OT de support. |
| GRD-TA-0002 | Ground Initial Access | Obtention d'un accès initial aux systèmes du segment sol. |
| GRD-TA-0003 | Ground Execution | Exécution de code malveillant sur l'infrastructure du segment sol. |
| GRD-TA-0004 | Ground Persistence | Maintien de l'accès aux systèmes du segment sol malgré les redémarrages et changements de credentials. |
| GRD-TA-0005 | Ground Privilege Escalation | Élévation de privilèges pour accéder aux interfaces de contrôle mission critiques. |
| GRD-TA-0006 | Ground Lateral Movement | Déplacement latéral dans les réseaux du segment sol, pivot de l'IT vers l'OT et les systèmes de contrôle mission. |
| GRD-TA-0007 | Ground Collection | Collecte de données de mission, de télémétrie et d'informations opérationnelles depuis les systèmes du segment sol. |
| GRD-TA-0008 | Ground Exfiltration | Vol de données depuis l'infrastructure du segment sol. |
| GRD-TA-0009 | Ground Impact | Disruption, dégradation ou destruction des opérations du segment sol et des capacités de contrôle mission. |

### 3.3 Segment Link — 6 tactiques

| ID | Nom | Description |
|---|---|---|
| LNK-TA-0001 | Link Signal Reconnaissance | Identification et caractérisation des liaisons de communication entre segments sol et spatial. |
| LNK-TA-0002 | Link Signal Interception | Interception et écoute des communications entre segments sol et spatial. |
| LNK-TA-0003 | Link Signal Injection | Injection de commandes ou données malveillantes dans les liaisons de communication. |
| LNK-TA-0004 | Link Signal Jamming | Disruption ou déni de communication entre segments sol et spatial par interférence électromagnétique. |
| LNK-TA-0005 | Link Signal Spoofing | Falsification ou rejeu de signaux légitimes pour tromper les systèmes sol ou spatiaux. |
| LNK-TA-0006 | Link Protocol Exploitation | Exploitation de vulnérabilités dans les protocoles de communication spatiale (CCSDS, TT&C, ISL). |

### 3.4 Segment Space — 6 tactiques

| ID | Nom | Description |
|---|---|---|
| SPC-TA-0001 | Space Initial Compromise | Obtention d'un accès initial aux systèmes du segment spatial (bus satellite, charge utile). |
| SPC-TA-0002 | Space Payload Manipulation | Altération, désactivation ou détournement des opérations de charge utile satellite. |
| SPC-TA-0003 | Space Firmware Tampering | Ciblage du firmware embarqué incluant mises à jour OTA malveillantes et manipulation de l'IA embarquée. |
| SPC-TA-0004 | Space Data Exfiltration | Extraction de données sensibles depuis les systèmes du segment spatial. |
| SPC-TA-0005 | Space Denial of Service | Disruption ou dégradation des opérations et services satellitaires. |
| SPC-TA-0006 | Space Orbital Impact | Conséquences physiques en orbite : manipulation du contrôle d'attitude, modification des paramètres orbitaux. |

---

## 4. Conventions de nommage

### 4.1 Identifiants de tactiques

```
Format : {SEGMENT}-TA-{NUMÉRO}

Préfixes de segment :
  GRD  = Ground (Sol)
  LNK  = Link (Liaison)
  SPC  = Space (Spatial)

Numérotation : 4 chiffres, démarrant à 0001
Exemples : GRD-TA-0001, LNK-TA-0003, SPC-TA-0006
```

### 4.2 Identifiants de techniques

```
Format : {SEGMENT}-TE-{NUMÉRO}

Exemples : GRD-TE-0001, LNK-TE-0015, SPC-TE-0008
```

### 4.3 Identifiants de sous-techniques

```
Format : {SEGMENT}-TE-{NUMÉRO}.{SOUS-NUMÉRO}

Exemples : GRD-TE-0001.001, LNK-TE-0015.003, SPC-TE-0008.002
```

### 4.4 Identifiants STIX 2.1

Les objets GLSS utilisent le namespace STIX étendu avec le préfixe `x-mitre-` pour compatibilité avec l'écosystème ATT&CK.

```
Matrice :   x-mitre-matrix--glss-framework
Tactique :  x-mitre-tactic--glss-{segment}-{shortname}
Technique : attack-pattern--glss-{segment}-{shortname}
```

### 4.5 Propriétés GLSS étendues

Chaque objet STIX GLSS porte la propriété custom `x_glss_segment` avec une valeur parmi : `ground`, `link`, `space`.

---

## 5. Format de données STIX 2.1

### 5.1 Structure du bundle

La matrice GLSS est distribuée sous forme d'un bundle STIX 2.1 (`matrix/glss-matrix.json`).

```json
{
  "type": "bundle",
  "id": "bundle--glss-framework-v1.1",
  "spec_version": "2.1",
  "created": "2026-02-15T00:00:00.000Z",
  "modified": "2026-02-15T00:00:00.000Z",
  "objects": [
    { "type": "x-mitre-matrix", "..." },
    { "type": "x-mitre-tactic", "..." },
    { "type": "attack-pattern", "..." }
  ]
}
```

### 5.2 Objet Matrix

```json
{
  "type": "x-mitre-matrix",
  "id": "x-mitre-matrix--glss-framework",
  "name": "GLSS Framework Matrix",
  "description": "Ground-Link-Space Security — Unified threat modeling framework for space systems cybersecurity.",
  "tactic_refs": [
    "x-mitre-tactic--glss-ground-reconnaissance",
    "x-mitre-tactic--glss-ground-initial-access",
    "..."
  ]
}
```

### 5.3 Objet Tactic

```json
{
  "type": "x-mitre-tactic",
  "id": "x-mitre-tactic--glss-ground-reconnaissance",
  "name": "Ground Reconnaissance",
  "description": "Techniques used to gather information about ground segment infrastructure.",
  "x_mitre_shortname": "ground-reconnaissance",
  "x_glss_segment": "ground",
  "created": "2026-02-15T00:00:00.000Z",
  "modified": "2026-02-15T00:00:00.000Z"
}
```

### 5.4 Objet Technique (attack-pattern)

```json
{
  "type": "attack-pattern",
  "id": "attack-pattern--glss-grd-te-0001",
  "name": "Ground Station Network Scanning",
  "description": "Adversary scans ground station networks to identify active services, open ports, and network topology.",
  "kill_chain_phases": [
    {
      "kill_chain_name": "glss-framework",
      "phase_name": "ground-reconnaissance"
    }
  ],
  "x_glss_segment": "ground",
  "x_glss_id": "GRD-TE-0001",
  "x_glss_tactic_ref": "GRD-TA-0001",
  "x_glss_detection": "Monitor for anomalous network scanning activity on ground station subnets.",
  "x_glss_mitigation": "Implement network segmentation and IDS/IPS on ground station perimeters.",
  "x_glss_platforms": ["Mission Control Center", "Ground Station"],
  "x_glss_data_sources": ["Network Traffic", "Firewall Logs", "IDS Alerts"],
  "x_glss_severity": "medium",
  "x_glss_mapping_attack": ["T1046"],
  "x_glss_mapping_sparta": [],
  "x_glss_mapping_spaceshield": [],
  "external_references": [
    {
      "source_name": "glss-framework",
      "external_id": "GRD-TE-0001",
      "url": "https://glss-framework.org/techniques/GRD-TE-0001"
    }
  ]
}
```

### 5.5 Propriétés custom GLSS

| Propriété | Type | Description |
|---|---|---|
| `x_glss_segment` | string | Segment GLSS : `ground`, `link`, `space` |
| `x_glss_id` | string | Identifiant GLSS court (ex: `GRD-TE-0001`) |
| `x_glss_tactic_ref` | string | Référence vers la tactique parente (ex: `GRD-TA-0001`) |
| `x_glss_detection` | string | Méthode de détection recommandée |
| `x_glss_mitigation` | string | Contre-mesure recommandée |
| `x_glss_platforms` | list[string] | Plateformes cibles (MCC, Ground Station, Satellite Bus, etc.) |
| `x_glss_data_sources` | list[string] | Sources de données pour la détection |
| `x_glss_severity` | string | Sévérité : `low`, `medium`, `high`, `critical` |
| `x_glss_mapping_attack` | list[string] | IDs ATT&CK correspondants |
| `x_glss_mapping_sparta` | list[string] | IDs SPARTA correspondants |
| `x_glss_mapping_spaceshield` | list[string] | IDs SPACE-SHIELD correspondants |

---

## 6. Règles de mapping

### 6.1 Principes de mapping

Les mappings GLSS vers d'autres référentiels respectent les règles suivantes :

1. **Relation N:M** — Une technique GLSS peut correspondre à plusieurs techniques d'un autre référentiel, et inversement.
2. **Mapping justifié** — Chaque correspondance doit être accompagnée d'une justification textuelle.
3. **Mapping versionné** — Chaque fichier de mapping spécifie la version du référentiel cible.
4. **Absence de mapping** — Si aucune correspondance n'existe, le champ reste vide (liste vide `[]`).

### 6.2 Structure des fichiers de mapping

Les fichiers de mapping sont stockés dans `mappings/` au format JSON.

```json
{
  "metadata": {
    "glss_version": "1.1",
    "target_framework": "MITRE ATT&CK",
    "target_version": "v15",
    "last_updated": "2026-02-15",
    "authors": ["GLSS Framework Team"]
  },
  "mappings": [
    {
      "glss_id": "GRD-TE-0001",
      "glss_name": "Ground Station Network Scanning",
      "target_ids": ["T1046"],
      "target_names": ["Network Service Discovery"],
      "relationship": "equivalent",
      "justification": "Both describe network scanning for service discovery. GLSS contextualizes to ground station infrastructure."
    }
  ]
}
```

### 6.3 Types de relation

| Relation | Signification |
|---|---|
| `equivalent` | Correspondance directe, même concept |
| `subset` | La technique GLSS est un sous-ensemble de la technique cible |
| `superset` | La technique GLSS englobe la technique cible |
| `related` | Concepts liés mais non identiques |
| `none` | Aucune correspondance dans le référentiel cible |

### 6.4 Référentiels de mapping supportés

| Référentiel | Fichier | État |
|---|---|---|
| MITRE ATT&CK | `mappings/attack-mapping.json` | En cours |
| SPARTA | `mappings/sparta-mapping.json` | En cours |
| SPACE-SHIELD | `mappings/spaceshield-mapping.json` | En cours |
| MITRE ATLAS | `mappings/atlas-mapping.json` | Planifié |
| NIST CSF v2.0 | `mappings/nist-csf-mapping.json` | Planifié |
| IEC 62443 | `mappings/iec62443-mapping.json` | Planifié |

---

## 7. Modèle de kill chain

### 7.1 Définition

Une kill chain GLSS est une séquence ordonnée de techniques traversant un ou plusieurs segments. Elle modélise le parcours complet d'un adversaire depuis la reconnaissance initiale jusqu'à l'impact final.

### 7.2 Types de kill chains

| Type | Description | Exemple |
|---|---|---|
| **Mono-segment** | Attaque confinée à un seul segment | Ransomware sur système MCC (Ground only) |
| **Bi-segment** | Attaque traversant deux segments | Compromission sol → injection uplink (Ground → Link) |
| **Tri-segment** | Attaque traversant les trois segments | Sol → Liaison → Spatial (kill chain complète) |

### 7.3 Format de kill chain

Les kill chains sont documentées dans `kill-chains/` au format JSON.

```json
{
  "id": "kill-chain--glss-viasat-kasat",
  "name": "Viasat KA-SAT Attack (2022)",
  "description": "Modélisation de l'attaque russe contre le réseau Viasat KA-SAT le 24 février 2022.",
  "type": "tri-segment",
  "created": "2026-02-15",
  "steps": [
    {
      "order": 1,
      "segment": "ground",
      "tactic": "GRD-TA-0001",
      "technique": "GRD-TE-XXXX",
      "description": "Reconnaissance du réseau de management VPN de Viasat.",
      "evidence": "Exploitation d'un VPN mal configuré pour accéder au réseau de gestion."
    },
    {
      "order": 2,
      "segment": "ground",
      "tactic": "GRD-TA-0002",
      "technique": "GRD-TE-XXXX",
      "description": "Accès initial via exploitation de vulnérabilité VPN.",
      "evidence": "Compromission du système de management réseau."
    },
    {
      "order": 3,
      "segment": "ground",
      "tactic": "GRD-TA-0006",
      "technique": "GRD-TE-XXXX",
      "description": "Mouvement latéral vers le segment de gestion des terminaux.",
      "evidence": "Pivot depuis le réseau corporate vers l'infrastructure de gestion des modems."
    },
    {
      "order": 4,
      "segment": "link",
      "tactic": "LNK-TA-0003",
      "technique": "LNK-TE-XXXX",
      "description": "Injection de commandes de mise à jour firmware via le système de gestion.",
      "evidence": "Commande de flash firmware envoyée aux modems SurfBeam2."
    },
    {
      "order": 5,
      "segment": "ground",
      "tactic": "GRD-TA-0009",
      "technique": "GRD-TE-XXXX",
      "description": "Impact : AcidRain wiper déployé sur ~30 000 modems, bricking irréversible.",
      "evidence": "Perte de service pour des dizaines de milliers d'utilisateurs en Europe."
    }
  ],
  "references": [
    {
      "title": "Viasat Incident Report",
      "url": "https://news.viasat.com/blog/corporate/ka-sat-network-cyber-attack-overview"
    }
  ]
}
```

### 7.4 Visualisation

Les kill chains peuvent être visualisées dans la matrice GLSS comme des parcours colorés traversant les colonnes (tactiques) et les lignes (segments). L'outil GLSS Navigator (planifié) offrira cette capacité de visualisation.

---

## 8. Conformité réglementaire

### 8.1 NIS2 (Directive UE 2022/2555)

GLSS facilite la conformité NIS2 pour les opérateurs spatiaux (secteur "Espace" — Annexe I, catégorie 11) en fournissant :

- **Article 21(a) — Analyse de risques :** La matrice GLSS sert de catalogue de menaces pour l'identification des risques par segment.
- **Article 21(b) — Gestion des incidents :** Les kill chains GLSS structurent les playbooks de réponse à incident par segment.
- **Article 21(d) — Sécurité de la chaîne d'approvisionnement :** Les tactiques Ground couvrent explicitement les risques supply chain.
- **Article 21(e) — Gestion des vulnérabilités :** Les techniques GLSS identifient les vulnérabilités exploitables par segment.

### 8.2 Cyber Resilience Act (CRA)

GLSS supporte la conformité CRA pour les produits numériques à composante spatiale :

- **Évaluation des vulnérabilités :** Les techniques GLSS par segment permettent une analyse exhaustive des surfaces d'attaque.
- **Gestion des mises à jour de sécurité :** Les tactiques Space Firmware Tampering (SPC-TA-0003) couvrent les risques liés aux mises à jour OTA.
- **Documentation technique :** Les fiches techniques GLSS alimentent la documentation CRA requise.

### 8.3 ECSS-Q-ST-80C

GLSS s'articule avec le standard ESA d'assurance produit logiciel :

- Les tactiques Space s'alignent sur les exigences de sécurité logicielle embarquée ECSS.
- Les kill chains GLSS complètent les analyses de risques ECSS par une dimension cybersécurité adversariale.

---

## 9. Règles d'extension

### 9.1 Ajout d'une tactique

L'ajout d'une nouvelle tactique nécessite :

1. Justification démontrant qu'aucune tactique existante ne couvre l'objectif adverse décrit.
2. Rattachement explicite à un segment (Ground, Link, Space).
3. Attribution d'un identifiant conforme à la Section 4.1.
4. Création de l'objet STIX 2.1 conforme à la Section 5.3.
5. Validation par les maintainers via Pull Request.

### 9.2 Ajout d'une technique

L'ajout d'une nouvelle technique nécessite :

1. Rattachement à une tactique existante.
2. Description incluant : objectif, méthode, détection, contre-mesure.
3. Au moins une preuve documentée (incident réel, recherche publiée, CVE, démonstration Red Team).
4. Attribution d'un identifiant conforme à la Section 4.2.
5. Création de l'objet STIX 2.1 conforme à la Section 5.4.
6. Mapping vers au moins un référentiel existant (ATT&CK, SPARTA, ou SPACE-SHIELD).
7. Validation par les maintainers via Pull Request.

### 9.3 Modification d'un objet existant

Toute modification d'un objet existant doit :

1. Incrémenter le champ `modified` avec la date de modification.
2. Documenter le changement dans CHANGELOG.md.
3. Conserver la rétro-compatibilité des identifiants (un ID ne peut jamais être réattribué).

### 9.4 Dépréciation

Un objet déprécié n'est jamais supprimé. Il reçoit la propriété `x_glss_deprecated: true` et une propriété `x_glss_deprecated_reason` expliquant la raison. L'identifiant reste réservé et ne peut pas être réutilisé.

---

## 10. Versioning

### 10.1 Schéma de version

GLSS utilise le versioning sémantique (SemVer) :

```
MAJOR.MINOR.PATCH

MAJOR : Changement de modèle conceptuel (ex: ajout d'un 4e segment)
MINOR : Ajout de tactiques ou modifications structurelles
PATCH : Ajout de techniques, corrections, enrichissements
```

### 10.2 Historique

| Version | Date | Changements |
|---|---|---|
| 1.0 | 2026-01-15 | Version initiale — Publication académique (mémoire MACYB) |
| 1.1 | 2026-02-15 | Restructuration STIX 2.1, repository GitHub, site web |

---

## 11. Distribution et licence

### 11.1 Licence

GLSS Framework © 2026 par Badaoui Salah, Hajer Boudhir, Achille Lele.

Distribué sous licence Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0).

### 11.2 Attribution requise

Toute utilisation doit créditer les auteurs et le framework :

```
Basé sur le GLSS Framework (https://glss-framework.org)
© 2026 Badaoui Salah, Hajer Boudhir, Achille Lele
Licence : CC BY-SA 4.0
```

### 11.3 Points de contact

- Site web : https://glss-framework.org
- Email : contact@glss-framework.org
- GitHub : https://github.com/GLSS-FRAMEWORK/glss-framework

---

*Fin du document — GLSS Framework Spécification Technique v1.1*
