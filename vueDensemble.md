# Vue d'ensemble de la configuration Home Assistant

Date de revue: 2026-03-11

## Resume executif
- La base de configuration est fonctionnelle et modulaire (packages + dashboards YAML + includes principaux).
- La structure melange des fichiers "source" (YAML) et des fichiers runtime HA (base SQLite, .storage, cache).
- Les scripts appeles par `home.yaml` existent maintenant dans `scripts.yaml`.
- Une protection `andre_mode` empeche desormais la fermeture des stores cibles (salon + zone salle a manger configuree ici via `cover.porte_patio`).

## Structure du workspace

### Dossiers principaux
- `.github/`: consignes Copilot/agent pour ce repo.
- `config/`: dossier de configuration Home Assistant (equivalent du `/config` runtime).
- `AGENTS.md`, `README.md`, `Note.txt`: documentation et notes de travail.

### Dossiers runtime detectes sous `config/`
- `.storage/`: registres internes HA (areas, devices, entities, labels, dashboards storage, auth, etc.).
- `.cache/`: cache d'icones/integrations.
- `home-assistant_v2.db` (+ `-shm`, `-wal`): base de donnees recorder.
- `image/`, `tts/`, `deps/`, `.cloud/`: artefacts systeme HA.

## Configuration chargee par Home Assistant

### Fichier racine
`config/configuration.yaml` charge actuellement:
- `default_config:`
- `homeassistant.packages: !include_dir_named packages`
- `frontend.themes: !include_dir_merge_named themes`
- `automation: !include automations.yaml`
- `script: !include scripts.yaml`
- `scene: !include scenes.yaml`
- `lovelace` en mode `storage` + dashboards YAML:
  - `home-lab` -> `dashboards/home.yaml`
  - `system-lab` -> `dashboards/system.yaml`

### Automations
`config/automations.yaml` contient 3 automatisations:
- `Test bureau 11:20` (position store 33)
- `Bbb` (position store 65)
- `Andre Mode | Bloquer fermeture stores salon et salle a manger`
  - Declenchement: passage a `closing` de `cover.salon` ou `cover.porte_patio`
  - Condition: `input_boolean.andre_mode == on`
  - Action: `cover.open_cover` sur l'entite qui etait en fermeture

### Scripts
- Fichier charge par HA: `config/scripts.yaml`
  - `store_chambre_ami`
  - `test_luc_deux`
  - `set_home_mode_on`
  - `set_home_mode_off`
  - `reload_yaml_configuration`
- Fichier non charge actuellement: `config/scripts/scripts.yaml`
  - `open_chambre_ami`
  - Ce fichier n'est pas reference par `configuration.yaml`.

### Packages
- `config/packages/core/00_helpers.yaml`
  - `input_boolean.home_mode`
  - `input_boolean.guest_mode`
  - `input_boolean.andre_mode`
  - `input_select.house_profile`
  - `timer.focus_session`
- `config/packages/core/10_notifications.yaml`
  - `binary_sensor.guest_mode_active` (template)
  - `binary_sensor.andre_mode_active` (template)
  - `sensor.house_profile` (template)
- `config/packages/rooms/living_room.yaml`
  - `input_boolean.living_room_evening_scene`

### Dashboards
- `config/dashboards/home.yaml`
  - cartes modes maison, boutons Visite ON/OFF, boutons Andre ON/OFF, controles piece, boutons scripts
- `config/dashboards/system.yaml`
  - etats systeme et notes d'administration

## Etat des incoherences

1. Scripts du dashboard
- Statut: corrige.
- Les scripts references par `home.yaml` sont presents dans `config/scripts.yaml`.

2. Presence de deux emplacements de scripts
- `config/scripts.yaml` (charge)
- `config/scripts/scripts.yaml` (non charge)
- Impact: confusion potentielle, mais sans blocage tant que le dashboard reference les scripts du fichier charge.

3. Secret de demonstration encore present
- `config/secrets.yaml` contient `some_password: welcome`.
- Impact: faible en local, mais a nettoyer avant partage/sauvegarde externe.

4. Ciblage salle a manger
- Le registre d'entites montre `cover.salon` et `cover.porte_patio` (pas de `cover.salle_a_manger` detecte).
- La protection `andre_mode` utilise donc `cover.porte_patio` comme store de la zone salle a manger dans l'etat actuel.

## Niveau de maturite actuel
- Base HA: bonne (includes, packages, dashboards separes).
- Industrialisation: bonne, avec un point de nettoyage restant sur l'organisation des scripts.
- Pret production locale: oui.

## Actions conseillees (ordre prioritaire)
1. Verifier dans HA que le store cible pour la salle a manger est bien `cover.porte_patio`.
2. Decider d'une strategie unique pour les scripts:
   - soit conserver uniquement `config/scripts.yaml`,
   - soit migrer vers `!include_dir_merge_named scripts`.
3. Nettoyer `config/secrets.yaml` (remplacer la valeur de test).
4. Lancer la verification YAML puis recharger `Scripts` et `Automations` dans HA.

## Note
Cette revue couvre la coherence fichiers + structure. La validation finale des devices/services se fait dans HA (Outils de developpement).
