# Clubshot Training Timer Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create and integrate a dedicated product page for the Clubshot Training Timer Android app, as well as its German Datenschutzerklärung (App Privacy Policy).

**Architecture:** Add static pages using 11ty (.njk templates), integrate translations in custom translation dictionary file (`src/_data/translations.js`), update common layout header/footer, and build with 11ty.

**Tech Stack:** 11ty (Eleventy), Nunjucks (njk), Bootstrap 5, HTML, CSS.

## Global Constraints
- Target 11ty output structure under `_site/`.
- Maintain standard Bootstrap 5 classes and responsive utility structure.
- Adhere strictly to the dual language setup (`de` and `en`).
- Do not commit changes to git unless explicitly asked.

---

### Task 1: Translations & Layout Updates

**Files:**
- Modify: `src/_data/translations.js`
- Modify: `src/_includes/layouts/base.njk`

**Interfaces:**
- Consumes: Existing base layout and translations module.
- Produces: Navigation menu with expanded dropdown and footer with App Privacy Policy link.

- [ ] **Step 1: Edit translations.js to add navigation, homepage promo, and header labels**

We need to add translation keys for `timerTitle`, `timerSubtitle`, `privacyAppTitle`, `timerComingSoon`, `timerHighlightTitle`, and `timerHighlightDesc`.

Replace the content in `src/_data/translations.js` with the updated lists:

```javascript
    de: {
        // ... existing de translations ...
        timerTitle: "Training Timer App",
        timerSubtitle: "Shot-Timer & Ergebnisrechner",
        privacyAppTitle: "Datenschutz (App)",
        timerComingSoon: "In Entwicklung (Coming Soon)",
        timerHighlightTitle: "Neu: Clubshot Training Timer",
        timerHighlightDesc: "Die ultimative, datenschutzfreundliche Trainingsbegleitung auf dem Schießstand mit hochpräziser akustischer Schusserfassung und split-time Analyse.",
        // ... existing de translations ...
    },
    en: {
        // ... existing en translations ...
        timerTitle: "Training Timer App",
        timerSubtitle: "Shot-Timer & Score Calculator",
        privacyAppTitle: "Privacy (App)",
        timerComingSoon: "Under Development (Coming Soon)",
        timerHighlightTitle: "New: Clubshot Training Timer",
        timerHighlightDesc: "The ultimate, privacy-friendly training companion on the shooting range with high-precision acoustic shot detection and split-time analysis.",
        // ... existing en translations ...
    }
```

- [ ] **Step 2: Update navbar dropdown and footer in base.njk**

Locate `<ul class="dropdown-menu shadow-sm border-0 mt-2"` in `src/_includes/layouts/base.njk` and insert the link for the Training Timer page.
Locate the footer in `src/_includes/layouts/base.njk` and add a link to the App's privacy policy next to the general Impressum.

```html
<!-- Inside the navbar dropdown in src/_includes/layouts/base.njk -->
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

<!-- Inside the footer in src/_includes/layouts/base.njk -->
<footer class="bg-body-tertiary py-4 mt-5">
    <div class="container text-center">
        <p class="mb-2 text-muted">&copy; 2026 Clubshot. {{ translations[lang].footerRight }}</p>
        <p class="mb-0">
            <a href="/{{ lang }}/imprint/" class="text-decoration-none text-muted small me-3">{{ translations[lang].imprint }}</a>
            <a href="/de/datenschutz-app/" class="text-decoration-none text-muted small">{{ translations[lang].privacyAppTitle }}</a>
        </p>
    </div>
</footer>
```

- [ ] **Step 3: Run Eleventy compilation to verify configuration integrity**

Run: `npm run build`
Expected: Succeeds without errors.

---

### Task 2: Homepage Promos

**Files:**
- Modify: `src/de/index.njk`
- Modify: `src/en/index.njk`

**Interfaces:**
- Consumes: Translations dictionary values `timerHighlightTitle`, `timerHighlightDesc`, `timerComingSoon`, `timerTitle`.
- Produces: Beautiful, responsive promo sections on the landing page for both German and English sites.

- [ ] **Step 1: Append the App promo block at the bottom of src/de/index.njk**

Open `src/de/index.njk` and add the promo section at the very end of the file:

```html
<div class="bg-body py-5 border-bottom">
    <div class="container">
        <div class="row align-items-center py-4 flex-row-reverse">
            <div class="col-lg-8 text-center text-lg-start mb-4 mb-lg-0 ps-lg-5">
                <span class="badge bg-danger rounded-pill mb-2 px-3 py-2 fw-semibold">{{ translations[lang].timerComingSoon }}</span>
                <h2 class="fw-bold mb-3 display-6">{{ translations[lang].timerHighlightTitle }}</h2>
                <p class="lead text-muted mb-4 fs-5">{{ translations[lang].timerHighlightDesc }}</p>
                <div class="d-flex flex-wrap justify-content-center justify-content-lg-start gap-3">
                    <a href="/{{ lang }}/training-timer/" class="btn btn-outline-primary btn-lg rounded-pill px-4 py-2 fs-6 fw-semibold">
                        Mehr erfahren
                    </a>
                </div>
            </div>
            <div class="col-lg-4 text-center">
                <div class="p-5 bg-body-tertiary rounded-4 border shadow-sm d-inline-block">
                    <span class="fs-1">⏱️</span>
                    <h3 class="fs-4 mt-3 mb-1">Clubshot App</h3>
                    <p class="text-muted small mb-0">Coming Soon für Android</p>
                </div>
            </div>
        </div>
    </div>
</div>
```

- [ ] **Step 2: Append the App promo block at the bottom of src/en/index.njk**

Open `src/en/index.njk` and append the corresponding English promo block:

```html
<div class="bg-body py-5 border-bottom">
    <div class="container">
        <div class="row align-items-center py-4 flex-row-reverse">
            <div class="col-lg-8 text-center text-lg-start mb-4 mb-lg-0 ps-lg-5">
                <span class="badge bg-danger rounded-pill mb-2 px-3 py-2 fw-semibold">{{ translations[lang].timerComingSoon }}</span>
                <h2 class="fw-bold mb-3 display-6">{{ translations[lang].timerHighlightTitle }}</h2>
                <p class="lead text-muted mb-4 fs-5">{{ translations[lang].timerHighlightDesc }}</p>
                <div class="d-flex flex-wrap justify-content-center justify-content-lg-start gap-3">
                    <a href="/{{ lang }}/training-timer/" class="btn btn-outline-primary btn-lg rounded-pill px-4 py-2 fs-6 fw-semibold">
                        Learn More
                    </a>
                </div>
            </div>
            <div class="col-lg-4 text-center">
                <div class="p-5 bg-body-tertiary rounded-4 border shadow-sm d-inline-block">
                    <span class="fs-1">⏱️</span>
                    <h3 class="fs-4 mt-3 mb-1">Clubshot App</h3>
                    <p class="text-muted small mb-0">Coming Soon for Android</p>
                </div>
            </div>
        </div>
    </div>
</div>
```

- [ ] **Step 3: Build & verify output HTML compilation**

Run: `npm run build`
Expected: Succeeds and produces homepage changes in `_site/de/index.html` and `_site/en/index.html`.

---

### Task 3: Dedicated Training Timer Pages

**Files:**
- Create: `src/de/training-timer.njk`
- Create: `src/en/training-timer.njk`

**Interfaces:**
- Consumes: `base.njk` layout.
- Produces: Polished product pages detailing the app features in German and English.

- [ ] **Step 1: Create the German page `src/de/training-timer.njk`**

Write the complete contents including the exact detailed copy:

```html
---
layout: base.njk
title: Clubshot Training Timer App
description: Die ultimative Trainingsbegleitung auf dem Schießstand mit hochpräziser akustischer Schusserfassung und split-time Analyse.
lang: de
---
<div class="container py-5">
    <!-- Hero Section -->
    <div class="text-center py-5 mb-5 bg-body-tertiary rounded-4 border shadow-sm px-4">
        <span class="badge bg-danger rounded-pill px-3 py-2 fw-semibold mb-3">Android</span>
        <span class="badge bg-primary rounded-pill px-3 py-2 fw-semibold mb-3">{{ translations[lang].timerComingSoon }}</span>
        <h1 class="display-4 fw-bold mb-3">Clubshot - Training Timer & Ergebnisrechner</h1>
        <p class="lead text-muted mx-auto" style="max-width: 800px;">
            Die ultimative, datenschutzfreundliche Trainingsbegleitung auf dem Schießstand. Entwickelt von Sportschützen für Sportschützen!
        </p>
        <p class="text-muted mt-3 mb-0">
            Egal ob Kurz- oder Langwaffe, Einzeltraining oder Disziplinen-Vorbereitung – Clubshot vereint einen hochpräzisen akustischen Shot-Timer mit einem intelligenten Ergebnisrechner und Disziplinen-Manager direkt auf deinem Smartphone. Erfasse Schussserien vollautomatisch per Mikrofon, analysiere deine Schusskadenzen (Splits) und verwalte deine Trainingsergebnisse sekundenschnell.
        </p>
    </div>

    <!-- Feature Grid / Rows -->
    <h2 class="display-6 fw-bold text-center mb-5">🚀 Die Highlights für dein Training</h2>

    <div class="row g-5 py-3">
        <!-- Feature 1 -->
        <div class="col-md-6">
            <div class="p-4 border rounded-4 h-100 bg-body shadow-sm">
                <div class="fs-1 mb-3">⏱️</div>
                <h3 class="fw-bold fs-4">1. Hochpräziser Shot-Timer & Intelligente Schusserfassung</h3>
                <ul class="text-muted mt-3">
                    <li><strong>Vollautomatische Audio-Erkennung:</strong> Unser hochsensibler DSP-Algorithmus erkennt Schüsse (oder Klatschgeräusche beim Trockentraining) in Echtzeit und stoppt den Timer automatisch beim letzten erwarteten Schuss.</li>
                    <li><strong>Echtzeit-Bandsperre (Buzzer-Blanking):</strong> Der laute Startgong (Buzzer) wird mathematisch ausgeblendet – kein Fehltrigger beim Start.</li>
                    <li><strong>Intelligente Echounterdrückung (AGC):</strong> Ein adaptiver Pegelfilter verhindert doppelte Erfassungen durch Raumhall, Mündungsgas oder Überschallknalle (Sonic Booms).</li>
                    <li><strong>Flüsterleiser Delay-Start:</strong> Frei konfigurierbarer Zufalls-Startverzögerer (z.B. 3–7 Sek.) für eine realistische Vorbereitungsphase.</li>
                </ul>
            </div>
        </div>

        <!-- Feature 2 -->
        <div class="col-md-6">
            <div class="p-4 border rounded-4 h-100 bg-body shadow-sm">
                <div class="fs-1 mb-3">🎯</div>
                <h3 class="fw-bold fs-4">2. Vollständige BDS & DSB Regelwerk-Konformität</h3>
                <p class="text-muted">Trainiere exakt nach den offiziellen Sportordnungen des Bundes Deutscher Sportschützen (BDS) und des Deutschen Schützenbundes (DSB):</p>
                <ul class="text-muted">
                    <li><strong>BDS Speed-Schießen:</strong> Exakte Zeitlimits und Wertungen für Pistole, Büchse und Flinte.</li>
                    <li><strong>BDS Zeitserie & Fertigkeit (Langwaffe 50m / 100m):</strong> Automatisches Navigieren durch alle 3 gleich aufgebauten Stages einer Session inklusive Einblendung der Pflicht-Nachlade-Aufforderungen.</li>
                    <li><strong>DSB 2.53 (Schnellfeuer Pistole 9mm):</strong> Präzisions- und Duellserien mit exakter Taktung.</li>
                    <li><strong>Freestyle-Modus (IPSC):</strong> Freies Schießtraining mit individuell einstellbaren Par-Zeiten.</li>
                </ul>
            </div>
        </div>

        <!-- Feature 3 -->
        <div class="col-md-6">
            <div class="p-4 border rounded-4 h-100 bg-body shadow-sm">
                <div class="fs-1 mb-3">📊</div>
                <h3 class="fw-bold fs-4">3. Professionelle Split-Time Analyse (Schusskadenz)</h3>
                <ul class="text-muted mt-3">
                    <li>Sieh deine Schusszeiten nicht relativ zur Startzeit, sondern relativ zum vorherigen Schuss (echte Splits).</li>
                    <li>Erkenne Rhythmusstörungen sofort durch die Anzeige deiner durchschnittlichen Zwischenzeit (Average Split) pro Stage.</li>
                    <li><strong>Interaktive Schall-Hüllkurve:</strong> Betrachte den exakten Amplitudenverlauf deines Schießens auf einem übersichtlichen Diagramm (X-Achse in Sekunden).</li>
                    <li><strong>Nachträgliche Threshold-Korrektur:</strong> Schuss verpasst oder zu leise geklatscht? Passe den Schwellenwert (in dB) direkt am Diagramm an, um Schüsse nachträglich automatisch neu zu berechnen!</li>
                </ul>
            </div>
        </div>

        <!-- Feature 4 -->
        <div class="col-md-6">
            <div class="p-4 border rounded-4 h-100 bg-body shadow-sm">
                <div class="fs-1 mb-3">✏️</div>
                <h3 class="fw-bold fs-4">4. Ergebnisrechner & Flexibles Scoring</h3>
                <p class="text-muted">Trage deine Ringe nach dem Schießen auf drei bequeme Weisen ein:</p>
                <ul class="text-muted">
                    <li><strong>Manueller Ergebnisrechner (Ergebnisrechner):</strong> Tippe auf einzelne Schüsse und gib deren genauen Ringwert (1–10, X) über die intuitive Zielscheiben-Tastatur ein.</li>
                    <li><strong>Schnelle Ring-Gesamteingabe:</strong> Keine Lust auf Einzeleingabe? Trage dein Gesamtergebnis (z.B. 92 / 100) einfach über ein praktisches Texteingabefeld ein.</li>
                    <li><strong>Deferred Scoring (Später eintragen):</strong> Absolviere erst alle Stages hintereinander weg und trage deine Schießergebnisse am Ende des Trainings in Ruhe gesammelt in deiner Session-Übersicht nach.</li>
                </ul>
            </div>
        </div>

        <!-- Feature 5 -->
        <div class="col-md-6">
            <div class="p-4 border border-primary border-opacity-25 rounded-4 h-100 bg-body shadow-sm">
                <div class="fs-1 mb-3">🔒</div>
                <h3 class="fw-bold fs-4 text-primary">5. Maximale Privatsphäre & Offline-First</h3>
                <ul class="text-muted mt-3">
                    <li><strong>100 % lokal & netzwerkunabhängig:</strong> Deine Schussaufnahmen und Trainingsdaten verlassen niemals dein Gerät. Die App benötigt keine Internetverbindung und funktioniert im tiefsten Keller-Schießstand absolut zuverlässig.</li>
                    <li><strong>Keine Raw-Audio-Speicherung:</strong> Es werden ausschließlich komprimierte Lautstärke-Hüllkurven (RMS-Daten) gespeichert, um deine Privatsphäre absolut zu schützen.</li>
                </ul>
            </div>
        </div>

        <!-- Side Features -->
        <div class="col-md-6">
            <div class="p-4 border rounded-4 h-100 bg-body shadow-sm d-flex flex-column justify-content-center">
                <h3 class="fw-bold fs-4 mb-3">📱 Weitere Features</h3>
                <ul class="text-muted mb-0">
                    <li><strong>Bandeigenes Design:</strong> Modernes Material 3 Light/Dark-Theme, inspiriert von myclubshot.de.</li>
                    <li><strong>Editierbares Schützenprofil:</strong> Verwalte deinen Namen, Schützenverein und passe par-time presets sowie Standard-Delays an deine Vorlieben an.</li>
                    <li><strong>Historie & Session-Manager:</strong> Behalte den Überblick über all deine absolvierten Trainingseinheiten auf einer übersichtlichen Startseite (Session-History). Sessions können jederzeit eingesehen, editiert oder dauerhaft gelöscht werden.</li>
                </ul>
            </div>
        </div>
    </div>

    <div class="text-center mt-5 py-4">
        <p class="text-muted">Möchtest du benachrichtigt werden, sobald die Android App im Play Store verfügbar ist?</p>
        <a href="/de/contact/" class="btn btn-primary rounded-pill px-5 py-3 shadow-sm fw-bold">Kontaktier uns!</a>
    </div>
</div>
```

- [ ] **Step 2: Create the English page `src/en/training-timer.njk`**

Write the translated page:

```html
---
layout: base.njk
title: Clubshot Training Timer App
description: The ultimate training companion on the shooting range with high-precision acoustic shot detection and split-time analysis.
lang: en
---
<div class="container py-5">
    <!-- Hero Section -->
    <div class="text-center py-5 mb-5 bg-body-tertiary rounded-4 border shadow-sm px-4">
        <span class="badge bg-danger rounded-pill px-3 py-2 fw-semibold mb-3">Android</span>
        <span class="badge bg-primary rounded-pill px-3 py-2 fw-semibold mb-3">{{ translations[lang].timerComingSoon }}</span>
        <h1 class="display-4 fw-bold mb-3">Clubshot - Training Timer & Score Calculator</h1>
        <p class="lead text-muted mx-auto" style="max-width: 800px;">
            The ultimate, privacy-friendly training companion on the shooting range. Developed by sports shooters for sports shooters!
        </p>
        <p class="text-muted mt-3 mb-0">
            Whether short or long gun, individual training or discipline preparation - Clubshot combines a high-precision acoustic shot timer with an intelligent result calculator and discipline manager directly on your smartphone. Record shooting series fully automatically via microphone, analyze your shot cadences (splits) and manage your training results in seconds.
        </p>
    </div>

    <!-- Feature Grid / Rows -->
    <h2 class="display-6 fw-bold text-center mb-5">🚀 Highlights for Your Training</h2>

    <div class="row g-5 py-3">
        <!-- Feature 1 -->
        <div class="col-md-6">
            <div class="p-4 border rounded-4 h-100 bg-body shadow-sm">
                <div class="fs-1 mb-3">⏱️</div>
                <h3 class="fw-bold fs-4">1. High-Precision Shot Timer & Intelligent Detection</h3>
                <ul class="text-muted mt-3">
                    <li><strong>Fully Automatic Audio Recognition:</strong> Our highly sensitive DSP algorithm detects shots (or clapping sounds during dry-fire training) in real-time and stops the timer automatically on the last expected shot.</li>
                    <li><strong>Real-Time Bandstop (Buzzer Blanking):</strong> The loud start chime (buzzer) is mathematically masked out – no false triggering at the start.</li>
                    <li><strong>Intelligent Echo Suppression (AGC):</strong> An adaptive level filter prevents double detection caused by room echo, muzzle gas, or sonic booms.</li>
                    <li><strong>Silent Delay Start:</strong> Freely configurable random start delay (e.g., 3–7 seconds) for a realistic preparation phase.</li>
                </ul>
            </div>
        </div>

        <!-- Feature 2 -->
        <div class="col-md-6">
            <div class="p-4 border rounded-4 h-100 bg-body shadow-sm">
                <div class="fs-1 mb-3">🎯</div>
                <h3 class="fw-bold fs-4">2. Full BDS & DSB Rules Compliance</h3>
                <p class="text-muted">Train exactly according to the official sporting regulations of the Association of German Sports Shooters (BDS) and the German Shooting Federation (DSB):</p>
                <ul class="text-muted">
                    <li><strong>BDS Speed Shooting:</strong> Exact time limits and scores for pistol, rifle, and shotgun.</li>
                    <li><strong>BDS Time Series & Skill (Long Gun 50m / 100m):</strong> Automatic navigation through all 3 identically structured stages of a session, including mandatory reload prompts.</li>
                    <li><strong>DSB 2.53 (Rapid Fire Pistol 9mm):</strong> Precision and duel series with exact timing.</li>
                    <li><strong>Freestyle Mode (IPSC):</strong> Free shooting training with individually adjustable par times.</li>
                </ul>
            </div>
        </div>

        <!-- Feature 3 -->
        <div class="col-md-6">
            <div class="p-4 border rounded-4 h-100 bg-body shadow-sm">
                <div class="fs-1 mb-3">📊</div>
                <h3 class="fw-bold fs-4">3. Professional Split-Time Analysis (Shot Cadence)</h3>
                <ul class="text-muted mt-3">
                    <li>See your shot times relative to the previous shot (true splits) rather than just relative to the start time.</li>
                    <li>Detect rhythm issues immediately with average split time metrics per stage.</li>
                    <li><strong>Interactive Sound Envelope:</strong> View the exact amplitude envelope of your shooting sequence on an intuitive graph (seconds on X-axis).</li>
                    <li><strong>Retrospective Threshold Correction:</strong> Shot missed or clapped too softly? Adjust the DB threshold directly on the graph to automatically recalculate shots instantly!</li>
                </ul>
            </div>
        </div>

        <!-- Feature 4 -->
        <div class="col-md-6">
            <div class="p-4 border rounded-4 h-100 bg-body shadow-sm">
                <div class="fs-1 mb-3">✏️</div>
                <h3 class="fw-bold fs-4">4. Results Calculator & Flexible Scoring</h3>
                <p class="text-muted">Record your ring values in three convenient ways after shooting:</p>
                <ul class="text-muted">
                    <li><strong>Manual Scoring:</strong> Tap on individual shots and input their precise ring value (1–10, X) using the intuitive target keyboard overlay.</li>
                    <li><strong>Fast Total Input:</strong> Don't want to enter every shot? Save your overall score (e.g., 92 / 100) instantly using a simple text input.</li>
                    <li><strong>Deferred Scoring:</strong> Run all stages consecutively and log scores gathered at the end of training in peace.</li>
                </ul>
            </div>
        </div>

        <!-- Feature 5 -->
        <div class="col-md-6">
            <div class="p-4 border border-primary border-opacity-25 rounded-4 h-100 bg-body shadow-sm">
                <div class="fs-1 mb-3">🔒</div>
                <h3 class="fw-bold fs-4 text-primary">5. Maximum Privacy & Offline-First</h3>
                <ul class="text-muted mt-3">
                    <li><strong>100% Local & Independent:</strong> Your audio captures and training logs never leave your device. The app requires zero network access, working reliably deep in basement ranges.</li>
                    <li><strong>No Raw Audio Saving:</strong> Only compressed volume envelopes (RMS data) are persisted, keeping your ambient voice and surroundings completely private.</li>
                </ul>
            </div>
        </div>

        <!-- Side Features -->
        <div class="col-md-6">
            <div class="p-4 border rounded-4 h-100 bg-body shadow-sm d-flex flex-column justify-content-center">
                <h3 class="fw-bold fs-4 mb-3">📱 More Features</h3>
                <ul class="text-muted mb-0">
                    <li><strong>Consistent Aesthetic:</strong> Modern Material 3 Light/Dark theme inspired by myclubshot.de design.</li>
                    <li><strong>Custom Profile:</strong> Manage name, shooting club, and save target delays or par presets.</li>
                    <li><strong>History & Session Manager:</strong> Keep track of historical sessions on a cleanly laid out home screen to edit or delete entries anytime.</li>
                </ul>
            </div>
        </div>
    </div>

    <div class="text-center mt-5 py-4">
        <p class="text-muted">Want to get notified as soon as the Android App is published in the Play Store?</p>
        <a href="/en/contact/" class="btn btn-primary rounded-pill px-5 py-3 shadow-sm fw-bold">Contact us!</a>
    </div>
</div>
```

- [ ] **Step 3: Run build & verify file creation**

Run: `npm run build`
Expected: Succeeds and generates `_site/de/training-timer/index.html` and `_site/en/training-timer/index.html`.

---

### Task 4: App Datenschutzerklärung (Privacy Policy)

**Files:**
- Create: `src/de/datenschutz-app.njk`
- Create: `src/en/datenschutz-app.njk`

**Interfaces:**
- Consumes: `base.njk` layout, Impressum details (Marc Aschmann).
- Produces: GDPR-compliant privacy policies in German and English for the mobile application.

- [ ] **Step 1: Create the German policy `src/de/datenschutz-app.njk`**

Write the detailed GDPR statement matching the offline-first specifications:

```html
---
layout: base.njk
title: Datenschutzerklärung (App)
description: Datenschutzerklärung für die Clubshot Training Timer Mobile App.
lang: de
---
<div class="container py-5">
    <div class="row justify-content-center">
        <div class="col-lg-8 mt-4">
            <h1 class="display-5 fw-bold mb-4">Datenschutzerklärung (Mobile App)</h1>
            <p class="lead text-muted mb-5">
                Datenschutz hat für uns einen extrem hohen Stellenwert. Diese Datenschutzerklärung klärt Sie über die Art, den Umfang und Zweck der Erhebung und Verwendung von Daten innerhalb unserer mobilen Applikation „Clubshot“ (nachfolgend „App“) auf.
            </p>

            <div class="mb-5">
                <h3 class="fs-4 fw-bold">1. Verantwortlicher</h3>
                <p>
                    Verantwortlicher im Sinne der Datenschutz-Grundverordnung (DSGVO) und anderer nationaler Datenschutzgesetze ist:
                </p>
                <p class="border-start border-primary border-3 ps-3">
                    EDV Beratung Aschmann<br>
                    Inhaber: Marc Aschmann<br>
                    Johannesstr. 5<br>
                    72657 Altenriet<br>
                    Deutschland<br>
                    E-Mail: <a href="mailto:info@myclubshot.de">info@myclubshot.de</a>
                </p>
            </div>

            <div class="mb-5">
                <h3 class="fs-4 fw-bold">2. Grundprinzip: 100 % Offline-First & Lokale Verarbeitung</h3>
                <p>
                    Unsere App ist nach dem Prinzip des „Privacy by Design“ entwickelt. 
                </p>
                <ul>
                    <li><strong>Keine Serververbindung:</strong> Die App benötigt keine Internetverbindung für ihre Kernfunktionen und stellt keine unaufgeforderten Netzwerkverbindungen zu unseren Servern her.</li>
                    <li><strong>Keine Benutzerregistrierung:</strong> Es ist keine Erstellung eines Accounts oder eine Registrierung erforderlich.</li>
                    <li><strong>Lokaler Speicher:</strong> Alle eingegebenen Trainingsdaten, Schützenprofile und Sportergebnisse werden ausschließlich verschlüsselt oder unverschlüsselt im lokalen Sandboxed-Speicher Ihres Smartphones abgelegt.</li>
                </ul>
            </div>

            <div class="mb-5">
                <h3 class="fs-4 fw-bold">3. Mikrofonzugriff & Schusserfassung</h3>
                <p>
                    Zur Erkennung von Schussschallereignissen (bzw. Klatschgeräuschen beim Trockentraining) benötigt die App Zugriff auf das Mikrofon Ihres Geräts.
                </p>
                <ul>
                    <li><strong>Echtzeit-Analyse:</strong> Die Audiosignale werden ausschließlich in Echtzeit auf dem Prozessor des Endgeräts durch mathematische DSP-Algorithmen (Digital Signal Processing) analysiert.</li>
                    <li><strong>Keine Speicherung von Rohaudio:</strong> Es werden zu keinem Zeitpunkt Tonaufnahmen (Stimmen, Gespräche oder Umgebungsgeräusche) auf dem Speicher abgelegt oder übertragen.</li>
                    <li><strong>RMS-Hüllkurvendaten:</strong> Gespeichert wird lediglich die komprimierte Lautstärken-Hüllkurve (RMS-Werte in Dezibel relativ zur Zeitachse), um den Schussverlauf visuell im Diagramm darzustellen. Stimmen oder Gespräche lassen sich daraus nicht rekonstruieren.</li>
                </ul>
            </div>

            <div class="mb-5">
                <h3 class="fs-4 fw-bold">4. Analysedienste & Drittanbieter-SDKs</h3>
                <p>
                    Wir setzen bewusst <strong>keine</strong> Analysedienste (wie z.B. Firebase Analytics, Google Analytics), keine Werbe-Netzwerke (wie z.B. AdMob) und keine sonstigen Tracking-SDKs von Drittanbietern ein. Ihre Nutzungsgewohnheiten bleiben komplett privat.
                </p>
            </div>

            <div class="mb-5">
                <h3 class="fs-4 fw-bold">5. Ihre Rechte (Betroffenenrechte)</h3>
                <p>
                    Da alle Daten ausschließlich auf Ihrem Gerät gespeichert sind und wir keinen Zugriff darauf haben, können Sie Ihre Rechte auf Löschung, Auskunft und Berichtigung jederzeit vollkommen selbstständig ausüben:
                </p>
                <ul>
                    <li>Sie können alle Trainingsdaten direkt in den App-Einstellungen dauerhaft löschen.</li>
                    <li>Durch Deinstallation der App werden sämtliche im App-Sandbox-Bereich abgelegten lokalen Daten und Einstellungen unwiderruflich von Ihrem Smartphone gelöscht.</li>
                </ul>
            </div>

            <div class="mb-5">
                <h3 class="fs-4 fw-bold">6. Änderungen dieser Datenschutzerklärung</h3>
                <p>
                    Wir behalten uns vor, diese Datenschutzerklärung anzupassen, damit sie stets den aktuellen rechtlichen Anforderungen entspricht oder um Änderungen unserer Dienste in der Datenschutzerklärung umzusetzen (z. B. bei der Einführung neuer App-Funktionen).
                </p>
                <p class="text-muted small">Stand: Juli 2026</p>
            </div>

            <div class="text-center mt-5">
                <a href="/{{ lang }}/" class="btn btn-outline-secondary rounded-pill px-4">{{ translations[lang].home }}</a>
            </div>
        </div>
    </div>
</div>
```

- [ ] **Step 2: Create the English privacy pointer page `src/en/datenschutz-app.njk`**

For the English route `/en/datenschutz-app/` we will redirect/link gracefully to the German legally binding policy, explaining the offline privacy policy:

```html
---
layout: base.njk
title: Privacy Policy (App)
description: Privacy policy information for the Clubshot Training Timer Mobile App.
lang: en
---
<div class="container py-5">
    <div class="row justify-content-center">
        <div class="col-lg-8 mt-4">
            <h1 class="display-5 fw-bold mb-4">Privacy Policy (Mobile App)</h1>
            
            <div class="p-4 bg-body-tertiary border rounded-4 mb-5">
                <h3 class="fs-5 fw-bold text-primary">🔒 100% Privacy & Offline-First</h3>
                <p class="text-muted mb-0">
                    The Clubshot app is built entirely "Offline-First". It does not connect to the internet, does not use trackers, and stores all training data strictly on your device.
                </p>
            </div>

            <p class="lead mb-4">
                The legally binding Privacy Policy for our mobile application is written in German (Datenschutzerklärung) to comply with local GDPR standards.
            </p>

            <div class="text-center py-4">
                <a href="/de/datenschutz-app/" class="btn btn-primary rounded-pill btn-lg px-4 shadow-sm fw-semibold">
                    Read German Datenschutzerklärung ↗
                </a>
            </div>

            <div class="text-center mt-5">
                <a href="/{{ lang }}/" class="btn btn-outline-secondary rounded-pill px-4">{{ translations[lang].home }}</a>
            </div>
        </div>
    </div>
</div>
```

- [ ] **Step 3: Run final site-wide build validation**

Run: `npm run build`
Expected: Succeeds and compiles all pages cleanly under `_site/`.
Verify existence of the following build files:
- `_site/de/training-timer/index.html`
- `_site/en/training-timer/index.html`
- `_site/de/datenschutz-app/index.html`
- `_site/en/datenschutz-app/index.html`
