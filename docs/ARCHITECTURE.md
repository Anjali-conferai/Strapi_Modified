# System Architecture

[Back to Index](README.md)

---

## Overview

The JKYOG digital ecosystem is composed of multiple interconnected systems. Strapi sits at the center as the **single source of truth** for all content. This document maps how every piece connects.

---

## The Big Picture

```
+------------------------------------------------------------------+
|                        CONTENT CREATORS                          |
|                   (You, via Strapi Dashboard)                    |
+------------------------------------------------------------------+
                              |
                         MANAGE DATA
                              |
                              v
+------------------------------------------------------------------+
|                       STRAPI CMS                                 |
|                                                                  |
|  +------------------+  +------------------+  +----------------+  |
|  | Content Manager  |  | Media Library    |  | Plugins        |  |
|  |                  |  |                  |  |                |  |
|  | - App Events     |  | - Event Banners  |  | - Tickets &    |  |
|  | - Live Streams   |  |   (1024x768)     |  |   Promocodes   |  |
|  | - Email Temps    |  | - Gallery Images |  | - Import/Export |  |
|  | - Announcements  |  | - Thumbnails     |  | - Meilisearch  |  |
|  | - Articles       |  |                  |  |                |  |
|  | - Audios/Books   |  |                  |  |                |  |
|  | - Announcements  |  |                  |  |                |  |
|  +------------------+  +------------------+  +----------------+  |
|                                                                  |
|  +------------------------------------------------------------+  |
|  |                    REST API (Auto-Generated)                |  |
|  |  Base: https://prod-strapi.jkyog.org/api/                  |  |
|  |  Endpoints: /appevents, /livestreams, /apparticles, ...     |  |
|  +------------------------------------------------------------+  |
+------------------------------------------------------------------+
          |                    |                     |
          v                    v                     v
  +---------------+    +---------------+    +-----------------+
  |   WEBFLOW     |    |   RKT APP     |    |     BREVO       |
  |   (Website)   |    |   (Mobile)    |    |   (Email)       |
  |               |    |               |    |                 |
  | radhakrishna  |    | iOS / Android |    | Registration    |
  | temple.net    |    | app           |    | confirmations   |
  |               |    |               |    | & marketing     |
  | - Event pages |    | - Events feed |    | emails          |
  | - Registration|    | - Articles    |    |                 |
  |   forms       |    | - Audios      |    |                 |
  | - Seva pages  |    | - Books       |    |                 |
  +---------------+    +---------------+    +-----------------+
          |
          v
  +---------------+
  |   PAYMENT     |
  |   GATEWAY     |
  |               |
  | Processes     |
  | ticket        |
  | payments for  |
  | paid events   |
  +---------------+
```

---

## System Components

### 1. Strapi CMS (The Core)

| Aspect | Detail |
|--------|--------|
| **Role** | Central content repository and API server |
| **Technology** | **Strapi v4.10.7** on **Node.js 18.20.5** |
| **Language** | JavaScript (config/API), TypeScript (custom plugins) |
| **Production URL** | `https://prod-strapi.jkyog.org/admin` |
| **Staging URL** | `https://jkyog-strapi-staging.up.railway.app/admin` |
| **Hosting** | Railway (staging), Docker container with Node.js 20 (production) |
| **Database** | **PostgreSQL** (production, database: `jkyogi`), SQLite (local dev fallback) |
| **Content Types** | 88 total (15 Single Types + 73 Collection Types), 39 reusable components |
| **API Limit** | Max 100 records per request, populate depth up to 5 levels |

**Key characteristic:** The backend is defined by **JSON configuration files** in the Git repository, not by procedural code. These files define field types, constraints, validation rules, and entity relationships.

### Infrastructure Services

> For complete details on each service (caching, media, video, search), see [Data & API Reference](DATA.md) and [Environment Variables](ENVIRONMENT-VARIABLES.md).

| Service | Technology |
|---------|-----------|
| **API Caching** | Redis (1-hour TTL, invalidates on update/delete but **NOT on create**) |
| **Media Storage** | AWS S3 + CloudFront CDN |
| **Video Processing** | Mux |
| **Full-Text Search** | Meilisearch (Railway) |
| **Error Tracking** | Sentry |

### API Access Control (Public Role)

> For complete Public role permission details, see [Data & API Reference -- Public Role Permissions](DATA.md#public-role-permissions).

The Public role must have `find` and `findOne` enabled for each content type the frontend consumes. If missing, the API returns 403 Forbidden.

### 2. Frontend Consumers

Strapi serves content to **four** downstream consumers via REST API and webhooks:

| Consumer | Type | Connection | What It Consumes |
|----------|------|-----------|-----------------|
| **JKYog 2.0 Frontend** | Website | REST API + Webhooks | Events, retreats, blogs, website content |
| **RKT Dallas 2.0 Frontend** | Website | REST API + Webhooks | Temple events, sevas, live streams |
| **JKYog Mobile App** | iOS / Android | REST API | Events, articles, audios, books, announcements, stories, challenges |
| **JKYog 2.0 Backend (NestJS)** | API Server | **Webhooks** | Receives content change notifications for event sync, course updates, content propagation |

#### Webflow (RKT Dallas Website)

| Aspect | Detail |
|--------|--------|
| **URL** | `https://www.radhakrishnatemple.net` |
| **Registration Forms** | Embedded forms with Code Embed elements containing `eventUuid` |
| **Key Integration Point** | `eventUuid` on line 25 of Code Embed links the form to a Strapi event |

#### NestJS Backend (Webhook Integration)

> For complete webhook configuration, event types, and entity-backend mapping, see [Data & API Reference -- Webhooks](DATA.md#webhooks).

The NestJS backend receives automatic webhook notifications when content changes in Strapi, using `STRAPI_WEBHOOK_TOKEN` for authentication. Webhooks notify the backend instantly, but **caching controls the frontend** -- the website may still serve stale data until the cache expires or is purged.

### 3. Brevo (Email Marketing)

| Aspect | Detail |
|--------|--------|
| **Role** | Sends registration confirmation and marketing emails |
| **Connection to Strapi** | `BrevoContactListName` field in App Events maps registrants to Brevo mailing lists |
| **Format** | List names follow the pattern `/rkt-[eventname]` |

### 4. Payment Gateway

| Aspect | Detail |
|--------|--------|
| **Role** | Processes payments for paid events |
| **Connection to Strapi** | Ticket prices and types are defined in the Tickets and Promocodes plugin |
| **Key Risk** | If tickets are not configured, the form shows $0 |

---

## Data Flow: Event Creation (End-to-End)

This is the most important flow in the system. Every numbered step shows which system is involved:

```
Step  System          Action
----  ------          ------
 1    Media Library   Upload banner image (1024x768)
 2    Strapi          Create App Event (all fields)
 3    Strapi          Create Event Email Template
 4    Strapi          Create Live Stream (Sevas + Schedule)
 5    Strapi          Link Event <-> Live Stream <-> Email Template
 6    Strapi          Save & Publish
                        |
                        | API auto-updates
                        v
 7    Webflow         Copy registration component to event page
 8    Webflow         Update eventUuid in Code Embed (line 25)
 9    Webflow         Verify form field IDs
10    Webflow         Publish
                        |
                        | (if paid event)
                        v
11    Strapi          Create ticket in Tickets & Promocodes
                        |
                        | User registers
                        v
12    Brevo           Confirmation email sent via BrevoContactListName
13    Payment         Payment processed (if paid)
```

---

## Data Flow: API Request Lifecycle

When a user visits the Upcoming Events page, this happens:

```
User's Browser          Webflow Frontend           Redis Cache         Strapi API        PostgreSQL
     |                       |                         |                    |                  |
     | 1. Opens event page   |                         |                    |                  |
     |---------------------->|                         |                    |                  |
     |                       | 2. GET /api/appevents   |                    |                  |
     |                       |   ?populate=*            |                    |                  |
     |                       |   &sort=StartTime:asc    |                    |                  |
     |                       |   &filters[EventPlatform]|                    |                  |
     |                       |     [$containsi]=RKT     |                    |                  |
     |                       |------------------------>|                    |                  |
     |                       |                         |                    |                  |
     |                       |                    CACHE HIT?                 |                  |
     |                       |                    /        \                 |                  |
     |                       |                  YES         NO               |                  |
     |                       |                   |          |                |                  |
     |                       |   3a. Cached JSON |   3b. Forward request     |                  |
     |                       |<------------------|--------->|                |                  |
     |                       |                   |          | 4. Query DB    |                  |
     |                       |                   |          |--------------->|                  |
     |                       |                   |          |<---------------|                  |
     |                       |                   |          | 5. Cache result|                  |
     |                       |                   |<---------|  (TTL: 1 hour) |                  |
     |                       |   6. JSON response|          |                |                  |
     |                       |<------------------|----------|                |                  |
     |                       |                         |                    |                  |
     | 7. Rendered event     |                         |                    |                  |
     |    cards (title, date,|                         |                    |                  |
     |    image, free/paid)  |                         |                    |                  |
     |<----------------------|                         |                    |                  |
```

### Caching Behavior

> For complete caching details (TTL, cached content types, purge guidelines), see [Data & API Reference -- Caching](DATA.md#api-caching-redis).

**Key rule:** Cache invalidates on update/delete but **NOT on create**. New entries may take up to 1 hour to appear. Purge the cache or re-save the entry to force it.

---

## Entity Relationship Map

How the main Strapi Collection Types relate to each other:

```
+------------------+       +------------------------+
|   App Event      |------>|  Event Email Template  |
|                  |       +------------------------+
|  - EventTitle    |
|  - StartTime     |       +------------------------+
|  - ImageURL      |------>|  Live Stream           |
|  - uuid          |       |                        |
|  - PricingType   |       |  - EventSevas[]        |
|  - isEvent       |       |    - Title             |
|  - EventPlatform |       |    - Price             |
|  - EventLocation |       |  - Schedule[]          |
|  - WebsiteURL    |       |    - Date              |
|  - Content[]     |       |    - StartTime         |
|    - Highlights   |       |    - EndTime           |
|    - Gallery      |       |    - Details           |
|    - Text         |       +------------------------+
+------------------+
         |
         |  uuid links to
         v
+------------------+       +------------------------+
| Webflow Form     |       |  Brevo Mailing List    |
| (Code Embed)     |       |  /rkt-[eventname]      |
| eventUuid=...    |       +------------------------+
+------------------+
         |
         |  (if paid)
         v
+------------------+
| Ticket           |
| - Type           |
| - Name           |
| - Price          |
| - Currency       |
| - Quantity       |
+------------------+
```

---

## Environments

| Environment | Strapi Admin | API Base | Purpose |
|-------------|-------------|----------|---------|
| **Production** | `https://prod-strapi.jkyog.org/admin` | `https://prod-strapi.jkyog.org/api/` | Live content -- public facing |
| **Staging** | `https://jkyog-strapi-staging.up.railway.app/admin` | `https://jkyog-strapi-staging.up.railway.app/api/` | Testing -- safe to experiment |

> **Rule:** Always test changes on Staging before applying to Production. Staging and Production are separate instances with separate databases.

---

## Technology Stack Summary

| Layer | Technology | Version | Purpose |
|-------|-----------|---------|---------|
| CMS | Strapi | 4.10.7 | Headless CMS, REST API generation |
| Runtime | Node.js | 18.20.5 | Server runtime |
| Database | PostgreSQL | Latest | Content storage (production) |
| Database | SQLite | -- | Local development fallback |
| Cache | Redis (ioredis) | 5.7.0 | API response caching (1hr TTL) |
| Media Storage | AWS S3 | -- | Image/file storage |
| CDN | CloudFront | -- | Global media delivery |
| Video | Mux | 2.5.1 plugin | Video transcoding and streaming |
| Search | Meilisearch | 0.10.0 plugin | Full-text content search |
| Monitoring | Sentry | 4.24.4 plugin | Error tracking |
| Website | Webflow | -- | Public site, event pages, registration forms |
| Website | JKYog 2.0 Frontend | -- | JKYog website (consumes REST API) |
| Mobile App | RKT App (iOS/Android) | -- | Mobile content delivery |
| Backend API | NestJS (JKYog 2.0 Backend) | -- | Event sync, content propagation (via webhooks) |
| Email | Brevo (Sendinblue) | -- | Registration confirmations, marketing |
| Hosting | Railway | -- | Staging environment |
| Containerization | Docker (node:20) | -- | Production deployment |
| Version Control | Git | -- | Backend schema and configuration |

> For custom field types and the complete component library, see [Content Model](MODEL.md).

---

## Related Docs

- [Data & API Reference](DATA.md) -- API endpoints, caching, query patterns, webhooks, media
- [Content Model](MODEL.md) -- content types, fields, components, enums
- [Business Rules](BUSINESS.md) -- content domains, user roles, permissions
- [Environment Variables](ENVIRONMENT-VARIABLES.md) -- all config variables
- [API & Backend](API-AND-BACKEND.md) -- how the API works for content editors
- [Creating Events](CREATING-EVENTS.md) -- step-by-step event creation flow
