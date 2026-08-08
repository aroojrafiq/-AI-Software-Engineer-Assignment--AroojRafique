# Purelane Shopify Theme – Troopod AI Engineer Assignment

This repository contains a custom Shopify theme built as part of the Troopod AI Product Engineer take‑home assignment. The goal was to convert a static HTML design prototype (`purelane-homepage.html`) into five fully functional, merchant‑editable Shopify sections using the Dawn theme as a foundation.

The result is a pixel‑perfect reproduction of the V2 light‑theme prototype that pulls real Shopify product data, survives reordering in the Theme Editor, and adheres to strict performance and accessibility standards.

---

## 🚀 Live Demo

- **Store URL:** [https://purelane-dev-assignment.myshopify.com](https://purelane-dev-assignment.myshopify.com)  
- **Password:** `shausk`

---

## 📦 Key Features

- **Pixel‑Perfect Design** – Replicates the V2 light‑theme, including glassmorphism effects (`glass`, `glass-2`), custom Outfit/Inter typography, and responsive breakpoints from 375px up.
- **Merchant‑Editable** – All text, badges, product selections, and prices are configurable via the Theme Editor. No hardcoded content.
- **Real Shopify Data** – Products, prices, discounts, and `custom.rating` / `custom.review_count` metafields are pulled directly from the platform.
- **Five Core Sections** – Hero, Shop Grid, Best Selling Combos, Bundles, and Reviews Marquee.
- **Reusable Components** – Product cards are abstracted into a `product-card.liquid` snippet for consistency across the Shop and Combos sections.
- **Theme Editor Resilient** – Section-scoped CSS ensures that adding, removing, or reordering sections never breaks other parts of the theme.
- **Performance & Accessibility** – Lazy loading, CSS `will-change`, `prefers-reduced-motion` fallbacks, and `aria-label` attributes are implemented throughout.
- **Web Component Carousel** – The Hero section uses a custom `<purelane-hero>` Web Component to ensure the JavaScript lifecycle survives the Shopify Theme Editor.

---

## 🛠️ Tech Stack

- **Shopify Liquid** – Template and schema logic
- **HTML5 / CSS3 / Vanilla JS** – Markup, styling, and interactivity
- **Dawn Theme** – Minimal, unmodified base theme
- **Google Fonts** – Outfit & Inter
- **Shopify Metafields** – For dynamic ratings and review counts

---

## 🧩 Metafield Definitions

To power the dynamic star ratings and review counts on product cards, create the following Product Metafields:

| Namespace & Key | Type | Purpose |
| :--- | :--- | :--- |
| `custom.rating` | Number / Decimal | Average rating (e.g. `4.8`) |
| `custom.review_count` | Number / Integer | Total review count (e.g. `124`) |

---

## 📁 Repository Structure
├── assets/
│ └── base.css – Global styles (glass, reveal, scenes)
├── snippets/
│ ├── product-card.liquid – Reusable product card
│ └── scenes-background.liquid – Static mint‑green background layer
├── sections/
│ ├── purelane-hero.liquid
│ ├── purelane-shop.liquid
│ ├── purelane-combos.liquid
│ ├── purelane-bundles.liquid
│ └── purelane-reviews.liquid
├── layout/
│ └── theme.liquid – Master layout with global reveal script
└── README.md

## ⚙️ Setup Instructions

1. **Upload the theme** to your Shopify Development store (or any store running Dawn).
2. **Create Metafields** (as defined above) in your Shopify Admin.
3. **Seed Products** – Upload at least 8 products, including:
   - One product marked "Sold Out"
   - One product with **no featured image**
   - One product with a **very long title**
4. **Configure Sections** – In the Theme Editor, add the five Purelane sections and assign the appropriate Collections and Block products.

---

## 🧠 Architectural Decisions & Trade-offs

### 1. Section‑Scoped CSS vs Global Styles
I deliberately kept section‑specific CSS prefixed with `purelane-` inside each section's `{% style %}` block. While this results in some repetition, it ensures that if a merchant deletes or reorders a section in the Theme Editor, the other sections remain fully functional. Only truly universal classes (`.glass`, `.rv`, `:focus-visible`) are centralized in `assets/base.css`.

### 2. Web Component for Hero Carousel
The Hero carousel uses a vanilla `<purelane-hero>` Web Component. This binds the carousel's `IntersectionObserver` and timer logic directly to the DOM element. This prevents the JavaScript from losing its references when the Shopify Theme Editor dynamically adds, removes, or reorders blocks inside the section.

### 3. Static Background vs Full Water Animation
The original prototype included a complex SVG water animation and scroll‑driven scene crossfade. To prioritize Core Web Vitals and ensure the background remains stable regardless of section order, I replaced this with a static mint‑green gradient (`.s1`). The `data-scene` attributes remain present on the sections—they can easily be re‑enabled if the merchant chooses to implement a lighter dynamic version later.

### 4. Cart Form Implementation
To bypass a known Shopify Liquid parser bug (`new_comment` error) that caused the Bundles section to crash, the add‑to‑cart buttons are implemented as standard HTML `<form>` elements with hidden variant inputs rather than the `{% form 'product' %}` Liquid tag. This preserves the native Shopify cart functionality while keeping the codebase bug‑free.

---

## 🤖 Reflections on AI Workflow

**What I delegated:**  
I used AI to convert the static HTML/CSS into Shopify Liquid schemas and to quickly extract the complex SVG data.

**Where it failed me:**  
- **Environment Blindness:** The AI struggled with the constraints of the Dawn theme and Shopify's custom elements. It didn't recognize that removing `<product-form>` wrappers would break the native AJAX cart drawer.
- **Over‑correcting:** The AI repeatedly tried to "improve" the source design by dropping `.rv` reveal classes or changing the `44ch` typographic limits. I had to enforce the "pixel‑accurate" rule strictly to prevent it from rewriting the CSS architecture.

**What I'd systematise for a larger project:**  
I would build a "Dawn Context" prompt library that feeds the AI a strict ruleset (e.g., "Always preserve `44ch` widths", "Never remove `rv` classes") before generating code.

---

## 🔮 Future Improvements

With more time, I would:
1. Abstract the product card, combo card, and bundle tier logic into a single, highly reusable snippet to keep the codebase DRY.
2. Create a "Review" Metaobject to allow the marketing team to manage a central database of reviews instead of using static Theme Editor blocks.
3. Refactor the background water animation to use CSS `@property` or a WebGL shader to reduce DOM repaints and improve battery life on mobile.

---

## 📝 License

This project is submitted solely for the Troopod AI Product Engineer assignment and is not intended for public distribution or commercial use. All rights remain with the author.
