# Cafet Atlas Live

Source publique des données de l'Atlas de la Cafet SMP. L'interface reste un fichier HTML autonome : aucun serveur web ni base de données ne sont nécessaires.

## Fichiers

- `atlas_live.json` : données publiques synchronisées, au format défini dans `schema.json`.
- `schema.json` : schéma JSON du document exporté.
- `EXPORTER_SPEC.md` : spécification du futur pont Fabric côté serveur.
- `atlas.html` : interface Atlas autonome (à ajouter depuis le fichier fourni séparément si elle n'apparaît pas encore à la racine).

## URL des données

`https://raw.githubusercontent.com/Clembaguette/cafet-atlas-live/main/atlas_live.json`

Dans le bloc **Atlas Live** de l'interface, colle cette URL puis clique sur **Charger**. L'actualisation automatique est prévue toutes les 60 secondes.

## Confidentialité

Ce dépôt est public. Il ne doit contenir que les informations explicitement destinées à être visibles par les joueurs : équipes, noms, couleurs, claims, capitales et tailles de zones. Les identifiants numériques de joueurs peuvent être conservés lorsqu'aucune correspondance fiable avec les noms n'existe.

Ne jamais publier de sauvegarde du monde, de fichiers Xaero privés, de waypoints, de positions de joueurs, de données de scoreboard (y compris objectifs internes), ni de données Waystones.

Le jeton GitHub de publication devra être limité à ce dépôt avec la permission **Contents: Read and write**, stocké uniquement dans la configuration locale du serveur et jamais dans ce dépôt, le datapack ou le HTML.

## État

La structure de données et la spécification de l'exporteur sont préparées. Le mod Fabric de synchronisation n'est pas encore implémenté ni validé en jeu.