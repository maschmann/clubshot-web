# Design Specification: Schießbuch Generator Integration & Dependency Update

**Date:** 2026-06-17  
**Status:** Approved  
**Author:** Gemini CLI  

---

## 1. Project Background & Context
Clubshot-web is a static website built with Eleventy (11ty) and styled with Bootstrap 5. It serves as the landing and informational page for Clubshot, a smart club management solution optimized for German shooting associations (DSB and BDS). The codebase supports two languages: German (`de`) and English (`en`), handled via a custom translation dictionary file (`src/_data/translations.js`).

---

## 2. Goals & Scope
1. **Schießbuch Generator Integration (Approach A):**
   - Introduce a "Services" (Dienste) dropdown in the main navigation bar.
   - Retain access to the core features page under "Vereinsverwaltung" (Club Management).
   - Add a direct external link to `https://generator.myclubshot.de` ("Schießbuch Generator") opening in a new tab.
   - Add a high-visibility, dedicated Call-to-Action (CTA) section on the Home page promoting the Schießbuch Generator as a free, independent tool.
   - Add a descriptive service card on the Features page for the Schießbuch Generator.
2. **Dependency Updates:**
   - Update `@11ty/eleventy` to version `3.1.6`.
   - Ensure the site builds cleanly after updating and that no styling/JS behaviors are broken.
3. **UI/UX & Mobile Compatibility Check:**
   - Validate accessibility, mobile layout, and alignment.
   - Ensure light/dark mode toggling works seamlessly across the navigation changes.

---

## 3. Technical Changes

### A. Translation Entries (`src/_data/translations.js`)
Add key-value pairs for both German and English:

```javascript
// German (de)
services: "Dienste",
clubManagement: "Vereinsverwaltung (Funktionen)",
generatorTitle: "Schießbuch Generator",
generatorDesc: "Der kostenlose und unabhängige Schießbuch-Generator für alle Sportschützen. Erstellen Sie Ihr persönliches Schießbuch nach DSB, BDS, DSU oder BDMP Vorgaben und exportieren Sie es als druckfertiges PDF.",
generatorCta: "Schießbuch erstellen",
generatorFeatureTitle: "Kostenloser Schießbuch Generator",
generatorFeatureDesc: "Nutzen Sie unseren kostenlosen, eigenständigen Schießbuch-Generator. Erstellen Sie Ihr Schießbuch ganz einfach online nach den offiziellen Vorgaben der deutschen Schützenverbände und laden Sie es als PDF herunter — komplett ohne Anmeldung.",

// English (en)
services: "Services",
clubManagement: "Club Management (Features)",
generatorTitle: "Shooting Log Generator",
generatorDesc: "The free and independent shooting log generator for all sports shooters. Create your personal shooting log according to sports shooting association standards and export it as a print-ready PDF.",
generatorCta: "Create Shooting Log",
generatorFeatureTitle: "Free Shooting Log Generator",
generatorFeatureDesc: "Use our free, standalone shooting log generator. Easily create your shooting log online according to official German shooting association standards and download it as a PDF — completely registration-free.",
```

### B. Navbar Dropdown (`src/_includes/layouts/base.njk`)
Replace the direct `nav-link` pointing to the Features page:

```html
<li class="nav-item">
    <a class="nav-link" href="/{{ lang }}/features/">{{ translations[lang].features }}</a>
</li>
```

With a Bootstrap 5 dropdown element:

```html
<li class="nav-item dropdown">
    <a class="nav-link dropdown-toggle" href="#" id="servicesDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
        {{ translations[lang].services }}
    </a>
    <ul class="dropdown-menu" aria-labelledby="servicesDropdown">
        <li>
            <a class="dropdown-item" href="/{{ lang }}/features/">{{ translations[lang].clubManagement }}</a>
        </li>
        <li><hr class="dropdown-divider"></li>
        <li>
            <a class="dropdown-item" href="https://generator.myclubshot.de" target="_blank" rel="noopener">
                {{ translations[lang].generatorTitle }} ↗
            </a>
        </li>
    </ul>
</li>
```

### C. Homepage Highlight Section (`src/de/index.njk` & `src/en/index.njk`)
Append a full-width call-to-action block immediately below the 3-column feature list container:

```html
<div class="bg-body-tertiary py-5 border-top border-bottom">
    <div class="container">
        <div class="row align-items-center py-4">
            <div class="col-lg-8 text-center text-lg-start mb-4 mb-lg-0">
                <span class="badge bg-primary rounded-pill mb-2">Free Tool</span>
                <h2 class="fw-bold mb-3">{{ translations[lang].generatorTitle }}</h2>
                <p class="lead text-muted mb-0">{{ translations[lang].generatorDesc }}</p>
            </div>
            <div class="col-lg-4 text-center text-lg-end">
                <a href="https://generator.myclubshot.de" target="_blank" rel="noopener" class="btn btn-primary btn-lg rounded-pill px-5 py-3 shadow-sm">
                    {{ translations[lang].generatorCta }} ↗
                </a>
            </div>
        </div>
    </div>
</div>
```

### D. Features Page Service Card (`src/de/features.njk` & `src/en/features.njk`)
Add the Schießbuch Generator card to the layout grid:

```html
<div class="col-md-6">
    <div class="p-4 border border-primary border-opacity-25 rounded-3 h-100 bg-body shadow-sm d-flex flex-column justify-content-between">
        <div>
            <div class="fs-1 mb-3 text-primary">📖</div>
            <h3 class="fs-4">{{ translations[lang].generatorFeatureTitle }}</h3>
            <p class="text-muted">{{ translations[lang].generatorFeatureDesc }}</p>
        </div>
        <div class="mt-4">
            <a href="https://generator.myclubshot.de" target="_blank" rel="noopener" class="btn btn-outline-primary rounded-pill px-4">
                {{ translations[lang].generatorCta }} ↗
            </a>
        </div>
    </div>
</div>
```

---

## 4. Quality & UX Requirements
- **Responsive Layout:** Ensure the dropdown collapse button works correctly on mobile viewports.
- **Active Navigation States:** Verify that navigation items retain contrast and readability.
- **Light/Dark Theme Integration:** Test that background elements, dropdown background color, borders, and text update properly when switching themes.
- **Links Verification:** All external links must have `target="_blank"` and `rel="noopener"`.
