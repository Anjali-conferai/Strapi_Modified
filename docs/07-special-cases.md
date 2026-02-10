# Special Cases

[Back to Index](README.md) | Previous: [Editing LMS Lessons](06-editing-lms-lessons.md) | Next: [Dos and Don'ts](08-dos-and-donts.md)

---

## Bhakti Kirtan Retreat

The Bhakti Kirtan Retreat page works differently from standard events. If you are assigned to this page, read this section carefully.

### What's Different

| Aspect | Standard Events | Bhakti Kirtan Retreat |
|--------|----------------|----------------------|
| Registration | Embedded form in Webflow page | No registration block -- buttons link directly to URLs |
| Seva Opportunities | Part of the event page | Linked via URL buttons |
| Watch Live | Not typically used | Button links to a page that embeds the Strapi Live Stream |

### How It Works

1. **No registration section** exists in the Webflow page.
2. Instead, you **directly assign web URLs** to link buttons (Register, Seva, Watch Live).
3. Maintain a web URL similar to the slug provided in Strapi.
4. The **Watch Live** button redirects to a separate page that embeds content from the Strapi Live Stream entry.

### Strapi Setup

The App Event in Strapi is set up normally:

![Bhakti Kirtan Retreat App Event in Strapi -- showing event details, ImageURL, WebsiteURL](images/docx_image14.png)

The Live Stream entry contains the embedded content for the Watch Live page:

![Bhakti Kirtan Retreat Live Stream with embedded HTML/CSS content](images/docx_image10.png)

### The Live Registration Page

The registration uses the jkyog.org event registration system with the event slug in the URL:

![Bhakti Kirtan Retreat registration page on jkyog.org](images/docx_image19.png)

### Key Points

- You still need to create the App Event and Live Stream in Strapi.
- Payment tickets must be set up in Tickets and Promocodes (see [Payment Tickets](05-payment-tickets.md)).
- The Webflow page uses **direct URL links** instead of embedded registration forms.
- The Watch Live URL should point to the page that embeds the Live Stream from Strapi.

---

## Adding Future Special Cases

If you encounter another event type that doesn't follow the standard flow, document it here with:

1. What makes it different from the standard flow
2. The specific Strapi setup required
3. Any Webflow-side differences
4. Screenshots of the key screens

---

## Next Steps

- [Dos and Don'ts](08-dos-and-donts.md) -- rules, common mistakes, and glossary
