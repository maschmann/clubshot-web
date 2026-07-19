# Design Specification: Clubshot Training Timer Integration & App Privacy Policy

**Date:** 2026-07-19  
**Status:** Approved  
**Author:** Gemini CLI  

---

## 1. Project Background & Context
Clubshot-web is a static landing and informational page for "Clubshot", a smart club management solution optimized for German shooting associations (DSB and BDS). The site is built with Eleventy (11ty) and styled with Bootstrap 5, supporting German (`de`) and English (`en`) via a custom translation dictionary file (`src/_data/translations.js`).

We are introducing:
1. A dedicated product page for **Clubshot Training Timer**, an Android mobile app currently in development. It features high-precision acoustic shot-timer functionality, split-time analysis, association rule compliance (BDS & DSB), and offline-first storage.
2. A dedicated **Datenschutzerklärung (Privacy Policy)** page in German for the mobile app, ensuring full GDPR compliance.

---

## 2. Goals & Scope
1. **Dedicated Product Page (`/de/training-timer/` and `/en/training-timer/`):**
   - High-tech, engaging hero layout highlighting Android-only development status ("Coming Soon") and its 100% offline-first nature.
   - Structured sections detailing its 5 key features (Shot-Timer DSP, BDS/DSB compliance, split-time analytics, flexible scoring, and offline privacy).
   - Styled with standard Bootstrap 5 utilities and clean visual structures.
2. **Datenschutzerklärung (Privacy Policy) (`/de/datenschutz-app/`):**
   - Fully legally compliant GDPR document explaining local-only storage, real-time microphone analysis (DSP) without saving raw audio, absence of trackers, and complete user control.
3. **Navigation and Layout Integration:**
   - Expand the main "Dienste / Services" navbar dropdown to cleanly direct users to:
     - Vereinsverwaltung (Club Management)
     - Training Timer App
     - Schießbuch-Generator (Free Tool)
   - Add App Privacy Policy links in the footer.
   - Update `src/_data/translations.js` with new localized strings.

---

## 3. Technical Changes

### A. Translation Dictionary Update (`src/_data/translations.js`)
We will add keys for navigation labels and general metadata:

```javascript
// German (de) keys
timerTitle: "Training Timer App",
timerSubtitle: "Shot-Timer & Ergebnisrechner",
privacyAppTitle: "Datenschutz (App)",
timerComingSoon: "In Entwicklung (Coming Soon)",
timerHighlightTitle: "Neu: Clubshot Training Timer",
timerHighlightDesc: "Die ultimative, datenschutzfreundliche Trainingsbegleitung auf dem Schießstand mit hochpräziser akustischer Schusserfassung und split-time Analyse.",

// English (en) keys
timerTitle: "Training Timer App",
timerSubtitle: "Shot-Timer & Score Calculator",
privacyAppTitle: "Privacy (App)",
timerComingSoon: "Under Development (Coming Soon)",
timerHighlightTitle: "New: Clubshot Training Timer",
timerHighlightDesc: "The ultimate, privacy-friendly training companion on the shooting range with high-precision acoustic shot detection and split-time analysis.",
```

### B. Navbar & Footer Updates (`src/_includes/layouts/base.njk`)
We will expand the navbar dropdown list to include the Training Timer:

```html
<ul class="dropdown-menu shadow-sm border-0 mt-2" aria-labelledby="servicesDropdown">
    <li>
        <a class="dropdown-item py-2" href="/{{ lang }}/features/">{{ translations[lang].clubManagement }}</a>
    </li>
    <li>
        <a class="dropdown-item py-2 fw-medium text-primary" href="/{{ lang }}/training-timer/">{{ translations[lang].timerTitle }}</a>
    </li>
    <li><hr class="dropdown-divider my-1"></li>
    <li>
        <a class="dropdown-item py-2 text-muted" href="https://generator.myclubshot.de" target="_blank" rel="noopener">
            {{ translations[lang].generatorTitle }} ↗
        </a>
    </li>
</ul>
```

The footer will include a direct link to the App's Privacy Policy:
```html
<p class="mb-0">
    <a href="/{{ lang }}/imprint/" class="text-decoration-none text-muted small me-3">{{ translations[lang].imprint }}</a>
    <a href="/de/datenschutz-app/" class="text-decoration-none text-muted small">{{ translations[lang].privacyAppTitle }}</a>
</p>
```

### C. Homepage Promo Card (`src/de/index.njk` & `src/en/index.njk`)
To drive traffic, we will append a beautiful promo card for the Training Timer app in a new section of the landing page.

### D. New Dedicated Pages
1. **`src/de/training-timer.njk` & `src/en/training-timer.njk`**: Layout referencing `base.njk`. Incorporates the rich feature list.
2. **`src/de/datenschutz-app.njk`**: Layout referencing `base.njk`. Stating 100% offline-first processing under EDV Beratung Aschmann.

---

## 4. Verification Plan
- Build the static site using Eleventy (`npm run build`).
- Verify navigation dropdown triggers correctly.
- Verify that both localized `/training-timer/` pages load beautifully in both light and dark modes.
- Verify that `/de/datenschutz-app/` renders correctly and is linked from the footer.
- Ensure that the English version `/en/datenschutz-app/` is handled gracefully or directs to the official German document.
