# 640 Ko : cadrage du projet

Titre provisoire. Document de référence pour le développement de la V1, en 8 jours (environ 8 h par jour).

---

## 1. Le concept

Le joueur incarne un **programme des années 90** qui se réveille sur une machine d'aujourd'hui. Pour s'adapter au monde moderne, il absorbe des **fragments** de ce monde (récolte), puis les combine en **modules** de compréhension (fabrication). Mais il a une contrainte de son époque : **une mémoire minuscule**.

L'originalité ne vient pas de la complexité des règles, mais de :

- **l'angle** : un programme daté qui découvre et s'approprie le monde moderne, avec son vocabulaire d'époque ;
- **les décisions** : chaque tour pose une question (prendre un risque ? garder ou effacer ?) ;
- **l'interface qui se modernise** : le jeu commence comme un écran des années 90 et devient une interface moderne au fil de la progression. Le design est une mécanique de jeu.

Références d'esprit : *Balatro* et *Slay the Spire* (peu de règles, de vrais choix), *Melvor Idle* (interface de type application), les jeux textuels comme *A Dark Room*.

Ton : léger et un peu décalé, avec une pointe de mélancolie. **Aucun scénario à écrire** : l'univers passe uniquement par les noms des fragments et des modules, leurs descriptions courtes, et les messages du journal.

---

## 2. Les règles du jeu (V1)

Trois règles, pas plus.

### Règle 1 : la mémoire est limitée

- Le programme dispose d'une **capacité** de **16 cases** au départ.
- Chaque fragment occupe **1, 2 ou 3 cases** selon sa taille.
- La somme des tailles des fragments en mémoire ne peut jamais dépasser la capacité.
- En V1, il n'y a **pas de placement spatial** (pas de puzzle façon Tetris) : la grille se remplit dans l'ordre, et seule l'occupation totale compte.

### Règle 2 : la récolte est un pari

Le joueur lance une récolte en choisissant sa **profondeur** :

| Profondeur | Durée | Chances de rareté | Risque |
|---|---|---|---|
| **Rapide** | environ 2 s | surtout des fragments communs | aucun |
| **Profonde** | environ 4 s | plus de chances de rare, épique, légendaire | risque de **corruption** |

- **Corruption** : avec une certaine probabilité, un fragment déjà en mémoire (tiré au hasard) est **détruit**. Le journal l'annonce.
- Les probabilités exactes sont des constantes, ajustées à la fin (équilibrage).
- Le fragment récolté arrive dans une **zone tampon** :
  - s'il y a assez de place, le joueur le **garde** (il entre en mémoire) ou le **rejette** ;
  - s'il n'y a pas assez de place, le joueur doit **effacer** des fragments de sa mémoire pour lui faire de la place, ou le **rejeter**.
- Tant qu'un fragment attend dans la zone tampon, **aucune nouvelle récolte** n'est possible.

C'est la décision centrale du jeu : garder ce fragment rare et encombrant, ou ces petits fragments utiles ?

### Règle 3 : combiner libère de la place

- Une **recette** demande des fragments précis (2 ou 3) et produit un **module**.
- Les fragments utilisés **quittent la mémoire** : combiner libère de la place.
- Un module ne prend **pas** de place, est obtenu **une seule fois**, et apporte un **bonus permanent**, par exemple :
  - **+2 cases** de capacité ;
  - **corruption réduite** lors des récoltes profondes ;
  - **meilleures chances** de rareté ;
  - **passage au palier d'interface suivant**.

### La progression et la fin

- Chaque module augmente une **jauge de compatibilité** (en pourcentage).
- La V1 se termine quand tous les modules sont obtenus (compatibilité 100 %) : un écran de fin s'affiche, avec un bouton pour **recommencer**.

### Les raretés

| Rareté | Couleur | Remarque |
|---|---|---|
| Commun | gris | |
| Peu commun | vert | |
| Rare | bleu | |
| Épique | violet | |
| Légendaire | doré | |

La rareté est toujours affichée **aussi en texte** (jamais seulement par la couleur), pour l'accessibilité.

### Les trois paliers d'interface

| Palier | Esthétique |
|---|---|
| **1. Années 90** (départ) | texte monochrome vert ou ambré sur fond noir, police à chasse fixe, cadres en caractères |
| **2. Années 2000** | quelques couleurs, bordures, boutons en relief |
| **3. Aujourd'hui** | interface moderne : couleurs de rareté, arrondis, ombres légères, petites animations |

Les paliers sont débloqués par des modules précis.

---

## 3. Le contenu de la V1

Volume visé :

- une **quinzaine de fragments** (avec nom, description courte, rareté, taille, emoji) ;
- une **dizaine de recettes**, donc une dizaine de modules ;
- **trois paliers** d'interface.

Les fragments sont des éléments du monde moderne, que le programme décrit avec son regard d'époque. Exemple de ton (provisoire) :

> 🔔 **Notification** (commun, 1 case) : « Un petit télégramme qui clignote sans arrêt. Personne ne semble jamais le lire. »

**Règle de travail importante** : commencer avec des **noms provisoires** (« Fragment A », « Module 1 ») pour développer, et n'écrire les textes définitifs qu'en fin de projet (jour 7). Écrire les textes trop tôt est le piège principal en temps.

---

## 4. Hors périmètre de la V1

Ces idées sont volontairement **exclues** de la V1. Elles pourront constituer une V2.

- comptes joueurs et authentification ;
- placement spatial dans la grille (puzzle façon Tetris) ;
- récolte automatique (idle) qui continue page fermée ;
- zones ou régions à explorer ;
- succès, statistiques détaillées, classement ;
- sons et musique ;
- déploiement en ligne (voir la note sur SQLite plus bas) ;
- contenu étendu (plus de fragments, de modules, de paliers).

Toute nouvelle idée en cours de route va dans une liste « V2 » du README, et non dans le code de la V1.

---

## 5. Les choix techniques

| Domaine | Choix |
|---|---|
| Framework | Next.js (App Router) avec TypeScript |
| Style | Tailwind CSS |
| Base de données | SQLite, avec Prisma |
| Joueur | un seul joueur, sans compte |
| Versionnement | Git et GitHub dès le premier jour, petits commits réguliers |

### Ce qui va en base de données, et ce qui n'y va pas

Distinction importante, à comprendre dès le jour 1 :

- **Les données de jeu fixes** (la liste des fragments, des recettes, des modules, les probabilités) ne changent jamais pendant une partie : ce sont des **constantes TypeScript**, dans un fichier de données (par exemple `lib/game-data.ts`). Pas en base.
- **L'état de la partie** (les fragments en mémoire, le fragment en zone tampon, les modules obtenus, le journal) change en jouant : il est **en base de données**.

En base, on ne stocke donc que des **identifiants** qui renvoient aux constantes (par exemple « le fragment `notification` est en mémoire »), et non les noms ou les descriptions.

### L'architecture prévue

```
app/
├── layout.tsx            → layout racine (polices, palier d'interface)
├── page.tsx              → l'écran de jeu
└── modules/page.tsx      → le catalogue des modules (obtenus et à découvrir)

lib/
├── game-data.ts          → les constantes : fragments, recettes, modules, probabilités
├── game-logic.ts         → les fonctions pures : tirage pondéré, calcul de place, recette réalisable…
├── game-state.ts         → la lecture de l'état de la partie en base (server-only)
├── prisma.ts             → la connexion à la base
└── actions/game.ts       → les Server Actions : récolter, garder, rejeter, effacer, combiner, recommencer

types/
└── game.ts               → les types : Rarity, Fragment, Recipe, Module, GameState…

components/               → Atomic Design, comme dans Abricot
```

### Qui fait quoi

| Rôle | Où |
|---|---|
| Décrire le contenu du jeu | `game-data.ts` (constantes) et `types/game.ts` (types) |
| Calculer (sans rien lire ni écrire) | `game-logic.ts` (fonctions pures, faciles à tester) |
| Lire l'état de la partie | `game-state.ts`, appelé par des composants serveur |
| Modifier l'état de la partie | les Server Actions de `actions/game.ts`, puis `refresh()` |
| Afficher et réagir au joueur | les composants client (barre de récolte, boutons, grille interactive) |

**Le hasard est tiré sur le serveur**, dans la Server Action de récolte : le composant client se contente d'afficher la barre de progression, puis de déclencher l'action.

### Note sur SQLite et le déploiement

SQLite stocke la base dans un **fichier**. Les hébergeurs comme Vercel ne conservent pas les fichiers écrits par l'application : la base y serait perdue. La V1 tourne donc **en local**. Un déploiement demandera de passer à une base hébergée (par exemple PostgreSQL), ce qui est prévu en V2.

---

## 6. Le planning jour par jour

Chaque journée se termine par un **objectif testable**. Si un objectif n'est pas atteint, on le termine le lendemain matin **avant** d'ajouter quoi que ce soit, quitte à réduire le contenu prévu.

### Jour 1 : les fondations

- Créer le projet Next (TypeScript, Tailwind, App Router), le dépôt GitHub, un README minimal.
- Installer et configurer Prisma avec SQLite (en suivant la documentation officielle à jour).
- Écrire le schéma de la base : l'état de la partie (capacité, compatibilité, palier), les fragments en mémoire, le fragment en zone tampon, les modules obtenus, le journal.
- Écrire les types du jeu dans `types/game.ts`.

**Objectif testable** : la migration crée la base ; Prisma Studio (l'outil de visualisation de Prisma) affiche les tables vides.

### Jour 2 : les données et la logique pure

- Remplir `game-data.ts` avec du contenu **provisoire** (5 fragments, 3 recettes suffisent pour commencer).
- Écrire les fonctions pures de `game-logic.ts` : place occupée, place libre, tirage d'une rareté selon des probabilités pondérées, tirage d'un fragment, vérifier si une recette est réalisable.
- Lire l'état de la partie et **afficher la grille de mémoire** (en lecture seule) dans un composant serveur.

**Objectif testable** : la page affiche une grille de 16 cases, remplie avec des fragments ajoutés à la main dans Prisma Studio.

### Jour 3 : la récolte rapide et la zone tampon

- Le bouton de récolte (composant client) : barre de progression pendant la durée, puis appel de la Server Action.
- La Server Action de récolte rapide : tirage, mise en zone tampon, ligne de journal.
- Garder ou rejeter le fragment en zone tampon.
- Le journal des événements.

**Objectif testable** : on peut récolter, garder ou rejeter, et la mémoire se remplit ; le journal raconte ce qui s'est passé.

### Jour 4 : la mémoire pleine et la récolte profonde

- Effacer un fragment de la mémoire.
- Le cas de la mémoire pleine : faire de la place pour le fragment en attente.
- La récolte profonde : autres probabilités, et la corruption.

**Objectif testable** : on peut remplir la mémoire, faire des choix pour faire de la place, et perdre un fragment par corruption.

### Jour 5 : la fabrication

- La liste des recettes, avec l'indication de celles qui sont réalisables.
- La Server Action de fabrication : vérification, retrait des fragments, ajout du module, application du bonus, mise à jour de la compatibilité.
- La page du catalogue des modules.

**Objectif testable** : on peut fabriquer un module ; la mémoire se libère et le bonus s'applique (par exemple, la capacité passe à 18 cases).

### Jour 6 : les paliers d'interface et la fin de partie

- Les trois paliers : le palier courant est lu depuis l'état de la partie et appliqué à l'interface (par exemple avec un attribut sur la balise `<html>` et des variables CSS par palier).
- L'écran de fin (compatibilité 100 %) et le bouton pour recommencer.

**Objectif testable** : en jouant, l'interface passe des années 90 à aujourd'hui ; une partie complète peut être menée jusqu'au bout.

### Jour 7 : le contenu et l'accessibilité

- Écrire les **textes définitifs** : les fragments, les modules, les messages du journal.
- Compléter le contenu (une quinzaine de fragments, une dizaine de modules).
- Accessibilité : journal annoncé aux lecteurs d'écran (`aria-live`), navigation au clavier, focus visible, rareté toujours écrite en texte, respect de `motion-reduce`.
- Gestion des erreurs : chaque Server Action renvoie un état d'erreur clair (fabrication impossible, mémoire insuffisante…).

**Objectif testable** : une partie complète, jouable au clavier, avec les vrais textes.

### Jour 8 : la marge

- Corriger les bugs, équilibrer les probabilités.
- Finaliser le README : présentation, captures d'écran, installation, choix techniques, liste V2.
- Commenter le code (JSDoc pour les fonctions exportées).
- `npm run build` sans erreur ni avertissement.

**Objectif testable** : quelqu'un qui clone le dépôt peut installer et jouer en suivant le README.

---

## 7. Les notions, dans l'ordre où elles arrivent

| Jour | Notions consolidées (déjà vues dans Abricot) | Notions nouvelles |
|---|---|---|
| 1 | création d'un projet Next, Tailwind, `@theme`, Git | **Prisma** et **SQLite** : schéma, migration, Prisma Studio |
| 2 | types, unions, `Record`, `interface`, composants serveur, `map` et `key` | **fonctions pures**, **tirage pondéré** (`Math.random`), `reduce` |
| 3 | Server Actions, `useActionState`, `<Suspense>`, `refresh()` | **`useState`** (état de la récolte), **`setTimeout`** et **`useEffect`** (la barre de progression) |
| 4 | gestion des erreurs, rétrécissement de type | logique de règles métier sur le serveur |
| 5 | `filter`, `every`, `some`, `find` | **transactions** Prisma (plusieurs écritures qui réussissent ou échouent ensemble) |
| 6 | variables CSS, classes conditionnelles | **thème dynamique** (attribut sur `<html>`, variables CSS par palier) |
| 7 | accessibilité (`aria-*`), validation | **`aria-live`**, gestion du focus, `motion-reduce` |
| 8 | README, commentaires, build | |

---

## 8. Les règles de travail

- **Une notion à la fois** : ne rien utiliser qui n'a pas été compris.
- **Tester avant d'avancer** : chaque étape se termine par une vérification concrète.
- **Un commit par étape réussie**, avec un message clair.
- **Noter les écarts et les idées** dans le README (liste V2), plutôt que d'élargir la V1.
- **Commenter au fur et à mesure** (JSDoc pour les fonctions exportées), comme sur Abricot.
- **Si un jour déborde** : réduire le contenu, jamais les fondations ni les tests.
