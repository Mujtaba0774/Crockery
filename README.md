# Crockery.pk — Online Store

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB) ![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black) ![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white) ![React Router](https://img.shields.io/badge/React%20Router-CA4245?style=for-the-badge&logo=reactrouter&logoColor=white) ![Tailwind CSS](https://img.shields.io/badge/Tailwind%20CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)

Front-end for **Crockery.pk**, a Pakistani online shop for crockery and tableware: dinner sets, tea and coffee sets, plates, bowls, mugs, glassware, cutlery, storage jars, kids' sets, gift sets and seasonal collections. It is a single-page React app with a shop, category and product pages, a cart, a wishlist, a mock checkout and an English/Urdu language toggle.

![Home page](https://mujtabaasif.vercel.app/assets/projects-screenshots/crockery/home.webp)

**Portfolio:** https://mujtabawd.vercel.app/

## Tech stack

| Area | Choice |
| --- | --- |
| Framework | React 19 (JavaScript / JSX) |
| Build tool | Vite 8 |
| Routing | react-router-dom 6 (`BrowserRouter`) |
| Styling | Tailwind CSS 3 plus custom classes in `src/index.css` (Sora for headings, Manrope for body) |
| Icons | lucide-react |
| State | React context (`StoreContext`, `LocaleContext`), saved to `localStorage` |

There is no backend or database. Products, translations and page text are hard-coded in `src/`.

## Getting started

Requires Node.js 20.19+ or 22.12+ (the minimum for Vite 8).

```bash
npm install
npm run dev        # http://localhost:5173
```

| Script | What it does |
| --- | --- |
| `npm run dev` | Start the Vite dev server with hot reload |
| `npm run build` | Production build into `dist/` |
| `npm run preview` | Serve the production build locally |
| `npm run lint` | Run ESLint (currently reports 9 errors; see "Known issues") |

## Project structure

```
src/
├── main.jsx               Entry point: wraps <App> in LocaleProvider and StoreProvider
├── App.jsx                Layout (PromoBar, Header, Footer) and all <Route>s
├── index.css              Tailwind directives, Google Fonts, custom classes (surface-card, gold-pill, btn-*, animations)
├── context/
│   ├── StoreContext.jsx   Cart and wishlist state + actions, saved to localStorage
│   └── LocaleContext.jsx  Current language (en / ur), t() lookup, toggle()
├── data/
│   ├── products.js        ALL products (12 items)
│   └── translations.js    English and Urdu strings used by t()
├── components/            PromoBar, Header, Hero, BrandStrip, ProductSection, ProductCard,
│                          CategorySection, Testimonials, Newsletter, Footer
└── pages/                 One component per route (see "Pages and URLs")
public/
├── favicon.svg, icons.svg
└── products/              4 product photos (not referenced by the code yet)
```

`PROJECT_STRUCTURE.md` describes the original "SHOP.CO" template this project started from. It is out of date; this README replaces it.

## Pages and URLs

Routing uses react-router's `BrowserRouter`, defined in [src/App.jsx](src/App.jsx). Every page has its own URL, and the browser back and forward buttons work.

| URL | Page | Component |
| --- | --- | --- |
| `/` | Home | `Home` |
| `/shop` | All products with filters | `Shop` |
| `/category/:slug` | Products in one category, e.g. `/category/dinner-sets` | `CategoryPage` |
| `/product/:id` | Product detail, e.g. `/product/bone-china-dinner-4pc-01` | `ProductPage` |
| `/cart` | Cart | `Cart` |
| `/wishlist` | Wishlist | `Wishlist` |
| `/checkout` | Delivery details, payment method, order summary | `Checkout` |
| `/about` | About us | `About` |
| `/contact` | Contact details | `Contact` |
| `/faq` | FAQ | `FAQ` |
| `/returns` | Return and exchange policy | `ReturnPolicy` |

There is no catch-all route. An unknown URL shows the header and footer with an empty page.

Category slugs are mapped to category names in `categoryMap` at the top of [src/pages/CategoryPage.jsx](src/pages/CategoryPage.jsx): `dinner-sets`, `tea-coffee-sets`, `plates-dishes`, `bowls`, `cups-mugs`, `serving-cookware`, `glassware`, `cutlery-accessories`, `storage-canisters`, `kids-crockery`, `luxury-gifts`, `seasonal-festive`.

## Header navigation

- **Shop**, **On Sale** and **New Arrivals** all link to `/shop`. The chevron next to "Shop" does not open a dropdown.
- **Brands** links to `/about`.
- On the right: the **UR / EN** language toggle, **cart** and **wishlist** icons with item badges, and an **account** icon that does nothing yet.
- Below 768px wide, the links move into a menu opened by a hamburger button. See "Known issues": on phones this button is currently pushed off-screen.

Category pages are reached from the six tiles in the home page "Featured Categories" section. The other six categories have URLs but no links.

## Content

All content lives in code:

- **Products**: [src/data/products.js](src/data/products.js). Each product has `id`, `name`, `category`, `subcategory`, `material`, `pieces`, `price` (PKR), `currency`, `images` (URLs), `colors`, `stock`, `rating`, `description` and `urDescription`.
- **Translations**: [src/data/translations.js](src/data/translations.js). Only header, hero, section-title and promo-bar strings are translated.
- **Home-page category tiles**: the `categories` array in [src/components/CategorySection.jsx](src/components/CategorySection.jsx).
- **Testimonials**: [src/components/Testimonials.jsx](src/components/Testimonials.jsx).
- **About, Contact, FAQ, Returns**: text written directly in each page component.

### Adding a product

1. Add an object to the array in `src/data/products.js` with a unique `id`. That `id` becomes the URL `/product/<id>`.
2. Set `category` to exactly one of the names in `categoryMap`. For example, use `'Tea & Coffee Sets'`, not `'Tea and Coffee'`, or the product won't appear on its category page.
3. Put photos in `public/products/` and reference them as `/products/<file>.jpg`.

### Adding a category

1. Add `slug: 'Name'` to `categoryMap` in `CategoryPage.jsx`.
2. Optionally add a tile to `categories` in `CategorySection.jsx` so it can be reached from the home page.

## How state is stored

| Data | Where | Key |
| --- | --- | --- |
| Cart (`[{ ...product, qty }]`) | `localStorage` | `store_cart` |
| Wishlist (`[product]`) | `localStorage` | `store_wishlist` |
| Language (`en` / `ur`) | `localStorage` | `locale` |

The cart and wishlist survive a refresh, but only in the same browser. There are no user accounts and no server.

## Current feature status

| Feature | Status |
| --- | --- |
| Home page sections | Working |
| Shop filters (material, colour, max price, minimum pieces, in-stock only) | Working |
| Category and product pages | Working, but 8 of 12 product photos are broken (see below) |
| Cart (add, change quantity, remove, clear, subtotal) | Working, saved in the browser |
| Wishlist (add and remove from cards and product page) | Working, saved in the browser |
| Checkout | Mock only: shows an alert and empties the cart. No order is sent. |
| English / Urdu toggle | Partial: about 20 strings are translated, and the layout does not switch to right-to-left |
| Header search | Not connected |
| Newsletter form | Not connected (submitting reloads the page) |
| Account, testimonial arrows, social icons | Buttons with no action |

## Known issues

1. **Broken product images.** 9 of the 13 Unsplash image URLs return 404. This affects 8 of the 12 products, the second gallery image of the Classic Porcelain Dinner Set, and the Bowls and Luxury / Gift Sets tiles on the home page. The 4 photos in `public/products/` belong to the 4 products whose images still load, but the code does not use them yet.
2. **Stock photos show food.** The pictures that do load are food photos, such as ribs on the hero, not the crockery being sold.
3. **The mobile menu can't be reached.** At 390px wide, the header is 445px wide. The hamburger button sits off the right edge, and `html { overflow-x: hidden }` stops users from scrolling to it.
4. **Checkout has no validation.** Orders can be "placed" with an empty name, address or cart.
5. **Related products is always empty.** The product page matches on `subcategory`, and every product has a different one.
6. **Cart badge** counts cart lines, not items. Two of the same set shows as 1.
7. **The "Serving & Cooking Ware" category** has no products.
8. **Footer** has 6 links to `#`, and uses plain `<a href>` tags, so each click reloads the whole page.
9. **Lint** reports 9 errors in the two context files: unused variables, an empty `catch` block, and fast-refresh export warnings. The build still succeeds.
10. **Page title** in `index.html` is still `shopco-frontend`.

## Deployment

Run `npm run build` and deploy the `dist/` folder to any static host (Vercel, Netlify, Cloudflare Pages, etc.).

Because the app uses `BrowserRouter`, the host must send every path to `index.html`. Otherwise, opening or refreshing a deep link such as `/product/...` returns a 404. No such rule is set up yet. On Vercel, add a `vercel.json`:

```json
{ "rewrites": [{ "source": "/(.*)", "destination": "/" }] }
```

On Netlify, add `public/_redirects` containing `/* /index.html 200`.

## Brand

| Token | Hex | Use |
| --- | --- | --- |
| Deep red | `#9b3e2e` (`red-600`) | Accent, buttons, links, focus outline |
| Dark red | `#7a2e23` (`red-700`) | Logo gradient, headings |
| Warm white | `#faf9f7` (`primary-50`) | Page background |
| Soft beige | `#f5f3f1` (`primary-100`) | Subtle surfaces |
| Charcoal | `#2f2b27` (`primary-800`) | Body text |

These colours are defined as Tailwind `primary-*` and `red-*` scales in `tailwind.config.js`. Headings use **Sora** and body text uses **Manrope**.
