<div align="center">

# Axium Hub

### Valorant Team Hub — Plateforme de gestion d'équipes esports compétitives

[![Live](https://img.shields.io/badge/🌐_Live-axium--hub.com-C0392B?style=for-the-badge)](https://axium-hub.com)
![Status](https://img.shields.io/badge/Status-En_développement_actif-2ECC71?style=for-the-badge)
![Type](https://img.shields.io/badge/Dépôt-Privé_(produit_commercial)-555555?style=for-the-badge)

> ⚠️ Ce dépôt est privé — Axium Hub est un produit en développement actif.  
> Ce README présente l'architecture, les fonctionnalités et les décisions techniques du projet.

</div>

---

## 📸 Aperçu

<table>
  <tr>
    <td align="center"><b>Accueil</b></td>
    <td align="center"><b>Organisation</b></td>
  </tr>
  <tr>
    <td><img src="./screenshots/screen_welcome.png" alt="Accueil" /></td>
    <td><img src="./screenshots/screen_org.png" alt="Organisation" /></td>
  </tr>
  <tr>
    <td align="center"><b>Playbook</b></td>
    <td align="center"><b>Lineups interactifs</b></td>
  </tr>
  <tr>
    <td><img src="./screenshots/screen_playbook.png" alt="Playbook" /></td>
    <td><img src="./screenshots/screen_lineups.png" alt="Lineups" /></td>
  </tr>
  <tr>
    <td align="center"><b>Team Agenda</b></td>
    <td align="center"><b>Match History</b></td>
  </tr>
  <tr>
    <td><img src="./screenshots/screen_agenda.png" alt="Agenda" /></td>
    <td><img src="./screenshots/screen_matchhistory.png" alt="Match History" /></td>
  </tr>
</table>

---

## 🎯 Problème résolu

Les équipes esports compétitives jonglent entre Discord, Google Sheets, et des fichiers partagés pour gérer leurs stratégies, leur calendrier, et leur historique de matchs. **Axium Hub centralise tout ça** en une seule plateforme dédiée, construite par des joueurs pour des joueurs.

---

## ✨ Fonctionnalités

| Module | Description |
|---|---|
| 🏢 **Organisation** | Gestion multi-équipes avec membres, rôles et permissions granulaires |
| 📖 **Playbook** | Banque de stratégies organisée par carte (16 maps Valorant) |
| 🗺️ **Lineups** | Carte interactive pour placer et visualiser les lineups par agent |
| 📅 **Team Agenda** | Calendrier d'équipe avec disponibilités des joueurs, export Google Calendar |
| ⚔️ **Match History** | Historique complet des matchs avec filtres avancés (carte, composition, adversaire) |
| 📹 **VOD Review** | Lien vers les VODs associées à chaque match |
| 📣 **Annonces** | Système d'annonces internes par équipe |
| 🔔 **Discord Webhooks** | Notifications automatiques vers les serveurs Discord de l'équipe |

---

## 🏗️ Architecture

Axium Hub est construit selon les principes du **Domain-Driven Design (DDD)** avec une **Clean Architecture** stricte.

### Bounded Contexts (8)

```
axium-hub/
├── identity/          # Authentification, utilisateurs, sessions
├── organization/      # Orgs, équipes, membres, permissions
├── strategy/          # Playbook, stratégies par carte
├── lineup/            # Lineups interactifs (carte + agents)
├── match/             # Historique de matchs, séries, scores
├── vod/               # Gestion des vidéos de review
├── scheduling/        # Calendrier, événements, disponibilités
└── gamedata/          # Intégration Valorant API (agents, maps)
```

### Communication inter-domaines

Les bounded contexts communiquent exclusivement via un **`InMemoryEventBus` maison** — aucun couplage direct entre domaines.

```
[identity] → UserRegistered → [organization] → MemberAdded → [scheduling]
```

### Stack technique

**Frontend**
```
React 19 · React Router DOM v7 · Tailwind CSS v4 · Vite 7 · Context API
Déploiement : Vercel (SPA)
```

**Backend**
```
Node.js (ESM) · Express v5 · Awilix (IoC / injection de dépendances)
Auth.js · Multer · Déploiement : Railway (API REST)
```

**Base de données**
```
PostgreSQL · Prisma ORM v7 · Supabase (DB + Storage)
```

**Tests**
```
Vitest · Supertest
```

---

## 🔐 Système d'authentification

Un des aspects les plus complexes du projet — le système d'auth est complet et production-ready :

- **JWT + tokenVersion** — invalidation côté serveur sans liste noire
- **2FA TOTP** via `otplib` (compatible Google Authenticator)
- **Vérification email obligatoire** à l'inscription (nodemailer)
- **Verrouillage progressif** après tentatives échouées : 5 → 15 → 30 → 60 min
- **Alertes IP** par géolocalisation — notification si connexion depuis un nouvel emplacement
- **bcryptjs** pour le hashage des mots de passe

---

## 🔌 Intégrations externes

| Service | Usage |
|---|---|
| **Valorant API** | Sync des agents, maps, et données de jeu |
| **Google Calendar API** | Export de l'agenda d'équipe |
| **Discord Webhooks** | Notifications automatiques (matchs, annonces) |
| **ip-api.com** | Géolocalisation des connexions pour alertes de sécurité |
| **Supabase Storage** | Stockage des assets (logos, VODs) |

---

## 🚀 Déploiement

Architecture découplée — frontend et backend sont déployés indépendamment :

```
[Client]  →  Vercel (SPA React)
                  ↓ proxy via vercel.json
[API]     →  Railway (Express v5)
                  ↓
[DB]      →  Supabase (PostgreSQL)
```

---

## 👥 Équipe

Projet développé à deux dans le cadre du programme de génie logiciel de l'ÉTS Montréal.

| | Rôle |
|---|---|
| **Sacha Burelle** | Co-fondateur · Full-stack · Architecture |
| **Wassim Ouali** | Co-fondateur · Full-stack |

---

<div align="center">

**[🌐 Visiter axium-hub.com](https://axium-hub.com)**

*Construit avec passion par des joueurs, pour des joueurs.*

</div>
