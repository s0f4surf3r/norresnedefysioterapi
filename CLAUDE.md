# Nørre Snede Fysioterapi — CLAUDE.md

## Projekt-Übersicht
Kunden-Website für Nørre Snede Fysioterapi (Dänemark).
Erstellt März 2026 mit dem Website-Template (Jochen Hornung / J-THRUST).

**Live:** https://norresnedefysioterapi.statichost.page (oder ähnlich — nach deploy prüfen)
**GitHub:** https://github.com/s0f4surf3r/norresnedefysioterapi
**Original-Site (alt):** https://norresnedefysioterapi.dk

---

## Tech-Stack
- **Generator:** Eleventy 2.0.1 + Nunjucks (`.njk`)
- **Hosting:** statichost.eu — deploy via GitHub webhook automatisch
- **Build-Config:** `statichost.yml` → `npm install && npx @11ty/eleventy`, public: `_site`
- **Sprache:** Dänisch

---

## Build & Deploy
```bash
npm run build      # lokaler Build → _site/
npm start          # Eleventy dev server (port 8082)
```
Jeder `git push` triggert automatisch einen neuen Build auf statichost.eu.

**WICHTIG:** `.md`-Dateien mit HTML-Blöcken immer als `.njk` umbenennen, sonst parsiert Eleventy die Leerzeilen als leere `<p>`-Tags (Whitespace-Bug).

---

## Projektstruktur
```
src/
  _layouts/base.njk          # Haupt-Layout: Header, Nav, Footer
  css/style.css              # Alle Styles
  images/                    # Alle Bilder (siehe unten)
  index.njk                  # Forside (Homepage)
  behandlinger.njk           # Behandlungen
  traening.njk               # Træning & Hold
  klinikken.njk              # Team/Klinik-Info
  firma.njk                  # Firmafysioterapi
  priser.njk                 # Preise (war .md, jetzt .njk!)
  kontakt.md                 # Kontaktseite
  persondatapolitik.md       # Datenschutz
  admin/                     # perfectCMS Admin-Panel
```

---

## Design
- **Farbpalette:** Nordisch/hell
  - `--color-bg: #F6F4F0` (warmes Weiß)
  - `--color-primary: #ECEBE5`
  - `--color-accent: #5A7E6A` (Sage-Grün)
  - `--color-secondary: #8C9098` (Grau)
  - `--color-warm: #B88C5A` (Warm-Braun)
- **Logo:** `/images/logo.svg` — Recraft-generiertes SVG (gehende Figur, Sage-Grün, Line-Art)
- **Icons:** Alle inline SVG (stroke: currentColor, kein fill) — KEINE Emojis
- **Schrift:** System-Stack (`-apple-system, BlinkMacSystemFont…`)

---

## Bilder
Alle aus der Original-Website gescrapt (März 2026):
- `hero-morgen.jpg` / `hero-aften.jpg` — Hero-Bild Forside
- `undersoegelse.jpg` — Untersuchung
- `massage.jpg` — Massage
- `akupunktur.jpg` — Akupunktur (Hochformat 3:4)
- `idraetsfysioterapi.jpg` — Idrætsfysioterapi
- `saaler.jpg` / `saaler-2.jpg` — Indlægssåler
- `traening-foto.jpg` — Trainingsfotos
- `logo.svg` — Klinik-Logo (Recraft-generiert)
- Team-Fotos: `Jens-scaled.jpg`, `Rasmus-scaled.jpg`, `Camilla-scaled.jpg`, `Diana-scaled.jpg`, `Christian-scaled.jpg`, `sandra-1-e1695124651214.jpg`
- **Fehlt:** Foto von Christoffer Knudsen (CK-Platzhalter im Klinikken-HTML)

---

## Team (klinikken.njk)
- **Jens Yeoman** — Gründer/Inhaber, Fysioterapeut seit 1989
- **Rasmus Yeoman** — Sohn, Fysioterapeut + Osteopat (Abschluss 2025)
- **Camilla** — Fysioterapeut
- **Diana** — Fyzioterapeut
- **Christian** — Físioterapeut
- **Christoffer Knudsen (CK)** — kein Foto vorhanden, Platzhalter aktiv
- **Sandra** — (war Sandras Osteopathin-Kontakt, jetzt als erster Kunde)

---

## Bekannte Eigenheiten / Fallstricke
1. **`.njk` statt `.md`** für Seiten mit HTML-Blöcken (Eleventy Whitespace-Bug)
2. **Flex-Gap + Text-Span**: In `.site-name` (flex) erzeugt jeder direkte `<span>` ein eigenes Flex-Item mit dem `gap`. Header-Name daher in einem einzigen `<span>`.
3. **Fotostreifen mobile**: `forside-photos` → bei ≤768px: `grid-template-columns: 1fr 1fr` (2×2), sonst `repeat(4, 1fr)` + `height: 300px`
4. **Safari**: `height: 100%` auf Grid-Kindern braucht explizite Höhe am Container. `aspect-ratio` allein reicht nicht für gleiche Höhen bei unterschiedlichen Spaltenbreiten.
5. **WhatsApp-Bilder** im Projektroot — NICHT committen (sind vom Kunden)

---

## CSS-Klassen (wichtigste)
- `.service-grid` — 3-spaltig, Cards mit SVG-Icon + H3 + P
- `.service-card` — weißer Hintergrund, border-radius 16px
- `.service-icon` — 44×44px, grüner Hintergrund, SVG 22×22px
- `.forside-photos` — 4-Bild-Streifen Forside
- `.behandling-photo-strip` — 3-Bild-Streifen (behandlinger.njk)
- `.behandling-akupunktur` — 2-spaltig: Foto + Service-Grid
- `.stats-row` — horizontale Stats mit `border-right`-Trennern
- `.info-strip` / `.info-strip-inner` — Info-Leiste (dunkel)
- `.section` + `.wrapper` — Standard-Sektionslayout

---

## Offene Aufgaben
- [ ] Foto von Christoffer Knudsen vom Kunden anfordern
- [ ] Favicon aus Logo-SVG ableiten (aktuell: Original-Favicon der alten Site)
- [ ] Live-URL verifizieren nach deploy
- [ ] Mobile: alle Breakpoints final durchprüfen (iPad, 375px, 430px)
