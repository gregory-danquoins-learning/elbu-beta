# CLAUDE.md — Elbu (racine)

Ce fichier est chargé quel que soit le sous-projet ouvert dans Claude Code.
Il donne une vue globale du projet **Electricity Business (Elbu)**.

## Description du projet

Application web de **location de bornes de recharge de véhicules électriques entre particuliers**.
Les utilisateurs peuvent proposer leur borne à la location et en réserver une appartenant à un autre particulier.

## Structure du monorepo

```
elbu/
  elbub/     ← API REST Spring Boot  (backend)
  ebuf/      ← Application Angular   (frontend) — à venir
  uploads/   ← Médias uploadés (avatars, photos de bornes, vidéos)
               Servis par le backend via GET /uploads/**
```

## Relations entre les projets

| Aspect | Valeur |
|---|---|
| Backend URL (dev) | `http://localhost:8080` |
| Frontend URL (dev) | `http://localhost:4200` |
| Auth | JWT via cookies HTTP-only (`access_token` 15 min · `refresh_token` 7 jours) |
| CORS | Backend autorise `http://localhost:4200` avec `credentials: true` |
| Médias | Stockés dans `elbu/uploads/`, servis en public sur `/uploads/**` |

## Authentification — ce que le frontend doit savoir

- Les cookies sont posés automatiquement par le backend (`SameSite=Strict`, `HttpOnly`)
- Le frontend n'a **jamais** accès aux tokens (pas de localStorage, pas de header Authorization)
- Toutes les requêtes API doivent inclure `withCredentials: true`
- En cas de 401 sur un appel API, tenter un refresh via `POST /auth/refresh` avant de rediriger vers le login

## Endpoints publics (pas d'auth requise)

| Route | Usage |
|---|---|
| `POST /auth/register` | Inscription |
| `POST /auth/login` | Connexion |
| `POST /auth/verify-email` | Vérification email |
| `POST /auth/resend-verification` | Renvoyer le mail de vérification |
| `POST /auth/forgot-password` | Demander un reset password |
| `POST /auth/reset-password` | Réinitialiser le mot de passe |
| `POST /auth/refresh` | Rafraîchir l'access token |
| `POST /auth/logout` | Déconnexion |
| `GET /uploads/**` | Accès public aux médias |

## Règles partagées

- L'email est toujours normalisé en minuscules côté backend
- Les rôles sont toujours en majuscules (`USER`, `ADMIN`)
- Les dates/heures sont en `Instant` ISO-8601 UTC
- Les réponses d'erreur suivent toujours `{ status, message, path, timestamp }`

## Sous-projets

- **Backend** → voir `elbub/CLAUDE.md` pour les détails Spring Boot
- **Frontend** → voir `ebuf/CLAUDE.md` pour les détails Angular (à créer)
