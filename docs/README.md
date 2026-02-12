# JKYOG Strapi CMS -- Documentation

> Internal knowledge base for the JKYOG team. Start here if you are new.

---

## Quick Start

**First day?** Start with the [Onboarding Guide](ONBOARDING.md) -- it walks you through your entire first week, day by day.

---

## Table of Contents

| # | Document | What It Covers |
|---|----------|---------------|
| 1 | [Getting Started](GETTING-STARTED.md) | What is Strapi, environments, login |
| 2 | [Strapi Overview](STRAPI-OVERVIEW.md) | Dashboard, sidebar, Collection Types |
| 3 | [Creating Events](CREATING-EVENTS.md) | Full event creation flow (the main workflow) |
| 4 | [Webflow Integration](WEBFLOW-INTEGRATION.md) | Registration form setup, eventUuid |
| 5 | [Payment Tickets](PAYMENT-TICKETS.md) | Tickets and Promocodes for paid events |
| 6 | [Special Cases](SPECIAL-CASES.md) | Bhakti Kirtan Retreat and other edge cases |
| 7 | [API & Backend](API-AND-BACKEND.md) | API endpoint, data logic, backend architecture |
| -- | **Reference Guides** | |
| 8 | [Data & API Reference](DATA.md) | All API endpoints, query patterns, caching, webhooks, media |
| 9 | [Content Model](MODEL.md) | Content types, fields, components, enums, glossary |
| 10 | [Business Rules](BUSINESS.md) | Content domains, user roles, permissions, safe zones |
| 11 | [Environment Variables](ENVIRONMENT-VARIABLES.md) | All environment and config variables |
| 12 | [Architecture](ARCHITECTURE.md) | System architecture, tech stack, data flow diagrams |
| 13 | [Troubleshooting](TROUBLESHOOTING.md) | Symptom-based issue resolution (19 common problems) |
| 14 | [Onboarding](ONBOARDING.md) | Day-by-day first-week walkthrough for new team members |

---

## Which Guide Do I Need?

| I want to... | Go to |
|-------------|-------|
| Log in for the first time | [Getting Started](GETTING-STARTED.md) |
| Understand the Strapi sidebar and menus | [Strapi Overview](STRAPI-OVERVIEW.md) |
| Create a new event (satsang, retreat, festival) | [Creating Events](CREATING-EVENTS.md) |
| Set up the registration form on the website | [Webflow Integration](WEBFLOW-INTEGRATION.md) |
| Add payment/ticket pricing to an event | [Payment Tickets](PAYMENT-TICKETS.md) |
| Work on the Bhakti Kirtan Retreat page | [Special Cases](SPECIAL-CASES.md) |
| Check what I can and cannot do | [Business Rules -- Permissions](BUSINESS.md#permissions--safe-zones) |
| Look up a term I don't understand | [Glossary](MODEL.md#glossary) |
| Find an API endpoint or query pattern | [Data & API Reference](DATA.md) |
| See all fields for a content type | [Content Model](MODEL.md) |
| Understand how the API and backend work | [API & Backend](API-AND-BACKEND.md) |
| Know what `isEvent` or `PricingType` actually do | [API & Backend -- Data Logic](API-AND-BACKEND.md#data-logic----critical-fields) |
| Understand the full system architecture | [Architecture](ARCHITECTURE.md) |
| Check environment variables or URLs | [Environment Variables](ENVIRONMENT-VARIABLES.md) |
| Fix something that's broken | [Troubleshooting](TROUBLESHOOTING.md) |
| Onboard as a new team member (day-by-day plan) | [Onboarding](ONBOARDING.md) |

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
