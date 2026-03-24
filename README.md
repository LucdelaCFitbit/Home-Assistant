# Home Assistant Workspace

Ce workspace est organise pour separer:
- la configuration Home Assistant executable dans le dossier [config](config)
- la documentation et les fichiers de travail a la racine du projet

## Arborescence actuelle

- [config](config): racine de la configuration Home Assistant
- [.github](.github): instructions Copilot pour ce workspace
- [AGENTS.md](AGENTS.md): notes de comportement agent
- [README.md](README.md): documentation de structure
- [Note.txt](Note.txt): notes utilisateur

## Structure Home Assistant detaillee

### Fichiers principaux
- [config/configuration.yaml](config/configuration.yaml): point d'entree Home Assistant
- [config/automations.yaml](config/automations.yaml): automatisations globales
- [config/scripts.yaml](config/scripts.yaml): scripts reutilisables
- [config/scenes.yaml](config/scenes.yaml): definitions de scenes

### Packages
- [config/packages](config/packages): logique modulaire par domaine et par piece
- [config/packages/core/00_helpers.yaml](config/packages/core/00_helpers.yaml): helpers de base
- [config/packages/core/10_notifications.yaml](config/packages/core/10_notifications.yaml): template entities
- [config/packages/rooms/living_room.yaml](config/packages/rooms/living_room.yaml): exemple de logique par piece

### Dashboards YAML
- [config/dashboards/home.yaml](config/dashboards/home.yaml): dashboard usage quotidien
- [config/dashboards/system.yaml](config/dashboards/system.yaml): dashboard administration/diagnostic
- [config/dashboards/README.md](config/dashboards/README.md): conventions dashboards

## Logique de configuration

Le fichier [config/configuration.yaml](config/configuration.yaml) orchestre toute la config:
- inclut automations, scripts et scenes
- active les packages via include_dir_named
- declare les dashboards YAML Home et System

## Conventions de nommage et organisation

- Utiliser des alias lisibles, par exemple:
  - System | ...
  - Mode | ...
  - Room | ...
- Garder des entity_id en minuscules avec underscores
- Un fichier = une responsabilite principale
- Preferer les packages pour les entites et logiques metier

## Workflow recommande

1. Modifier les fichiers dans [config](config).
2. Dans Home Assistant, lancer la verification YAML.
3. Recharger automations/scripts quand possible.
4. Redemarrer Home Assistant uniquement si necessaire.

## Point important sur le chemin runtime

Cette structure suppose que le contenu du dossier [config](config) est celui deploye dans le dossier runtime Home Assistant /config.

Si ton montage actuel correspond deja directement au /config de Home Assistant, alors il faut utiliser les fichiers a la racine du workspace et non dans un sous-dossier config.
