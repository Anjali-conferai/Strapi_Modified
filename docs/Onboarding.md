# Onboarding Guide

[Back to Index](README.md)

> **Welcome to the JKYOG team!** This guide walks you through your first week working with Strapi. Follow it step by step -- by the end, you'll be confident creating events, managing content, and understanding how everything connects.

---

## Before You Start

You will need:
- [ ] Strapi login credentials (email + password) -- get these from your team lead
- [ ] Access to the Webflow Editor (if you'll be managing registration forms)
- [ ] This documentation bookmarked

---

## Day 1: Login & Explore

### Goal: Get oriented with the Strapi dashboard

**Time estimate:** 30 minutes

#### Step 1: Log into Staging (NOT Production)

Start with the **Staging** environment so you can explore safely:

| Environment | URL |
|-------------|-----|
| **Staging** (start here) | `https://jkyog-strapi-staging.up.railway.app/admin` |
| Production (later) | `https://prod-strapi.jkyog.org/admin` |

1. Open the Staging URL
2. Enter your email and password
3. Click **Login**

![Login Screen](images/strapi_login2_image1.jpeg)

**Detailed guide:** [Getting Started](01-getting-started.md)

#### Step 2: Explore the Sidebar

After logging in, spend 10 minutes clicking through the sidebar:

![Dashboard](images/strapi_login2_image2.jpeg)

| Section | What to Do |
|---------|-----------|
| **Content Manager** | Click it. Browse the Collection Types list on the left. |
| **Media Library** | Click it. Look at the folder structure, especially "Event Banner Image (1024 x 768)". |
| **Tickets and Promocodes** | Click it. See how tickets are organized by event. |

> **Do NOT click** Content-Type Builder, Settings, Plugins, or Marketplace. These are admin-only.

**Detailed guide:** [Strapi Overview](02-strapi-overview.md)

#### Step 3: Browse Existing Content

1. Go to **Content Manager > App Events**
2. Click on any published event (look for the green "Published" badge)
3. Scroll through all the fields -- notice EventTitle, ImageURL, StartTime, PricingType, uuid, Content components, LinkedStreams, Email Template

This is what you'll be creating soon.

![Content Manager with Collection Types](images/strapi_login2_image3.jpeg)

#### Day 1 Checkpoint

- [ ] I can log into Staging
- [ ] I know where Content Manager, Media Library, and Tickets are in the sidebar
- [ ] I've opened an existing App Event and browsed its fields
- [ ] I understand which sidebar sections I should NOT touch

---

## Day 2: Understand the System

### Goal: Learn how Strapi connects to the website and app

**Time estimate:** 20 minutes

#### Read These Two Documents

1. **[Architecture](Architecture.md)** -- the big picture of how Strapi, Webflow, the mobile app, and Brevo all connect
2. **[API & Backend](09-api-and-backend.md)** -- how the API works and which fields control what

#### Key Concepts to Understand

| Concept | Why It Matters |
|---------|---------------|
| Strapi is a **headless CMS** | It has no frontend -- the website and app fetch data via API |
| **Publishing = Live** | The moment you click Publish, the content goes live. No deploy step. |
| `isEvent` controls visibility | `true` = shows on Upcoming Events; `false` = hidden |
| `PricingType` controls pricing | `free` or `paid` -- must match ticket setup |
| `EventPlatform` must be `RKT` | Otherwise the event won't appear on the RKT website |
| `eventUuid` links Webflow to Strapi | The registration form uses this to know which event to register for |

#### Day 2 Checkpoint

- [ ] I understand the flow: Strapi -> API -> Website/App
- [ ] I know what `isEvent`, `PricingType`, `EventPlatform`, and `eventUuid` do
- [ ] I understand the difference between Save (draft) and Publish (live)

---

## Day 3: Create Your First Event (on Staging)

### Goal: Go through the full event creation flow on Staging

**Time estimate:** 45-60 minutes

Follow the [Creating Events](03-creating-events.md) guide step by step, using **Staging**. Here's the high-level flow:

```
Upload Banner ──> Create App Event ──> Add Content ──> Create Email Template
                                                              |
                                                              v
            Save & Publish <── Link Everything <── Create Live Stream
```

#### Practice Checklist

- [ ] Upload a test image to Media Library (1024 x 768)
- [ ] Copy the image URL using the chain link icon
- [ ] Create a new App Event (or duplicate an existing one)
- [ ] Fill in: EventTitle, EventSubtitle, ImageURL, StartTime
- [ ] Set: `isEvent = true`, `EventPlatform = RKT`, `PricingType = free`
- [ ] Copy the **UUID** (practice this -- you'll need it every time)
- [ ] Add a Highlights component under Content
- [ ] Add a Gallery component
- [ ] Create or duplicate an Email Template
- [ ] Create or duplicate a Live Stream with at least one Seva
- [ ] Link the Live Stream and Email Template to the event
- [ ] **Save** first, then **Publish**

> **This is on Staging** -- you can make mistakes safely. Try things out.

#### Day 3 Checkpoint

- [ ] I've created an event end-to-end on Staging
- [ ] I know how to upload images, copy URLs, and copy UUIDs
- [ ] I understand the difference between Highlights (SelectTab) and Gallery
- [ ] I've linked a Live Stream and Email Template to an event

---

## Day 4: Learn the Supporting Workflows

### Goal: Understand tickets, Webflow integration, and LMS lessons

**Time estimate:** 30 minutes

#### Read and Practice

| Task | Guide | Practice On Staging |
|------|-------|-------------------|
| Set up payment tickets | [Payment Tickets](05-payment-tickets.md) | Create a test ticket for your Day 3 event |
| Webflow registration form | [Webflow Integration](04-webflow-integration.md) | Read the guide -- you'll practice this with your lead |
| Edit an LMS lesson | [Editing LMS Lessons](06-editing-lms-lessons.md) | Edit a test lesson title on Staging |

#### Day 4 Checkpoint

- [ ] I know where Tickets and Promocodes is and how to add a ticket
- [ ] I understand the eventUuid flow (even if I haven't done it in Webflow yet)
- [ ] I've edited an LMS lesson on Staging (Save + Publish)

---

## Day 5: Learn the Rules & Edge Cases

### Goal: Know what to do and what to avoid

**Time estimate:** 20 minutes

#### Read These Documents

1. **[Dos and Don'ts](08-dos-and-donts.md)** -- permissions, common mistakes, and the glossary
2. **[Special Cases](07-special-cases.md)** -- how Bhakti Kirtan Retreat works differently
3. **[Troubleshooting](Troubleshooting.md)** -- bookmark this for when things go wrong

#### The Golden Rules

| Rule | Why |
|------|-----|
| **Always Save before Publish** | Unsaved changes are lost |
| **Always Save before switching tabs** | Navigating away discards unsaved work |
| **Always copy the UUID** | Registration forms need it |
| **Always set up tickets for paid events** | Otherwise the price shows as $0 |
| **Always update eventUuid when duplicating** | Old UUID = registrations go to wrong event |
| **Always remove old Email Template when duplicating** | Old template = wrong confirmation email |
| **Never touch Content-Type Builder or Settings** | Can break the entire system |
| **Test on Staging first** | Production changes are immediately live |

#### Day 5 Checkpoint

- [ ] I know the Safe Zones and Danger Zones
- [ ] I've read the common mistakes and understand the consequences
- [ ] I know where to find the Troubleshooting guide
- [ ] I understand the Bhakti Kirtan Retreat exception

---

## Week 2+: Production Work

Once your team lead gives you the go-ahead:

1. **Log into Production** at `https://prod-strapi.jkyog.org/admin`
2. Start with **low-risk tasks** (editing existing events, updating LMS lessons)
3. Graduate to **creating new events** under supervision
4. Eventually handle the **full flow independently** (Strapi + Webflow + Tickets)

---

## Quick Reference Card

```
DAILY WORKFLOW:
  1. Log in to the correct environment (Staging or Production)
  2. Navigate to Content Manager
  3. Create/edit the content entry
  4. SAVE first
  5. PUBLISH second
  6. Verify on the website/app

AFTER DUPLICATING AN EVENT:
  1. Update ALL fields (title, dates, images, URLs)
  2. Remove old Email Template -> assign new one
  3. Remove old LinkedStreams -> assign new Live Stream
  4. Copy the new UUID
  5. Update eventUuid in Webflow Code Embed (line 25)

SOMETHING BROKE?
  -> Check Troubleshooting.md
  -> Screenshot the issue
  -> Contact your team lead
```

---

## All Documentation Links

| Document | What It Covers |
|----------|---------------|
| [Getting Started](01-getting-started.md) | Login, environments, first steps |
| [Strapi Overview](02-strapi-overview.md) | Dashboard, sidebar, Collection Types |
| [Creating Events](03-creating-events.md) | Full event creation workflow |
| [Webflow Integration](04-webflow-integration.md) | Registration forms and eventUuid |
| [Payment Tickets](05-payment-tickets.md) | Tickets and Promocodes |
| [Editing LMS Lessons](06-editing-lms-lessons.md) | LMS lesson content |
| [Special Cases](07-special-cases.md) | Bhakti Kirtan Retreat |
| [Dos and Don'ts](08-dos-and-donts.md) | Permissions, mistakes, glossary |
| [API & Backend](09-api-and-backend.md) | API endpoint, data model, backend |
| [Architecture](Architecture.md) | System architecture and tech stack |
| [Troubleshooting](Troubleshooting.md) | Issue resolution guide |
