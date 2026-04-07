# Metrics & Risk Mitigation

## Success Metrics

| Metric | Target | Timeframe |
|--------|--------|-----------|
| LCP (Largest Contentful Paint) | < 2.5s | At launch |
| CLS (Cumulative Layout Shift) | < 0.1 | At launch |
| INP (Interaction to Next Paint) | < 200ms | At launch |
| SEO Ranking — "Storytelling for kids" | Top 10 | 6 months post-launch |
| SEO Ranking — niche party keywords | Top 10 | 6 months post-launch |
| Cross-platform registration increase | +15% | 6 months post-launch |
| Blog post cadence | 2 posts/week | Ongoing |
| Etsy click attribution | 100% tracked | At launch |

---

## Risk Register

### Risk 1: Etsy Link Rot
**Problem:** Etsy product URLs change or listings get removed, breaking all historical blog links.

**Mitigation:** Product Proxy system — all links point to `/products/[slug]`. Destination URL stored in Firestore. One DB update fixes all historical links simultaneously. See [ARCHITECTURE.md](./ARCHITECTURE.md#the-product-proxy).

---

### Risk 2: Google "AI Slop" Filter
**Problem:** Automated blog posts get flagged as low-quality AI content and penalized in search rankings.

**Mitigation:**
- All posts must include real-world data (Etsy review quotes, app usage stats)
- Maximum 2 automated posts per week
- Quality threshold check before auto-publish
- Posts framed as "experience-based" storytelling, not product descriptions
- EEAT requirements enforced at the pipeline level (see [CONTENT_STRATEGY.md](./CONTENT_STRATEGY.md#eeat-compliance))

---

### Risk 3: Mobile CLS (Cumulative Layout Shift)
**Problem:** Animations, ads, and product blocks cause the page to jump when they load, harming Core Web Vitals and UX.

**Mitigation:**
- All product block containers have pre-defined CSS aspect ratios
- All Rive animation containers have fixed dimensions set before `.riv` loads
- All AdSense / "Celebration Note" containers have fixed dimensions before ad loads
- Images use `width` and `height` attributes (or CSS aspect-ratio) to reserve space

---

### Risk 4: Etsy API Rate Limits
**Problem:** Direct Etsy API calls from the frontend hit rate limits, causing failed data loads.

**Mitigation:**
- Never call Etsy API directly from the frontend or client
- All Etsy data fetching runs through Firebase Cloud Functions (middleware proxy)
- Etsy review data is cached in Firestore after fetch — subsequent reads come from cache
- Use a dedicated SEO data provider for backlink statistics (not Etsy API)

---

### Risk 5: Platform Dependency (Etsy / App Store)
**Problem:** Algorithm changes on Etsy or App Store reduce visibility and traffic.

**Mitigation:** This entire project is the mitigation. The hub owns the audience, the SEO, and the customer data. Etsy is a fulfillment destination, not a discovery channel.

---

### Risk 6: Content Automation Breaks Brand Voice
**Problem:** AI-generated posts feel generic, diluting brand trust.

**Mitigation:**
- All AI output is based on real triggers (actual reviews, actual story releases)
- Posts go through a review stage — owner can edit or suppress before publish
- Prompt templates enforce the brand's storytelling tone ("How one family used...")
- Volume cap (2/week) ensures quality over quantity

---

## Monitoring & Alerts (Recommended)

| Signal | Tool | Action |
|--------|------|--------|
| Core Web Vitals regression | GA4 / Search Console | Investigate immediately |
| Blog post auto-publish failure | Cloud Function logs | Review queue manually |
| Etsy API error rate spike | Firebase monitoring | Switch to cached data |
| Etsy redirect 404s | GA4 custom event | Update Firestore product record |
| Ranking drop > 5 positions | Search Console | Audit recent content quality |
