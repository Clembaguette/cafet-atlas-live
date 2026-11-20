# Spécification de l'exporteur serveur

## Source des données v1.2.5

Les stockages persistants identifiés dans le datapack :

- `conquest:db` : source de vérité des équipes, claims et composants de zones ;
- `conquest:flood` : état de travail du calcul des zones ; ne pas exporter ;
- `conquest:top` : cache de classement ; inutile pour reconstruire l'Atlas.

Dans `conquest:db` :

- `teams` est une liste d'équipes. Champs utilisés par le pack et l'Atlas : `id`, `name`, `leader`, `members[{pid}]`, `claims[{k}]`, `color`, `hex`, `cname`, `cap`.
- `claims` est un compound indexé par une clé texte `"cx,cz"`. Un claim contient notamment `team`, `owner`, `cx`, `cz`, `cid`, `cap`, `vis`, `x`, `y`, `z` et éventuellement `name`.
- `comps` est un compound indexé par identifiant de composant ; `size` donne la taille de la zone connectée.

## Export attendu

```json
{
  "schema_version": 1,
  "pack_version": "1.2.5",
  "generated_at": "2026-11-20T20:00:00Z",
  "db": {
    "teams": [],
    "claims": {},
    "comps": {}
  }
}
```

Le mod doit copier les trois structures depuis le stockage `conquest:db` sur le thread serveur, puis sérialiser une copie immuable. Ne jamais modifier le stockage source.

## Publication GitHub

- Le dépôt public ne contient que les données explicitement publiques et les fichiers nécessaires à l'Atlas.
- Le jeton GitHub doit être limité à ce dépôt avec la permission **Contents: Read and write**, stocké dans une configuration serveur non versionnée.
- Publier uniquement quand le contenu change (comparaison SHA-256), avec un délai anti-rebond de 30 à 60 secondes.
- Lire le SHA courant du fichier via l'API Contents, puis faire un PUT conditionné par ce SHA ; gérer les conflits en relisant l'état distant.
- Timeout, erreurs HTTP, quota ou absence de réseau ne doivent jamais bloquer le thread serveur. Effectuer les requêtes dans un worker et conserver le dernier export local en cas d'échec.
- Ne pas journaliser le jeton. Ne jamais inclure `conquest:flood`, `conquest:top`, les objectifs `cq_*`, les waypoints ou des données de position des joueurs.

## Identifiants des joueurs

Les membres et propriétaires sont stockés sous forme de `pid` numériques. Tant qu'une correspondance persistante fiable avec les noms des joueurs n'est pas établie, conserver ces identifiants et afficher un libellé de repli dans l'Atlas.

## Étape d'implémentation restante

Valider la version précise de Fabric Loader, Fabric API et les mappings pour Minecraft 26.3, puis implémenter l'accès à `CommandStorage` avec ces mappings. Compiler et tester sur une copie du monde : chargement, équipe, claim, capitale, dissolution et redémarrage. Ne pas présenter le pont comme validé avant ces essais.