# Strapi Overview

[Back to Index](README.md) | Previous: [Getting Started](GETTING-STARTED.md) | Next: [Creating Events](CREATING-EVENTS.md)

---

## The Dashboard Sidebar

After logging in, the left sidebar is your main navigation:

```
STRAPI DASHBOARD
|
|-- Content Manager        <-- Where you create/edit ALL content
|
|-- PLUGINS
|   |-- Content-Type Builder   <-- DO NOT TOUCH (admin only)
|   |-- Media Library          <-- Upload images/files
|   |-- Meilisearch            <-- Search indexing (admin only)
|   |-- Import Export          <-- Bulk data operations
|   |-- Tickets and Promocodes <-- Payment tickets for events
|   |-- Subscription Promocodes
|
|-- GENERAL
|   |-- Plugins               <-- DO NOT TOUCH
|   |-- Marketplace           <-- DO NOT TOUCH
|   |-- Settings              <-- DO NOT TOUCH (admin only)
```

---

## Content Manager

This is where you spend 90% of your time. It contains **39 Collection Types** -- think of each as a category of content (like a database table).

![Content Manager with Collection Types](images/strapi_login2_image3.jpeg)

---

## Collection Types You Will Work With

| Collection Type | What It Does | How Often |
|----------------|--------------|-----------|
| **App Events** | Temple events (satsangs, retreats, festivals) | Frequently |
| **Event Email Templates** | Registration confirmation emails | Per event |
| **Live Streams** | Seva/sponsorship options + schedules | Per event |
| **App Articles** | Blog/article content | Occasionally |
| **App Audios** | Audio content for the app | Occasionally |
| **App Books** | Book listings | Rarely |
| **App Announcements** | In-app notifications | As needed |

---

## Collection Types You Should NOT Touch (Without Permission)

| Collection Type | Why |
|----------------|-----|
| App Home Page Content Order | Controls app homepage layout |
| App Version | App versioning -- critical |
| App SMEx Categories/Series | Structured content -- needs coordination |
| Basic Page | Affects site-wide pages |

---

## Media Library

The Media Library is where you upload and manage images and files. You access it from the **Plugins** section in the sidebar.

Key folder: **Event Banner Image (1024 x 768)** -- this is where all event banner images go.

---

## Tickets and Promocodes

Found under **Plugins** in the sidebar. This is where you create payment tickets for paid events. See [Payment Tickets](PAYMENT-TICKETS.md) for the full workflow.

---

## Settings You Should Ignore

The following are admin-only sections. Do not modify them:

- **Content-Type Builder** -- changes the database schema, can break APIs
- **Settings** -- user roles, API tokens, webhooks
- **Plugins / Marketplace** -- system-level plugin management

---

## Next Steps

- [Creating Events](CREATING-EVENTS.md) -- the main workflow you will use most
- [Business Rules](BUSINESS.md) -- full list of what you can and cannot do
