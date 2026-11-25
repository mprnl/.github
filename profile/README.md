# mprnl

Organisation personnelle autour de BACKSTAGE, mon assistant personnel
auto-hébergé : chat IA, rappels, calendrier, mails, mémoire, photo.

Le noyau et la distribution des modules sont publics : n'importe qui peut
installer BACKSTAGE, lire son code et écrire un module.

## Dépôts

| Dépôt | Visibilité | Rôle |
| --- | --- | --- |
| [backstage](https://github.com/mprnl/backstage) | public | application complète (Next.js 16 PWA), noyau, modules intégrés, documentation |
| [BS-modules](https://github.com/mprnl/BS-modules) | public | `catalog.json` : liste des modules connus, lue en lecture seule par l'application |
| [BS-watch-later](https://github.com/mprnl/BS-watch-later) | public | module externe : liste de liens à regarder plus tard (page, rail, panneau, carte d'accueil) |
| [BS-daily-quote](https://github.com/mprnl/BS-daily-quote) | public | module externe de référence : une citation par jour, locale et déterministe |
| [BS-accreditations](https://github.com/mprnl/BS-accreditations) | public | module externe : demandes d'accréditation photo depuis Gmail, relance J+7 |
| [.github](https://github.com/mprnl/.github) | public | ce profil, la carte des dépôts et la politique de visibilité |

## Politique de visibilité

Un module du catalogue s'installe **sans surprise** :

- **public** : `bun run module:add <url>#<ref>` fait un `git clone` nu, sans
  credentials. C'est le cas par défaut, et le seul qui demande zéro
  configuration.
- **privé** : le clone a besoin d'un jeton. La marche à suivre (jeton
  fine-grained, `GH_TOKEN`, `gh auth login`) est documentée dans
  [`docs/self-hosting.md`](https://github.com/mprnl/backstage/blob/main/docs/self-hosting.md).
  Une entrée privée du catalogue doit déclarer `"visibility": "private"` pour
  que l'interface annonce « installation avec token ».

Le catalogue ne doit jamais déclarer privé un dépôt qui est public, ni
l'inverse : l'écart est un défaut, pas un détail.

Aucun jeton n'apparaît dans un catalogue, un `modules.lock.json`, un log ou un
commit. Le lock garde toujours l'URL publique du dépôt.

## Où vit la documentation

Tout est dans [`backstage/docs/`](https://github.com/mprnl/backstage/tree/main/docs) :

- [`docs/README.md`](https://github.com/mprnl/backstage/blob/main/docs/README.md) : point d'entrée, ordre de lecture ;
- [`docs/self-hosting.md`](https://github.com/mprnl/backstage/blob/main/docs/self-hosting.md) : installation, variables, modules ;
- [`docs/architecture.md`](https://github.com/mprnl/backstage/blob/main/docs/architecture.md) : invariants et distribution des modules.

## Conventions

- `main` est la branche protégée de [backstage](https://github.com/mprnl/backstage) :
  pull request obligatoire, checks obligatoires, pas de force-push, pas de
  suppression de branche. Le mainteneur merge sans seconde approbation, parce
  que le dépôt est mono-mainteneur.
- Un dépôt `BS-*` porte un seul module, avec son `module.ts` à la racine, ses
  tests et son `README.md`. Il n'importe jamais le noyau : seulement
  `@backstage/module-kit`.
- Les workflows réutilisables d'organisation viendront quand les dépôts `BS-*`
  auront une CI à factoriser (issue #284). D'ici là, chacun reste autonome.
