# Content Model Reference

[Back to Index](README.md)

---

## Overview

The JKYog Strapi CMS has **88 content types** (15 Single Types + 73 Collection Types) and **39 reusable components** across 8 categories. This document is the single reference for understanding the data model -- what you can fetch, what fields each type has, and how types relate to each other.

---

## Content Type Inventory

### Single Types (15)

Single Types have one entry each. They power configuration and layout:

| Content Type | API ID | Domain |
|-------------|--------|--------|
| GiftShop Mega Menu | `web-app-mega-menu` | Gift Shop |
| GiftShop Home Banner | `homepage-banner` | Gift Shop |
| GiftShop About Us | `homepage-about-us` | Gift Shop |
| GiftShop Footer | `giftshop-footer-section` | Gift Shop |
| GiftShop Header | `giftshop-header-config` | Gift Shop |
| GiftShop Payment Config | `giftshop-payment-config` | Gift Shop |
| GiftShop Terms of Use | `giftshop-terms-of-use` | Gift Shop |
| GiftShop Privacy Policy | `giftshop-privacy-policy` | Gift Shop |
| GiftShop Cookie Policy | `giftshop-cookie-policy` | Gift Shop |
| GiftShop Copyright | `giftshop-copyright-trademark` | Gift Shop |
| GiftShop Product Lists | `gift-shop-home-trending-item`, etc. | Gift Shop |
| GiftShop Sitemap | `gift-shop-sitemap-page` | Gift Shop |
| GiftShop Advertisement | `gift-shop-advertisement-bar` | Gift Shop |
| App Home Page Order | `apphomepagecontentorder` | Mobile App |
| SMEx Configuration | `appsmex` | SMEx |

### Collection Types (73) -- Key Types by Domain

#### Mobile App (17 types)

| Content Type | API ID | Relations |
|-------------|--------|-----------|
| App Events | `appevent` | 14+ relations (see deep dive below) |
| App Videos | `appvideo` | Topics, Collections |
| App Audios | `appaudio` | Topics, Collections |
| App Articles | `apparticle` | Topics |
| App Books | `appbook` | Topics, Challenges |
| App Lyrics | `applyric` | Events |
| App Newsletters | `appnewsletter` | Topics |
| App Quotes | `appquote` | -- |
| App Collections | `appcollection` | Videos, Articles, Audios, Lyrics |
| App Courses | `appcourse` | Topics |
| App Topics | `apptopic` | Videos, Audios, Articles |
| App Stories | `appstorycollection` | Story Items (Mux video) |
| App Challenges | `appchallenge` | Challenge Days, Books |
| App Challenge Days | `appchallengeday` | Events |
| App Announcements | `appannouncement` | Events, Videos, Articles, Books |
| App Radio | `appradio` | -- |
| App Version | `appversion` | -- |

#### Event Management (9 types)

| Content Type | API ID | Relations |
|-------------|--------|-----------|
| Website Events | `event` | Categories, Retreats |
| Featured Events | `featured-event` | -- |
| Retreats | `retreat` | Events |
| Live Streams | `live-stream` | Events |
| Event Email Templates | `event-email-template` | Events |
| Coordinator Email Templates | `coordinator-email-template` | Events |
| Event Notifications | `event-notification` | Events |
| Satsang Centers | `appsatsangcenter` | Events |
| Testimonials | `testimonial` | Events, Basic Pages |

#### Online Classes (5 types)

| Content Type | API ID |
|-------------|--------|
| Online Classes | `online-class` |
| Class Schedules | `online-class-schedule` |
| Normalized Schedules | `normalized-class-schedule` |
| Class Categories | `online-class-category` |
| Class Testimonials | `online-class-testimonial` |

#### Website Content (8+ types)

| Content Type | API ID |
|-------------|--------|
| Basic Pages | `basic-page` |
| Blogs | `web-app-blog` |
| FAQs | `web-app-faq` |
| Carousels | `carousel` / `jkyogcarousel` |
| Navbar | `navbar` / `navbar-item` |
| Banners | `banner` / `heading-banner` |
| Posts | `post` |

---

## Enumeration Values

These are the hard-coded dropdown values defined in the backend schema. You cannot enter values outside these lists.

| Enum | Values | Used In |
|------|--------|---------|
| **Language** | `en`, `hi` | App content types |
| **TimeZone** | `EST`, `CST`, `MST`, `IST`, `BST`, `PST` | Events, schedules |
| **EventLocation** | `Online`, `In-person`, `Both` | `appevent` |
| **PricingType** | `free`, `paid` | `appevent`, email templates |
| **EventType** | `Retreat`, `Featured`, `MeetSwamiji`, `LifeTransformation`, `LifeTransformation2`, `LifeTransformation3`, `General`, `Yuth`, `SATSANG` | `appevent` (multi-select) |
| **EventPlatform** | `JKYog`, `RKT`, `Krishna Bhagavad Gita` | `appevent` (multi-select) |
| **DifficultyLevel** | `Beginner`, `Intermediate`, `Advanced` | LMS courses |
| **LMS Category** | `Vedic Philosophy`, `Meditation` | LMS courses |
| **QuestionType** | `single_choice`, `multiple_choice`, `true_false`, `short_answer`, `pairs` | Quiz questions |
| **Practice Type** | `prayer`, `kirtan`, `aarti` | LMS practice activities |
| **Day** | (weekdays) | Online class schedules |
| **BillingPeriod** | `monthly`, `yearly`, `weekly`, `quarterly` | Promocodes |

---

## Component Library (39 Components)

Components are reusable field groups embedded inside content types.

### app-event (26 components)

Used by `appevent`, `basic-page`, and `live-stream`:

| Component | Fields | Purpose |
|-----------|--------|---------|
| `text` | Text (CKEditor) | Rich text content block |
| `highlighted-text` | Text (CKEditor) | Emphasized text block |
| `video` | Video (media), Link, PreviewImageUrl | Video embed with preview |
| `carousel` | Title, CarouselContent[] | Image carousel section |
| `carousel-content` | Title, Image | Individual carousel slide |
| `gallery` | Title, Images[] | Image gallery section |
| `select-tab` | Title, SelectedTabContent[] | Tabbed content section |
| `selected-tab-content` | Title, Image, Description (CKEditor) | Tab panel content |
| `schedule` | Date, StartTime, EndTime, TimeZone, Venue, Stream, Details, Sevas[] | Event schedule entry |
| `event-schedule` | StartDate, EndDate, SubEventsSchedules[] | Multi-day event schedule |
| `sub-event-schedule` | StartDate, EndDate, SubEvents[], Description, Venue | Sub-event schedule block |
| `sub-event` | Name, StartTime, EndTime | Individual sub-event time slot |
| `sevas` | Title, Description, Price, SponsorsList, InrPrice, SevaEmailTemplate | Seva/sponsorship option |
| `seva-email` | Subject, EventType, PricingType, TextBody, HtmlBody | Seva confirmation email |
| `seo-content` | PageTitle, MetaDescription | SEO metadata |
| `faq` | Question, Answer | FAQ entry (rich text) |
| `past-event-glimpses` | Description, Videos[], Images[] | Past event media showcase |
| `post-register` | Description, Banner | Post-registration thank you content |
| `sponsors-info` | SponsorLogos[], Text (CKEditor) | Event sponsor section |
| `signups` | Title, Description, Link | External signup link |
| `nav-bar` | Title, Link, IsOpenInNewTab | Navigation link item |
| `platforms` | StreamIFrame, ChatIFrame, Default | Livestream platform embed |
| `highlight` | Title, Description, Video, Link | Program highlight card |
| `stats` | Stats[], Image[] | Statistical showcase with images |
| `stat` | Name, Quantity | Individual statistic |
| `past-event-glimpses-videos` | Title, Video, Description, Date | Past event video entry |

### app-lms (7 components)

Used by LMS lesson dynamic zones:

| Component | Fields | Purpose |
|-----------|--------|---------|
| `video` | Video content fields | Lesson video |
| `review` | Review fields | Lesson review section |
| `quiz` | Title, Position, IsRequired, Instructions, PassingScore, MaxAttempts, TimeLimit, Questions[] | Interactive quiz |
| `quiz-question` | Position, QuestionText, QuestionType, Points, Explanation, ImageURL, Choices[], Pairs[] | Quiz question |
| `quiz-choice` | Text, IsCorrect | Multiple choice answer |
| `quiz-pair` | Word, Definition | Matching pair |
| `practice` | Title, Position, IsRequired, Type (prayer/kirtan/aarti), VideoURL, Lyrics, AudioURL | Guided practice |

### Other Components

| Category | Component | Purpose |
|----------|-----------|---------|
| app-challenge-day | `downloadable-asset` | Downloadable resource (PDF, DOCX) |
| app-quiz | `quiz_answer` | Quiz activity answer structure |
| app-stream | `stream` | Challenge day livestream embed |
| email-list | `email-list` | Event coordinator email list |
| navbar | `navbar-link` | Navigation bar link |
| promocode | `billing-period` | Subscription billing period |

---

## Schema Deep Dive: App Event (58 Fields)

The most important content type. This is the complete field reference.

### Core Fields

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| **EventTitle** | String | Yes | -- | Event name (1-100 chars) |
| **EventSubtitle** | String | Yes | -- | Short description (1-100 chars) |
| **EventDescription** | Rich Text (CKEditor) | No | -- | Full event description with formatting |
| **Language** | Enum | Yes | `en` | `en` (English) or `hi` (Hindi) |

### Date & Time Fields

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| **StartTime** | DateTime | Yes | -- | Event start date and time |
| **EndTime** | DateTime | No | -- | Event end date and time (null = single-day) |
| **TimeZone** | Enum | Yes | `CST` | `EST`, `CST`, `MST`, `IST`, `BST`, `PST` |

### Location Fields

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| **EventLocation** | Enum | Yes | `Online` | `Online`, `In-person`, `Both` |
| **Venue** | Custom (Google Maps) | Yes | -- | Google Maps venue picker with geocoding. Stores address, lat/lng. |

### Contact Fields

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| **ContactEmail** | Email | Yes | -- | Event contact email |
| **ContactPhoneNumber** | String | Yes | -- | Event contact phone number |
| **WhatsAppGroupUrl** | String | No | -- | WhatsApp community/group link |

### URL & Image Fields

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| **BannerURL** | String (URL) | Yes | -- | Primary banner image URL (1024x768). Stored on AWS S3, served via CloudFront CDN. |
| **ImageURL** | String (URL) | Yes | -- | Event image URL (used in listings) |
| **HomepageImage** | String (URL) | No | -- | Image specifically for homepage display |
| **WebsiteURL** | String (URL) | No | -- | JKYog website event page link |
| **RktdWebsiteURL** | String (URL) | No | -- | RKT Dallas website event page link |

### Configuration Booleans

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| **IsEvent** | Boolean | Yes | `true` | Controls visibility on the Upcoming Events page |
| **IsRegistration** | Boolean | Yes | `true` | Controls the registration/link button on the frontend |
| **HasIFrame** | Boolean | Yes | `false` | Whether the event page contains an iFrame embed |
| **QrScanRequired** | Boolean | Yes | `true` | Whether QR code check-in is required |
| **ExternalRegistration** | Boolean | Yes | `false` | If `true`, registration is handled externally |

### Pricing & Classification

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| **PricingType** | Enum | Yes | -- | `free` or `paid`. Controls pricing badge and payment flow |
| **EventType** | Multi-select | Yes | -- | One or more of: `Retreat`, `Featured`, `MeetSwamiji`, `LifeTransformation`, `LifeTransformation2`, `LifeTransformation3`, `General`, `Yuth`, `SATSANG` |
| **EventPlatform** | Multi-select | Yes | `JKYog` | One or more of: `JKYog`, `RKT`, `Krishna Bhagavad Gita` |

### Marketing & SEO Fields

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| **BrevoContactListName** | String | Yes | -- | Brevo email list name (unique). Format: `/rkt-[eventname]` |
| **EventURLSlug** | String | Yes | -- | URL-safe event identifier (unique, regex validated) |
| **HighlightsTitle** | String | Yes | -- | Title for the Highlights section |
| **uuid** | Auto-UUID | No | Auto | Auto-generated unique identifier for Webflow registration |

### LTP (Life Transformation Program) Fields

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| **LTPBannerURL** | String (URL) | No | -- | Life Transformation Program banner image |
| **LTPSaintURL** | String (URL) | No | -- | Life Transformation Program saint image |
| **DisclaimerMessage** | Rich Text (CKEditor) | No | -- | Disclaimer text displayed on the event page |

### Content Components (Dynamic Zone)

The **Content** field is a dynamic zone allowing flexible page layouts:

| Component | Purpose | Key Fields |
|-----------|---------|------------|
| **Text** | Rich text content block | Text (CKEditor) |
| **HighlightedText** | Emphasized text block | Text (CKEditor) |
| **Carousel** | Image carousel with titled slides | Title, CarouselContent[] |
| **Gallery** | Image gallery | Title, Images[] |
| **Video** | Video embed with preview | Video (media), Link, PreviewImageUrl |
| **SelectTab** | Tabbed content (used for Highlights) | Title, Tabs[] |

### Other Component Fields

| Field | Component Type | Repeatable | Description |
|-------|---------------|------------|-------------|
| **FAQ** | FAQ | Yes | Question + Answer pairs |
| **PastEventGlimpses** | Past Event Glimpses | No | Past event media showcase |
| **SeoContent** | SEO Content | No | PageTitle + MetaDescription |
| **ThankYouMessage** | Post-Register | No | Post-registration thank-you message |
| **BannerVideo** | Video | No | Banner video embed |
| **LTProgramSchedule** | Event Schedule | Yes | Multi-day schedule |
| **LTProgramHighlights** | Highlight | Yes | Program highlight cards |
| **LTProgramStats** | Stats | No | Statistical showcase |
| **PostLTProgramSupport** | Highlight | Yes | Post-program support cards |
| **EventCoordinatorEmails** | Email List | Yes | Coordinator email list |
| **SponsorsInfo** | Sponsors Info | No | Sponsor logos + text |
| **Signups** | Signups | No | External signup link |

### Relations (Links to Other Content Types)

| Field | Relation Type | Target | Description |
|-------|--------------|--------|-------------|
| **LinkedStreams** | One-to-Many | App Live Stream | Associated livestream entries |
| **LiveStreams** | One-to-One | Live Stream | Website livestream page |
| **EventEmailTemplates** | Many-to-Many | Event Email Template | Registration confirmation emails |
| **CoordinatorEmailTemplate** | Many-to-One | Coordinator Email Template | Internal notification email |
| **EventNotifications** | Many-to-Many | Event Notification | Push notification templates |
| **Testimonials** | Many-to-Many | Testimonial | Event testimonials |
| **LinkedAppannouncements** | Many-to-One | App Announcement | Parent announcement |
| **LinkedChallengeDays** | One-to-Many | App Challenge Day | Challenge day content |
| **RelatedEvents** | Many-to-Many (self) | App Event | Related event links |
| **BasicPages** | Many-to-Many | Basic Page | Associated CMS pages |
| **LinkedSMEXMeetings** | One-to-Many | App SMEx Meeting | SMEx meeting links |
| **LinkedSatsangCenter** | Many-to-One | App Satsang Center | Host satsang center |
| **Lyrics** | Many-to-Many | App Lyric | Event kirtan lyrics |
| **app_story_collections** | One-to-Many | App Story Collection | Event story content |

---

## Schema Deep Dive: Basic Page

A flexible CMS page template with dynamic zone content.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **Title** | String | Yes | Unique, min: 1 |
| **Subtitle** | String | Yes | min: 1 |
| **Description** | CKEditor | No | Rich HTML content |
| **BannerURL** | String | Yes | Banner image URL |
| **ImageURL** | String | Yes | Page image URL |
| **URLSlug** | String | Yes | URL-friendly identifier |
| **uuid** | Auto-UUID | No | Auto-generated |
| **HasIFrame** | Boolean | Yes | iFrame embed flag (default: false) |

**Relations:** Testimonials (oneToMany), EventEmailTemplates (manyToMany), RelatedEvents (manyToMany)

**Dynamic Zone (Content):** Same 6 components as App Event: text, highlighted-text, carousel, gallery, video, select-tab

---

## Custom Field Types

| Custom Field | Plugin | Used In | Purpose |
|-------------|--------|---------|---------|
| CKEditor | `@ckeditor/strapi-plugin-ckeditor` | 15+ types | Rich text editing with Word import |
| venue-field | Custom plugin | Events, Satsang Centers | Google Maps venue picker with geocoding |
| uuid | `strapi-auto-uuid` | Events, Basic Pages | Auto-generated unique identifiers |
| multi-select | `strapi-plugin-multi-select` | Events (EventType, EventPlatform) | Multi-value enumeration fields |

---

## Dynamic Zones

| Content Type | Dynamic Zone Field | Available Components |
|-------------|-------------------|---------------------|
| App Event | `Content` | text, highlighted-text, carousel, gallery, video, select-tab |
| Basic Page | `Content` | text, highlighted-text, carousel, gallery, video, select-tab |
| LMS Lesson | `Components` | video, review, quiz, practice |

---

## Glossary

| Term | Definition |
|------|-----------|
| **App Event** | A Strapi Collection Type that represents an event (satsang, retreat, festival) |
| **Brevo** | Email marketing platform (formerly Sendinblue) used for sending confirmation and marketing emails |
| **BrevoContactListName** | The mailing list name in Brevo where registrants are added |
| **CDN** | Content Delivery Network -- a globally distributed network that caches and delivers content closer to users. Strapi uses **CloudFront** CDN. |
| **CloudFront** | Amazon's CDN service. All media uploaded to Strapi is stored on AWS S3 and served globally via CloudFront. |
| **Code Embed** | A Webflow element that contains custom HTML/JavaScript code |
| **Collection Type** | A category of content in Strapi (like a database table). There are 73 Collection Types. |
| **Content Manager** | The main section in Strapi where you create and edit content |
| **Content-Type Builder** | Admin tool that modifies the data schema -- DO NOT use |
| **CST** | Central Standard Time -- the default timezone for events |
| **Dynamic Zone** | A flexible Strapi field that allows adding multiple component types in any order. The **Content** field in App Events supports Text, HighlightedText, Carousel, Gallery, Video, SelectTab. |
| **EventAttributes** | The data model returned by the Strapi API for each event -- includes all 58 fields |
| **EventPlatform** | Multi-select dropdown. Values: `JKYog`, `RKT`, `Krishna Bhagavad Gita`. Must include `RKT` for the event to appear on the RKT website. |
| **eventUuid** | Unique identifier that connects a Webflow registration form to a Strapi event |
| **EventURLSlug** | URL-friendly version of the event name (e.g., `akshaya-tritiya`) |
| **Gallery** | A content component for displaying multiple images in an event |
| **Headless CMS** | A CMS that stores content and delivers it via APIs (no built-in frontend). Strapi v4.10.7 auto-generates REST API endpoints for each Content Type. |
| **Highlights** | A content component (SelectTab) used to showcase key event features |
| **IsRegistration** | Boolean field (default: `true`) that controls whether the registration button is active |
| **isEvent** | Boolean field (default: `true`) that controls visibility on the Upcoming Events page |
| **JKYOG** | JK Yog -- the parent organization |
| **Live Stream** | A Strapi Collection Type holding sevas and schedules for an event |
| **Media Library** | The file storage section in Strapi. Files are stored on **AWS S3** and served via **CloudFront CDN**. |
| **Meilisearch** | Full-text search engine integrated with Strapi for content indexing. |
| **Mux** | Video processing platform for upload, transcoding, and streaming of App Story Items. |
| **NestJS Backend** | The JKYog 2.0 Backend API server. Receives content change notifications from Strapi via **webhooks**. |
| **populate** | API query parameter that includes relational data in the response. Max depth: 5 levels. |
| **PricingType** | Enum: `free` or `paid`. Controls the pricing badge on the frontend. |
| **Production** | The live environment at `https://prod-strapi.jkyog.org/admin` |
| **Promocode** | A discount code for paid event tickets |
| **Publish** | Making content live and visible on the website/app |
| **Redis** | In-memory store for **API response caching** with 1-hour TTL. Cache invalidates on update/delete but **NOT on create**. |
| **REST API** | The interface through which the frontend fetches data from Strapi. Max 100 records per request. |
| **RKT** | Radha Krishna Temple |
| **Save** | Storing changes without making them public (draft state) |
| **SelectTab** | A content component type used for Highlights sections |
| **Sentry** | Error tracking and monitoring platform integrated with Strapi. |
| **Seva** | A sponsorship or service option for an event |
| **Single Type** | A Strapi content type with exactly one entry (e.g., homepage config, footer). |
| **Staging** | The test environment at `https://jkyog-strapi-staging.up.railway.app/admin` |
| **TTL** | Time-to-Live -- how long a cached response remains valid. Strapi uses a **1-hour TTL**. |
| **UUID** | Universally Unique Identifier -- used to link Strapi events to Webflow forms |
| **Webhook** | An automated HTTP notification sent when content changes in Strapi. |
| **Webflow** | The website builder platform where radhakrishnatemple.net is hosted |

---

## Related Docs

- [Data & API Reference](DATA.md) -- API endpoints, caching, query patterns
- [Business Rules](BUSINESS.md) -- content domains, user roles, permissions
- [Creating Events](CREATING-EVENTS.md) -- step-by-step event creation
- [Architecture](ARCHITECTURE.md) -- system overview, tech stack
