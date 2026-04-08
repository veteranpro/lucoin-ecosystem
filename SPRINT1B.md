# Sprint 1b — Handoff Doc

## Status
Landing page is live in `veteranpro/lucoin-ecosystem` repo.
Waitlist form uses a **placeholder JS handler** — needs Beehiiv embed swap.

---

## 1. Beehiiv Waitlist Integration

### Where the placeholder lives
`index.html` lines 254-289 contain two pieces to replace:

**A) The HTML form** (lines 254-271):
```html
<form id="waitlist-form" class="flex flex-col sm:flex-row gap-3 mb-4">
  <input
    type="email"
    required
    placeholder="your@email.com"
    class="flex-1 bg-card border border-white/10 text-white placeholder-muted/50 px-5 py-4 rounded-brand text-base focus:outline-none focus:border-gold transition-colors duration-200"
  >
  <button
    type="submit"
    class="bg-gold text-dark font-bold px-8 py-4 rounded-brand text-base hover:brightness-110 transition-all duration-200 shrink-0"
  >
    Join the waitlist
  </button>
</form>
<div id="waitlist-confirmation" class="hidden text-gold font-semibold text-lg py-3">
  You're on the list. &#10003;
</div>
<p class="text-muted/50 text-sm mt-2">No spam. No pitch deck. Just a founder who wants to build this right.</p>
```

**B) The JS handler** (lines 280-289):
```html
<script>
  document.getElementById('waitlist-form').addEventListener('submit', function(e) {
    e.preventDefault();
    var email = this.querySelector('input[type="email"]').value;
    if (email) {
      this.classList.add('hidden');
      document.getElementById('waitlist-confirmation').classList.remove('hidden');
    }
  });
</script>
```

### What to replace with
Replace **both** blocks (A and B) with the Beehiiv embed. Typical format:

```html
<iframe
  src="https://embeds.beehiiv.com/YOUR_PUBLICATION_ID"
  data-test-id="beehiiv-embed"
  width="100%"
  height="320"
  frameborder="0"
  scrolling="no"
  style="border-radius: 12px; background: transparent;"
></iframe>
<p class="text-muted/50 text-sm mt-4">No spam. No pitch deck. Just a founder who wants to build this right.</p>
```

Replace `YOUR_PUBLICATION_ID` with the actual ID from Beehiiv dashboard:
Settings > Embeds > Copy embed code > extract the UUID.

### Styling note
Beehiiv embeds render inside an iframe. The dark background may not match automatically.
In Beehiiv dashboard: Publication > Design > set background to `#0a0a0a`, accent to `#F5C518`, text to `#ffffff`.

---

## 2. OG Image Setup

### Current state
`index.html` lines 12 and 18 reference a placeholder URL:
```
https://lucoinecosystem.com/og-image.png
```

### What to do
1. Copy `lucoin_coin_512.png` from `C:\Users\Kam\OneDrive\Declaration\lucoin\` (or create a 1200x630 OG banner using the coin logo + "LuCoin Ecosystem" text)
2. Save as `og-image.png` in the repo root
3. Update the URL in both meta tags to match the deployed domain:
   - If Netlify: `https://lucoin-ecosystem.netlify.app/og-image.png`
   - If custom domain: `https://lucoinecosystem.com/og-image.png`
4. Also update `og:url` on line 13 to the actual live URL

---

## 3. Custom Domain Setup (Netlify)

### Prerequisites
- Domain registered (e.g. `lucoinecosystem.com`)
- Netlify site connected to `veteranpro/lucoin-ecosystem` repo

### Steps
1. **Connect repo to Netlify:**
   - Log in to netlify.com
   - "Add new site" > "Import an existing project" > GitHub > `veteranpro/lucoin-ecosystem`
   - Build command: (leave blank, it's a static site)
   - Publish directory: `.` (root)
   - Deploy

2. **Add custom domain:**
   - Site settings > Domain management > Add custom domain
   - Enter `lucoinecosystem.com`
   - Netlify will provide DNS records

3. **Configure DNS (at your registrar):**
   - Add `A` record: `@` -> `75.2.60.5` (Netlify load balancer)
   - Add `CNAME` record: `www` -> `lucoin-ecosystem.netlify.app`
   - Or: change nameservers to Netlify's (recommended for auto-SSL)

4. **Enable HTTPS:**
   - Site settings > Domain management > HTTPS > Verify DNS > Provision certificate
   - Netlify auto-provisions Let's Encrypt SSL (may take a few minutes after DNS propagates)

5. **Update meta tags:**
   - Replace all `https://lucoinecosystem.com` references in `index.html` with the final live URL
   - This includes `og:url`, `og:image`, and `twitter:image`

---

## File reference

| File | Lines | What |
|------|-------|------|
| `index.html` | 254-267 | Placeholder waitlist form HTML |
| `index.html` | 268-271 | Confirmation message + disclaimer |
| `index.html` | 280-289 | Placeholder JS submit handler |
| `index.html` | 12 | `og:image` URL |
| `index.html` | 13 | `og:url` |
| `index.html` | 18 | `twitter:image` URL |

## Assets on disk

| File | Location |
|------|----------|
| Coin 1024px | `C:\Users\Kam\OneDrive\Declaration\lucoin\lucoin_coin_1024.png` |
| Coin 512px | `C:\Users\Kam\OneDrive\Declaration\lucoin\lucoin_coin_512.png` |
| Logo dark bg | `C:\Users\Kam\OneDrive\Declaration\lucoin\lucoin_logo_dark.png` |
| Logo white bg | `C:\Users\Kam\OneDrive\Declaration\lucoin\lucoin_logo_white.png` |
| Symbol SVG | `C:\Users\Kam\OneDrive\Declaration\lucoin\lucoin_symbol.svg` |
| Favicons | `C:\Users\Kam\OneDrive\Declaration\lucoin\favicon_*.png` |
