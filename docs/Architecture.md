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
|  | - LMS Lessons    |  | - Thumbnails     |  | - Meilisearch  |  |
|  | - Articles       |  |                  |  |                |  |
|  | - Audios/Books   |  |                  |  |                |  |
|  | - Announcements  |  |                  |  |                |  |
|  +------------------+  +------------------+  +----------------+  |
|                                                                  |
|  +------------------------------------------------------------+  |
|  |                    REST API (Auto-Generated)                |  |
|  |  Base: https://prod-strapi.jkyog.org/api/                  |  |
|  |  Endpoints: /appevents, /livestreams, /applmslessons, ...   |  |
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
  | - Registration|    | - LMS lessons |    |                 |
  |   forms       |    | - Articles    |    |                 |
  | - Seva pages  |    | - Audios      |    |                 |
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
| **Technology** | Node.js headless CMS |
| **Production URL** | `https://prod-strapi.jkyog.org/admin` |
| **Staging URL** | `https://jkyog-strapi-staging.up.railway.app/admin` |
| **Hosting** | Railway (staging), dedicated server (production) |
| **Database** | Stores all Collection Type entries, relationships, and media metadata |

**Key characteristic:** The backend is defined by **JSON configuration files** in the Git repository, not by procedural code. These files define field types, constraints, validation rules, and entity relationships.

### 2. Webflow (The Website)

| Aspect | Detail |
|--------|--------|
| **Role** | Public-facing website for events, registration, and content |
| **URL** | `https://www.radhakrishnatemple.net` |
| **Connection to Strapi** | Frontend JavaScript fetches data from Strapi REST API |
| **Registration Forms** | Embedded forms with Code Embed elements containing `eventUuid` |
| **Key Integration Point** | `eventUuid` on line 25 of Code Embed links the form to a Strapi event |

### 3. RKT Mobile App

| Aspect | Detail |
|--------|--------|
| **Role** | Mobile app for events, LMS lessons, articles, audios, announcements |
| **Platform** | iOS and Android |
| **Connection to Strapi** | Fetches data via the same REST API, filtered by `EventPlatform = RKT` |
| **Content Types Used** | App Events, App LMS Lessons, App Articles, App Audios, App Books, App Announcements |

### 4. Brevo (Email Marketing)

| Aspect | Detail |
|--------|--------|
| **Role** | Sends registration confirmation and marketing emails |
| **Connection to Strapi** | `BrevoContactListName` field in App Events maps registrants to Brevo mailing lists |
| **Format** | List names follow the pattern `/rkt-[eventname]` |

### 5. Payment Gateway

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
User's Browser                  Webflow Frontend                 Strapi API
     |                               |                               |
     |  1. Opens event page          |                               |
     |------------------------------>|                               |
     |                               |  2. Fetch events               |
     |                               |  GET /api/appevents            |
     |                               |    ?populate=*                 |
     |                               |    &sort=StartTime:asc         |
     |                               |    &filters[EventPlatform]     |
     |                               |      [$containsi]=RKT          |
     |                               |    &filters[$or][StartTime]    |
     |                               |      [$gte]=now                |
     |                               |------------------------------>|
     |                               |                               |
     |                               |  3. JSON response with all    |
     |                               |     event data + relations    |
     |                               |<------------------------------|
     |                               |                               |
     |  4. Rendered event cards       |                               |
     |  (title, date, image,         |                               |
     |   free/paid badge, location)  |                               |
     |<------------------------------|                               |
```

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

| Layer | Technology | Purpose |
|-------|-----------|---------|
| CMS | Strapi (Node.js) | Content management, API generation |
| Website | Webflow | Public site, event pages, registration forms |
| Mobile App | RKT App (iOS/Android) | Mobile content delivery |
| Email | Brevo | Registration confirmations, marketing |
| Search | Meilisearch | Content indexing (admin only) |
| Hosting | Railway | Staging environment hosting |
| Version Control | Git | Backend schema and configuration |
| Database | PostgreSQL/SQLite | Content storage (managed by Strapi) |

---

## Related Docs

- [API & Backend](09-api-and-backend.md) -- deep dive into the API endpoint, query parameters, and data model
- [Strapi Overview](02-strapi-overview.md) -- sidebar navigation and Collection Types
- [Creating Events](03-creating-events.md) -- step-by-step event creation flow
- [Webflow Integration](04-webflow-integration.md) -- how Webflow connects to Strapi via eventUuid
