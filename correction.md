# Audit de la config Home Assistant

## À corriger en premier

1. **Dashboard Lumières incomplet** ([config/dashboards/lumieres.yaml](config/dashboards/lumieres.yaml))
***** fait 20261003 
   - La liste d'entités vient du YAML, pas du registre HA. Il manque par exemple `light.cuisine`, `light.chambre`, `light.exterieur`, `light.adresse`, `light.garage`, `light.table_*`, `light.bureau_luc_3`/`_4`, `light.salon_plafond`.
   - "Tout allumer" et "Tout éteindre" ne couvrent donc pas toute la maison.
   - Le compteur du résumé additionne les groupes Hue et leurs ampoules (65 `light.*` au total), donc le total est faux.
   - "Tout allumer" allume aussi la volière et le poulailler. À décider : les exclure ou non.

2. **Mot de passe Gmail en clair** ([config/secrets.yaml](config/secrets.yaml#L5))
   - Aucun `!secret` n'est utilisé dans la config (SMTP configuré via l'UI).
   - Supprimer ce secret et `some_password: welcome`.
   - Révoquer le mot de passe d'application chez Google.

3. **Courriels probablement jamais envoyés**
   - Le groupe `toutes_les_alertes` ne contient que `mobile_app_lucdelac`, la ligne SMTP est commentée ([config/configuration.yaml](config/configuration.yaml#L71)).
   - Les `target: lucdelac@gmail.com` des automatisations sont ignorés (ex. [config/automations.yaml](config/automations.yaml#L714)).

4. **Dashboard Nest Hub cassé** ([config/dashboards/bureau_luc_nesthub.yaml](config/dashboards/bureau_luc_nesthub.yaml#L11))
   - Il lit `sensor.nbjours1mars2027`, qui n'existe pas.
   - Le capteur réel est `sensor.nbjours15janvier2027` ([config/templates.yaml](config/templates.yaml#L33)).
   - Les commentaires parlent du 1er mars 2027, le capteur compte jusqu'au 15 janvier : choisir la bonne date.

5. **Automatisation "ZZ Test" active** ([config/automations.yaml](config/automations.yaml#L1140))
   - Le test Bureau Luc 15h30 a un vrai déclencheur horaire et parle tous les jours.
   - Retirer le trigger ou supprimer l'automatisation si ce n'est pas voulu.

## Affichage Pixel 9a

- **Pièces en 3 colonnes** dans [config/dashboards/pixel9a.yaml](config/dashboards/pixel9a.yaml) : trop étroit pour des cartes `display_type: picture`. Passer à 2 colonnes, comme `cellulaire.yaml`.
- **Trois versions du même dashboard mobile** : `pixel9a.yaml`, `cellulaire.yaml` (masqué) et la vue "Pixel 9a" de `home.yaml`. Garder `pixel9a.yaml` et supprimer le reste.
- **Accueil trop long** : mettre en haut le plus utilisé (modes, lumières) et déplacer les 10 cartes "Statuts maison" dans une sous-vue.
- **Accents incohérents** : "Entree", "Cote Luc" à côté de "Volière".
- **Lumières** : groupes Hue (`light.salon`, `light.foyer`, etc.) en haut, ampoules individuelles dans une sous-vue.

## Bonnes pratiques de code

- **Syntaxe mélangée** : les lignes [69](config/automations.yaml#L69), [137](config/automations.yaml#L137) et [290](config/automations.yaml#L290) utilisent l'ancien format `trigger:`/`platform:`/`action:`, comme [config/templates.yaml](config/templates.yaml#L42). Le reste utilise `triggers:`/`conditions:`/`actions:`.
- **`call-service`/`service:` dans les dashboards** ([config/dashboards/cellulaire.yaml](config/dashboards/cellulaire.yaml), [config/dashboards/home.yaml](config/dashboards/home.yaml)) : passer à `perform-action`/`perform_action`, comme `pixel9a.yaml`.
- **Délais longs perdus au redémarrage** : le `delay: 12:00:00` de l'[auto-off Andre/Invite](config/automations.yaml#L435). Utiliser un trigger `state` avec `for: '12:00:00'` : plus simple, et il s'annule tout seul si le mode repasse à OFF.
- **Code mort** : `scripts.yaml` contient des centaines de lignes commentées, et les automatisations "ZZ Test" sont mélangées à la production. Les supprimer (git garde l'historique) ou les déplacer dans un fichier séparé.
- **Valeurs codées en dur** : adresse courriel (4 fois) et adresse MAC de la TV. Utiliser `!secret`.
- **Capteurs Kelvin constants** ([config/templates.yaml](config/templates.yaml#L2)) : des `input_number` seraient modifiables depuis le dashboard et ne polluent pas l'historique.
- **`Temps porte ouverte`** : capteur texte qui relit son propre état avec `this.state`. Fragile, un capteur numérique (secondes) ou `timestamp` serait plus robuste.

## Déjà bien

- Packages bien organisés.
- IDs stables.
- Script de vérification des stores avec nouvelles tentatives et notifications.
- Confirmation sur les actions sensibles.
- `.gitignore` protège `secrets.yaml` et `.storage`.
