# @semanticintent/recall-ui

Standard component library for [RECALL](https://recall.cormorantforaging.dev) — reusable `.rcpy` copybooks for nav, footer, hero, stat grids, card sections, and themes.

## Install

```sh
npm install @semanticintent/recall-ui
```

Requires `@semanticintent/recall` v0.6.0 or later.

## Usage

```cobol
ENVIRONMENT DIVISION.
   COPY FROM "@semanticintent/recall-ui/themes/dark.rcpy".

COMPONENT DIVISION.
   COPY FROM "@semanticintent/recall-ui/components/nav.rcpy".
   COPY FROM "@semanticintent/recall-ui/components/hero.rcpy".
   COPY FROM "@semanticintent/recall-ui/components/stat-section.rcpy".
   COPY FROM "@semanticintent/recall-ui/components/footer.rcpy".

PROCEDURE DIVISION.
   RENDER.
      DISPLAY SITE-NAV     WITH DATA SITE-LOGO, NAV-ITEMS.
      DISPLAY PAGE-HERO    WITH DATA HERO-TITLE, HERO-SUBTITLE, CTA-LABEL, CTA-HREF.
      DISPLAY STAT-SECTION WITH DATA SECTION-TITLE, STATS, STAT-COLUMNS.
      DISPLAY SITE-FOOTER  WITH DATA FOOTER-TEXT.
   STOP RUN.
```

Components use the `ACCEPTS` / `WITH DATA` pattern — your DATA DIVISION field names must match what each component declares it accepts.

---

## Themes

### `themes/dark.rcpy`

Dark terminal theme. IBM Plex Mono primary, green accent.

```cobol
ENVIRONMENT DIVISION.
   COPY FROM "@semanticintent/recall-ui/themes/dark.rcpy".
```

| Token | Value |
|---|---|
| `COLOR-BG` | `#080808` |
| `COLOR-TEXT` | `#f0f0f0` |
| `COLOR-ACCENT` | `#00ff41` |
| `COLOR-BORDER` | `#2a2a2a` |
| `FONT-PRIMARY` | IBM Plex Mono |
| `FONT-SECONDARY` | IBM Plex Sans |

## Components

All components declare their data contract via `ACCEPTS`. The caller passes DATA DIVISION fields using `WITH DATA` at invocation.

---

### `components/nav.rcpy`

Sticky top navigation bar with logo and links.

**ACCEPTS:** `SITE-LOGO` `NAV-ITEMS`

```cobol
DATA DIVISION.
   WORKING-STORAGE SECTION.
      01 SITE-LOGO PIC X(20) VALUE "ACME".
   ITEMS SECTION.
      01 NAV-ITEMS.
         05 NAV-ITEM-1      PIC X(30) VALUE "Docs".
         05 NAV-ITEM-1-HREF PIC X(80) VALUE "/docs.html".
         05 NAV-ITEM-2      PIC X(30) VALUE "GitHub".
         05 NAV-ITEM-2-HREF PIC X(80) VALUE "https://github.com/you/repo".

PROCEDURE DIVISION.
   RENDER.
      DISPLAY SITE-NAV WITH DATA SITE-LOGO, NAV-ITEMS.
   STOP RUN.
```

---

### `components/footer.rcpy`

Centered page footer.

**ACCEPTS:** `FOOTER-TEXT`

```cobol
DATA DIVISION.
   WORKING-STORAGE SECTION.
      01 FOOTER-TEXT PIC X(100) VALUE "ACME Corp — MIT License".

PROCEDURE DIVISION.
   RENDER.
      DISPLAY SITE-FOOTER WITH DATA FOOTER-TEXT.
   STOP RUN.
```

---

### `components/hero.rcpy`

Full-width hero section. SPLIT layout — headline and subtitle on the left, CTA button on the right.

**ACCEPTS:** `HERO-TITLE` `HERO-SUBTITLE` `CTA-LABEL` `CTA-HREF`

```cobol
DATA DIVISION.
   WORKING-STORAGE SECTION.
      01 HERO-TITLE    PIC X(60)  VALUE "Build faster with RECALL.".
      01 HERO-SUBTITLE PIC X(200) VALUE "Declarative web pages from plain text.".
      01 CTA-LABEL     PIC X(20)  VALUE "Get Started".
      01 CTA-HREF      PIC X(80)  VALUE "/docs.html".

PROCEDURE DIVISION.
   RENDER.
      DISPLAY PAGE-HERO WITH DATA HERO-TITLE, HERO-SUBTITLE, CTA-LABEL, CTA-HREF.
   STOP RUN.
```

---

### `components/stat-section.rcpy`

Labelled section wrapping a STAT-GRID. Use for KPIs, metrics, and case study stats.

**ACCEPTS:** `SECTION-TITLE` `STATS` `STAT-COLUMNS`

Each stat item needs a `-VALUE` and `-LABEL` field pair:

```cobol
DATA DIVISION.
   WORKING-STORAGE SECTION.
      01 SECTION-TITLE PIC X(40) VALUE "Key Metrics".
      01 STAT-COLUMNS  PIC 9     VALUE "4".
   ITEMS SECTION.
      01 STATS.
         05 STAT-1.
            10 STAT-1-VALUE PIC X(10) VALUE "2,451".
            10 STAT-1-LABEL PIC X(30) VALUE "FETCH Score".
         05 STAT-2.
            10 STAT-2-VALUE PIC X(10) VALUE "6/6".
            10 STAT-2-LABEL PIC X(30) VALUE "Dimensions".

PROCEDURE DIVISION.
   RENDER.
      DISPLAY STAT-SECTION WITH DATA SECTION-TITLE, STATS, STAT-COLUMNS.
   STOP RUN.
```

---

### `components/card-section.rcpy`

Labelled section wrapping a CARD-LIST. Use for feature grids, resource listings, portfolio entries.

**ACCEPTS:** `SECTION-TITLE` `CARDS`

```cobol
DATA DIVISION.
   WORKING-STORAGE SECTION.
      01 SECTION-TITLE PIC X(40) VALUE "Features".
   ITEMS SECTION.
      01 CARDS.
         05 CARD-1.
            10 CARD-1-TITLE PIC X(60)  VALUE "Zero dependencies".
            10 CARD-1-BODY  PIC X(200) VALUE "Every compiled page is a single self-contained HTML file.".
            10 CARD-1-HREF  PIC X(80)  VALUE "/docs.html".

PROCEDURE DIVISION.
   RENDER.
      DISPLAY CARD-SECTION WITH DATA SECTION-TITLE, CARDS.
   STOP RUN.
```

---

## LOAD FROM + recall-ui

Combine `LOAD FROM` (v0.7) with recall-ui components to drive pages entirely from JSON data:

```cobol
DATA DIVISION.
   LOAD FROM "brief.json".

COMPONENT DIVISION.
   COPY FROM "@semanticintent/recall-ui/components/hero.rcpy".
   COPY FROM "@semanticintent/recall-ui/components/stat-section.rcpy".

PROCEDURE DIVISION.
   RENDER.
      DISPLAY PAGE-HERO    WITH DATA HERO-TITLE, HERO-SUBTITLE, CTA-LABEL, CTA-HREF.
      DISPLAY STAT-SECTION WITH DATA SECTION-TITLE, STATS, STAT-COLUMNS.
   STOP RUN.
```

Where `brief.json` contains scalar fields (`HERO-TITLE`, `CTA-HREF`, etc.) and array fields (`STATS`).

---

## License

MIT — [semanticintent](https://github.com/semanticintent)
