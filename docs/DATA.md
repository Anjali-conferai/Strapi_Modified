# Data & API Reference

[Back to Index](README.md)

---

## Overview

This is the single reference for how data flows in and out of the Strapi CMS. It covers API configuration, all endpoints, query patterns, caching, webhooks, media loading, search, and permissions. If you need to fetch data from Strapi, start here.

---

## API Configuration

| Setting | Value | Notes |
|---------|-------|-------|
| **Base URL** | `/api` | All endpoints prefixed with this |
| **Default Limit** | 100 records per request | Max records returned in a single API call |
| **Max Limit** | 100 records per request | Cannot be increased via query params |
| **With Count** | `true` | Total count included in responses (useful for pagination) |
| **Populate Depth** | 5 levels | Via `strapi-plugin-populate-deep`. Use `?populate=deep` for full nesting |
| **GraphQL** | Not installed | All content is consumed exclusively via REST |

### Standard REST Operations

All collection types expose:

| Method | Pattern | Purpose |
|--------|---------|---------|
| GET | `/api/{plural-name}` | List entries (paginated) |
| GET | `/api/{plural-name}/:id` | Get single entry |
| POST | `/api/{plural-name}` | Create entry |
| PUT | `/api/{plural-name}/:id` | Update entry |
| DELETE | `/api/{plural-name}/:id` | Delete entry |

Single types expose:

| Method | Pattern | Purpose |
|--------|---------|---------|
| GET | `/api/{singular-name}` | Get the entry |
| PUT | `/api/{singular-name}` | Update the entry |
| DELETE | `/api/{singular-name}` | Delete the entry |

---

## Authentication

| Method | Mechanism | Use Case |
|--------|-----------|----------|
| **API Tokens** | `Authorization: Bearer <token>` | Machine-to-machine API access (frontends, backend sync) |
| **Admin JWT** | `Authorization: Bearer <jwt>` | Admin panel authentication |
| **Users & Permissions** | Session-based | End-user authentication via `/api/auth/local` |

### Users & Permissions Endpoints

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/api/auth/local` | Login with email/password |
| POST | `/api/auth/local/register` | Register new user |
| POST | `/api/auth/forgot-password` | Initiate password reset |
| POST | `/api/auth/reset-password` | Complete password reset |
| GET | `/api/auth/email-confirmation` | Confirm email address |

---

## Public Role Permissions

The **Public** role controls what unauthenticated users (website visitors, app users) can access via the REST API.

**Where to configure:** `Settings > Users and Permissions Plugin > Roles > Public`

![Public Role Permissions](images/WhatsApp%20Image%202026-02-12%20at%2022.21.28.jpeg)

### Content Type Permissions

| Action | Should be enabled? | Purpose |
|--------|-------------------|---------|
| **find** | Yes | Allows listing/querying entries (e.g., `GET /api/appevents`) |
| **findOne** | Yes | Allows fetching a single entry (e.g., `GET /api/appevents/42`) |
| **create** | **No** | Would allow anyone to create entries -- security risk |
| **update** | **No** | Would allow anyone to modify entries -- security risk |
| **delete** | **No** | Would allow anyone to delete entries -- security risk |

### Plugin Permissions

Plugins also have their own permission settings that must be enabled manually:

| Plugin | Permissions to Enable | Why |
|--------|-----------------------|-----|
| **Tickets and Promocodes** | `find`, `findOne` | Frontend needs ticket types and prices for paid events |
| **Upload (Media Library)** | `find`, `findOne` | Frontend needs to resolve media URLs |
| **Other custom plugins** | `find`, `findOne` as needed | Any plugin exposing API routes to the frontend |

> **This is a manual step every time a new content type or plugin is added.** If a content type's permissions aren't enabled, the API returns 403 Forbidden. See [Troubleshooting -- T-18](TROUBLESHOOTING.md#t-18-content-not-loading-on-website--api-returns-403).

---

## Content API Endpoints

### Mobile App Content

| Content Type | REST Endpoint | Key Query Patterns |
|-------------|---------------|-------------------|
| App Events | `/api/appevents` | `?populate=deep` for full event with components, relations |
| App Videos | `/api/appvideos` | `?filters[Language]=en&populate=apptopics` |
| App Audios | `/api/appaudios` | `?filters[Language]=en&populate=apptopics,appcollection` |
| App Articles | `/api/apparticles` | `?filters[Language]=en&populate=apptopics` |
| App Books | `/api/appbooks` | `?populate=apptopics,appchallenge` |
| App Lyrics | `/api/applyrics` | `?filters[Language]=en` |
| App Newsletters | `/api/appnewsletters` | `?populate=apptopics` |
| App Quotes | `/api/appquotes` | `?filters[Language]=en` |
| App Collections | `/api/appcollections` | `?populate=deep` |
| App Courses | `/api/appcourses` | `?populate=deep` |
| App Topics | `/api/apptopics` | `?populate=appvideos,appaudio,apparticles` |
| App Challenges | `/api/appchallenges` | `?populate=appchallengedays` |
| App Challenge Days | `/api/appchallengedays` | `?populate=deep` |
| App Announcements | `/api/appannouncements` | `?populate=deep` |
| App Stories | `/api/appstorycollections` | `?populate=appstoryitems` |
| App Version | `/api/appversions` | Latest version check |
| Home Page Order | `/api/apphomepagecontentorders` | Section ordering |
| App Radio | `/api/appradios` | Stream URLs |

### Event Management

| Content Type | REST Endpoint | Key Query Patterns |
|-------------|---------------|-------------------|
| App Events | `/api/appevents` | `?filters[EventType][$contains]=Featured` |
| Website Events | `/api/events` | `?populate=deep` |
| Featured Events | `/api/featured-events` | Homepage display |
| Retreats | `/api/retreats` | `?populate=deep` |
| Live Streams | `/api/live-streams` | `?populate=Content,Schedule,NavBar` |
| Satsang Centers | `/api/appsatsangcenters` | `?populate=appevents` |

### Online Classes

| Content Type | REST Endpoint | Key Query Patterns |
|-------------|---------------|-------------------|
| Online Classes | `/api/online-classes` | `?populate=online_class_schedules,online_class_categories` |
| Class Count | `/api/online-classes/count-only` | Custom route: total count only |
| Normalized Schedules | `/api/normalized-class-schedules` | UTC-converted schedules |

### Gift Shop (Single Types)

| Content Type | REST Endpoint |
|-------------|---------------|
| Mega Menu | `/api/web-app-mega-menu` |
| Home Banner | `/api/homepage-banner` |
| About Us | `/api/homepage-about-us` |
| Footer | `/api/giftshop-footer-section` |
| Header | `/api/giftshop-header-config` |
| Payment Config | `/api/giftshop-payment-config` |
| Terms of Use | `/api/giftshop-terms-of-use` |
| Privacy Policy | `/api/giftshop-privacy-policy` |
| Cookie Policy | `/api/giftshop-cookie-policy` |
| Copyright | `/api/giftshop-copyright-trademark` |

### Custom API Endpoints

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/get-sevas` | Fetch sevas by IDs (raw SQL for performance) |
| GET | `/api/get-seva-email-templates/:sevaId` | Fetch seva email templates by seva ID |
| GET | `/api/get-signups` | Fetch signups by IDs |
| GET | `/api/online-classes/count-only` | Get total count of online classes |

---

## API Query Patterns

### Populate (Include Relations)

```
# Include all first-level relations
?populate=*

# Deep populate (all levels, up to depth 5)
?populate=deep

# Specific relations only
?populate=LinkedStreams,EventEmailTemplates

# Nested specific relations
?populate=Modules.Lessons.Components
```

### Filtering

```
# Exact match
?filters[Language][$eq]=en

# Contains (case-insensitive)
?filters[EventPlatform][$containsi]=RKT

# Greater than or equal
?filters[StartTime][$gte]=2026-01-01

# OR logic
?filters[$or][0][StartTime][$gte]=2026-01-01&filters[$or][1][EndTime][$gte]=2026-01-01
```

### Sorting

```
# Ascending
?sort=StartTime:asc

# Descending
?sort=createdAt:desc

# Multiple fields
?sort=StartTime:asc,EventTitle:asc
```

### Pagination

```
# Page-based (default)
?pagination[page]=1&pagination[pageSize]=25

# Offset-based
?pagination[start]=0&pagination[limit]=25
```

### The Events API Query (Main Frontend Example)

```
https://prod-strapi.jkyog.org/api/appevents
  ?populate=*
  &sort=StartTime:asc
  &filters[EventPlatform][$containsi]=RKT
  &filters[$or][0][StartTime][$gte]=2026-02-12
  &filters[$or][1][EndTime][$gte]=2026-02-12
```

This query: includes all relations, sorts by soonest first, filters to RKT platform events, and includes events where either start or end date is in the future.

---

## API Caching (Redis)

REST API responses are cached in Redis to improve performance. This has important implications for content editors and frontend developers.

| Setting | Value |
|---------|-------|
| Cache Backend | Redis (`strapi-plugin-rest-cache`) |
| TTL (Time-to-Live) | **3,600 seconds (1 hour)** |
| Max Entries | 32,767 |
| Invalidate on **Create** | **No** |
| Invalidate on **Update** | Yes |
| Invalidate on **Delete** | Yes |
| Max Retries | 3 per request |

### Cache Behavior

| Action | Cache Effect | When Content Appears |
|--------|-------------|---------------------|
| **Create** a new entry and Publish | Cache is **NOT** invalidated | May take **up to 1 hour** to appear |
| **Update** an existing entry | Cache **IS** invalidated | Appears **immediately** |
| **Delete** an entry | Cache **IS** invalidated | Removed **immediately** |

> **Publishing does NOT guarantee immediate visibility.** You may need to **purge the cache** or re-save the entry (edit + publish) to force new entries to appear.

### Cached Content Types

| Content Type | API UID |
|-------------|---------|
| App Version | `api::appversion.appversion` |
| App Topics | `api::apptopic.apptopic` |
| App Quotes | `api::appquote.appquote` |
| App Events | `api::appevent.appevent` |
| App Courses | `api::appcourse.appcourse` |
| App Collections | `api::appcollection.appcollection` |
| App Announcements | `api::appannouncement.appannouncement` |
| App Videos | `api::appvideo.appvideo` |
| App Articles | `api::apparticle.apparticle` |
| App Audios | `api::appaudio.appaudio` |
| App Books | `api::appbook.appbook` |

> Content types not in this list are not cached and always return fresh data.

---

## Webhooks

Webhooks are automated HTTP notifications sent by Strapi to downstream systems when content changes.

### How Webhooks Work

```
Strapi Admin Panel
        |
        |  Content Created / Updated / Deleted / Published
        v
+---------------------+
|   Strapi Webhook     |
|   Engine             |
|   (populateRelations |
|    = false)          |
+----------+----------+
           |
           |  POST request with JSON payload
           |  Authorization: Bearer STRAPI_WEBHOOK_TOKEN
           v
+---------------------+
|  JKYog 2.0 Backend  |
|  (NestJS)           |
|                     |
|  Handles:           |
|  - Event sync       |
|  - Course updates   |
|  - Content          |
|    propagation      |
+---------------------+
```

### Configuration

- **Where to manage:** `Settings > Webhooks` in the Strapi Admin Panel (Super Admin access required)
- **Authentication:** `Authorization: Bearer STRAPI_WEBHOOK_TOKEN`
- **Populate Relations:** Disabled by default (`WEBHOOKS_POPULATE_RELATIONS=false`)

### Webhook Event Types

| Event | Trigger | Example |
|-------|---------|---------|
| `entry.create` | New content entry created | Admin creates a new App Event |
| `entry.update` | Content entry updated | Admin edits an event's title |
| `entry.delete` | Content entry deleted | Admin deletes an event |
| `entry.publish` | Content entry published | Admin clicks Publish |
| `entry.unpublish` | Content entry unpublished | Admin unpublishes a live event |
| `media.create` | Media file uploaded | New banner image uploaded |
| `media.update` | Media file updated | Media metadata changed |
| `media.delete` | Media file deleted | Old image removed |

### Key Entities and Backend Actions

| Content Type | Backend Action |
|-------------|---------------|
| **App Events** | Event sync, notification triggers |
| **App Announcements** | Cross-platform announcement propagation |
| **Live Streams** | Livestream schedule and seva updates |
| **App Articles / Audios / Books** | Content library sync |

> Webhooks notify the backend, but **caching controls the frontend**. Even after a webhook fires, the website may still show stale data until the cache expires or is purged.

---

## Media Loading

### Storage: AWS S3 + CloudFront CDN

| Setting | Value |
|---------|-------|
| Provider | AWS S3 (`@strapi/provider-upload-aws-s3`) |
| Delivery | CloudFront CDN |
| ACL | `public-read` (default) |
| Signed URL Expiry | 900 seconds (15 min) |

Media URLs in API responses follow this pattern:

```
https://{CDN_URL}/{CDN_ROOT_PATH}/{filename}
```

### Video: Mux

For video content (App Story Items), Mux handles upload, transcoding, and streaming:

| URL Type | Pattern |
|----------|---------|
| Stream | `https://stream.mux.com/{playbackId}.m3u8` |
| Thumbnail | `https://image.mux.com/{playbackId}/thumbnail.png` |

### CSP Domains to Whitelist

| Domain | Purpose |
|--------|---------|
| `https://{CDN_URL}` | CloudFront CDN (all images/files) |
| `https://stream.mux.com` | Mux video streaming |
| `https://image.mux.com` | Mux video thumbnails |
| `https://maps.googleapis.com` | Google Maps (venue fields) |

### Media Fields by Content Type

| Content Type | Media Fields | Type |
|-------------|-------------|------|
| `appevent` | BannerURL, ImageURL | External URL strings |
| `event` | banner_image, featured_image, list_page_image | Strapi media |
| `retreat` | banner_image, retreat_benefits_bg, separator_icon | Strapi media |
| `homepage-banner` | imgUrl | Strapi media |
| `appstoryitem` | UploadedVideo (mux-asset) | Mux video |
| `appchallengeday` | downloadable-asset component (File) | Strapi media |

---

## Search (Meilisearch)

Full-text search is powered by Meilisearch, hosted on Railway.

| Setting | Value |
|---------|-------|
| Host | `https://meilisearch-production-3630.up.railway.app/` |
| Plugin | `strapi-plugin-meilisearch` v0.10.0 |

Content types are indexed via the Strapi Admin Panel (`Settings > Meilisearch`). The plugin automatically syncs content on create, update, and delete.

---

## Data Flow Diagrams

### Content Publishing Flow

```
Content Editor (Admin Panel)
        |
        | Create / Update / Publish
        v
   Strapi CMS (v4.10.7)  --->  PostgreSQL (jkyogi DB)
        |
   +----|----------+----------+
   |    |          |          |
   v    v          v          v
Webhook  Redis    Meilisearch  AWS S3
Engine   Cache    Index        (media)
   |     Invalidate  Update
   v
Downstream Consumers:
  - JKYog 2.0 Frontend
  - RKT Dallas 2.0 Frontend
  - JKYog Mobile App
  - JKYog 2.0 Backend (NestJS)
```

### Media Upload Flow

```
Admin User uploads file
        |
        v
Strapi Upload Provider (aws-s3)
        |
        v
AWS S3 Bucket (public-read ACL)
        |
        v
CloudFront CDN (CDN_URL)
        |
   +----|----+
   |         |
   v         v
Web Apps   Mobile App
(img src)  (img URL)
```

---

## Related Docs

- [Content Model](MODEL.md) -- content types, fields, components, enums
- [Business Rules](BUSINESS.md) -- content domains, user roles, permissions
- [Environment Variables](ENVIRONMENT-VARIABLES.md) -- all config variables
- [Architecture](ARCHITECTURE.md) -- system overview, tech stack
- [Troubleshooting](TROUBLESHOOTING.md) -- common issues and fixes
