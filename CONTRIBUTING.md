# Contributing to GLSS Framework

Merci de votre intérêt pour GLSS ! Ce document décrit comment contribuer au projet.

## Code de Conduite

En contribuant, vous acceptez de respecter notre [Code de Conduite](CODE_OF_CONDUCT.md).

## Types de Contributions

### 🆕 Nouvelles Techniques (TTPs)

Les nouvelles techniques doivent inclure :
- **ID** : Format `T-GND-XXXX` (Ground), `T-LNK-XXXX` (Link), ou `T-SPC-XXXX` (Space)
- **Nom** : Descriptif et concis
- **Description** : Explication détaillée de la technique
- **Tactique(s)** : Rattachement à une ou plusieurs tactiques GLSS
- **Segment** : Ground, Link, ou Space
- **Détection** : Méthodes de détection recommandées
- **Contre-mesures** : Mitigations applicables
- **Références** : Sources documentées (CVE, publications, incidents réels)
- **Mapping** : Correspondances avec ATT&CK/SPARTA/SPACE-SHIELD si applicable

Format requis : **STIX 2.1** (voir `matrix/` pour des exemples)

### 🔗 Nouveaux Mappings

Les mappings vers d'autres référentiels doivent :
- Utiliser le format JSON standardisé (voir `mappings/`)
- Inclure une justification pour chaque correspondance
- Être validés par au moins 2 reviewers

### 📝 Documentation

Améliorations de docs, corrections, traductions, tutoriels — toujours bienvenus.

### 🛠️ Outils

Scripts, intégrations SIEM/SOAR, exporteurs — dans le dossier `tools/`.

## Processus de Contribution

### 1. Fork & Clone

```bash
git clone https://github.com/VOTRE-USERNAME/glss-framework.git
cd glss-framework
git remote add upstream https://github.com/glss-framework/glss-framework.git
```

### 2. Créer une branche

```bash
git checkout -b feature/description-courte
```

Conventions de nommage :
- `feature/` — Nouvelle fonctionnalité ou technique
- `fix/` — Correction
- `docs/` — Documentation
- `mapping/` — Nouveau mapping

### 3. Commit

```bash
git commit -m "Add: T-GND-0042 Cloud API Key Extraction"
```

Préfixes de commit :
- `Add:` — Nouveau contenu
- `Fix:` — Correction
- `Update:` — Mise à jour
- `Docs:` — Documentation
- `Map:` — Mapping

### 4. Pull Request

- Ouvrez une PR vers `main`
- Remplissez le template de PR
- Attendez la review d'un maintainer

## Review Process

- Les nouvelles techniques nécessitent **2 approvals** (dont 1 maintainer)
- Les corrections mineures nécessitent **1 approval**
- Les maintainers ont le dernier mot sur les décisions architecturales

## Questions ?

Ouvrez une [Issue](https://github.com/glss-framework/glss-framework/issues) ou contactez-nous à contact@glss-framework.org.
