# Getting Started

[Back to Index](README.md) | Next: [Strapi Overview](02-strapi-overview.md)

---

## What is Strapi?

Strapi is the **headless CMS** (Content Management System) used by JKYOG (JK Yog / Radha Krishna Temple) to manage content for:

- The **JKYOG website** (built on Webflow)
- The **RKT mobile app**
- **Event registration pages**
- **LMS (Learning Management System) lessons**

Content created in Strapi gets consumed by the website and app via APIs. You do NOT code anything in Strapi -- you manage content through its admin panel.

---

## Environments

There are two Strapi instances. Always know which one you are working in.

| Environment | URL | Use For |
|-------------|-----|---------|
| **Production** | `https://prod-strapi.jkyog.org/admin` | Live content -- changes go public immediately |
| **Staging** | `https://jkyog-strapi-staging.up.railway.app/admin` | Testing -- safe to experiment |

> **Rule:** Always test on **Staging** first before making production changes.

---

## Logging In

1. Open the Strapi admin URL in your browser.
2. Enter your **Email** and **Password**.
3. Click **Login**.

![Login Screen](images/strapi_login2_image1.jpeg)

> You will receive credentials from your team lead. Do NOT share them.

---

## What You See After Login

After logging in, you land on the **Strapi Dashboard**:

![Dashboard](images/strapi_login2_image2.jpeg)

The left sidebar is your main navigation. It contains:

- **Content Manager** -- where you create and edit all content
- **Plugins** -- Media Library, Tickets, Import/Export, etc.
- **General** -- system settings (admin only)

For a detailed breakdown of every sidebar item and what it does, continue to [Strapi Overview](02-strapi-overview.md).

---

## Next Steps

- [Strapi Overview](02-strapi-overview.md) -- understand the dashboard and Collection Types
- [Creating Events](03-creating-events.md) -- jump straight into the main workflow
