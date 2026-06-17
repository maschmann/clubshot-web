# Schießbuch Generator Integration & Dependency Update Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Integrate the external Schießbuch Generator service at https://generator.myclubshot.de as a dropdown navigation item, homepage CTA section, and features card, and update `@11ty/eleventy` dependency to version `3.1.6`.

**Architecture:** Extend Eleventy dynamic translations model to store generator text. Convert static nav links into a Bootstrap 5 dropdown menu in the base template layout. Add responsive CSS/HTML component layout blocks for the CTA section and features card.

**Tech Stack:** Eleventy v3, Bootstrap v5.3.8, Nunjucks templates.

## Global Constraints
- Target Date: 2026-06-17
- All external links must open in a new tab (`target="_blank" rel="noopener"`).
- German (`de`) and English (`en`) localized versions must be identical in structure and copy meaning.
- All layouts must scale gracefully from extra-small mobile screen sizes (320px width) up to desktop resolutions.

---

### Task 1: Dependency Update

**Files:**
- Modify: `package.json`

**Interfaces:**
- Produces: Updated `@11ty/eleventy` package reference at version `3.1.6`.

- [ ] **Step 1: Modify package.json to reference latest stable Eleventy version**

Change `"@11ty/eleventy": "^3.1.2"` to `"@11ty/eleventy": "^3.1.6"` under `dependencies`.

- [ ] **Step 2: Install dependencies**

Run: `npm install --silent`
Expected: Installs Eleventy v3.1.6 cleanly without lockfile conflicts.

- [ ] **Step 3: Run project build command to verify backwards compatibility**

Run: `npm run build`
Expected: Succeeds and logs writing of files using v3.1.6.

- [ ] **Step 4: Commit dependency changes**

```bash
git add package.json package-lock.json
git commit -m "chore: update eleventy to 3.1.6"
```

---

### Task 2: Translate Service & Generator Copy

**Files:**
- Modify: `src/_data/translations.js`

**Interfaces:**
- Produces: New language dictionary keys: `services`, `clubManagement`, `generatorTitle`, `generatorDesc`, `generatorCta`, `generatorFeatureTitle`, `generatorFeatureDesc`.

- [ ] **Step 1: Add new translation entries to src/_data/translations.js**

Add translation keys for German and English.

In `src/_data/translations.js`:
```javascript
// German (de) section:
services: "Dienste",
clubManagement: "Vereinsverwaltung (Funktionen)",
generatorTitle: "Schießbuch Generator",
generatorDesc: "Der kostenlose und unabhängige Schießbuch-Generator für alle Sportschützen. Erstellen Sie Ihr persönliches Schießbuch nach DSB, BDS, DSU oder BDMP Vorgaben und exportieren Sie es als druckfertiges PDF.",
generatorCta: "Schießbuch erstellen",
generatorFeatureTitle: "Kostenloser Schießbuch Generator",
generatorFeatureDesc: "Nutzen Sie unseren kostenlosen, eigenständigen Schießbuch-Generator. Erstellen Sie Ihr Schießbuch ganz einfach online nach den offiziellen Vorgaben der deutschen Schützenverbände und laden Sie es als PDF herunter — komplett ohne Anmeldung.",

// English (en) section:
services: "Services",
clubManagement: "Club Management (Features)",
generatorTitle: "Shooting Log Generator",
generatorDesc: "The free and independent shooting log generator for all sports shooters. Create your personal shooting log according to sports shooting association standards and export it as a print-ready PDF.",
generatorCta: "Create Shooting Log",
generatorFeatureTitle: "Free Shooting Log Generator",
generatorFeatureDesc: "Use our free, standalone shooting log generator. Easily create your shooting log online according to official German shooting association standards and download it as a PDF — completely registration-free.",
```

- [ ] **Step 2: Run build to verify file validity**

Run: `npm run build`
Expected: Build succeeds with 0 errors.

- [ ] **Step 3: Commit translation changes**

```bash
git add src/_data/translations.js
git commit -m "feat(i18n): add translations for services dropdown and generator service"
```

---

### Task 3: Navigation Dropdown Menu

**Files:**
- Modify: `src/_includes/layouts/base.njk`

**Interfaces:**
- Consumes: `services` and `clubManagement` from `src/_data/translations.js`.

- [ ] **Step 1: Replace direct Features nav link with Bootstrap 5 dropdown in base.njk**

Locate:
```html
<li class="nav-item">
    <a class="nav-link" href="/{{ lang }}/features/">{{ translations[lang].features }}</a>
</li>
```

Replace with:
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

- [ ] **Step 2: Validate the build**

Run: `npm run build`
Expected: Passes.

- [ ] **Step 3: Commit navigation updates**

```bash
git add src/_includes/layouts/base.njk
git commit -m "feat(nav): convert features link to services dropdown with external generator"
```

---

### Task 4: Homepage Highlight CTA Section

**Files:**
- Modify: `src/de/index.njk`
- Modify: `src/en/index.njk`

**Interfaces:**
- Consumes: `generatorTitle`, `generatorDesc`, `generatorCta` from translation dictionary.

- [ ] **Step 1: Append CTA section to German homepage (src/de/index.njk)**

Append this HTML snippet right before the closing `/div` of the template (or after the main container ends):
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

- [ ] **Step 2: Append CTA section to English homepage (src/en/index.njk)**

Add the same snippet above before the final button container's closing or outside container in `src/en/index.njk`.

- [ ] **Step 3: Run build to verify correct compilation**

Run: `npm run build`
Expected: Writes pages without errors.

- [ ] **Step 4: Commit homepage CTA changes**

```bash
git add src/de/index.njk src/en/index.njk
git commit -m "feat(home): add dedicated schiessbuch generator cta section"
```

---

### Task 5: Features Page Service Card

**Files:**
- Modify: `src/de/features.njk`
- Modify: `src/en/features.njk`

**Interfaces:**
- Consumes: `generatorFeatureTitle`, `generatorFeatureDesc`, `generatorCta` from dictionary.

- [ ] **Step 1: Add card to German features page (src/de/features.njk)**

Add this card under the existing grid of features (e.g. right before the layout grid ends):
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

- [ ] **Step 2: Add card to English features page (src/en/features.njk)**

Add the exact same snippet into `src/en/features.njk` grid.

- [ ] **Step 3: Build and verify compilation**

Run: `npm run build`
Expected: Build passes.

- [ ] **Step 4: Commit features page changes**

```bash
git add src/de/features.njk src/en/features.njk
git commit -m "feat(features): add free schiessbuch generator service card"
```

---

### Task 6: UI/UX & Content Verification

**Files:**
- Verify: All built static files in `_site/` directory

- [ ] **Step 1: Run comprehensive build**

Run: `npm run build`
Expected: Writes all files under `_site/` correctly.

- [ ] **Step 2: Validate the generated HTML files**

Verify that all external generator links have `target="_blank" rel="noopener"`.
Verify that German and English content is completely translated, contains no raw tags, and is grammatically correct.
Verify that navigation handles dark theme correctly.
