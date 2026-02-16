# Second Brain s NotebookLM — Interaktivní Workshop

Kompletní online příručka/workshop o Google NotebookLM. Určeno pro ~2hodinový živý workshop vedený lektorem, ale funguje i samostatně jako reference.

## 🚀 Live verze

Workshop je dostupný na: **[Netlify URL po deployi]**

## 📋 Obsah workshopu

### 8 hlavních bloků (~2 hodiny):

1. **Úvod: Proč NotebookLM** (~10 min) — Vznik, RAG princip, srovnání s chatboty
2. **Klíčové koncepty** (~15 min) — Slovníček důležitých pojmů
3. **Plány a limity** (~5 min) — Cenové plány a co dostanete zdarma
4. **Hands-on: První zápisník** (~20 min) — Praktický návod krok za krokem
5. **Příprava zdrojů** (~15 min) — Epistemické inženýrství a best practices
6. **Workflow a prompty** (~25 min) — 26+ kopírovatelných promptů pro různé účely
7. **Tipy, triky a chyby** (~15 min) — Praktické rady a nejčastější chyby
8. **Strategie a budoucnost** (~10 min) — Kdy použít, kdy ne, a kam Google míří
9. **Cheat Sheet** — Kompaktní reference na jedné stránce

## ✨ Funkce

- ✅ **26+ kopírovatelných promptů** s jedním kliknutím
- ✅ **Interaktivní checkboxy** pro hands-on sekci
- ✅ **Dark/light mode** s přepínačem
- ✅ **Progress tracking** — vidíte, kde ve workshopu jste
- ✅ **Responzivní design** — funguje na desktopu i mobilu
- ✅ **Accordion komponenty** pro lepší přehlednost
- ✅ **Timeline** vývoje NotebookLM
- ✅ **Do/Don't karty** pro nejčastější chyby
- ✅ **Self-contained** — jeden HTML soubor, funguje offline

## 🛠 Technické detaily

- **Formát:** Single-page HTML aplikace s React (přes CDN)
- **Styling:** Inline CSS, žádné externí závislosti
- **Font:** Inter z Google Fonts
- **Velikost:** ~115 KB (kompletní obsah 50k+ znaků z příručky)

## 📦 Deployment

Workshop je automaticky deployován na Netlify z `main` branch. Každý push spustí nový build.

### Lokální vývoj

Stačí otevřít `workshop.html` v prohlížeči:

```bash
open workshop.html
# nebo
python3 -m http.server 8000
```

## 📝 Zdrojový materiál

Veškerý obsah pochází z `second-brain-notebooklm.md` — kompletní textová příručka (50 400 znaků, 15 kapitol) o Google NotebookLM, sestavená v únoru 2026.

## 🔧 Jak přispět

1. Forkni repo
2. Udělej změny v `workshop.html`
3. Otevři Pull Request

## 📄 Licence

© 2026 Miloš Čermák — Workshop materiál pro AI školení
