# API Integration & Backend Architecture

[Back to Index](README.md) | Previous: [Special Cases](SPECIAL-CASES.md)

---

## Why This Matters

Every field you fill in the Strapi dashboard directly controls what appears on the website and app. Strapi is a **headless CMS** -- it has no frontend of its own. Instead, the frontend (radhakrishnatemple.net) fetches data from Strapi's API in real-time. If you set a field incorrectly, it shows up incorrectly on the live site instantly.

This document explains how that connection works.

---

## How It All Connects

```
+---------------------+       +-------------------+       +-------------------+
|   BACKEND (Git)     |       |   STRAPI ADMIN    |       |   FRONTEND        |
|                     |       |   (Dashboard)     |       |   (Website/App)   |
|  JSON schema files  | ----> |  UI forms based   | ----> |  API queries       |
|  define fields,     |       |  on those schemas  |       |  fetch & display  |
|  types, constraints |       |  you fill in data  |       |  the data live    |
+---------------------+       +-------------------+       +-------------------+
     DEFINITION                   MANAGEMENT                 CONSUMPTION
```

| Stage | Who | What Happens |
|-------|-----|-------------|
| **Definition** | Developers | Define permitted values, field types, and constraints in JSON schema files within the Strapi Git repository |
| **Management** | You (Admin) | Enter actual content (text, images, dates) via the Strapi dashboard based on those definitions |
| **Consumption** | Frontend code | Queries the API, receiving data in the exact format prescribed by the backend JSON schemas |

> **Key insight:** The backend is fundamentally defined by **configuration files** rather than complex procedural code. These JSON files specify data types, constraints, unique values, and mandatory requirements for every field.

---

## API Configuration & Authentication

> For complete API configuration, authentication methods, and Public role permissions, see the [Data & API Reference](DATA.md).

---

## The Events API Endpoint

The frontend retrieves event data from this endpoint:

```
https://prod-strapi.jkyog.org/api/appevents?populate=*&sort=StartTime:asc&filters[EventPlatform][$containsi]=RKT
```

### Query Parameters Explained

| Parameter | Value | What It Does |
|-----------|-------|-------------|
| `populate` | `*` | Includes **all relational data** (Venues, Live Streams, etc.) in the response -- without this, you only get IDs |
| `sort` | `StartTime:asc` | Organizes events **chronologically**, soonest first |
| `filters[EventPlatform][$containsi]` | `RKT` | Restricts results to events tagged with the **RKT** (Radha Krishna Temple) platform |
| Date filters | `$or` logic | Includes events where **either** `StartTime` or `EndTime` >= current date (ensures multi-day events still show during their run) |

### What This Means for You

- When you set **EventPlatform** to `RKT` in the dashboard, the event will appear on the RKT website.
- When you set **StartTime**, you are controlling the **sort order** on the Upcoming Events page.
- If you leave **EventPlatform** empty or set it to something other than `RKT`, the event will **not** appear on the RKT website.

---

## Data Logic -- Critical Fields

Two fields in the App Event form have outsized impact on how events appear on the frontend. Getting these wrong is one of the most common mistakes.

### `isEvent` (Boolean)

| Value | Effect on Frontend |
|-------|-------------------|
| **`true`** | The event appears on the **Upcoming Events** page |
| **`false`** | The event is **hidden** from the Upcoming Events page (still accessible via direct URL) |

> **When to use:** Set `isEvent = true` for events you want publicly listed. Set it to `false` for events that should only be accessible via a direct link (e.g., internal events, or events not yet ready for public visibility).

### `PricingType` (String)

| Value | Effect on Frontend |
|-------|-------------------|
| **`free`** | Event displays the **"Free"** badge; no payment flow |
| **`paid`** | Event displays the **"Paid"** badge; payment/ticket flow is activated |

> **When to use:** Always set this accurately. If an event is paid but `PricingType` is set to `free`, users will not see pricing or be prompted to pay. If tickets are not configured in the Tickets and Promocodes plugin, a paid event will show **$0**. See [Payment Tickets](PAYMENT-TICKETS.md).

### How These Fields Work Together

```
                Is it a real event?
                       |
                  isEvent = true?
                  /            \
                YES              NO
                |                |
        Shows on            Hidden from
      Upcoming Events     Upcoming Events
                |
          PricingType?
          /          \
       "free"       "paid"
         |             |
    Free badge     Paid badge
    No payment     Ticket flow
                   activated
```

---

## Data Model -- EventAttributes

When the frontend queries the API, it receives the following key fields for each event:

| Field | Type | Description | Set In Dashboard |
|-------|------|-------------|-----------------|
| **EventTitle** | String | The primary name of the event | EventTitle field |
| **StartTime** | ISO Date | The date and time the event begins | StartTime field |
| **EndTime** | ISO Date (nullable) | The date and time the event ends | EndTime field |
| **IsEvent** | Boolean | Determines if the item appears on the Upcoming Events page | isEvent toggle |
| **IsRegistration** | Boolean | Controls whether the registration/link button is active | IsRegistration toggle |
| **EventLocation** | Enum | Values: `Online`, `In-person`, `Both` | EventLocation dropdown |
| **PricingType** | Enum | Determines if event is `free` or `paid`. Controls pricing badge and payment flow | PricingType dropdown |
| **EventType** | Multi-select | Event classification(s): `Retreat`, `Featured`, `MeetSwamiji`, `LifeTransformation`, `LifeTransformation2`, `LifeTransformation3`, `General`, `Yuth`, `SATSANG` | EventType dropdown |
| **EventPlatform** | Multi-select | Which platform(s) show this event: `JKYog`, `RKT`, `Krishna Bhagavad Gita` | EventPlatform dropdown |
| **uuid** | String | Unique identifier used by Webflow registration forms | Auto-generated |
| **ImageURL** | String | Banner image URL | Paste from Media Library |
| **WebsiteURL** | String | Direct link to the event page | WebsiteURL field |
| **EventURLSlug** | String | URL-friendly event name | EventURLSlug field |
| **ContactEmail** | String | Contact email displayed on the event page | ContactEmail field |
| **ContactPhoneNumber** | String | Contact phone displayed on the event page | ContactPhoneNumber field |

> Every field you fill in the Strapi dashboard maps directly to one of these API response fields. The frontend reads them and renders the page accordingly -- there is no manual step in between.

---

## Backend Architecture (For Developers)

> This section is for team members who need to understand the Git repository structure. If you only work in the Strapi dashboard, you can skip this.

### JSON-Based Configuration

The Strapi Git repository is primarily composed of **JSON files** that define the structure of Content Types. There is very little procedural code -- the backend is driven by configuration.

### What the JSON Files Define

| Concern | How It's Handled |
|---------|-----------------|
| **Data Validation** | Field types and constraints -- enforcing unique values, restricting inputs to numbers or text, setting mandatory requirements |
| **Relationship Mapping** | Entity relationships (e.g., linking an Event to a Venue, an Event to a Live Stream) defined in schemas, allowing the API to return complex nested data |
| **API Endpoints** | Strapi auto-generates REST API endpoints for each Content Type. Creating a new Collection Type automatically creates its `/api/` endpoint |
| **Permissions** | Which roles can read/write which Content Types |

### Auto-Generated Endpoints

When a developer creates a new Collection Type (e.g., `AppEvent`) in the Strapi schema, the system automatically generates:

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/appevents` | GET | List all events (with optional filters, sorting, pagination) |
| `/api/appevents/:id` | GET | Get a single event by ID |
| `/api/appevents` | POST | Create a new event |
| `/api/appevents/:id` | PUT | Update an existing event |
| `/api/appevents/:id` | DELETE | Delete an event |

> **You do not call these endpoints directly.** The frontend code handles all API calls. Your job is to ensure the data in Strapi is correct -- the API delivers it automatically.

---

## API Caching & Real-Time Sync

> For the complete caching reference (Redis config, TTL, cached content types, purge guidelines), see the [Data & API Reference -- Caching](DATA.md#api-caching-redis).

**Key points for content editors:**

- **Updates and deletes** invalidate the cache immediately.
- **New entries** do NOT invalidate the cache -- they may take up to 1 hour to appear. **Purge the cache** or re-save the entry to force it.
- Publishing does **not** guarantee immediate visibility due to caching.

---

## Custom API Endpoints & Enumeration Values

> For the full list of custom endpoints, see [Data & API Reference -- Custom API Endpoints](DATA.md#custom-api-endpoints).

> For all enumeration values (Language, TimeZone, EventLocation, PricingType, EventType, EventPlatform, etc.), see [Content Model -- Enumeration Values](MODEL.md#enumeration-values).

---

## Webhooks

> For the complete webhook reference (event types, configuration, entity-backend mapping), see [Data & API Reference -- Webhooks](DATA.md#webhooks).

**Key points for content editors:**

- Webhooks fire **automatically** on every create, update, delete, publish, and unpublish.
- They notify the NestJS backend to sync content -- you don't need to do anything.
- Webhooks are **separate from caching** -- the backend gets notified instantly, but the frontend may still serve cached data.
- **Do NOT modify webhook settings** unless you are a Super Admin.

---

## Media Storage

> For complete media configuration (S3, CloudFront, Mux, CSP domains), see [Data & API Reference -- Media Loading](DATA.md#media-loading).

All media is stored on **AWS S3** and served via **CloudFront CDN**. Videos are handled by **Mux**.

---

## Quick Reference

| I want to... | The field/setting that controls it |
|-------------|-----------------------------------|
| Show an event on the Upcoming Events page | `isEvent = true` |
| Hide an event from public listing | `isEvent = false` |
| Mark an event as free | `PricingType = free` |
| Mark an event as paid | `PricingType = paid` + set up tickets |
| Show an event on the RKT website | `EventPlatform` contains `RKT` |
| Enable the registration button | `IsRegistration = true` |
| Control event order on the page | `StartTime` (sorted ascending) |
| Include venue details in the API response | Populate the Venue relation |

---

## Related Docs

- [Data & API Reference](DATA.md) -- complete API endpoints, caching, query patterns, webhooks, media
- [Content Model](MODEL.md) -- content types, fields, components, enums
- [Business Rules](BUSINESS.md) -- permissions, safe zones, content domains
- [Creating Events](CREATING-EVENTS.md) -- step-by-step event creation in the dashboard
- [Payment Tickets](PAYMENT-TICKETS.md) -- setting up tickets for paid events
- [Webflow Integration](WEBFLOW-INTEGRATION.md) -- connecting the registration form via eventUuid
