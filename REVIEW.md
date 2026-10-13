# burnedforambience.com - review and rebuild notes

Reviewed 13 October 2026. Live site runs Shopify with the **Debut** theme (v17.9.3), a theme Shopify retired in 2021.

## What the live site gets wrong (and what the rebuild fixes)

| # | Problem on the live site | Why it costs sales | Rebuild |
|---|---|---|---|
| 1 | Homepage is one static illustration, a one-line tagline and a "Shop now" button. No products above the fold. | Visitors from ads bounce before seeing a price. | Hero with real photography, a product card inside the hero, bestsellers within one scroll. |
| 2 | "WE ONLY SHIP TO THE REPUBLIC OF CYPRUS" in caps at the top, as a warning. | Reads as a restriction. Should read as a promise. | "Island-wide delivery, Cyprus" in the trust strip; free-delivery progress bar in the cart. |
| 3 | Navigation has 11 items including "Waks Nature collection" and "Room Spray Collection!" that link to the homepage. Two duplicate "Diffuser Refills" collections, two "monthly selection" collections (one misspelled "os"). | Confusing, and dead links kill trust. | Six categories: Candles, Diffusers, Refills, Charms, Gifts, Soaps. Every link resolves. |
| 4 | Product titles like "Original / Oud" with handle `classic-gardenia`, "OUD Scent ...Original Collection" (a duplicate), "Navy blue , white and light blue  eye shaped charm 🪬". | Looks unmanaged; impossible to search; duplicates split reviews and stock. | Clean names, one Oud, charms described consistently, emoji stripped. |
| 5 | Descriptions are missing on 6 products (Oak Fire, Jasmine, Orange Grove, Apple & Cinnamon, 500 ml refill, Oud duplicate) and inconsistent elsewhere. | No reason to buy. | Every product has a one-line scent blurb; the scent pyramid (top/heart/base), net weight, burn time and dimensions render as structured specs. |
| 6 | The refill system, the brand's best idea ("Lift it. Replace it. Relight it.") is buried inside one product description. | It's the repeat-purchase engine and nobody sees it. | Dedicated dark chapter on the homepage with the €14 vs €26 comparison; refill cross-sell on every candle page; refill upsell in the cart. |
| 7 | Charms (22 of 57 products, €48–€58, high margin) are listed under "Christmas baubles and charms 🎄🎅🏻". | The wedding/christening bomboniera market is the real opportunity and the site hides it. | Aegean-blue chapter on the homepage, "Charms" in the main nav, occasion copy, gift-wrapping promise. |
| 8 | No scent guidance. 25 refill scents, 14 diffuser scents, 15 candles, with no way to choose. | Choice paralysis. | Interactive scent finder: pick a mood, get three products with a reason. |
| 9 | No newsletter incentive, no social proof, no trust signals. | Nothing to convert the undecided. | 10% welcome offer, trust strip, "since 2016", care and delivery accordions. |
| 10 | No sticky cart, no quick add, no cart drawer. | Every add-to-cart leaves the page. | Cart drawer with quantity, free-shipping progress, upsell, and a checkout button that opens Shopify checkout with the right variants. |
| 11 | "Sold out" shown on 4 of 4 featured products on the homepage. | The first impression is "nothing available". | Sold-out items drop to the bottom of collections; bestsellers chosen from in-stock products. |
| 12 | About page is two sentences. No story, no materials, no care, no returns policy anywhere. | Luxury pricing with no justification. | Story page with values, candle-care guide, delivery and returns, contact with hours and phone. |

## What's in this folder

```
index.html      Homepage: hero, trust strip, categories, bestsellers, refill chapter,
                scent finder, charms chapter, editions, newsletter
shop.html       Collection page with filters, sort, in-stock toggle (shop.html#candles etc.)
product.html    Product page (product.html#original-amber etc.) with gallery, variants,
                scent pyramid, specs, cross-sell, accordions, sticky mobile bar
about.html      Story, values, candle care, delivery & returns, contact
ads.html        Internal: 6 ad concepts, funnel structure, hooks bank, Reels shot list,
                offer ladder and email flows
css/style.css   Design system (tokens at the top)
js/app.js       Header/footer, cart, search, scent finder, page renderers. CONFIG at the top.
js/products.js  All 57 live products with real prices, variant IDs, stock and scent data,
                pulled from the Shopify store on 13 Oct 2026
assets/img      88 product photos downloaded from the store's CDN (900 px)
```

Open `index.html` in a browser. Everything works offline. Add-to-bag, cart, search and the finder are live. "Checkout securely" builds a real Shopify cart permalink for the live store, so a test order can go through today.

## Placeholders to confirm before launch

These are written into `js/app.js` CONFIG and in copy. They are reasonable defaults, not facts from the live site:

- **Free delivery over €60.** Live site shows no threshold. Set it just above your current average order value.
- **WELCOME10 / 10% off first order.** Create the code in Shopify > Discounts and connect the form to Shopify Email or Klaviyo.
- **14-day returns, 48-hour breakage replacement.** No policy exists on the live site. Edit `about.html` to match what you actually offer.
- **Contact and newsletter forms** show a confirmation but don't send. Wire to Shopify's contact form / Klaviyo.
- **"Email me when it's back"** needs a back-in-stock app.

## Taking it live

Two routes:

1. **Shopify theme (recommended).** Move to a current theme (Dawn or a paid theme such as Prestige or Impact) and rebuild these sections with the theme editor. The CSS tokens, copy, product names, category structure and section order in this folder are the spec. Clean the product data in Shopify admin first: rename products, delete the Oud duplicate (`classic-gardenia`), delete the duplicate collections, add descriptions to the six empty products, add the scent pyramid as metafields.
2. **Headless storefront.** Keep this front end, swap `js/products.js` for the Shopify Storefront API, and keep Shopify for checkout. More work, more control.

Either way, keep Shopify for checkout. The payment methods, Shop Pay and the customer accounts are already there.

## Advertising readiness (added 13 Oct 2026)

- `js/tracking.js` loads Meta Pixel, Google tag (GA4 + Google Ads) and optionally TikTok, **only after cookie consent**. Fill in the IDs at the top of the file.
- Events fired by the site: `view_item`, `view_item_list`, `search`, `add_to_cart`, `begin_checkout`, `generate_lead`. Purchase fires on Shopify's thank-you page through the Shopify apps (Facebook & Instagram, Google & YouTube), which also send server-side events.
- UTM parameters, `fbclid` and `gclid` are stored for 30 days and appended to the Shopify checkout link, so Shopify's order attribution works.
- Every page has canonical, Open Graph and Twitter card tags. The homepage carries `Store` and `WebSite` schema; product pages inject `Product` schema with price and availability.
- `feed/products.csv` is a Merchant Center / Meta Catalog feed: 129 variants with live prices, stock and CDN images, linking to the live Shopify product URLs.
- `ads.html#setup` holds the step-by-step account setup table.

**One SEO caveat for this prototype:** product pages here use hash URLs (`product.html#amber`), which search engines index as one page. The Shopify build gives every product its own URL, so this disappears at launch.
