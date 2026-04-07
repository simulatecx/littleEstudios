# Feature Specifications

## 4.1 Automated Content Engine

The blog is an **Activity-Driven Engine**, not a manual task.

### Trigger Sources

| Trigger | Blog Output |
|---------|-------------|
| New 5-star Etsy review | "Experience" post synthesizing the review into a storytelling article |
| New story released in StoryStork.AI | Post about how families use the story (e.g., bedtime, routines) |
| Manual | Owner-written or approved content |

### AI Augmentation Pipeline

1. Trigger fires (Etsy review / StoryStork release)
2. Cloud Function captures raw event data
3. Vertex AI (Gemini 2.5 Flash) synthesizes into a full "Storytelling" article
4. Real-world data is embedded (review quotes, app stats, usage examples) for EEAT compliance
5. Post is staged for review or auto-published based on quality threshold
6. Tags, product refs, and ad_theme are auto-assigned

### Content Cadence

- **Target: 2 high-quality posts per week**
- Posts must include real-world data points to pass Google's AI-spam detection
- Avoid generic AI output — all posts must read as experience-based, human-utility articles

---

## 4.2 Cross-Sell & Ad Injection

### Contextual Banner Rules

| Blog Tag / Theme | Injected Ad |
|-----------------|-------------|
| `bedtime`, `storytime`, `routine` | StoryStork.AI banner |
| `crafting`, `diy`, `activity` | GetJoyBox banner |
| `birthday`, `party`, `first-birthday` | Etsy Party Pack product block |

Ads are styled as **"Celebration Notes"** — small, elegant pop-ins that look like handwritten cards or stickers. They must never feel like interruptions.

### Attribution Tracking

Every product link click logs:
- `blog_post_id` — which post generated the click
- `device` — mobile / desktop / tablet
- `product_slug` — which product was clicked
- `destination` — where the user was sent (Etsy or internal)
- `clicked_at` — timestamp

This data feeds GA4 custom events for Etsy redirection attribution.

---

## 4.3 Product Proxy System

See [ARCHITECTURE.md](./ARCHITECTURE.md#the-product-proxy) for full technical spec.

### Summary
- All product links use internal slugs: `/products/[slug]`
- Destination is stored in Firestore, not in HTML
- Owner can switch any product from Etsy → Internal Shop with one DB update
- Zero historical link rot

---

## 4.4 Google AdSense Integration

- AdSense blocks are styled within the design system as "Celebration Notes"
- Positioned contextually within blog content (not sidebar banners)
- Must not cause CLS — containers must have defined dimensions before ad loads
- Internal cross-sells (StoryStork.AI, GetJoyBox) use the same visual treatment as AdSense units for consistency

---

## 4.5 Interactive Mascot (Rive)

The brand mascot lives in the UI and reacts to user behavior:

| User Action | Mascot Reaction |
|-------------|----------------|
| Scroll past a product block | Mascot "points" to the item with excitement |
| Hover over nametag / banner | Mascot "tucks" the item under its wing |
| Click "View on Etsy" / "Save for Later" | Confetti burst (pastel, vector-based) |
| Click a registration link (StoryStork / GetJoyBox) | Mascot "cheers" |

All Rive assets must be:
- Vector-based `.riv` format
- Sub-100kb file size
- Non-blocking (loaded async, containers have pre-defined aspect ratios)
