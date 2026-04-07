# Architecture

## Stack

| Layer | Technology | Reason |
|-------|-----------|--------|
| Frontend | Next.js (TypeScript) | SSR, type safety, App Router |
| Hosting | Firebase App Hosting | CDN edge-caching, global distribution |
| Database | Firebase Firestore (NoSQL) | Product metadata, tracking links, blog content |
| Functions | Firebase Cloud Functions | Automation triggers, Etsy proxy, AI content generation |
| Animation | Rive.app | Interactive vector mascot, sub-100kb assets |
| AI / Content | Google Vertex AI (Gemini 2.5 Flash) | Speed + cost-optimized content synthesis |
| Analytics | GA4 with custom events | Etsy redirection attribution |
| 3D Previews | Spline | JoyBox 3D view *(future state)* |

---

## Why SSR (Server-Side Rendering)

2026 search crawlers prioritize **Instant HTML**. Client-side rendered content:
- Fails Core Web Vitals (LCP, CLS, INP)
- Ranks poorly due to delayed content paint
- Risks "invisible" product blocks during indexing

All pages must be SSR or statically generated via Next.js. No client-only rendering for SEO-critical content.

---

## Hosting & CDN

- Firebase App Hosting provides global edge-caching
- Images served from CDN with pre-defined aspect ratios in CSS (prevents CLS)
- `.riv` animation files must be vector-based to stay sub-100kb
- All static assets cached at edge nodes

---

## Data Layer: Firestore Schema (Conceptual)

### `products` collection
```
{
  id: string,
  slug: string,               // e.g., "starter-kit"
  title: string,
  destination: "etsy" | "internal",
  etsy_url: string,           // never exposed directly to frontend
  internal_url: string,       // used when destination = "internal"
  category: string[],         // ["party", "first-birthday", "stickers"]
  active: boolean
}
```

### `blog_posts` collection
```
{
  id: string,
  slug: string,
  title: string,
  content: string,
  tags: string[],
  trigger_source: "etsy_review" | "storystork_release" | "manual",
  published_at: timestamp,
  product_refs: string[],     // product slugs referenced in post
  ad_theme: "bedtime" | "crafting" | "birthday" | null
}
```

### `tracking_events` collection
```
{
  id: string,
  blog_post_id: string,
  product_slug: string,
  device: string,
  clicked_at: timestamp,
  destination: "etsy" | "internal"
}
```

---

## The Product Proxy

**Critical Rule: No hardcoded Etsy URLs anywhere in the codebase or database.**

All product links route through `/products/[slug]` which resolves destination at runtime:

```
User clicks link → /products/starter-kit
                 → Cloud Function looks up Firestore
                 → Logs tracking event
                 → 302 redirect to etsy_url or internal_url
```

### The Switch

To migrate a product from Etsy to an internal shop:
1. Update `destination` field in Firestore from `"etsy"` to `"internal"`
2. All historical blog posts and backlinks automatically resolve to the new destination
3. Zero code changes required

---

## Etsy API Policy

**Never scrape Etsy directly.**

- Use a middleware proxy (Cloud Function) for any Etsy data fetching
- Use a dedicated SEO data provider for backlink statistics
- Rate-limit all Etsy API calls within Cloud Functions
- Cache Etsy review data in Firestore to reduce API calls

---

## Core Web Vitals Targets

| Metric | Target |
|--------|--------|
| LCP (Largest Contentful Paint) | < 2.5s |
| CLS (Cumulative Layout Shift) | < 0.1 |
| INP (Interaction to Next Paint) | < 200ms |

All image/animation containers must have pre-defined CSS aspect ratios to prevent layout shift on load.
