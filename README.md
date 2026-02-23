# IIBS Absence App

Application de gestion des absences de l'IIBS, composée de:
- un backend API Node.js/Express/TypeORM (`iibS-absence-back`)
- un frontend Angular (`iibS-absence-front`)

## Stack
- Frontend: Angular 21, Angular Material, RxJS
- Backend: Node.js, Express 5, TypeORM, PostgreSQL, Zod, JWT
- Infra: Docker (front/back), Vercel (front), Render (back)

## Structure
```text
Absence-app/
├── iibS-absence-front/   # Angular app
├── iibS-absence-back/    # API REST + PostgreSQL
└── README.md
```

## Prerequis
- Node.js 20+
- npm 10+
- Docker + Docker Compose (optionnel mais recommande)
- PostgreSQL local si execution sans Docker

## Lancement rapide (local)

### 1) Backend
```bash
cd iibS-absence-back
npm install
npm run dev
```

Le backend demarre sur `http://localhost:3000`.
Swagger: `http://localhost:3000/api-docs`

### 2) Frontend
```bash
cd iibS-absence-front
npm install
npm start
```

Le frontend demarre sur `http://localhost:4200`.

## Variables d'environnement backend
Fichier: `iibS-absence-back/.env`

Exemple minimal:
```env
PORT=3000
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=postgres
DB_NAME=iibs_absence
JWT_SECRET=secretkey
```

## Docker

### Backend + PostgreSQL (compose)
```bash
cd iibS-absence-back
docker-compose up --build
```

Services:
- API: `http://localhost:3000`
- PostgreSQL: `localhost:5432`

### Frontend (image statique nginx)
```bash
cd iibS-absence-front
docker build -t iibs-absence-front .
docker run -p 8080:80 iibs-absence-front
```

Frontend via Docker: `http://localhost:8080`

## Build production

### Backend
```bash
cd iibS-absence-back
npm run build
npm start
```

### Frontend
```bash
cd iibS-absence-front
npm run build
```

Sortie: `iibS-absence-front/dist/iibs-absence-front`

## Deploiement

### Frontend sur Vercel
Le front inclut `vercel.json`.

```bash
cd iibS-absence-front
npm ci
npm run build:vercel
```

Configurer le projet Vercel avec la racine `iibS-absence-front`.

### Backend sur Render
Le backend est prevu pour Render (URL deja referencee dans la config Swagger):
- `https://iibs-absence-back.onrender.com`

## API principale
Base URL locale: `http://localhost:3000/api`

Exemples de routes:
- `POST /auth/login`
- `POST /auth/register`
- `GET /classes`
- `GET /cours`
- `POST /absences`
- `GET /stats/cours-par-professeur`

## Comptes et roles
Roles supportes:
- `RP`
- `PROF`
- `ATTACHE`
- `ETUDIANT`

Le menu front et les permissions API sont controles par role (JWT + middleware backend).

## Commandes utiles

### Front
```bash
cd iibS-absence-front
npm start
npm run build
npm test
```

### Back
```bash
cd iibS-absence-back
npm run dev
npm run build
npm start
```

## Notes d'architecture
- Separation par couches sur le front: `core` (services/modeles), `application/facades`, `features`, `shared/components`.
- Composants partages introduits pour reduire la duplication (`page-header`, `empty-state`).
- Routing front en lazy loading pour diminuer le bundle initial.

## Liens utiles
- Front README detaille: `iibS-absence-front/README.md`
- Back README detaille: `iibS-absence-back/README.md`
