# Dos and Don'ts

[Back to Index](README.md) | Previous: [Special Cases](07-special-cases.md)

---

## What You CAN Do

| Action | Where |
|--------|-------|
| Create, edit, and publish **App Events** | Content Manager > App Events |
| Create, edit, and publish **Event Email Templates** | Content Manager > Event Email Templates |
| Create, edit, and publish **Live Streams** | Content Manager > Live Streams |
| Edit **App LMS Lessons** | Content Manager > App LMS Lessons |
| Edit **App Articles** | Content Manager > App Articles |
| Edit **App Audios** | Content Manager > App Audios |
| Edit **App Books** | Content Manager > App Books |
| Create **App Announcements** | Content Manager > App Announcements |
| Upload images and files to **Media Library** | Plugins > Media Library |
| Create **Tickets and Promocodes** | Plugins > Tickets and Promocodes |
| Duplicate existing entries for faster setup | Click **Duplicate** on any entry |

---

## What You Should NOT Do

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

---

## Common Mistakes and How to Avoid Them

### 1. Forgetting to update the eventUuid in Webflow

**What happens:** Registrations go to the wrong event.

**Prevention:** After duplicating a Webflow registration form, always update the `eventUuid` on line 25 of the Code Embed. See [Webflow Integration](04-webflow-integration.md).

---

### 2. Not removing the old Email Template when duplicating an event

**What happens:** Confirmation emails reference the wrong event.

**Prevention:** After duplicating an event, immediately delete the old Email Template link and create or assign a new one.

---

### 3. Skipping ticket setup for paid events

**What happens:** Registration form shows **$0** as the price.

**Prevention:** Always create tickets in Tickets and Promocodes for paid events. See [Payment Tickets](05-payment-tickets.md).

---

### 4. Forgetting to Save before Publish

**What happens:** Changes may be lost.

**Prevention:** Always click **Save** first, then **Publish**. Strapi may not always auto-save.

---

### 5. Publishing on Production without testing on Staging

**What happens:** Errors appear on the live website.

**Prevention:** Test all changes on Staging first. Only move to Production once verified.

---

### 6. Wrong image dimensions for event banners

**What happens:** Images appear stretched or cropped incorrectly.

**Prevention:** Always upload event banners at **1024 x 768** pixels to the designated Media Library folder.

---

### 7. Missing or wrong field IDs in Webflow registration forms

**What happens:** Form submissions fail silently.

**Prevention:** Verify all 7 field IDs match exactly: `firstName`, `lastName`, `email`, `city`, `countryCode`, `phone`, `joinwhatsapp`.

---

## Glossary

| Term | Definition |
|------|-----------|
| **App Event** | A Strapi Collection Type that represents an event (satsang, retreat, festival) |
| **Brevo** | Email marketing platform used for sending confirmation and marketing emails |
| **BrevoContactListName** | The mailing list name in Brevo where registrants are added |
| **Code Embed** | A Webflow element that contains custom HTML/JavaScript code |
| **Collection Type** | A category of content in Strapi (like a database table) |
| **Content Manager** | The main section in Strapi where you create and edit content |
| **Content-Type Builder** | Admin tool that modifies the data schema -- DO NOT use |
| **CST** | Central Standard Time -- the default timezone for events |
| **EventPlatform** | Which platform the event belongs to (e.g., RKT for Radha Krishna Temple) |
| **eventUuid** | Unique identifier that connects a Webflow registration form to a Strapi event |
| **EventURLSlug** | URL-friendly version of the event name (e.g., `akshaya-tritiya`) |
| **Gallery** | A content component for displaying multiple images in an event |
| **Headless CMS** | A CMS that stores content and delivers it via APIs (no built-in frontend) |
| **Highlights** | A content component (SelectTab) used to showcase key event features |
| **isEvent** | A boolean field in App Events -- typically set to `false` |
| **JKYOG** | JK Yog -- the parent organization |
| **Live Stream** | A Strapi Collection Type holding sevas (sponsorships) and schedules for an event |
| **LMS** | Learning Management System -- used for structured educational content |
| **Media Library** | The file storage section in Strapi for images and documents |
| **Meilisearch** | A search indexing plugin in Strapi -- admin only |
| **PricingType** | Field that indicates if an event is `free` or `paid` |
| **Production** | The live environment at `https://prod-strapi.jkyog.org/admin` |
| **Promocode** | A discount code for paid event tickets |
| **Publish** | Making content live and visible on the website/app |
| **RKT** | Radha Krishna Temple |
| **Save** | Storing changes without making them public (draft state) |
| **SelectTab** | A content component type used for Highlights sections |
| **Seva** | A sponsorship or service option for an event |
| **Staging** | The test environment at `https://jkyog-strapi-staging.up.railway.app/admin` |
| **UUID** | Universally Unique Identifier -- used to link Strapi events to Webflow forms |
| **Webflow** | The website builder platform where jkyog.org is hosted |

---

## Quick Reference Card

```
SAFE ZONES (You can work here freely):
  - Content Manager > App Events
  - Content Manager > Event Email Templates
  - Content Manager > Live Streams
  - Content Manager > App LMS Lessons
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

[Back to Index](README.md)
