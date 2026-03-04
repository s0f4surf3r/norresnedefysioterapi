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

### Allgemein
1. **`.njk` statt `.md`** für Seiten mit HTML-Blöcken (Eleventy Whitespace-Bug)
2. **WhatsApp-Bilder** im Projektroot — NICHT committen (sind vom Kunden)

### Header
3. **Flex-Gap + Text-Span**: In `.site-name` (flex) erzeugt jeder direkte `<span>` ein eigenes Flex-Item mit dem `gap`. Header-Name daher in einem einzigen `<span>` — NICHT aufteilen.

### Foto-Grids
4. **`forside-photos` Desktop**: `height: 300px` + `overflow: hidden` ist korrekt (1 Reihe). Im Mobile-Breakpoint MUSS `height: auto` gesetzt sein, sonst schneidet die fixe Höhe die 2. Reihe ab.
5. **`aspect-ratio` auf Grid-Kindern**: Reicht allein nicht für gleiche Zellhöhen bei unterschiedlichen Spaltenbreiten. Immer auch explizite Höhe am Container oder `height: auto` auf den Bildern setzen.

### Footer / Info-Strip Ausrichtung
6. **Footer-Spalten**: `.footer-inner` hat `repeat(3, 1fr)`. NICHT auf `2fr 1fr 1fr` o.ä. ändern — das bricht die vertikale Ausrichtung mit dem `.info-strip` darüber.
7. **Padding-Ebene**: Horizontales Padding gehört auf `.footer-inner` + `.footer-bottom`, NICHT auf `.site-footer` selbst. Sonst verschiebt sich der Inhalt auf breiten Viewports durch unterschiedliche Zentrierungs-Referenzpunkte.
8. **Footer-Tagline-Länge**: Text in der ersten Footer-Spalte darf max. ~45 Zeichen pro Zeile haben, sonst Zeilenumbruch. Aktuell: "Siden 1989 hjælper vi i Nørre Snede og omegn." (46 Zeichen) — grenzwertig, nicht verlängern.

### Icons
9. **Nur inline SVG** — keine Emojis, keine externen Icon-Libraries. `stroke="currentColor"`, `fill="none"`, `stroke-width="1.5"`, `stroke-linecap="round"`, `stroke-linejoin="round"`.

### Info-Strip Spaltenanzahl
10. **Info-Strip muss dieselbe Spaltenanzahl haben wie der Footer (3)**. `.info-strip-inner` nutzt `auto-fit` — wenn eine Seite mehr als 3 `info-block`-Kinder hat, entstehen mehr Spalten und die Ausrichtung zum Footer bricht. Forside hat genau 3 Blöcke (Ring til os · Åbningstider · Træningscenter).

### Open Graph
11. **OG-Tags vollständig in `base.njk` pflegen**: `og:title`, `og:description`, `og:type`, `og:site_name`, `og:locale`, `og:image` (mit width/height). Das `og:image` zeigt auf `team-foto.jpg` — bei Domain-Wechsel URL anpassen. Ohne vollständige OG-Tags kein Vorschaubild bei Facebook/WhatsApp/LinkedIn.

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
