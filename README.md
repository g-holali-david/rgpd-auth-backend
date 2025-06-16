# TP RGPD – Partie 2.3 : Backend pour une logique de frontend de gestion de token

## Choix de la partie
J’ai choisi de réaliser la **partie 2.3 du TP** :  
> **Créer une logique de backend pour une logique de frontend de gestion de token**, en respectant les bonnes pratiques de développement logiciel et de sécurité (RGPD-compliance, JWT, SOLID…).

---

## Présentation du projet

**Nom du projet** : `secureprofile-backend`  
**Auteur** : GAVI Holali David  
**Technos** : Spring Boot, Spring Security, JWT, PostgreSQL, Docker, GitHub Actions

Ce backend assure une **gestion sécurisée de l’authentification**, la gestion de rôles et de sessions avec des **tokens JWT signés**, dans le respect des bonnes pratiques RGPD.

---

## Composants RGPD-Compliant implémentés

### 1. Authentification & Tokens

- Génération de **JWT signés** (durée de vie courte)
- Implémentation de **refresh tokens** stockés en base
- Endpoint `/auth/refresh` pour générer un nouveau JWT sans se reconnecter
- **Blacklist** des tokens invalidés (logout)
- Protection contre le **brute-force** via système de blocage temporaire

### 2. Architecture modulaire (principes SOLID)

- Code organisé par **responsabilités** : `controller`, `model`, `repository`, `security`, `validation`, etc.
- Annotations personnalisées pour la **validation forte** des champs (`@StrongPassword`)
- Séparation nette des couches : service, filtre, config, etc.

### 3. Sécurité et RGPD

- **Chiffrement AES** des emails/usernames (confidentialité des données personnelles)
- **Hashage des mots de passe** avec `BCrypt`
- Audit de certaines actions critiques (connexion, logout)
- Planification automatique de nettoyage des tokens expirés via `Spring Scheduler`
- `@PreAuthorize` et rôles (`USER`, `ADMIN`) pour sécuriser les accès aux routes sensibles

---

## ⚙️ SAST / Analyse de sécurité

Un outil de **SAST (Static Application Security Testing)** a été intégré au pipeline CI :

### 🔍 Semgrep (via GitHub Actions)
```yaml
- name: Run Semgrep for static security analysis
  uses: returntocorp/semgrep-action@v1
  with:
    config: "p/default"
```

Ce scan détecte les vulnérabilités courantes (injections, mauvaise gestion des tokens, erreurs d’autorisation…).

---

## Résumé des Endpoints principaux

| Méthode | URL                         | Description                              |
|--------|-----------------------------|------------------------------------------|
| POST   | `/auth/register`            | Inscription avec rôle `USER` par défaut  |
| POST   | `/auth/login`               | Authentification et retour des tokens    |
| POST   | `/auth/refresh`             | Génère un nouveau token d’accès          |
| POST   | `/auth/logout`              | Invalide le refresh token                |
| GET    | `/users/me`                 | Récupère les infos du profil             |
| PATCH  | `/users/password`           | Modifier son mot de passe                |
| DELETE | `/users/me`                 | Supprimer son compte                     |
| GET    | `/admin/users`              | Voir tous les utilisateurs               |
| PATCH  | `/admin/users/{id}/enable`  | Activer un compte                        |
| PATCH  | `/admin/users/{id}/disable` | Désactiver un compte                     |
| POST   | `/admin/change-role`        | Modifier le rôle d’un utilisateur        |

---

## Docker & .env

Le projet est conteneurisé (`Dockerfile`) et les variables sensibles sont chargées via un fichier `.env` externe (bonnes pratiques RGPD).

---

## CI/CD avec GitHub Actions

- Compilation
- Vérifications de sécurité (Semgrep)
- Tests (à compléter)
- Intégration continue

---


## Dépôt GitHub

👉 [Lien vers le dépôt GitHub du projet](https://github.com/ton-utilisateur/secureprofile-backend)

---

## Conclusion

Ce projet répond aux attentes de la **partie 2.3 du TP RGPD** :

- Signature de JWT avec gestion des tokens
- Backend structuré selon les bonnes pratiques
- Sécurité RGPD : chiffrement, audit, validation, nettoyage, rôles
- Analyse SAST intégrée via GitHub Actions

---

> Projet réalisé dans le cadre du TP "RGPD et sécurisation des SI" – Mastère IPSSI, Juin 2025.
