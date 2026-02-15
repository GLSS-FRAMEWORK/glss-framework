<p align="center">
  <img src="https://img.shields.io/badge/License-CC%20BY--SA%204.0-blue.svg" alt="License: CC BY-SA 4.0">
  <img src="https://img.shields.io/badge/Version-1.1-green.svg" alt="Version 1.1">
  <img src="https://img.shields.io/badge/Status-Active-brightgreen.svg" alt="Status: Active">
  <img src="https://img.shields.io/badge/Format-STIX%202.1-orange.svg" alt="Format: STIX 2.1">
</p>

<h1 align="center">GLSS Framework</h1>
<h3 align="center">Ground-Link-Space Security</h3>
<p align="center"><em>Le premier cadre unifié de modélisation des cybermenaces pour les systèmes spatiaux.</em></p>

<p align="center">
  <a href="https://glss-framework.org">🌐 Website</a> •
  <a href="#architecture">📐 Architecture</a> •
  <a href="#getting-started">🚀 Getting Started</a> •
  <a href="#ecosystem">🔗 Ecosystem</a> •
  <a href="#contributing">🤝 Contributing</a>
</p>

---

## Le Problème

Les systèmes spatiaux sont devenus des cibles prioritaires pour les menaces cyber (attaque Viasat KA-SAT 2022, spoofing GPS, interférences Starlink). Pourtant, **aucun référentiel existant ne couvre la kill chain complète** d'une attaque sur infrastructure spatiale :

| Framework | Couverture | Limite |
|-----------|-----------|--------|
| MITRE ATT&CK | IT/OT terrestre | Ignore le spatial |
| SPARTA | Segment spatial | Pas de segment sol |
| SPACE-SHIELD | Espace + liaisons | Institutionnel ESA, pas de sol |
| TREKS | Véhicules spatiaux | Scope limité |

**GLSS comble ce vide** en unifiant ces frameworks dans un modèle tri-segmentaire cohérent.

---

## Architecture

GLSS décompose tout système spatial en **3 segments fondamentaux** :

```
┌─────────────────────────────────────────────────┐
│                  🛰️ SPACE                        │
│  Satellite bus • Payload • Firmware • OTA • AI   │
│  ← SPARTA, SPACE-SHIELD, TREKS, ATLAS           │
├─────────────────────────────────────────────────┤
│                  📡 LINK                         │
│  RF Up/Downlink • Optical • ISL • TT&C (CCSDS)  │
│  ← NIST IR 8401, SPACE-SHIELD                   │
├─────────────────────────────────────────────────┤
│                  🏢 GROUND                       │
│  Control Centers • Ground Stations • IT/OT •     │
│  Cloud • MLOps • Supply Chain                    │
│  ← ATT&CK, IEC 62443, ATLAS                     │
└─────────────────────────────────────────────────┘
```

Chaque segment contient des **tactiques** et **techniques** spécifiques, au format **STIX 2.1**, permettant l'intégration native dans les outils SIEM/SOAR/CTI.

---

## Getting Started

### Cloner le repository

```bash
git clone https://github.com/glss-framework/glss-framework.git
cd glss-framework
```

### Explorer la matrice

```bash
# Matrice complète (STIX 2.1)
cat matrix/glss-matrix.json

# Techniques par segment
ls matrix/ground/
ls matrix/link/
ls matrix/space/
```

### Utiliser les mappings

```bash
# Mapping GLSS ↔ ATT&CK
cat mappings/attack-mapping.json

# Mapping GLSS ↔ SPARTA
cat mappings/sparta-mapping.json

# Mapping GLSS ↔ SPACE-SHIELD
cat mappings/spaceshield-mapping.json
```

---

## Structure du Repository

```
glss-framework/
├── README.md
├── LICENSE                    # CC BY-SA 4.0
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── GOVERNANCE.md
├── CHANGELOG.md
├── matrix/
│   ├── glss-matrix.json       # Matrice complète STIX 2.1
│   ├── ground/                # Tactiques & techniques — Segment Sol
│   ├── link/                  # Tactiques & techniques — Segment Liaison
│   └── space/                 # Tactiques & techniques — Segment Spatial
├── mappings/
│   ├── attack-mapping.json    # GLSS ↔ MITRE ATT&CK
│   ├── sparta-mapping.json    # GLSS ↔ SPARTA
│   └── spaceshield-mapping.json # GLSS ↔ SPACE-SHIELD
├── kill-chains/               # Exemples de scénarios multi-segments
├── docs/                      # Documentation technique
└── tools/                     # Scripts & utilitaires
```

---

## Ecosystem

GLSS articule **16 référentiels** en une couche d'intégration supérieure :

### Intégration native (fusion directe)
- **MITRE ATT&CK** — Framework de menaces IT/OT
- **SPARTA** — Menaces sur systèmes spatiaux
- **SPACE-SHIELD** — Taxonomie ESA/SCCoE
- **TREKS** — Kill chain véhicules spatiaux
- **MITRE ATLAS** — Menaces adversariales IA/ML

### Articulation (mapping structuré)
- NIST CSF v2.0, NIST 800-53, NIST IR 8401
- ISO 27001, IEC 62443
- EBIOS RM, ECSS-Q-ST-80C
- CCSDS 350.1

### Conformité (vérification réglementaire)
- NIS2 (Directive EU)
- Cyber Resilience Act (CRA)
- ENISA Space Threat Landscape

---

## Use Cases

### 🔍 SOC Spatial
Structurez vos règles de détection SIEM/SOAR par segment GLSS. Créez des règles Sigma basées sur les techniques GLSS pour une couverture Ground + Link + Space.

### 📋 Audit NIS2 / CRA
Utilisez le mapping GLSS ↔ NIS2 Article 21 pour vérifier l'identification des menaces et la proportionnalité des contre-mesures sur les 3 segments.

### 🔴 Red Team Spatial
Planifiez des exercices multi-segments : compromission cloud (Ground) → injection uplink (Link) → modification firmware (Space). Kill chain complète tracée dans la matrice.

---

## Contributing

Les contributions sont bienvenues ! Consultez [CONTRIBUTING.md](CONTRIBUTING.md) pour les guidelines.

### Types de contributions
- 🆕 Nouvelles techniques (TTPs) avec preuves documentées
- 🔗 Nouveaux mappings vers d'autres référentiels
- 📝 Améliorations de documentation
- 🛠️ Outils et scripts d'intégration
- 🐛 Corrections et améliorations

### Processus
1. Fork le repository
2. Créez une branche (`git checkout -b feature/nouvelle-technique`)
3. Committez vos changements (`git commit -m 'Add: technique T-GND-0042'`)
4. Push (`git push origin feature/nouvelle-technique`)
5. Ouvrez une Pull Request

---

## Governance

GLSS est gouverné par la **Fondation GLSS** selon le modèle décrit dans [GOVERNANCE.md](GOVERNANCE.md).

### Rôles
- **Maintainers** — Fondateurs, droits de merge sur main
- **Committers** — Contributeurs réguliers avec confiance acquise
- **Contributors** — Toute personne soumettant des PRs
- **Advisory Board** — Conseil Scientifique + Conseil Industriel

---

## Certification

Le programme de certification GLSS forme des professionnels capables d'appliquer le framework :

| Niveau | Public | Durée |
|--------|--------|-------|
| **GLSS Practitioner** | SOC Analysts, Ingénieurs Cyber | 3 jours |
| **GLSS Expert** | Architectes Sécurité, Red Teamers | 5 jours |
| **GLSS Instructor** | Formateurs, Académiques | 2 jours |

Plus d'informations sur [glss-framework.org](https://glss-framework.org).

---

## Authors

- **Badaoui Salah** — [@badaoui-salah](https://github.com/badaoui-salah)
- **Hajer Boudhir** — [@hajer-boudhir](https://github.com/hajer-boudhir)
- **Achille Lele** — [@achille-lele](https://github.com/achille-lele)

---

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-sa/4.0/).

You are free to:
- **Share** — copy and redistribute the material in any medium or format
- **Adapt** — remix, transform, and build upon the material for any purpose, including commercially

Under the following terms:
- **Attribution** — You must give appropriate credit
- **ShareAlike** — If you remix, you must distribute under the same license

---

<p align="center">
  <a href="https://glss-framework.org">glss-framework.org</a> · 
  <a href="mailto:contact@glss-framework.org">contact@glss-framework.org</a>
</p>
<p align="center"><sub>© 2026 Fondation GLSS. Ground-Link-Space Security Framework.</sub></p>
