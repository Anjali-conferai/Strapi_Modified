# Insights: Adding Events on Strapi

Summary of the main takeaways from the Strapi event-management documentation.

---

## 1. Strapi structure

- **Content Manager** → New events, emails, Sevas  
- **Media Library** → Event images  
- **App Events** → Event records  
- **Event Email Templates** → Registration emails  
- **Live Streams** → Sevas (sponsorship / donation options)

---

## 2. Event images

- Use **Event Banner Images (1024 × 768)** folder in Media Library.  
- After upload, use the **chain link** icon to copy the image URL and reuse it when creating the event.

---

## 3. Creating an event (App Event)

- **Required / important fields:** Banner Image URL, contact (email/phone), **brevocontactlistname** (`/rkt-[eventname]`), **EventURLSlug** (`[eventname]`), **UUID** (needed for registration).  
- **Settings:** `isEvent: false`, **EventType:** General, **EventPlatform:** RKT.  
- **Before publish:** Save; if duplicating, remove the old Email Template and create/link the correct Live Stream.  
- **Order:** Save the event before opening Live Stream or Email Template tabs.

---

## 4. Highlights

- In the event’s content section: **Add a Component** → `selectTab`, title = `Highlights`.  
- Inside Highlights: use the **+** to add items with **Title**, **Image**, and **Description**.

---

## 5. Gallery

- Same content section: **Add a Component** → `Gallery`.  
- Add title and upload images, then Save and Publish.

---

## 6. Email template

- Use existing or new template; set subject and body, link to the event (or attach from the App Event), then Save and Publish.

---

## 7. Live stream (Sevas)

- **Live Streams** → Create New Entry (or duplicate if Sevas match).  
- Add event title, then **Event Sevas**: for each seva set **Title** (e.g. “Grand Sponsor”) and **Price**.  
- Link the live stream to the event (or add it from the event). Save and Publish.

---

## 8. Schedule (multi-day / recurring)

- In **Content Manager → Live Streams**, open the event’s live stream.  
- In **Schedule**, add entries with: **Title**, **Start/End Date & Time**, **Location**, **Description**.  
- For recurring events, Sevas can be skipped in schedule items. Save and Publish the Live Stream.

---

## 9. Webflow registration

- Copy a working event registration component (e.g. from Ram Navami, Hanuman Jayanti, etc.).  
- Paste into the event page and ensure form IDs: `firstName`, `lastName`, `email`, `city`, `countryCode`, `phone`, `joinwhatsapp`.  
- In the code embed, **line 25**: set **eventUuid** to the UUID from the Strapi App Event.  
- Save, then Publish.

---

## 10. Payment tickets (e.g. retreats)

- In Strapi: **Tickets and Promocode** in the left panel.  
- Search by website/page name, set quantity (via “show”), then add ticket and details.  
- If this is not done, the amount can show as **$0**.

---

## 11. Special case: Bhakti Kirtan Retreat

- No registration block in the Webflow page; buttons use direct web URLs.  
- Keep URLs aligned with the slug.  
- “Watch live” points to the page that embeds the Strapi event’s **Live Stream**.

---

## Quick checklist

| Area              | Critical point                                      |
|-------------------|-----------------------------------------------------|
| Event creation    | UUID, EventURLSlug, brevocontactlistname, Live Stream link |
| Duplicated events | Remove old Email Template; link correct Live Stream |
| Registration      | eventUuid in Webflow embed (line 25)                |
| Payments          | Create ticket in Tickets and Promocode or amount shows $0 |
| Multi-day events  | Use Schedule under Live Streams                     |
