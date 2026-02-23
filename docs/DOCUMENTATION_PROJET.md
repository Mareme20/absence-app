# Documentation Professionnelle - IIBS Absence App

## 1. Présentation du projet

**IIBS Absence App** est une plateforme web de gestion des absences académiques.
Elle centralise :

- l'authentification et la gestion des rôles,
- la gestion des entités pédagogiques (classes, professeurs, étudiants, cours),
- l'enregistrement des absences,
- la soumission et le traitement des justifications,
- la consultation des indicateurs statistiques.

Le projet est structuré en deux applications :

- `iibS-absence-front` : application Angular (interface utilisateur),
- `iibS-absence-back` : API REST Node.js/Express + base PostgreSQL.

## 2. Objectifs métiers

- Digitaliser le suivi des absences pour réduire les traitements manuels.
- Sécuriser les accès selon le profil utilisateur.
- Donner une visibilité décisionnelle via des statistiques.
- Standardiser les processus de justification d'absence.

## 3. Périmètre fonctionnel

### 3.1 Authentification

- Inscription et connexion utilisateur.
- Gestion JWT côté frontend/backend.

### 3.2 Gestion académique

- CRUD classes.
- CRUD professeurs.
- CRUD étudiants.
- Planification et gestion des cours.

### 3.3 Gestion des absences

- Enregistrement d'absences par les profils autorisés.
- Soumission de justificatifs par l'étudiant.
- Traitement des justifications par les profils administratifs.

### 3.4 Statistiques

- Cours par professeur.
- Cours par classe.
- Top des étudiants absents.
- Étudiants au-dessus du seuil d'heures d'absence.

## 4. Rôles et droits

- `RP` : administration globale, statistiques, opérations sensibles.
- `PROF` : gestion des cours et absences.
- `ATTACHE` : suivi opérationnel, statistiques.
- `ETUDIANT` : consultation personnelle et justifications.

## 5. Architecture technique

## 5.1 Vue d'ensemble

```text
Navigateur
   |
   v
Frontend Angular (Vercel)
   |
   v
Backend Express/TypeORM (Render)
   |
   v
PostgreSQL (Neon Postgres)
```

### 5.2 Frontend (`iibS-absence-front`)

- Angular 21 (standalone components)
- Angular Material
- RxJS / TypeScript
- Architecture en couches :
  - `core/` : services HTTP, guards, interceptors, modèles,
  - `application/facades/` : orchestration UI,
  - `features/` : pages métier,
  - `shared/components/` : composants réutilisables.

### 5.3 Backend (`iibS-absence-back`)

- Node.js + Express 5
- TypeORM 0.3
- PostgreSQL
- Zod (validation)
- JWT (authentification)
- Swagger (documentation API)

## 6. Sécurité et conformité technique

- Authentification par token JWT.
- Middleware de contrôle des rôles backend.
- Validation des données entrantes.
- CORS configuré pour :
  - environnement local (`http://localhost:4200`),
  - domaine frontend Vercel,
  - previews Vercel (`iib-s-absence-front-*.vercel.app`).
- Connexion PostgreSQL avec SSL en production Render.

## 7. Environnements et configuration

### 7.1 Backend - variables principales

| Variable | Description |
|----------|-------------|
| `PORT` | Port HTTP du service |
| `DATABASE_URL` | URL de connexion PostgreSQL (prioritaire) |
| `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASSWORD`, `DB_NAME` | Configuration DB locale |
| `DB_SSL` | Active SSL PostgreSQL (`true` en prod) |
| `PGSSLMODE` | Mode SSL (`require` recommandé sur Render) |
| `JWT_SECRET` | Secret de signature JWT |
| `CORS_ORIGINS` | Liste d'origines autorisées (séparées par virgule) |

### 7.2 Frontend - variables principales

Fichiers :

- `iibS-absence-front/src/environments/environment.ts`
- `iibS-absence-front/src/environments/environment.prod.ts`

Variable clé :

- `apiUrl` (URL de base de l'API, ex. `https://iibs-absence-back.onrender.com/api`).

## 8. Déploiement

### 8.1 Frontend (Vercel)

- Root Directory : `iibS-absence-front`
- Install Command : `npm ci`
- Build Command : `npm run build:vercel`
- Output Directory : `dist/iibs-absence-front/browser`

URL de production :

- `https://iib-s-absence-front.vercel.app`

### 8.2 Backend (Render)

- Build Command : `npm run build`
- Start Command : `node dist/server.js`
- Variables Render configurées (DB + JWT + CORS + SSL)

URL de production :

- `https://iibs-absence-back.onrender.com`

Swagger :

- `https://iibs-absence-back.onrender.com/api-docs`

## 9. Qualité logicielle

- Typage TypeScript sur front et back.
- Structuration modulaire facilitant la maintenance.
- Build de production validé avant déploiement.
- Endpoints API documentés via Swagger.

## 10. Limites actuelles et axes d'amélioration

- Renforcer la stratégie de tests automatisés (unitaires/intégration/e2e).
- Ajouter une stratégie de migration TypeORM plutôt que `synchronize` en production.
- Mettre en place observabilité centralisée (logs, métriques, alertes).
- Ajouter une CI/CD complète (lint, tests, build, checks sécurité).

## 11. Roadmap suggérée

- V1.1 : durcissement sécurité + migrations DB.
- V1.2 : amélioration UX et tableau de bord avancé.
- V1.3 : notifications (email/in-app) et exports.
- V2.0 : multi-établissements et administration avancée.

## 12. Références

- README racine : `README.md`
- Documentation frontend : `iibS-absence-front/README.md`
- Documentation backend : `iibS-absence-back/README.md`
