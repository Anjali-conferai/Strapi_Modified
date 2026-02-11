# Event & Live Stream Creation on Strapi -- Steps

> Source: `EventLiveStreamcreationonStrapi.md` (exported from Scribe)
> Note: The original Scribe document contained screenshots at each step, but they were not included in the markdown export. No images were available to extract.

---

## Part 1: Creating / Duplicating an App Event (Steps 1-35)

### Login & Find the Event to Duplicate

| Step | Action |
|------|--------|
| 1 | Navigate to the Strapi App Events page: `https://prod-strapi.jkyog.org/admin/...` |
| 2 | Click **Login** |
| 3 | Click the **"Search for App Events"** field |
| 4 | Type the event name to search (e.g., "West Coast") and press Enter |
| 5 | Click **"Duplicate item line 0"** to duplicate the found event |

### Core Event Fields

| Step | Action |
|------|--------|
| 6 | Enter the **EventTitle** |
| 7 | Enter the **EventSubtitle** |
| 8 | Change the **Event Description** |
| 9 | Change the **ImageURL** (paste the banner image URL from Media Library) |

### Date, Location & Contact

| Step | Action |
|------|--------|
| 10 | Change the **Dates** (start/end) |
| 11 | Change the **Event Location** |
| 12 | Change the **Time Zone** |
| 13 | Change the **uuid** field |
| 14 | Change the **ContactEmail** |
| 15 | Change the **ContactPhoneNumber** |

### Pricing & Venue

| Step | Action |
|------|--------|
| 16 | Change the **PricingType** (free or paid) |
| 17 | Change the **Venue** |

### Content Components

| Step | Action |
|------|--------|
| 18 | Change the **main Text** -- update with event details |
| 19 | Add or remove extra fields under **Content** as per requirements |
| 20 | Add **Highlights**, if applicable |
| 21 | Provide the **Highlighted Text** |
| 22 | Provide details of **accommodation, terms & conditions, and pricing** |
| 23 | Add **past year glimpses** or any **testimonials** |
| 24 | Update the **FAQs** |

### SEO & Marketing Fields

| Step | Action |
|------|--------|
| 25 | Provide the **BrevoContactListName** |
| 26 | Change the **EventURLSlug** |
| 27 | Provide the **PageTitle** |
| 28 | Provide the **MetaDescription** |
| 29 | Provide the **ThankYouMessage** |

### Banner & Additional Details

| Step | Action |
|------|--------|
| 30 | Change the **BannerURL** |
| 31 | Provide **Additional Details** |
| 32 | Provide the **HighlightsTitle** |
| 33 | Provide the **HomepageImage** |
| 34 | Set the **EventPlatforms** |
| 35 | Click **Save** |

---

## Part 2: Creating / Duplicating a Live Stream (Steps 36-61)

### Find & Duplicate a Live Stream

| Step | Action |
|------|--------|
| 36 | Click **Live Stream** in the sidebar |
| 37 | Click **Search** |
| 38 | Type the event name (e.g., "west coast") and press Enter |
| 39 | Click the **"Search for Live Stream"** field |
| 40 | Click **"Duplicate item line 0"** |

### Edit Live Stream Details

| Step | Action |
|------|--------|
| 41 | Change the **Description** field |
| 42 | Change the **Title** |

### HelpDesk Code (Optional Styling)

| Step | Action |
|------|--------|
| 43 | You may change the codes for **HelpDesk** to make it more visually appealing. Use an existing event as a reference (e.g., Dallas Retreat code). |
| 44 | Click on the reference event (e.g., "Dallas Spiritual Retreat 2025") |
| 45 | Click the **copy icon** |
| 46 | **Copy the entire code** |
| 47 | Select the **HTML icon** and **paste** the copied code |
| 48 | Make changes to the **name, title, image address** as per the new event details |
| 49 | Update URLs (e.g., replace the WhatsApp Group URL with the new event's URL) |
| 50 | Preview -- it will look like this on the portal page |

### Schedule

| Step | Action |
|------|--------|
| 51 | Switch to the **Content Manager** tab |
| 52 | Update the **Schedule** |
| 53 | Change the **Date** |
| 54 | Change the **Start and End Time** |
| 55 | Change the **Time Zone** |
| 56 | Reset **Stream** |
| 57 | Change the **Details** (rich text -- activities, speakers, etc.) |
| 58 | Change the **Venue** |

### Sevas & NavBar

| Step | Action |
|------|--------|
| 59 | Add **Sevas** -- Title, Description, and Price |
| 60 | Update **NavBar** -- Title & Link |
| 61 | Click **Save** |

---

## Key Differences from Other Guides

This Scribe guide covers **additional fields** not mentioned in the original event creation docs:

| Field | New in This Guide |
|-------|------------------|
| **Event Description** | Rich text description of the event |
| **Venue** | Physical venue details |
| **PageTitle** | SEO page title |
| **MetaDescription** | SEO meta description |
| **ThankYouMessage** | Post-registration thank-you message |
| **BannerURL** | Separate from ImageURL |
| **HighlightsTitle** | Title for the highlights section |
| **HomepageImage** | Image for homepage display |
| **EventPlatforms** | Which platforms the event appears on |
| **HelpDesk code** | Custom HTML/CSS for the help desk section |
| **NavBar** | Navigation bar entries (Title & Link) |
| **FAQs** | Frequently asked questions section |
| **Accommodation / T&C / Pricing** | Detailed event logistics |
| **Testimonials / Past Year Glimpses** | Social proof content |

---

## Quick Checklist

### App Event
- [ ] Duplicate an existing event (e.g., West Coast)
- [ ] Update EventTitle, EventSubtitle, Description
- [ ] Update ImageURL and BannerURL
- [ ] Update Dates, Location, TimeZone
- [ ] Update uuid, ContactEmail, ContactPhoneNumber
- [ ] Set PricingType and Venue
- [ ] Update Content: main text, Highlights, accommodation, T&C, pricing
- [ ] Add past year glimpses / testimonials
- [ ] Update FAQs
- [ ] Set BrevoContactListName, EventURLSlug
- [ ] Set PageTitle, MetaDescription, ThankYouMessage
- [ ] Set HighlightsTitle, HomepageImage, EventPlatforms
- [ ] Click **Save**

### Live Stream
- [ ] Duplicate an existing Live Stream (e.g., West Coast)
- [ ] Update Title and Description
- [ ] (Optional) Copy and customize HelpDesk code from a reference event
- [ ] Update Schedule: Date, Start/End Time, TimeZone, Details, Venue
- [ ] Reset Stream
- [ ] Add Sevas: Title, Description, Price
- [ ] Update NavBar: Title & Link
- [ ] Click **Save**
