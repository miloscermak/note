# CLAUDE.md — Online workshop: Second Brain s NotebookLM

## Koncept projektu

Interaktivní online příručka/workshop o Google NotebookLM. Účastníci se jí proklikávají během živého ~2hodinového workshopu vedeného lektorem. Příručka funguje i samostatně jako reference po workshopu.

## Zdrojový materiál

Podkladem je textová příručka `/mnt/user-data/outputs/second-brain-notebooklm.md` (50 400 znaků, 15 kapitol). Veškerý obsah z ní má být zachován a převeden do interaktivní podoby.

## Formát výstupu

Jeden soubor `workshop.html` — self-contained single-page app (HTML + CSS + JS inline). Žádné externí závislosti kromě Google Fonts (volitelně). Funguje offline po prvním načtení.

## Cílová skupina

Účastníci AI workshopů — většinou firemní profesionálové, kteří NotebookLM neznají nebo ho používají základně. Česky. Technická gramotnost střední (umí používat web, Google Docs, ale nejsou vývojáři).

## Struktura a navigace

### Hlavní layout
- **Levý sidebar** (collapsible na mobilu): navigace po sekcích workshopu — vizuálně jako "kroky" (Step 1, Step 2...) s progress indikátorem
- **Hlavní obsah**: aktuální sekce s textem, prompty, tabulkami, obrázky
- **Horní lišta**: logo/název workshopu, progress bar, tlačítko "dark mode"

### Sekce workshopu (mapování na kapitoly příručky)

Workshop je rozdělen do **8 bloků** odpovídajících ~2 hodinám:

| Blok | Čas | Obsah (z příručky) | Interaktivní prvky |
|------|-----|--------------------|--------------------|
| 1. Úvod: Proč NotebookLM | ~10 min | Kap. 1 (vznik, RAG, srovnání) | Animovaný diagram RAG vs. běžný chatbot |
| 2. Klíčové koncepty | ~15 min | Kap. 2 (slovníček) | Expandovatelné karty s pojmy, hover tooltips |
| 3. Plány a limity | ~5 min | Kap. 3 (cenová tabulka) | Interaktivní tabulka s highlight aktuálního plánu |
| 4. Hands-on: První zápisník | ~20 min | Kap. 4 (rychlý start) | Krokový wizard s checkboxy "Hotovo" |
| 5. Příprava zdrojů | ~15 min | Kap. 5 (epistemické inženýrství) | Checklist "Kvalita zdrojů" |
| 6. Workflow a prompty | ~25 min | Kap. 6 + 7 + 8 (workflow, architektura, knihovna) | Prompt karty s tlačítkem "Kopírovat" |
| 7. Tipy, triky a chyby | ~15 min | Kap. 9 + 10 | Accordion s tipy, "Do/Don't" vizuální karty |
| 8. Strategie a budoucnost | ~10 min | Kap. 11-13 (API, strategie, timeline) | Interaktivní timeline |
| Cheat Sheet | Reference | Kap. 15 | Sticky/printable verze |

## Klíčové interaktivní prvky

### 1. Prompt karty s kopírováním
Každý prompt z knihovny (kap. 8) bude zobrazen jako vizuálně odlišená karta:
- Šedé/tmavé pozadí odlišující prompt od běžného textu
- Tlačítko **"📋 Kopírovat"** v pravém horním rohu (s animací "Zkopírováno ✓")
- Volitelný **tag** (Univerzální / Výzkum / Studium / Byznys / Obsah / Persona)
- JavaScript `navigator.clipboard.writeText()` pro kopírování

### 2. Expandovatelné sekce (accordion)
- Slovníčkové pojmy (kap. 2) — kliknutím rozbalit definici
- Tipy a triky (kap. 9) — sbalené ve výchozím stavu
- Chyby (kap. 10) — symptom viditelný, oprava po rozkliknutí

### 3. Progress tracking
- Checkboxy u hands-on kroků (kap. 4)
- Progress bar nahoře ukazující postup workshopem
- Stav uložen v paměti (React state), NIKOLIV v localStorage

### 4. Tabulky
- Cenové plány (kap. 3) — responzivní tabulka s highlight řádku při hoveru
- Srovnávací tabulky (chatbot vs. NotebookLM, kdy ano/ne)
- Mobilní verze: horizontální scroll nebo card layout

### 5. Vizuální prvky
- **RAG diagram**: jednoduchý SVG/CSS diagram ukazující tok "Dokument → Vyhledání → Generování → Citace"
- **Timeline** (kap. 14): horizontální nebo vertikální vizuální timeline s milníky
- **Do/Don't karty**: zelené (Do) a červené (Don't) karty pro chyby a best practices

### 6. Dark mode
- Toggle v horní liště
- Uložen v React state
- Respektuje system preference jako výchozí

## Design a vizuální styl

### Obecné principy
- Čistý, profesionální, ne "AI-hype" estetika
- Dostatek bílého prostoru (whitespace)
- Typografie: systémový font stack nebo Inter/Source Sans Pro z Google Fonts
- Barvy: neutrální základ (bílá/šedá), akcentová barva pro interaktivní prvky a progress

### Barevná paleta (light mode)
- Pozadí: `#FFFFFF` (hlavní), `#F8F9FA` (sidebar, karty)
- Text: `#1A1A2E` (hlavní), `#6B7280` (sekundární)
- Accent: `#4F46E5` (indigo — tlačítka, progress, aktivní navigace)
- Prompt karty: `#1E293B` pozadí, `#E2E8F0` text (tmavé karty vyčnívají)
- Úspěch: `#10B981` (zelená — "Zkopírováno", checkboxy)
- Varování: `#F59E0B` (žlutá — poznámky, upozornění)
- Chyba/Don't: `#EF4444` (červená)

### Barevná paleta (dark mode)
- Pozadí: `#0F172A` (hlavní), `#1E293B` (sidebar, karty)
- Text: `#E2E8F0` (hlavní), `#94A3B8` (sekundární)
- Accent: `#818CF8` (světlejší indigo)
- Prompt karty: `#334155` pozadí, `#F1F5F9` text

### Responzivita
- Desktop: sidebar + hlavní obsah (min-width 768px)
- Tablet: collapsible sidebar
- Mobil: hamburger menu, plná šířka obsahu, prompt karty full-width

## Technická specifikace

### Stack
- **React** (JSX) — single-file `.jsx` artifact
- **Tailwind CSS** (utility classes z base stylesheet)
- Žádné externí závislosti kromě toho, co je dostupné v Claude artifact prostředí
- Veškerý stav v React state (useState, useReducer) — ŽÁDNÝ localStorage

### Struktura kódu
```
WorkshopApp (hlavní komponenta)
├── Header (logo, progress bar, dark mode toggle)
├── Sidebar (navigace, progress indicators)
└── MainContent
    ├── SectionIntro (úvod s RAG diagramem)
    ├── SectionConcepts (accordion s pojmy)
    ├── SectionPricing (interaktivní tabulka)
    ├── SectionHandsOn (wizard s checkboxy)
    ├── SectionSources (checklist)
    ├── SectionWorkflows (workflow karty + prompt knihovna)
    ├── SectionTips (accordion + Do/Don't karty)
    ├── SectionStrategy (timeline + budoucnost)
    └── SectionCheatSheet (kompaktní reference)
```

### Komponenty k vytvoření
- `PromptCard` — prompt text + copy button + tag
- `AccordionItem` — expandovatelná sekce
- `StepWizard` — kroky s checkboxy
- `ComparisonTable` — responzivní tabulka
- `Timeline` — vizuální timeline s milníky
- `DosDonts` — zelené/červené karty
- `ProgressBar` — celkový postup workshopem
- `CopyButton` — tlačítko s clipboard API a feedback animací

## Obsah — co převzít z příručky

### VEŠKERÝ text z příručky se přenese, konkrétně:

1. **Kompletní text všech 15 kapitol** — žádný obsah se nevynechává
2. **Všech 26+ promptů** z knihovny (kap. 8) — jako PromptCard s kopírováním
3. **Všechny tabulky** — cenové plány, srovnání, workflow, cheat sheet
4. **Všechny tipy a triky** (kap. 9) — jako accordion
5. **Všechny chyby** (kap. 10) — jako Do/Don't karty
6. **Timeline** (kap. 14) — jako vizuální komponenta
7. **Cheat Sheet** (kap. 15) — jako samostatná sekce, ideálně "sticky" nebo printable

### Drobné úpravy textu pro workshop kontext:
- Přidat krátké "lektorské poznámky" na začátek každého bloku (co se teď bude dít, kolik to zabere)
- U hands-on sekce přidat explicitní instrukce "Teď si to vyzkoušejte" s checkboxy
- Prompt karty vizuálně odlišit od běžného textu

## Doručení

Jeden soubor uložený jako React artifact (`.jsx`), který se renderuje v Claude UI. Současně uložit do `/mnt/user-data/outputs/workshop.html` jako self-contained HTML pro případné nasazení na web.

## Poznámky pro implementaci

- Příručka je rozsáhlá (50k+ znaků). Kód bude dlouhý, ale MUSÍ obsahovat veškerý text — žádné zkracování nebo "lorem ipsum".
- Prompt karty jsou klíčový UX prvek — musí být vizuálně výrazné a kopírování musí fungovat na první klik.
- Progress tracking je "nice to have" pro workshop zážitek, ale nesmí blokovat navigaci.
- Sidebar navigace musí být jasná a umožnit skok na libovolnou sekci.
- Na mobilu musí být vše čitelné a použitelné — účastníci workshopu často sedí s telefonem.
