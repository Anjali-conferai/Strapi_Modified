# JKYOG Strapi CMS -- Documentation

> Internal knowledge base for the JKYOG team. Start here if you are new.

---

## Quick Start

**First day?** Start with the [Onboarding Guide](Onboarding.md) -- it walks you through your entire first week, day by day.

---

## Table of Contents

| # | Document | What It Covers |
|---|----------|---------------|
| 1 | [Getting Started](01-getting-started.md) | What is Strapi, environments, login |
| 2 | [Strapi Overview](02-strapi-overview.md) | Dashboard, sidebar, Collection Types, permissions |
| 3 | [Creating Events](03-creating-events.md) | Full event creation flow (the main workflow) |
| 4 | [Webflow Integration](04-webflow-integration.md) | Registration form setup, eventUuid |
| 5 | [Payment Tickets](05-payment-tickets.md) | Tickets and Promocodes for paid events |
| 6 | [Editing LMS Lessons](06-editing-lms-lessons.md) | App LMS lesson content |
| 7 | [Special Cases](07-special-cases.md) | Bhakti Kirtan Retreat and other edge cases |
| 8 | [Dos and Don'ts](08-dos-and-donts.md) | Permissions, common mistakes, glossary |
| 9 | [API & Backend](09-api-and-backend.md) | API endpoint, data model, data logic, backend architecture |
| -- | **Reference Guides** | |
| 10 | [Architecture](Architecture.md) | System architecture, tech stack, data flow diagrams |
| 11 | [Troubleshooting](Troubleshooting.md) | Symptom-based issue resolution (16 common problems) |
| 12 | [Onboarding](Onboarding.md) | Day-by-day first-week walkthrough for new team members |

---

## Which Guide Do I Need?

| I want to... | Go to |
|-------------|-------|
| Log in for the first time | [Getting Started](01-getting-started.md) |
| Understand the Strapi sidebar and menus | [Strapi Overview](02-strapi-overview.md) |
| Create a new event (satsang, retreat, festival) | [Creating Events](03-creating-events.md) |
| Set up the registration form on the website | [Webflow Integration](04-webflow-integration.md) |
| Add payment/ticket pricing to an event | [Payment Tickets](05-payment-tickets.md) |
| Edit a learning module or lesson | [Editing LMS Lessons](06-editing-lms-lessons.md) |
| Work on the Bhakti Kirtan Retreat page | [Special Cases](07-special-cases.md) |
| Check what I can and cannot do | [Dos and Don'ts](08-dos-and-donts.md) |
| Look up a term I don't understand | [Glossary](08-dos-and-donts.md#glossary) |
| Understand how the API and backend work | [API & Backend](09-api-and-backend.md) |
| Know what `isEvent` or `PricingType` actually do | [API & Backend -- Data Logic](09-api-and-backend.md#data-logic----critical-fields) |
| Understand the full system architecture | [Architecture](Architecture.md) |
| Fix something that's broken | [Troubleshooting](Troubleshooting.md) |
| Onboard as a new team member (day-by-day plan) | [Onboarding](Onboarding.md) |

---

## Environments

| Environment | URL |
|-------------|-----|
| Production | `https://prod-strapi.jkyog.org/admin` |
| Staging | `https://jkyog-strapi-staging.up.railway.app/admin` |

> Always test on **Staging** before making production changes.

---

## Images

All screenshots referenced in the documentation are stored in the [`images/`](images/) folder.
