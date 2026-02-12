# Business Rules & Content Domains

[Back to Index](README.md)

---

## Overview

The JKYog Strapi CMS manages content across 8 distinct domains, each powering different parts of the digital ecosystem. This document maps every domain to its frontend features, defines user roles and permissions, and documents the business rules that affect how content appears on the website and app.

---

## Content Domains

### Domain 1: Mobile App Content

The largest content domain, powering the JKYog spiritual learning mobile app with multimedia content.

| Content Type | API ID | Purpose |
|-------------|--------|---------|
| App Announcements | `appannouncement` | Promotional campaigns linking videos, articles, books, events |
| App Videos | `appvideo` | Spiritual video library with language and topic tagging |
| App Audios | `appaudio` | Audio content (kirtans, lectures) with temple deity associations |
| App Articles | `apparticle` | Written spiritual content organized by topics |
| App Books | `appbook` | Digital book catalog with challenge associations |
| App Lyrics | `applyric` | Kirtan and bhajan lyrics |
| App Newsletters | `appnewsletter` | Periodic spiritual newsletters |
| App Quotes | `appquote` | Daily inspirational quotes |
| App Collections | `appcollection` | Curated content bundles (videos, articles, audios, lyrics) |
| App Courses | `appcourse` | Topic-based learning courses |
| App Topics | `apptopic` | Content categorization taxonomy |
| App Stories | `appstorycollection` / `appstoryitem` | Instagram-style story content with Mux video |
| App Radio | `appradio` | Radio stream configuration |
| App Version | `appversion` | Mobile app version management |
| Home Page Order | `apphomepagecontentorder` | Mobile app home screen section ordering |
| Announcement Order | `appannouncementcontentorder` | Announcement section ordering |
| Feature Announcement | `appfeatureannouncement` | App feature highlight announcements |

### Domain 2: Event Management

Full-featured event lifecycle management for spiritual retreats, satsangs, and programs.

| Content Type | API ID | Purpose |
|-------------|--------|---------|
| App Events | `appevent` | Primary event entity (50+ fields) with ticketing, sevas, scheduling |
| Website Events | `event` | Website-facing event pages |
| Featured Events | `featured-event` / `webapp-featured-event` | Homepage-featured event highlights |
| Retreats | `retreat` | Multi-day spiritual retreat pages with FAQ |
| Live Streams | `live-stream` | Event livestream configuration with embedded iframes |
| Event Notifications | `event-notification` | Push notification templates per event |
| Event Email Templates | `event-email-template` | Registration confirmation email templates |
| Coordinator Email Templates | `coordinator-email-template` | Internal coordinator notification emails |
| Satsang Centers | `appsatsangcenter` | Physical venue management with Google Maps integration |

### Domain 3: Online Classes

Live and scheduled class management with timezone-aware scheduling.

| Content Type | API ID | Purpose |
|-------------|--------|---------|
| Online Classes | `online-class` | Class definitions with Zoom integration |
| Class Schedules | `online-class-schedule` | Recurring schedules with day/timezone |
| Normalized Schedules | `normalized-class-schedule` | UTC-converted schedules (auto-synced via cron) |
| Class Categories | `online-class-category` | Class type categorization |
| Class Testimonials | `online-class-testimonial` | Student testimonials |

### Domain 4: Gamification & Challenges

Engagement features to encourage daily spiritual practice.

| Content Type | API ID | Purpose |
|-------------|--------|---------|
| Challenges | `appchallenge` | Multi-day spiritual challenges |
| Challenge Days | `appchallengeday` | Daily challenge content with quizzes, videos, downloadable assets |
| Quizzes | `appquiz` | Standalone quiz content |
| Quiz Activities | `appquizactivity` | Quiz answer tracking structures |

### Domain 5: Gift Shop

E-commerce content for the JKYog spiritual gift shop storefront.

| Content Type | API ID | Purpose |
|-------------|--------|---------|
| GiftShop Mega Menu | `web-app-mega-menu` | Navigation menu structure (Single Type) |
| GiftShop Home Banner | `homepage-banner` | Hero CTA banner (Single Type) |
| GiftShop About Us | `homepage-about-us` | About section content (Single Type) |
| GiftShop Footer | `giftshop-footer-section` | Footer with social links (Single Type) |
| GiftShop Header | `giftshop-header-config` | Logo and header settings (Single Type) |
| GiftShop Payment Config | `giftshop-payment-config` | Google Pay and PayPal settings (Single Type) |
| GiftShop Policies | Various `giftshop-*` IDs | Terms, privacy, cookies, copyright (Single Types) |
| GiftShop Banners | `gift-shop-banner` | Slider banners (Collection) |
| GiftShop Categories | `gift-shop-content` | Featured product categories (Collection) |
| GiftShop Product Lists | Various `gift-shop-home-*` IDs | Curated product lists (Single Types) |
| GiftShop Advertisement | `gift-shop-advertisement-bar` | Coupon code promotion bar (Single Type) |

### Domain 6: Website Content

General-purpose content for the JKYog and RKT Dallas websites.

| Content Type | API ID | Purpose |
|-------------|--------|---------|
| Basic Pages | `basic-page` | Flexible CMS pages with dynamic zone content |
| Blogs | `web-app-blog` | Blog posts |
| FAQs | `web-app-faq` | Frequently asked questions |
| Testimonials | `testimonial` | User testimonials linked to events and pages |
| Carousels | `carousel` / `jkyogcarousel` | Image carousel content |
| Navbar | `navbar` / `navbar-item` | Navigation structure |
| Banners | `banner` / `heading-banner` | Page header banners |
| Posts | `post` | Blog/article posts with CKEditor |

### Domain 7: SMEx (Swamiji Meet Experience)

Content for virtual meetings with Swami Mukundananda.

| Content Type | API ID | Purpose |
|-------------|--------|---------|
| SMEx Configuration | `appsmex` | Global SMEx settings |
| SMEx Meetings | `appsmexmeeting` | Individual meeting instances linked to events |
| SMEx Categories | `appcategory` | Meeting categorization |
| SMEx Series | `appseries` | Multi-session meeting series |
| SMEx Videos | `appindividualvideo` | Individual recorded sessions |
| SMEx Live Sessions | `applivesession` | Live session management |

### Domain 8: Live Streaming

Multi-platform livestream configuration with schedule management (managed through the Live Streams content type under Event Management).

---

## Integration Map

Which frontends consume which content domains:

| Consumer | Domains Consumed | Connection |
|----------|-----------------|-----------|
| **JKYog 2.0 Frontend** | Events, Website Content, Gift Shop, Online Classes | REST API + Webhooks |
| **RKT Dallas 2.0 Frontend** | Events, Live Streaming, Website Content | REST API + Webhooks |
| **JKYog Mobile App** | Mobile App Content, Events, Gamification, SMEx | REST API |
| **JKYog 2.0 Backend (NestJS)** | Events, Mobile App Content | Webhooks (content sync) |

---

## User Roles

### Strapi Admin Panel Roles

| Role | Description | Capabilities |
|------|-------------|--------------|
| **Super Admin** | Full system access | All content CRUD, plugin management, webhook configuration, API tokens, user management, media library, i18n locales, import/export |
| **Editor** | Content management | Create, read, update, and publish content across all types; manage media library; limited plugin access |
| **Author** | Content creation | Create and manage own content; submit for review; limited publishing rights |

### API Consumer Roles

| Role | Description | Access |
|------|-------------|--------|
| **Public** | Unauthenticated API requests | Read access (`find`, `findOne`) to published content. This is what website visitors and app users get. |
| **Authenticated** | Authenticated API requests | Extended read access for user-specific content |

> For details on how Public role permissions work (and why they must be enabled manually), see [Data & API Reference -- Public Role Permissions](DATA.md#public-role-permissions).

---

## Permissions & Safe Zones

### What You CAN Do

| Action | Where |
|--------|-------|
| Create, edit, and publish **App Events** | Content Manager > App Events |
| Create, edit, and publish **Event Email Templates** | Content Manager > Event Email Templates |
| Create, edit, and publish **Live Streams** | Content Manager > Live Streams |
| Edit **App Articles** | Content Manager > App Articles |
| Edit **App Audios** | Content Manager > App Audios |
| Edit **App Books** | Content Manager > App Books |
| Create **App Announcements** | Content Manager > App Announcements |
| Upload images and files to **Media Library** | Plugins > Media Library |
| Create **Tickets and Promocodes** | Plugins > Tickets and Promocodes |
| Duplicate existing entries for faster setup | Click **Duplicate** on any entry |

### What You Should NOT Do

| Action | Why | Consequence |
|--------|-----|------------|
| Edit **Content-Type Builder** | Changes the database schema | Can break all APIs and the website/app |
| Modify **Settings** | User roles, API tokens, webhooks | Security risk, can lock people out |
| Touch **Plugins / Marketplace** | System-level plugin management | Can disable critical features |
| Edit **App Home Page Content Order** | Controls the app homepage layout | Breaks the app homepage for all users |
| Modify **App Version** | App versioning | Can force unwanted app updates |
| Delete or unpublish entries created by others | May be actively used | Breaks live website or app features |
| Change **API tokens** or **webhooks** | Backend infrastructure | Disconnects Strapi from the website/app |

> **Rule of thumb:** If a section is not listed under "What You CAN Do," do not touch it without asking your team lead.

### Quick Reference Card

```
SAFE ZONES (You can work here freely):
  - Content Manager > App Events
  - Content Manager > Event Email Templates
  - Content Manager > Live Streams
  - Content Manager > App Articles / Audios / Books / Announcements
  - Media Library
  - Tickets and Promocodes

DANGER ZONES (Ask before touching):
  - Content-Type Builder
  - Settings
  - Plugins / Marketplace
  - App Home Page Content Order
  - App Version
```

---

## Content Workflow

### Draft and Publish

- **87 of 88 content types** have draft-and-publish enabled
- Content follows a `draft` -> `published` lifecycle
- Only `normalized-class-schedule` skips draft/publish (auto-generated by cron)

| Action | What Happens |
|--------|-------------|
| **Save** | Stores changes as a draft. Not visible on the website/app. |
| **Publish** | Makes the content live. Visible to frontend consumers via the API. |
| **Unpublish** | Reverts published content to draft. Disappears from the API. |

> **Always Save before Publish.** Strapi may not auto-save. Navigating away from an entry discards unsaved changes.

### Localization

- **i18n plugin** is installed with English (`en`) as the default locale
- Language fields on mobile app content types support `en` (English) and `hi` (Hindi) at the field level
- The frontend can filter by language using `filters[Language][$eq]=en`

### Content Sync

- A cron job runs **hourly** to sync `OnlineClassSchedule` entries into `NormalizedClassSchedule` with UTC-converted times
- Full re-sync occurs every **7 days** to prevent drift

---

## Business Rules That Affect the Frontend

These field configurations directly control what appears on the website and app:

| Rule | Field | Effect |
|------|-------|--------|
| Event visibility | `isEvent = true` | Event appears on the Upcoming Events page |
| Event hidden | `isEvent = false` | Event hidden from listings (still accessible via direct URL) |
| Free event | `PricingType = free` | Free badge shown, no payment flow |
| Paid event | `PricingType = paid` | Paid badge shown, ticket flow activated. **Must** configure tickets or price shows $0 |
| Registration enabled | `IsRegistration = true` | Registration button active on the event page |
| Registration disabled | `IsRegistration = false` | Registration button hidden |
| RKT website display | `EventPlatform` includes `RKT` | Event appears on the RKT website |
| JKYog website display | `EventPlatform` includes `JKYog` | Event appears on the JKYog website |
| Cache delay on new entries | -- | New entries may take up to 1 hour to appear due to Redis cache. Purge cache or re-save the entry to force. |

---

## Related Docs

- [Data & API Reference](DATA.md) -- API endpoints, caching, query patterns
- [Content Model](MODEL.md) -- content types, components, field shapes
- [Architecture](ARCHITECTURE.md) -- system overview, tech stack
- [Creating Events](CREATING-EVENTS.md) -- step-by-step event creation
- [Troubleshooting](TROUBLESHOOTING.md) -- common issues and fixes
