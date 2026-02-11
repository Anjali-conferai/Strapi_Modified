# Troubleshooting Guide

[Back to Index](README.md)

---

## How to Use This Guide

Find your symptom in the table below and jump to the detailed fix. If your issue isn't listed, check the [Escalation](#escalation) section at the bottom.

### Quick Symptom Lookup

| # | Symptom | Jump To |
|---|---------|---------|
| 1 | Registration form does nothing / submissions fail | [T-01](#t-01-registration-form-does-nothing) |
| 2 | Registrations going to the wrong event | [T-02](#t-02-registrations-going-to-the-wrong-event) |
| 3 | Paid event shows $0 price | [T-03](#t-03-paid-event-shows-0) |
| 4 | Event not showing on the Upcoming Events page | [T-04](#t-04-event-not-showing-on-upcoming-events-page) |
| 5 | Event not showing on the RKT website at all | [T-05](#t-05-event-not-showing-on-rkt-website) |
| 6 | Wrong confirmation email sent after registration | [T-06](#t-06-wrong-confirmation-email-sent) |
| 7 | Event banner looks stretched or cropped | [T-07](#t-07-event-banner-looks-stretched-or-cropped) |
| 8 | Changes saved but not visible on the website | [T-08](#t-08-changes-saved-but-not-visible-on-website) |
| 9 | Changes lost after switching tabs in Strapi | [T-09](#t-09-changes-lost-after-switching-tabs) |
| 10 | Sevas not showing on the event page | [T-10](#t-10-sevas-not-showing-on-event-page) |
| 11 | Schedule not showing on the event page | [T-11](#t-11-schedule-not-showing-on-event-page) |
| 12 | LMS lesson changes not appearing in the app | [T-12](#t-12-lms-lesson-changes-not-appearing-in-app) |
| 13 | "Please fill all required fields" error on registration | [T-13](#t-13-please-fill-all-required-fields-error) |
| 14 | Registration button is missing or disabled | [T-14](#t-14-registration-button-missing-or-disabled) |
| 15 | Duplicate event has stale/wrong data | [T-15](#t-15-duplicate-event-has-stale-or-wrong-data) |
| 16 | HelpDesk section looks broken | [T-16](#t-16-helpdesk-section-looks-broken) |

---

## Detailed Fixes

### T-01: Registration Form Does Nothing

**Symptoms:** User fills out the registration form and clicks submit, but nothing happens. No error, no confirmation.

**Root Causes:**
1. Wrong or missing `eventUuid` in the Webflow Code Embed
2. Form field IDs don't match the expected values

**Fix:**
1. Open the event page in the **Webflow Editor**.
2. Double-click the **Code Embed** element.
3. Go to **line 25** -- verify the `eventUuid` matches the UUID from the Strapi App Event.
4. Check that all 7 form field IDs are correct:

| Required Field ID | Type |
|-------------------|------|
| `firstName` | Text |
| `lastName` | Text |
| `email` | Email |
| `city` | Text |
| `countryCode` | Text |
| `phone` | Text |
| `joinwhatsapp` | Checkbox |

5. Save and Publish in Webflow.

**Reference:** [Webflow Integration](04-webflow-integration.md)

---

### T-02: Registrations Going to the Wrong Event

**Symptoms:** Users register for Event A but the data appears under Event B. Confirmation email references the wrong event.

**Root Cause:** The `eventUuid` in the Webflow Code Embed still points to a previous event (common when duplicating registration components).

**Fix:**
1. Open the Strapi App Event > copy the correct **UUID**.
2. Open the Webflow Code Embed > replace the `eventUuid` on **line 25**.
3. Save and Publish in Webflow.

**Prevention:** Every time you duplicate a registration component, immediately update the `eventUuid`.

**Reference:** [Webflow Integration](04-webflow-integration.md)

---

### T-03: Paid Event Shows $0

**Symptoms:** The registration form displays "$0" or no price for an event that should be paid.

**Root Cause:** No ticket has been created in the Tickets and Promocodes plugin.

**Fix:**
1. Go to **Strapi > Tickets and Promocodes** (sidebar).
2. Search for the event name.
3. Click **Show** to expand it.
4. Click **+ Add ticket** and fill in: Type, Name, Price, Currency, Quantity.
5. Save.
6. Refresh the event page to verify the correct price appears.

**Also check:** Is `PricingType` set to `paid` in the App Event? If it's set to `free`, the frontend won't trigger the payment flow regardless of tickets.

**Reference:** [Payment Tickets](05-payment-tickets.md)

---

### T-04: Event Not Showing on Upcoming Events Page

**Symptoms:** The event exists in Strapi and is published, but does not appear on the Upcoming Events page.

**Root Causes (check in order):**

| Check | What to Look For | Fix |
|-------|-----------------|-----|
| 1. `isEvent` | Is it set to `true`? | Set `isEvent = true` and re-publish |
| 2. `StartTime` | Is the StartTime in the past? | The API filters out past events. Update the date. |
| 3. `EndTime` | Is EndTime set and in the past? | Update or clear EndTime |
| 4. Published? | Is the entry actually published (green) or still draft? | Click **Publish** |
| 5. `EventPlatform` | Does it contain `RKT`? | Set EventPlatform to `RKT` |

**Reference:** [API & Backend -- Data Logic](09-api-and-backend.md#data-logic----critical-fields)

---

### T-05: Event Not Showing on RKT Website

**Symptoms:** The event is published and `isEvent = true`, but still doesn't appear on the RKT website.

**Root Cause:** The `EventPlatform` field does not contain `RKT`. The API filters using `filters[EventPlatform][$containsi]=RKT`.

**Fix:**
1. Open the App Event in Strapi.
2. Set **EventPlatform** to include `RKT`.
3. Save, then Publish.

**Reference:** [API & Backend](09-api-and-backend.md#the-events-api-endpoint)

---

### T-06: Wrong Confirmation Email Sent

**Symptoms:** After registering, users receive a confirmation email for a different event.

**Root Cause:** When the event was duplicated, the old **Event Email Template** link was not removed.

**Fix:**
1. Open the App Event in Strapi.
2. Scroll to the **Email Template** field.
3. Remove the old template.
4. Create or assign the correct new Email Template.
5. Save, then Publish.

**Prevention:** After every event duplication, immediately remove the old Email Template link.

---

### T-07: Event Banner Looks Stretched or Cropped

**Symptoms:** The event banner image on the website appears distorted, pixelated, or incorrectly cropped.

**Root Cause:** The uploaded image does not match the required **1024 x 768** pixel dimensions.

**Fix:**
1. Resize the image to **1024 x 768 pixels** using any image editor.
2. Upload to **Media Library > Event Banner Image (1024 x 768)**.
3. Copy the new URL and update the **ImageURL** field in the App Event.
4. Save, then Publish.

---

### T-08: Changes Saved but Not Visible on Website

**Symptoms:** You updated fields and clicked Save, but the website still shows old content.

**Root Causes:**

| Check | Fix |
|-------|-----|
| Did you **Publish** after saving? | Save stores a draft. You must click **Publish** to go live. |
| Browser cache? | Hard-refresh the page (Ctrl+Shift+R / Cmd+Shift+R). |
| Correct environment? | Are you editing on **Staging** but checking **Production**? They are separate databases. |

**Key rule:** **Save** = draft. **Publish** = live. Always do both.

---

### T-09: Changes Lost After Switching Tabs

**Symptoms:** You were editing an App Event, switched to the Live Stream or Email Template tab, and came back to find your changes gone.

**Root Cause:** Strapi does not auto-save. Navigating away from an entry discards unsaved changes.

**Fix:** There is no recovery. You must re-enter the lost data.

**Prevention:** **Always click Save before** switching to another tab, entry, or section.

---

### T-10: Sevas Not Showing on Event Page

**Symptoms:** The event page on the website does not display any Seva/sponsorship options.

**Root Causes:**

| Check | Fix |
|-------|-----|
| Live Stream created? | Create a Live Stream entry with EventSevas |
| Sevas added? | Open the Live Stream > add sevas with Title and Price |
| Live Stream linked? | In the App Event, set **LinkedStreams** to your Live Stream |
| Live Stream published? | Publish the Live Stream entry |

**Reference:** [Creating Events -- Step 5](03-creating-events.md#step-5-create-live-stream-sevas--schedule)

---

### T-11: Schedule Not Showing on Event Page

**Symptoms:** The event page does not display a schedule/itinerary.

**Fix:**
1. Open the **Live Stream** entry linked to the event.
2. Scroll to the **Schedule** section.
3. Add entries with: Date, StartTime, EndTime, TimeZone, Details.
4. Save, then Publish the Live Stream.

---

### T-12: LMS Lesson Changes Not Appearing in App

**Symptoms:** You edited an App LMS Lesson but the changes don't show in the mobile app.

**Root Causes:**

| Check | Fix |
|-------|-----|
| Published? | Did you click both **Save** and **Publish**? |
| Correct entry? | Verify you edited the right lesson (check Title) |
| App cache? | The app may cache content. Force-close and reopen the app. |

**Reference:** [Editing LMS Lessons](06-editing-lms-lessons.md)

---

### T-13: "Please Fill All Required Fields" Error

**Symptoms:** User sees this error when submitting the registration form, even though they filled everything in.

**Root Cause:** One or more required form fields are missing from the Webflow form, or their IDs don't match.

**Fix:** Verify all 7 required field IDs are present and spelled exactly right: `firstName`, `lastName`, `email`, `city`, `countryCode`, `phone`, `joinwhatsapp`.

**Reference:** [Webflow Integration](04-webflow-integration.md#step-3-verify-input-field-ids)

---

### T-14: Registration Button Missing or Disabled

**Symptoms:** The event page has no registration button, or the button is grayed out / non-functional.

**Root Cause:** The `IsRegistration` field is set to `false` in the App Event.

**Fix:**
1. Open the App Event in Strapi.
2. Set **IsRegistration** to `true`.
3. Save, then Publish.

**Reference:** [API & Backend -- Data Model](09-api-and-backend.md#data-model----eventattributes)

---

### T-15: Duplicate Event Has Stale or Wrong Data

**Symptoms:** After duplicating an event, old data from the source event bleeds through (wrong title in emails, wrong registration link, etc.).

**Checklist after every duplication:**

- [ ] Update **EventTitle** and **EventSubtitle**
- [ ] Update **ImageURL** and **BannerURL**
- [ ] Update **StartTime**, **EndTime**, dates
- [ ] Update **uuid** (or verify the new one)
- [ ] Remove old **Email Template** link > assign new one
- [ ] Remove old **LinkedStreams** > assign new Live Stream
- [ ] Update **BrevoContactListName** (`/rkt-[neweventname]`)
- [ ] Update **EventURLSlug**
- [ ] Update **WebsiteURL**
- [ ] Update **ContactEmail** and **ContactPhoneNumber** (if different)
- [ ] Update `eventUuid` in the **Webflow Code Embed** (line 25)

---

### T-16: HelpDesk Section Looks Broken

**Symptoms:** The HelpDesk section on the event portal page shows broken HTML, missing images, or wrong text.

**Root Cause:** When copying HelpDesk code from a reference event (e.g., Dallas Retreat), not all URLs, names, and image addresses were updated.

**Fix:**
1. Open the **Live Stream** entry in Strapi.
2. Click the **HTML icon** to view the raw HelpDesk code.
3. Update all references: event name, title, image URLs, WhatsApp group URL.
4. Preview the result.
5. Save.

**Reference:** [EventLiveStreamcreationonStrapi - Steps](../EventLiveStreamcreationonStrapi%20-%20Steps.md) (Steps 43-50)

---

## Escalation

If your issue is not listed above or the fix doesn't work:

1. **Check the environment** -- are you on Staging or Production?
2. **Check permissions** -- do you have access to the relevant Collection Type?
3. **Screenshot the issue** -- capture the Strapi screen and the website/app screen.
4. **Contact your team lead** with:
   - What you were trying to do
   - What happened instead
   - Which environment (Staging/Production)
   - The event name or entry ID
   - Screenshots

> **Do NOT** try to fix issues by modifying Content-Type Builder, Settings, Plugins, or API tokens. These are admin-only areas and changes can break the entire system. See [Dos and Don'ts](08-dos-and-donts.md).

---

## Related Docs

- [Dos and Don'ts](08-dos-and-donts.md) -- common mistakes and how to prevent them
- [API & Backend](09-api-and-backend.md) -- understand what each field controls
- [Creating Events](03-creating-events.md) -- the correct step-by-step flow
- [Architecture](Architecture.md) -- how all the systems connect
