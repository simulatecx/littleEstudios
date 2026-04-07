# Content Strategy

## Goal

Rank organically for party planning, parenting, and storytelling keywords. Convert that traffic into Etsy sales and app registrations — without manual blogging overhead.

---

## Target Keywords (Initial Focus)

| Category | Target Keywords |
|----------|----------------|
| Primary | "Storytelling for kids", "bedtime stories for toddlers" |
| Party Planning | "Planning the perfect first birthday", "1 year old birthday ideas", "party name tags printable" |
| Product-Specific | "First birthday stickers", "party banner printable", "cute content packs for parties" |
| App-Specific | "StoryStork AI stories", "personalized bedtime stories app" |
| Cross-sell | "Monthly activity box for kids", "GetJoyBox subscription" |

---

## Content Pillars

### 1. Party Planning & Crafting
- How-to guides for themed parties
- Product showcases (Etsy items styled as editorial content)
- "Real family" stories featuring the products in use

### 2. Bedtime & Storytelling
- Tips for building bedtime routines
- Story recommendations and app features
- Reviews / testimonials synthesized from StoryStork.AI releases

### 3. Milestone Celebrations
- First birthdays, holidays, seasonal events
- Gift guide content (cross-sell to GetJoyBox)

---

## Automated Content Pipeline

### Trigger → Publish Flow

```
[Etsy 5-star review received]
        ↓
Cloud Function captures review text + product data
        ↓
Vertex AI (Gemini 2.5 Flash) synthesizes into storytelling article
        ↓
Real data embedded: quote, product name, family context
        ↓
Tags auto-assigned → ad_theme set → product_refs linked
        ↓
Post staged for review OR auto-published (based on quality score)
```

```
[New StoryStork.AI story released]
        ↓
Trigger event sent to Cloud Function
        ↓
AI writes: "How families used [story title] to beat bedtime blues"
        ↓
Embedded: story stats, reading time, themes
        ↓
Tagged: #BedtimeStories #Toddlers #StoryStork
        ↓
StoryStork.AI cross-sell injected contextually
```

---

## EEAT Compliance (Google's Experience, Expertise, Authoritativeness, Trust)

To avoid Google's AI-spam filters, every automated post must include:

| Requirement | Implementation |
|-------------|---------------|
| **Experience** | Real Etsy review quotes, actual family scenarios |
| **Expertise** | Product-specific tips (e.g., how to use the stickers for a party) |
| **Authoritativeness** | Internal links to related posts, product pages |
| **Trust** | Author attribution, date, real product data (star ratings, review count) |

**Rule:** No post goes live with only AI-generated text. Every post must include at least one piece of real-world data (a review quote, an app stat, a product detail).

---

## Content Cadence

- **Target:** 2 high-quality posts per week
- **Max automation:** 2 automated posts per week (one from Etsy trigger, one from StoryStork trigger)
- **Manual override:** Owner can always queue, edit, or suppress any auto-generated post
- **Avoid:** Posting more than once per day — signals AI spam to Google

---

## Internal Linking Rules

- Every blog post must link to at least 2 other internal posts
- Every blog post must reference at least 1 product via the proxy (`/products/[slug]`)
- Related posts grid shown at the bottom of every article
- Tags create filterable content clusters (e.g., all `#FirstBirthday` posts link together)

---

## Ad & Cross-Sell Placement

| Post Theme | Injected Unit |
|------------|--------------|
| Bedtime / Storytime | StoryStork.AI "Celebration Note" |
| Crafting / DIY | GetJoyBox "Celebration Note" |
| Birthday / Party | Etsy Party Pack product block |
| General | Google AdSense "Celebration Note" |

Placement: After the second paragraph and before the final section. Never at the top. Never more than 2 injected units per post.
