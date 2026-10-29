# 640 Ko

Petit jeu de récolte et de fabrication, développé avec Next.js, React et TypeScript.

Vous incarnez un programme des années 90 qui se réveille sur une machine d'aujourd'hui. Pour s'adapter, il récolte des **fragments** du monde moderne, puis les combine en **modules**, avec une contrainte de son époque : une mémoire minuscule.

> Projet en cours de développement (V1).

## Les règles en bref

1. **La mémoire est limitée** : 16 cases au départ, chaque fragment en occupe 1, 2 ou 3.
2. **La récolte est un pari** : une récolte rapide est sans risque, une récolte profonde offre de meilleures raretés mais peut corrompre un fragment déjà en mémoire.
3. **Combiner libère de la place** : une recette consomme des fragments et produit un module qui apporte un bonus permanent.

La partie se termine quand tous les modules sont obtenus. L'interface évolue au fil de la progression, des années 90 à aujourd'hui.

## Technologies

- Next.js (App Router) et React
- TypeScript
- Tailwind CSS
- SQLite avec Prisma

## Installation

```bash
git clone <url-du-depot>
cd 640ko
npm install
npm run dev
```

Le jeu est ensuite accessible sur http://localhost:3000.

> La mise en place de la base de données sera détaillée une fois Prisma configuré.

## Idées pour une V2

Les idées hors du périmètre de la V1 sont notées ici :

- comptes joueurs et authentification
- placement spatial dans la grille
- récolte automatique qui continue page fermée
- zones ou régions à explorer
- succès, statistiques, classement
- sons et musique
- déploiement en ligne (avec une base hébergée, par exemple PostgreSQL)
- contenu étendu