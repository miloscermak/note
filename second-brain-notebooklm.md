# Second Brain: Tipy a triky pro práci s NotebookLM

---

## O této příručce

Tato příručka je praktický manuál pro každého, kdo chce z Google NotebookLM vytěžit maximum. Nejde o marketingový leták ani o překlad nápovědy — jde o konkrétní návody, ověřené postupy a kopírovatelné šablony, které můžete použít okamžitě.

Příručka je postavená od základů k pokročilým technikám. Pokud NotebookLM otevíráte poprvé, začněte od začátku. Pokud ho už používáte, přeskočte rovnou na workflow, knihovnu promptů nebo tipy a triky.

Stav k únoru 2026. Platforma se vyvíjí; konkrétní limity a funkce se mohou měnit podle typu účtu a aktuálního rolloutu.

---

## 1. Jak a proč NotebookLM vznikl

**V této kapitole:** Příběh vzniku nástroje, problém, který řeší, a technický princip, na kterém stojí.

### Problém „zahlceného čtenáře"

V roce 2022 malý tým v Google Labs identifikoval mezeru na trhu. ChatGPT vyřešil „problém prázdné stránky" — pomáhal lidem psát. Ale nikdo pořádně neřešil „problém zahlceného čtenáře": studenti, analytici a výzkumníci se topili v PDF, přednáškových zápisech a firemních reportech. Nepotřebovali AI, která napíše báseň o pirátovi. Potřebovali AI, která jim řekne, co je v tom padesátistránkovém dokumentu, a přitom si nebude vymýšlet.

V polovině roku 2022 začala produktová manažerka Raiza Martin spolu s populárně-naučným autorem Stevenem Johnsonem pracovat na prototypu. Za šest týdnů vznikl první funkční nástroj. V květnu 2023 ho Google představil na konferenci I/O pod kódovým názvem **Project Tailwind**. V říjnu 2023 byl přejmenován na NotebookLM a zpřístupněn prvním uživatelům v USA. V říjnu 2024 Google odstranil označení „experimentální" a v prosinci 2024 spustil placenou verzi NotebookLM Plus.

### Co je RAG a proč je to důležité

Běžné chatboty (ChatGPT, Claude, Gemini) fungují tak, že generují odpovědi z obrovského tréninkového souboru — v podstatě z celého internetu. Když se jich zeptáte na něco, model „odhaduje" nejpravděpodobnější odpověď. Někdy se trefí, někdy si vymyslí přesvědčivě znějící nesmysl (halucinace).

NotebookLM používá jiný přístup, který se v oboru nazývá **RAG — Retrieval-Augmented Generation** (generování rozšířené o vyhledávání). Funguje ve dvou krocích:

1. **Retrieval (vyhledání):** Než začne odpovídat, AI nejdřív prohledá vaše nahrané dokumenty a najde relevantní pasáže.
2. **Generation (generování):** Teprve pak z nalezených pasáží sestaví odpověď — a ke každému tvrzení připojí citaci na konkrétní místo v konkrétním dokumentu.

Představte si to jako rozdíl mezi kolegou, který odpovídá „z hlavy" (a občas si vymýšlí), a kolegou, který si nejdřív otevře složku s dokumenty, najde relevantní stránku, a pak vám řekne, co tam stojí. NotebookLM je ten druhý kolega.

V odborném žargonu se tomuto přístupu říká „source-grounded cognition" — uzemněná kognice. V praxi to znamená dramaticky nižší míru halucinací (přibližně 13 %) ve srovnání s neuzemněnými modely (kolem 40 %).

### Pod kapotou: Gemini 3

NotebookLM v únoru 2026 pohání model Gemini 3, který přináší výrazně lepší reasoning (uvažování), multimodální porozumění (text + obrázky + audio) a nižší míru chyb při práci s hustým odborným textem. Kontextové okno pojme až 1 milion tokenů napříč celým zápisníkem.

### Srovnání s běžnými chatboty

| Aspekt | Běžný chatbot (ChatGPT, Claude) | NotebookLM |
|--------|--------------------------------|------------|
| **Zdroj znalostí** | Tréninková data (celý internet) | Výhradně vaše nahrané dokumenty |
| **Citace** | Často chybí nebo jsou vymyšlené | Přímé odkazy na konkrétní odstavce |
| **Kontextové okno** | Omezené na aktuální chat | Až 1 milion tokenů napříč celým zápisníkem |
| **Riziko halucinací** | Vysoké (model improvizuje) | Nízké (striktní adherence k textu) |
| **Hlavní síla** | Kreativita, generování, obecné znalosti | Analýza, syntéza a verifikace vašich dat |

### Pro koho je NotebookLM ideální

- **Studenti a akademici** — příprava na zkoušky, analýza studií, aktivní učení
- **Knowledge workers** — zpracování meetingových zápisků, firemní dokumentace, rozhodovací podklady
- **Autoři a analytici** — rešerše, validace tvrzení, tvorba obsahu z vlastních zdrojů
- **Týmy** — sdílení znalostní báze, onboarding, eliminace „institucionální amnézie"

### Kdy NotebookLM nepoužívat

NotebookLM není univerzální chatbot. Nehodí se na:

- **Obecné otázky o světě** — nemá přístup k internetu (kromě Deep Research). Na „Kdo vyhrál hokej?" použijte Perplexity nebo Google.
- **Kreativní brainstorming od nuly** — odpovídá jen z nahraných zdrojů, nemá „vlastní nápady". Na psaní sci-fi použijte ChatGPT nebo Claude.
- **Programování** — chybí integrace s IDE a exekuce kódu. Použijte Cursor nebo GitHub Copilot.
- **Akademické citace ve formátu BibTeX/Zotero** — neumí exportovat do citačních manažerů.
- **Vysoce citlivá data** — data procházejí přes Google servery; free verze může být přezkoumávána lidskými hodnotiteli. Pro citlivé dokumenty zvažte lokální nástroje.

---

## 2. Klíčové koncepty — slovníček, bez kterého se neobejdete

**V této kapitole:** Vysvětlení všech důležitých pojmů a mechanismů, které potřebujete znát, než začnete pracovat.

### Zápisník (Notebook / Sešit)

Základní organizační jednotka. Jeden zápisník = jeden projekt, jedno téma, jeden cíl. Zápisník obsahuje vaše zdroje, chat, poznámky a výstupy ze Studia. NotebookLM neumí pracovat napříč více zápisníky najednou a **neexistuje žádný způsob, jak prohledávat všechny zápisníky najednou** — proto je klíčové dělení podle projektů a popisné pojmenování zápisníků.

**Příklad:** Nemíchejte „Příprava na zkoušku z práva" a „Plánování dovolené" do jednoho zápisníku. Vytvořte dva.

### Zdroje (Sources)

Dokumenty, které do zápisníku nahrajete. NotebookLM pracuje jen s nimi. Podporované formáty:

- **Dokumenty:** PDF, Google Docs, Google Slides, Google Sheets, DOCX, Markdown, TXT
- **Web:** webové URL (jen textový obsah, obrázky a vnořená videa se neimportují), YouTube URL (z videa vytáhne transkript; jen veřejná videa s titulky)
- **Média:** audio soubory (MP3, WAV, MP4, M4A, AIFF, AAC, CAF, AMR, Opus — vytvoří transkript), obrázky (fotky ručních poznámek, whiteboardů, screenshotů)

Každý zdroj může mít maximálně **500 000 slov / 200 MB**. Zdroje jsou **statické kopie** — když změníte originál, zápisník se sám neaktualizuje. U Google Docs a Google Slides existuje možnost ruční aktualizace: v panelu Sources otevřete zdroj a pokud byl originál mezitím změněn, zobrazí se tlačítko „Click to sync with Google Drive". Tlačítko se ale zobrazí jen tehdy, když (a) originál byl skutečně změněn od posledního zobrazení a (b) máte k originálnímu souboru oprávnění k úpravám. U ostatních typů zdrojů (PDF, URL, audio) musíte smazat a nahrát znovu.

### Citace (Citations)

Srdce celého systému. Ke každé odpovědi NotebookLM připojí čísla odkazující na konkrétní pasáže ve vašich zdrojích. Najedete myší na citaci — uvidíte náhled původního textu. Kliknete — skočíte přímo na přesné místo v dokumentu. Tohle je zásadní změna oproti chatbotům, kde se pohybujete v „pravděpodobnostech".

### Studio

Panel na pravé straně rozhraní, kde generujete výstupy jedním (nebo několika) kliknutími:

- **Report / Study Guide / Briefing / FAQ** — strukturované textové výstupy
- **Audio Overview** — AI podcast ve formátech Deep Dive, Brief, Critique, Debate
- **Video Overview** — narativní video s vizuály (Pro/Ultra plány)
- **Slide Deck** — prezentace exportovatelná do Google Slides
- **Data Table** — strukturovaná tabulka s exportem do Google Sheets
- **Flashcards a kvízy** — pro aktivní učení
- **Mind mapa** — vizuální přehled vztahů mezi koncepty
- **Infografika** — jednostránkové vizuální shrnutí

### Poznámky (Notes)

Odpovědi z chatu si můžete uložit jako poznámky (Save to note). Poznámky zachovávají formát, tabulky i klikací citace. A klíčová funkce: poznámku můžete převést na zdroj (Convert to source) — tím se stane „kanonickou" součástí vaší znalostní báze.

### Configure Chat / Configure Notebook

Nastavení chování AI pro celý zápisník. Volíte styl (Default / Learning Guide / Custom), délku odpovědí a vlastní instrukce. U Custom stylu máte k dispozici **až 10 000 znaků** pro definici role, tónu, formátu a pravidel — to je dost na plnohodnotnou personu.

### Deep Research

Autonomní agent, který prohledá web, najde ~50 kvalitních zdrojů a sestaví citovanou výzkumnou zprávu. Výsledné zdroje pak můžete importovat do zápisníku. Skvělé na rychlé budování bibliografie k novému tématu.

### Fast Research

Rychlé vyhledání zdrojů (z webu nebo Google Drive) přímo v panelu Sources. Méně hloubkové než Deep Research, ale rychlejší na doplnění konkrétních materiálů.

### Data Tables

Jedna z nejvýznamnějších novinek konce roku 2025. AI z vašich dokumentů vytáhne proměnné a sestaví strukturovanou tabulku. Můžete definovat vlastní sloupce (ikonka tužky) a exportovat do Google Sheets. V praxi to šetří desítky minut týdně — akční položky z meetingů, srovnání konkurentů, extrakce metrik.

### Audio transkripce — skrytě užitečná funkce

Když do NotebookLM nahrajete audio soubor (MP3, WAV, M4A a další), automaticky se vytvoří **textový transkript**, který se stane plnohodnotným zdrojem. Můžete se na něj ptát, citace odkazují do transkriptu.

**Podporované formáty:** MP4, M4A, AIFF, AAC, CAF, AMR, WAV, MP3, Opus. Nepodporované formáty (FLAC, OGG, WMA, WebM) je nutné předem konvertovat.

**Jazyky:** Transkripce funguje ve 130+ jazycích včetně češtiny. Kvalita přepisu ale závisí na čistotě nahrávky.

**Jaký model se používá:** Google přesný model nezveřejňuje. Tým NotebookLM úzce spolupracuje s DeepMind audio týmem; na úrovni Google Cloud existuje řečový model Chirp 3, trénovaný na milionech hodin audia ve 100+ jazycích. Pravděpodobně jde o některou z těchto interních Google technologií.

**Funkce je označená jako experimentální.** Kvalita se výrazně liší podle podmínek nahrávky.

**Praktické tipy pro kvalitní transkripci:**
- Nahrávejte v tichém prostředí, kvalitním mikrofonem
- Mluvte zřetelně a ve standardní výslovnosti
- Udržujte konzistentní hlasitost
- Před nahráním si ověřte, že soubor funguje (přehrajte si ho)

**Příklad použití:** Nahrajete hodinový rozhovor s expertem jako MP3 → NotebookLM vytvoří transkript → ptáte se: „Jaké tři hlavní body expert zmiňuje? Cituj." → dostanete strukturovanou odpověď s odkazy na konkrétní místa v přepisu. Tento workflow je překvapivě silný pro zpracování schůzek, rozhovorů, přednášek nebo vlastních hlasových poznámek.

### Interaktivní Audio Overviews

AI vygeneruje podcast (dva virtuální hostitelé diskutují o vašich zdrojích) v 80+ jazycích. Novinkou je možnost se do podcastu „připojit" hlasem (tlačítko „Join" / „Intervene"), položit doplňující otázku a hostitelé vám odpoví na základě zdrojů. Interaktivní mód zatím funguje jen v angličtině.

---

## 3. Cenové plány a limity

**V této kapitole:** Přehled toho, co dostanete zdarma a za co se platí. Konkrétní čísla k únoru 2026.

| Vlastnost | Free | Plus | Pro ($19.99/měs.) | Ultra ($249.99/měs.) |
|-----------|------|------|-------------------|---------------------|
| **Zápisníky** | 100 | 200 | 500 | 500 |
| **Zdroje na zápisník** | 50 | 100 | 300 | 600 |
| **Limit slov na zdroj** | 500 000 | 500 000 | 500 000 | 500 000 |
| **Chat dotazy denně** | 50 | ~100 | ~250–500 | ~2 500–5 000 |
| **Audio Overviews denně** | 3 | ~6 | ~20 | ~200 |
| **Deep Research** | 10 relací/měs. | 20 relací/den | 50 relací/den | 200 relací/den |
| **Video Overviews** | Ne | Ne | Omezeně | Ano (plná kvalita) |
| **Týmové zápisníky** | Ne | Ano | Ano | Ano |

**Poznámka:** Google uvádí, že usage limity se mohou měnit. Čísla v tabulce jsou orientační k únoru 2026. Pokud vám v UI ukazuje jiná čísla, platí ta aktuální.

**Pro většinu uživatelů bohatě stačí Free tier.** Pokud narazíte na limity (typicky 50 dotazů denně nebo 50 zdrojů na zápisník), první řešení je optimalizace workflow (viz tipy níže), ne upgrade.

---

## 4. Rychlý start: od nuly k prvnímu výstupu za 15 minut

**V této kapitole:** Krok za krokem, jak začít. Žádná teorie navíc — rovnou do akce.

### Krok 1: Založte zápisník s jasným záměrem

Jděte na **notebooklm.google.com** → klikněte „Create" → pojmenujte ho tematicky.

- ✅ „Analýza trhu s tepelnými čerpadly Q1 2026"
- ✅ „Zkouška – Kognitivní psychologie"
- ✅ „Projekt Phoenix – Onboarding"
- ❌ „Poznámky" (příliš vágní)
- ❌ „Všechno" (zaručený chaos)

### Krok 2: Nahrajte 3–5 zdrojů

Začněte s malým, fokusovaným setem. Nemíchejte nesouvisející témata. Podporované formáty: PDF, Google Docs, Google Slides, Google Sheets, webové URL, YouTube URL, audio soubory, obrázky.

**Alternativa:** Pokud ještě nemáte vlastní dokumenty, spusťte **Deep Research** — zadejte téma a nechte agenta najít ~50 zdrojů z webu. Vyberte nejlepší a importujte je.

### Krok 3: Nastavte jazyk a personu

1. **Output Language** → čeština (ovlivní jazyk chatu i všech výstupů ze Studia)
2. **Configure Chat** → vyberte styl:
   - **Default** — univerzální
   - **Learning Guide** — vede vás krok za krokem, klade doplňující otázky (ideální pro učení)
   - **Custom** — vaše vlastní instrukce (viz knihovna person níže)

**Příklad Custom instrukce:**

> Jsi můj research editor a strategický poradce. Hledej rozpory mezi zdroji, mapuj trendy a rizika. Odpovídej česky, stručně a vždy s citacemi. Pokud něco ve zdrojích není, řekni to přímo — nevymýšlej si.

### Krok 4: Položte první otázky

Nezačínejte vágně („Co je v dokumentech?"). Buďte konkrétní:

- „Kde se zdroje neshodují? Cituj konkrétní pasáže."
- „Vytvoř executive brief na 200 slov s třemi příležitostmi a třemi riziky."
- „Jaké otázky tenhle výzkum nezodpovídá?"

### Krok 5: Vygenerujte první výstup ve Studiu

V panelu Studio vyberte to, co právě potřebujete:

- Potřebujete přehled → **Report** (Briefing / Study Guide / FAQ)
- Potřebujete procvičování → **Quiz / Flashcards**
- Potřebujete strukturovaná data → **Data Table**
- Potřebujete prezentaci → **Slide Deck**
- Potřebujete porozumění „na cestě" → **Audio Overview** (vyberte formát: Deep Dive, Brief, Critique nebo Debate)

### Krok 6: Uložte cenné odpovědi

U každé dobré odpovědi klikněte **Save to note**. Zachová se formát, tabulky i klikací citace. Poznámky můžete později převést na zdroj (Convert to source) — tím si vytvoříte „archiv znalostí".

---

## 5. Příprava zdrojů: epistemické inženýrství

**V této kapitole:** Proč je kvalita vstupů důležitější než kvalita promptů. Jak připravit dokumenty, aby NotebookLM podával nejlepší výsledky.

Efektivita výstupů z NotebookLM je přímo úměrná kvalitě vstupních dat. Nebo jinak: pokud do zápisníku nasypete nepořádek, dostanete zpátky elegantně formulovaný nepořádek.

### Zdrojová hygiena: co dělat a proč

| Co dělat | Jak přesně | Proč to funguje |
|----------|-----------|-----------------|
| **Pojmenování zdrojů** | „[Projekt] – [Typ] – [Datum] – [Verze]" | AI vnímá názvy zdrojů. „Kritika metodologie X od autora Y (2025).pdf" dává modelu okamžitý kontext, který zlepšuje přesnost citací. |
| **Snížení šumu** | Raději méně zdrojů, ale kvalitních; duplicity pryč | Šum zvyšuje riziko špatně zvolené pasáže. |
| **Rozdělení tlustých PDF** | Velký PDF rozdělit na kapitoly nebo logické části | Usnadníte citování a „selektivní práci" se zdroji (checkboxy). |
| **Čištění webových zdrojů** | Před nahráním jako URL si uvědomte, že NotebookLM stáhne celou stránku včetně menu a patiček. Pokud chcete čistý text, stáhněte stránku ručně (reader mode, export do Markdown, copy-paste hlavního textu) a nahrajte jako textový zdroj. | AI zpracovává vše, co dostane — včetně navigačních lišt a reklam. |
| **Kontrola textové vrstvy PDF** | Ujistěte se, že PDF obsahuje skutečný text, ne jen obrázky | Naskenované PDF bez OCR NotebookLM špatně přečte. |
| **Slučování krátkých dokumentů** | 12 krátkých emailů k jednomu tématu → jeden Google Doc s nadpisy | Ušetříte sloty zdrojů (limit je 50 u Free). |
| **Google Docs se záložkami** | Jeden Google Doc s tabs (leden, únor, březen) | NotebookLM je vidí všechny jako jeden zdroj, ale naviguje v nich. Pozor: obsah pod-záložek (sub-tabs) se neimportuje. |
| **Vlastní poznámky jako zdroje** | Notes → Convert to source | Z poznámek se stane „kanonická" vrstva: definice, rozhodnutí, glossary. |

### Technika „Mega-dokumentu"

Pokud máte tisíce drobných textů a narážíte na limit zdrojů, slučujte tématicky příbuzné soubory do jednoho velkého PDF nebo textového dokumentu **před nahráním**. Tento přístup nejen obchází limity, ale také zvyšuje schopnost modelu nacházet souvislosti v rámci jednoho kontextového okna.

### Pozor na limit 500 000 slov

Pokud PDF přesáhne 500 000 slov, NotebookLM **potichu přestane číst za touto hranicí** — bez varování. Rozdělte takový dokument na části a nahrajte je jako samostatné zdroje.

### Diverzifikace formátů

Nejrobustnější zápisníky kombinují různé typy zdrojů — teoretické texty (PDF), praktické ukázky (YouTube transkripty), data (Google Sheets) a osobní reflexe (audio záznamy). Čím pestřejší mix, tím bohatší analýza.

### Workflow „Triáda" — integrace s externími systémy

V praxi se osvědčil třífázový proces:

1. **Fáze sběru** (Obsidian / Notion): Nefiltrované poznámky a surový výzkum. Tady žije všechno.
2. **Fáze syntézy** (NotebookLM): Nahrávejte pouze vyčištěné klastry zdrojů pro hloubkovou analýzu.
3. **Fáze prezentace** (ChatGPT / Claude): Finální výstupy z NotebookLM přeneste do kreativnějších modelů pro design, typesetting nebo stylistické doladění.

---

## 6. Workflow pro různé role

**V této kapitole:** Pět ověřených pracovních postupů pro pět typických rolí. Každý workflow je „end-to-end" — od vstupu po konkrétní výstup.

### Workflow 1: Knowledge Worker — z meetingového chaosu do systému

**Scénář:** Potřebujete zpracovat 15 reportů o trhu a připravit executive brief.

**Postup:**
1. Vytvořte zápisník „Market Analysis Q1 2026"
2. Nahrajte reporty, analýzy konkurence, relevantní články
3. Nastavte personu: *„Jsi senior analytik. Vyhodnocuj data kriticky, hledej trendy a rizika."*
4. Spusťte sérii promptů:
   - *„Kde se zdroje neshodují? Cituj konkrétní pasáže."*
   - *„Vytvoř executive brief na 200 slov s třemi příležitostmi, třemi riziky a dvěma dalšími kroky."*
   - *„Vygeneruj srovnávací tabulku: vendor A vs B vs C — náklady, klíčové funkce, implementační rizika."*
5. Uložte výstupy jako Notes → použijte je jako vstupy pro další fázi
6. Ve Studiu vygenerujte **Slide Deck** pro management
7. Pro akční položky: Studio → **Data Table** → definujte sloupce (Owner, Action, Priority, Due date) → **Export to Sheets**

**Killer trik:** Tabulka „akce/owner/deadline" + export do Sheets. Citace se exportují zvlášť — skvělé pro audit.

### Workflow 2: Student — aktivní učení, ne pasivní shrnutí

**Scénář:** Příprava na zkoušku z 8 přednášek a 3 paperů.

**Postup:**
1. Vytvořte zápisník „Zkouška – Kognitivní psychologie"
2. Nahrajte přednáškové slidy, poznámky, papery (raději 5–15 kvalitních než 50 „šumu")
3. Configure Chat → **Learning Guide** (a klidně „Longer")
4. Studio → vygenerujte **Study Guide** + **Quiz**. Nastavte obtížnost a počet otázek.
5. U chyb v kvízu použijte **Explain** — dá vysvětlení s citacemi do zdrojů
6. Vygenerujte **Audio Overview** ve formátu „Deep Dive" → poslouchejte při procházce
7. Použijte interaktivní mód — připojte se do podcastu hlasem a ptejte se na to, čemu nerozumíte (zatím jen v angličtině)

**Killer trik:** Learning Guide + Explain u kvízů/flashcards. Deep Research pro identifikaci mezer: *„Co tenhle materiál nezodpovídá? Jaké otázky zůstávají otevřené?"*

### Workflow 3: Autor / Content Creator — živý dokument

**Scénář:** Píšete sérii článků o AI trendech.

**Postup:**
1. Vytvořte „living document" — jeden Google Doc, do kterého průběžně přidáváte nápady, citáty, postřehy
2. Propojte ho s NotebookLM jako jediný zdroj (+ případně druhý s prompty)
3. Když aktualizujete Google Doc, otevřete zdroj v NotebookLM a klikněte na „Click to sync with Google Drive" (zobrazí se, jen pokud byl Doc změněn)
4. Ptejte se: *„Jaké 3 nápady z mých poznámek mají největší potenciál pro virální článek? Proč?"*
5. Nechte si vygenerovat osnovu článku, social media captions, script pro video

**Proč to funguje:** Místo desítek zápisníků máte jeden dynamický, který roste s vámi.

**Bonus — Voice Fingerprint:** Nahrajte své předchozí texty a požádejte AI o analýzu vašeho stylu, slovní zásoby a rytmu vět. Tento „otisk hlasu" pak slouží jako instrukce pro generování nových draftů, které znějí autenticky jako vy.

### Workflow 4: Tým — onboarding a sdílená pravda

**Scénář:** Nový člen týmu potřebuje rychle pochopit projekt.

**Postup:**
1. Vytvořte sdílený zápisník „Project Phoenix – Onboarding"
2. Nahrajte SOP, produktové specifikace, zápisky z meetingů, brand voice dokument
3. Nastavte personu: *„Jsi onboarding buddy. Vysvětluj věci srozumitelně, s příklady z dokumentů."*
4. Nový člen se ptá přirozeným jazykem, dostane odpovědi s citacemi
5. Vygenerujte **Audio Overview** pro „listening onboarding"
6. Přidejte **kvízy** pro ověření, že kolega klíčové věci pochopil
7. Nastavte sdílení: editorům dovolte přidávat zdroje a poznámky; viewerům jen číst

**Pozor:** U veřejného sdílení záleží na typu účtu. Consumer účty podporují „Anyone with a link", u Workspace Enterprise/Education je to vypnuté.

### Workflow 5: Competitive Intelligence / Due Diligence

**Scénář:** Porovnáváte 5 vendorů pro nový nástroj.

**Postup:**
1. Použijte **Deep Research**: zadejte téma a nechte agenta najít a zpracovat ~50 zdrojů
2. Přidejte vlastní interní dokumenty (RFP, rozpočet, požadavky)
3. Zeptejte se: *„Vytvoř srovnávací tabulku všech vendorů: cena, klíčové funkce, rizika, reference. Cituj zdroje."*
4. Nechte vygenerovat **mind mapu** vztahů mezi kritérii
5. Exportujte **Data Table** do Google Sheets pro sdílení s týmem

---

## 7. Architektura instrukcí: jak psát prompty pro NotebookLM

**V této kapitole:** Proč se prompty pro NotebookLM liší od běžného chatování a jak psát instrukce, které vytáhnou maximum.

### Tři klíčové rozdíly oproti běžným chatbotům

**1. Důkazní režim:** NotebookLM používá citace z vašich zdrojů. Nad citací můžete „hover" a okamžitě vidíte citovanou pasáž, klikem skočíte do místa v dokumentu. V běžném chatbotu se pohybujete v pravděpodobnostech — tady máte důkazy.

**2. Implicitní omezení na vaše zdroje:** Chat odpovídá jen z dat ve zdrojích. Pokud chcete čistě kreativní úkol bez opory v textu, může to odmítnout. Dotazy musí být buď o zdrojích, nebo jasně označené jako kreativní transformace nad dodaným textem.

**3. Configure Chat jako per-zápisníkové řízení:** Nastavujete styl a délku jednou — platí pro celý zápisník. U Custom máte 10 000 znaků na instrukce, které ovlivňují i výstupy ze Studia.

### Doporučená struktura promptu

1. **Kontext:** Jaký typ zdrojů a pro koho to je.
2. **Úkol:** Co má vzniknout (konkrétní artefakt).
3. **Důkazy:** Požadavek na citace + pravidlo „když to není ve zdrojích, řekni to".
4. **Formát:** Tabulka / odrážky / checklist (dle potřeby).
5. **Kontrola:** „Vyjmenuj nejasnosti a navrhni, jaké zdroje chybí."

### Router Prompts: fázované zpracování

Místo jednoho masivního promptu používejte systém fázovaných instrukcí. Router Prompt funguje jako koordinační motor, který řídí AI skrze jednotlivé fáze projektu:

- **Identifikace fází:** I. Analýza → II. Návrh → III. Kritika
- **Definice hierarchie** pro strukturování odpovědí
- **Nastavení „bran" (gates):** vyžadují vaše schválení před přechodem k dalšímu kroku

**Příklad:** Místo *„Analyzuj tyhle dokumenty a napiš report"* zadejte:
1. *„Nejdřív identifikuj 5 hlavních témat napříč zdroji."* → schválíte
2. *„Teď ke každému tématu vypiš klíčové argumenty s citacemi."* → schválíte
3. *„Na závěr identifikuj rozpory a slepá místa."*

### Batchování promptů — šetřete denní limit

Free verze má 50 dotazů denně. Místo 5 samostatných otázek pošlete jednu strukturovanou:

> Nejdřív shrň hlavní témata. Pak najdi rozpory mezi zdroji. Pak navrhni 3 akční kroky. U všeho cituj.

---

## 8. Knihovna promptů — kopírujte a používejte

**V této kapitole:** 20+ konkrétních promptů v plném znění, rozdělených podle scénáře. Každý je připravený k okamžitému zkopírování.

### Univerzální prompty (fungují napříč rolemi)

**Prompt 1: Citační odpověď „jen z důkazů"**
> Odpověz pouze na základě vybraných zdrojů. Každé tvrzení podlož citací. Pokud něco ve zdrojích není, napiš „ve zdrojích nenalezeno".

**Prompt 2: Citační audit draftu**
> Projdi tento text [vložit]. Vypiš všechny věty, které nejsou přímo podložené zdroji. U podložených uveď citaci. Navrhni přeformulování nepodložených.

**Prompt 3: Konflikty a rozpory mezi zdroji**
> Najdi místa, kde si zdroje odporují (definice, čísla, doporučení). U každého rozporu popiš: A říká…, B říká…, možná příčina, co ověřit.

**Prompt 4: Mapování pojmů a definice**
> Vytvoř slovníček pojmů: termín → definice → citace → typický příklad ze zdroje.

**Prompt 5: Checklist z dokumentu**
> Z tohoto dokumentu udělej checklist. Každý bod: akce, kriterium splnění, citace.

### Prompty pro výzkum a analýzu

**Prompt 6: Detektor kognitivních rozporů**
> Analyzuj nahrané zdroje a identifikuj oblasti, kde se autoři neshodují. Uveď přímé citace pro každý protichůdný názor a vysvětli, proč k neshodě pravděpodobně dochází (rozdílná metodologie, čas vydání, ideologie).

**Prompt 7: Audit slepých míst (Source Gap)**
> Na základě aktuálních tržních trendů roku 2026 v oblasti [DOPLŇTE] identifikuj, co v těchto zdrojích chybí. Jaké kritické otázky autoři nepoložili a jaké předpoklady dělají bez důkazů?

**Prompt 8: Executive Briefing s analýzou rizik**
> Vytvoř shrnutí pro vedení o délce max. 500 slov. Obsahuj: 1. Třívětý výtah situace. 2. Pět klíčových zjištění. 3. Analýzu rizik s odkazem na konkrétní varování v dokumentech. 4. Tři doporučené akční kroky.

**Prompt 9: Epistemický rodokmen**
> Najdi v mých poznámkách „pacienta nula" — první zmínku o konceptu [X] — a zmapuj, jak se tato myšlenka vyvíjela, mutovala nebo byla vyvrácena v pozdějších dokumentech.

**Prompt 10: Dialektická debata**
> Ke konceptu [X] z tohoto textu sestav debatu dvou akademiků s protichůdnými pozicemi. Nech je vzájemně napadat nejslabší argumenty s využitím konkrétních pasáží z textů.

**Prompt 11: Srovnávací tabulka**
> Vytvoř srovnávací tabulku: [Vendor A] vs [Vendor B] vs [Vendor C]. Sloupce: Cena, Klíčové funkce, Implementační rizika, Reference. Cituj zdroje u každé buňky.

**Prompt 12: Témata a trendy**
> Identifikuj 5 hlavních témat, která se opakují napříč všemi zdroji. U každého uveď frekvenci výskytu, klíčové citace a vývojový trend.

### Prompty pro studium a učení

**Prompt 13: Zkouškový plán na týden**
> Z těchto zdrojů navrhni 7denní učební plán. Každý den: témata, 3 otázky na porozumění, 5 pojmů, návrh procvičení. Cituj.

**Prompt 14: Kvíz s vysvětlením**
> Vygeneruj 12 otázek. U každé: správná odpověď, krátké vysvětlení, citace, 1 častý omyl.

**Prompt 15: Sokratovský tutor**
> Neposkytuj mi přímé odpovědi. Místo toho mě veď k pochopení pomocí řady otázek založených na nahraných učebnicích. Pokud udělám chybu, nasměruj mě na konkrétní stranu v dokumentu.

**Prompt 16: Zjednodušení do „páté třídy"**
> Přepiš vysvětlení [DOPLŇTE ČÁST] do velmi jednoduché češtiny. Přidej přirovnání a 3 příklady ze zdrojů.

**Prompt 17: Perspektiva budoucího vědce**
> Za 100 let bude akademik analyzovat tento materiál. Co by kritizoval jako zastaralé a co by ocenil jako revoluční?

### Prompty pro byznys a produktivitu

**Prompt 18: Akční tabulka z meetingů**
> Vytvoř tabulku: Owner | Action | Priority | Due date | Dependencies | Evidence quote. Vyplň z transkriptů schůzek v tomto zápisníku.

**Prompt 19: Rozhodovací memo**
> Napiš decision memo: Kontext, Možnosti, Doporučení, Rizika, Nejasnosti. Každou sekci podlož citacemi.

**Prompt 20: Brief pro stakeholdery**
> Vytvoř briefing (max 1 strana): co se změnilo, co je další krok, blockers, co potřebujeme od [STAKEHOLDER]. Citace.

**Prompt 21: Onboarding FAQ**
> Z těchto interních zdrojů vytvoř FAQ pro nováčka. Vždy přidej citaci a odkaz na část dokumentu.

**Prompt 22: Živý glossary týmu**
> Vytvoř team glossary: termín, definice, kdo používá, typický příklad, citace. Na konci navrhni 10 termínů, které v dokumentech často padají, ale nejsou vysvětlené.

### Prompty pro tvorbu obsahu

**Prompt 23: Blogpost z rozhovoru**
> Převeď tento hodinový přepis rozhovoru do strukturované osnovy blogpostu o 800 slovech. Zachovej přesnou terminologii ze zdroje. Navrhni 3 varianty titulku.

**Prompt 24: Obsahový multikanálový generátor**
> Na základě dokumentu [NÁZEV] vytvoř: 1. LinkedIn post (háček, 3 vhledy, CTA). 2. Vlákno na Twitter (8 tweetů). 3. Newsletter sekci o 250 slovech. Zachovej přesnou terminologii ze zdroje.

**Prompt 25: Outline kapitoly s citacemi**
> Navrhni outline pro [TÉMA]. U každé podsekce uveď 2–3 citace (pasáže), které ji podporují.

**Prompt 26: Kritická revize argumentu**
> Buď přísný editor. Najdi logické skoky, nepodložené závěry a chybějící definice. Uveď, kde ve zdrojích by mělo být opřené.

### Persona prompty (pro Configure Notebook / Custom)

**Persona 1: Kritický výzkumník**
> Jsi přísný, ale férový výzkumný editor. Vždy hledej slabiny v argumentaci, ověřuj citace a upozorňuj na nepodložené závěry. Pokud je tvrzení správné, potvrď ho s citací. Pokud je problematické, vysvětli proč.

**Persona 2: Trpělivý tutor**
> Jsi trpělivý tutor, který mi pomáhá učit se nové koncepty. Vše vysvětluj jednoduše, s příklady ze zdrojů. Po každém vysvětlení polož kontrolní otázku. Pokud odpovím špatně, nasměruj mě na relevantní pasáž.

**Persona 3: Strategický poradce**
> Jsi senior konzultant. Na každý problém se dívej skrz framework: co funguje, co nefunguje, co chybí, co udělat jako první. Vždy cituj, vždy buď konkrétní, vždy navrhni další krok.

**Persona 4: Onboarding buddy**
> Jsi onboarding buddy. Vysvětluj věci srozumitelně, s příklady z dokumentů. Nikdy nepředpokládej, že nováček zná interní zkratky. U každé odpovědi uveď odkaz na relevantní část dokumentace.

---

## 9. Tipy, triky a méně známé funkce

**V této kapitole:** 20+ praktických tipů, které vám ušetří čas a zvednou kvalitu výstupů.

### Práce se zdroji

1. **Selektivní výběr zdrojů:** Nemusíte pracovat se všemi zdroji. Odškrtněte v levém panelu ty, které zrovna nepotřebujete — snížíte šum a zvýšíte přesnost odpovědí.

2. **Vypínání zdrojů za běhu:** Během konverzace můžete odškrtávat zdroje. AI okamžitě „zapomene" na vypnuté dokumenty — ideální pro izolování protichůdných teorií.

3. **YouTube Playlist import:** Plus a Pro verze umožňují vložit odkaz na celý YouTube playlist. AI vytvoří znalostní bázi z transkriptů.

4. **Screen Capture do zdroje:** Rozšíření pro Chrome (třetích stran) umožňuje vyfotit část obrazovky a okamžitě ji nahrát jako obrázkový zdroj pro vizuální analýzu.

5. **Google Docs tabs jako multi-zdroj:** Jeden Google Doc s několika záložkami se chová jako jeden zdroj, ale NotebookLM v záložkách naviguje. Šetříte sloty. Pozor: pod-záložky se neimportují.

### Citace a navigace

6. **Hover over references:** Najeďte myší na číslo citace v odpovědi — uvidíte náhled původní pasáže. Kliknutím skočíte přímo na přesné místo v dokumentu (deep link).

7. **Audio transkripce s časovými značkami:** Při nahrání MP3 AI vytvoří transkript s citacemi propojenými na přesný čas v nahrávce.

### Poznámky a archivace

8. **Notes jako archiv znalostí:** Ukládejte cenné odpovědi jako Notes. Později je můžete konvertovat na zdroje (Convert all notes to source) — tím uvolníte sloty pro nové dokumenty, ale neztratíte AI kontext.

9. **Notes ovlivňují Audio:** Pokud chcete, aby se podcast zaměřil na konkrétní věci, přidejte do Notes direktivy — AI je zohlední při generování audia.

10. **Mažte chat historii mezi tématy:** Pokud přecházíte na nové téma v rámci zápisníku, smažte chat (ikona nahoře) a začněte čistě. Kontext předchozího chatu může zkreslovat odpovědi.

### Studio a výstupy

11. **Batch-generování:** Místo klikání na jeden artefakt po druhém můžete označit více zdrojů a nechat najednou vygenerovat osnovu, kvíz i podcast.

12. **Formáty Audio Overviews:** Deep Dive (hloubkový rozhovor dvou hostů, ~20 min), The Brief (klíčové body za 2 minuty, jeden mluvčí), The Critique (konstruktivní zpětná vazba), The Debate (formální debata).

13. **Custom instrukce pro Audio hostitele:** Řekněte jim přesně, na co se zaměřit: *„Zaměřte se výhradně na rozpočtová rizika"* nebo *„Vysvětlete to jako technický deep-dive pro seniory."*

14. **Historie audio verzí:** NotebookLM uchovává předchozí verze vygenerovaných podcastů — můžete sledovat, jak se měnil výklad s přidáváním nových zdrojů.

15. **Slide Deck do Google Slides:** Generátor slide decků vytváří plně editovatelné prezentace přímo ve vašem Google Drive. Tip: podívejte se na „View custom prompt" a uvidíte, jaký prompt Studio použilo.

16. **Export vizuálních artefaktů:** Infografiky a mind mapy ze Studia lze exportovat ve formátu SVG pro další úpravy v grafických nástrojích.

17. **Data Tables — vlastní sloupce:** Klikněte na ikonku tužky u Data Table a přesně popište, jaké sloupce chcete. Čím konkrétnější popis, tím přesnější výstup.

### Pokročilé strategie

18. **Výstupy jako vstupy (řetězení):** Vygenerujte report → z reportu mind mapu → z mind mapy audio → z reportu flashcards s klíčovými čísly. Řetězení výstupů je to, co dělá z NotebookLM silný nástroj.

19. **Notebook jako prompt generátor:** Nahrajte do zápisníku nejlepší prompting guides a příklady. Pak se ptejte: *„Vytvoř prompt pro [use case]."* NotebookLM vygeneruje prompt založený na skutečném know-how, ne na generických šablonách.

20. **Detekce křížových souvislostí:** Dotaz *„Najdi vzorce, které se opakují napříč videem A a PDF dokumentem B"* aktivuje multimodální uvažování modelu.

21. **Mobilní integrace:** Pomocí aplikace Gemini na Android/iOS můžete k jakémukoliv chatu připojit existující zápisník z NotebookLM jako kontext. Audio Overviews podporují offline a background poslech v mobilní appce.

---

## 10. Nejčastější chyby a jak se jim vyhnout

**V této kapitole:** Sedm typických chyb, které snižují kvalitu výstupů, a konkrétní návody, jak je opravit.

### Chyba 1: Jeden mega-zápisník na všechno

**Symptom:** Nahrajete 40 nesouvisejících dokumentů → AI se ztrácí, odpovědi jsou generické a povrchní.

**Oprava:** Vytvořte menší, tematicky zaměřené zápisníky. Jedno téma = jeden zápisník. Uživatelé reportují dramaticky lepší výsledky po rozdělení jednoho velkého zápisníku na 3 tematické.

### Chyba 2: Nahrávání nekvalitních zdrojů

**Symptom:** Naskenované PDF bez textu, rozmazané fotky, dokumenty s minimálním obsahem.

**Oprava:** Vždy ověřte, že PDF obsahuje skutečný text (ne jen obrázky). Používejte OCR nástroje předem. U obrázků ověřte čitelnost. Pamatujte: „garbage in, garbage out" platí i tady.

### Chyba 3: Považovat výstupy za finální

**Symptom:** Bezmyšlenkovité kopírování AI odpovědí do prezentace nebo reportu.

**Oprava:** NotebookLM je partner, ne autopilot. Vždy zkontrolujte citace (hover → náhled), ověřte logiku, přidejte vlastní expertízu. AI může špatně interpretovat kontext nebo propojit nesouvisející věci. Zaveďte pravidlo „Human-in-the-loop" — každý výstup validujte kliknutím na citace, zejména u kritických čísel a dat.

### Chyba 4: Nevyužívání Custom Instructions

**Symptom:** Necháte defaultní nastavení → odpovědi jsou příliš obecné, s nepotřebnými úvody a shrnutími.

**Oprava:** Configure Notebook → Custom → vložte instrukce pro styl, formát a roli. Přidejte i negativní instrukce: *„Žádné intro, žádné shrnutí na konci, žádné follow-up návrhy."*

### Chyba 5: Plýtvání denním limitem dotazů

**Symptom:** 50 dotazů denně (Free) rychle vyčerpáte jednotlivými otázkami.

**Oprava:** Batchujte — kombinujte více otázek do jednoho strukturovaného promptu: *„Nejdřív X. Pak Y. Nakonec Z."* Jeden strukturovaný prompt nahradí 3–5 jednoduchých dotazů.

### Chyba 6: Ignorování limitu zdrojů

**Symptom:** Dlouhý dokument přesahující 500 000 slov — NotebookLM potichu přestane číst za hranicí, bez varování.

**Oprava:** Zkontrolujte délku dokumentu. Pokud přesahuje limit, rozdělte na části. NotebookLM vám neřekne, co přeskočil.

### Chyba 7: Mazání starých zdrojů bez archivace

**Symptom:** Mažete zdroje, abyste uvolnili sloty — ale ztrácíte AI kontext.

**Oprava:** Před smazáním použijte „Convert all notes to source" — AI shrnutí a vaše poznámky se stanou novým zdrojem. Pak bezpečně smažte originály.

### Bonusová chyba: Klam plynulosti (Fluency Heuristic)

Dobře napsaný a plynulý text od AI vyvolává v lidech pocit, že je pravdivý. Model má tendenci přidávat „přesvědčivý nátěr" k informacím, které v textu nejsou — například tvrzení o záměru autora, který není explicitně uveden. Toto je hlavní zdroj zbývajících halucinací.

**Náprava:** Implementujte do každého analytického promptu klauzuli: *„Uváděj pouze to, co je explicitně napsáno. Pokud interpretuješ nebo odvozuješ, označ to jako ‚analytický odhad'."*

---

## 11. API, integrace a použití mimo NotebookLM

**V této kapitole:** Jak propojit NotebookLM s dalšími nástroji, kdo má přístup k API a jak vytvořit chatovací rozhraní nad svými daty.

### Oficiální API: jen pro Enterprise

Google v září 2025 vydal **NotebookLM Enterprise API**, ale jen pro zákazníky s Google Cloud projektem a aktivovanou Enterprise službou. API umožňuje programaticky vytvářet zápisníky, přidávat zdroje, sdílet je a chatovat s nimi. Pro běžného (consumer) uživatele oficiální API neexistuje.

### Integrace přes Gemini app

Od ledna 2026 lze v aplikaci Gemini připojit zápisník z NotebookLM jako zdroj pro konverzaci. V praxi to znamená, že Gemini odpovídá na základě vašeho zápisníku místo obecných znalostí. To je fakticky „chatovací rozhraní nad vašimi daty" mimo samotný NotebookLM.

**Jak to funguje:** Otevřete Gemini app → klikněte na ikonu přílohy → vyberte NotebookLM → zvolte zápisník. Gemini pak má přístup ke zdrojům z tohoto zápisníku.

### Chat-only sdílení

Pokud sdílíte zápisník a nastavíte „chat-only" přístup, příjemce může jen chatovat s AI nad vašimi zdroji — nevidí samotné zdroje ani Studio. To je jednoduchý způsob, jak vytvořit chatovací rozhraní pro kolegy nebo klienty.

### Neoficiální nástroje

Existují nástroje třetích stran (Apify actor, open-source klienty na GitHubu), které automatizují práci s NotebookLM přes prohlížečovou automatizaci. Umožňují programaticky vytvářet zápisníky, přidávat zdroje, generovat podcasty a chatovat. Jde ale o neoficiální řešení, která mohou kdykoliv přestat fungovat a jejichž použití může být v rozporu s podmínkami služby.

---

## 12. Strategické rozhodování: kdy NotebookLM použít a kdy ne

**V této kapitole:** Jasná tabulka, která vám ušetří čas při rozhodování.

### Kdy ANO ✅

| Scénář | Proč NotebookLM |
|--------|----------------|
| Analýza vlastních dokumentů | Source-grounded odpovědi s citacemi, žádné halucinace z webu |
| Příprava na zkoušky | Flashcards, kvízy, study guides, audio přehledy, Learning Guide |
| Literature review | Porovnání zdrojů, hledání rozporů, identifikace mezer |
| Analýza 500stránkového soudního spisu | Excelentní adherence k textu |
| Meeting příprava a follow-up | Shrnutí zápisků, generování agend, akční položky do tabulek |
| Onboarding nových lidí | Stravitelné průvodce z firemní dokumentace s citacemi |
| Competitive analysis | Srovnávací tabulky, syntéza z více reportů, Data Tables |
| Content creation z vlastních zdrojů | Blogposty, prezentace, social media — vše s citační podporou |
| „Druhý mozek" | Centrální místo pro propojení a dotazování vlastních znalostí |
| Hluboký akademický výzkum nové oblasti | Deep Research agent pro automatický sběr literatury |

### Kdy NE ❌

| Scénář | Proč ne | Co místo toho |
|--------|---------|--------------|
| Obecné otázky o světě | Neprohledává web (kromě Deep Research) | ChatGPT, Perplexity, Gemini |
| Kreativní brainstorming od nuly | Nemá „vlastní nápady", odpovídá jen ze zdrojů | ChatGPT, Claude |
| Programování | Chybí IDE integrace a exekuce kódu | Cursor, GitHub Copilot, Claude Code |
| Citace v akademickém formátu | Neumí BibTeX, RIS, Zotero export | Zotero + Paperguide |
| Vysoce citlivá data | Data procházejí přes Google servery | Lokální LLM, Obsidian |
| Práce offline | Vyžaduje internet | Obsidian, lokální nástroje |
| Pokročilá týmová spolupráce | Sdílení je basic, žádné workflow nástroje | Notion, Confluence |

---

## 13. Kam Google s NotebookLM míří

**V této kapitole:** Co víme o strategii Googlu, co lze odvodit a co jsou zatím jen hypotézy.

### Co je jasné: NotebookLM jako pilíř Gemini ekosystému

Google systematicky integruje NotebookLM do svého širšího AI ekosystému. V prosinci 2025 integroval AI nástroje (včetně NotebookLM Plus) přímo do Workspace plánů — bez potřeby samostatného add-onu. Analytici to interpretují jako strategický tah ke zvýšení „lepivosti" ekosystému: když máte NotebookLM, Gemini, Gmail a Docs propojené dohromady, je těžší odejít ke konkurenci.

Klíčové signály:
- **Integrace NotebookLM jako zdroje v Gemini app** (leden 2026) — zápisníky se stávají znalostní základnou pro celý Google AI stack
- **Enterprise API** (září 2025) — programatický přístup naznačuje směřování k B2B monetizaci
- **NotebookLM Enterprise jako součást Google Cloud** — oddělená enterprise varianta s VPC-SC compliance a audit trails
- **Bezplatný upgrade pro studenty** v několika zemích (včetně přístupu k NotebookLM) — budování uživatelské báze od studentů

### Co lze odvodit: od pasivního asistenta k aktivnímu agentovi

Deep Research byl prvním krokem — NotebookLM přestal být jen „čtečkou dokumentů" a stal se agentem, který sám hledá informace. Další logický krok je hlubší automatizace: zápisníky, které se samy aktualizují, proaktivně upozorňují na nové informace nebo automaticky generují reporty na základě nových zdrojů.

Audio Overviews se vyvíjejí směrem k interaktivitě (hlasový vstup, formát Debate) a multijazyčnosti (80+ jazyků). Gemini 2.5 už nativně generuje dvou-hostitelský „NotebookLM-style" dialog — technologie se přesouvá z produktu do platformy.

### Hypotézy a spekulace

- **Prohledávání napříč zápisníky** — jedna z nejžádanějších funkcí v komunitě. Google zatím nemá řešení pro consumer verzi; v Enterprise je možné zápisníky propojit jako zdroj dat pro Gemini Enterprise. Je pravděpodobné, že se nějaká forma cross-notebook vyhledávání objeví.
- **Hlubší integrace s Google Meet** — automatické nahrávání meetingů přímo do zápisníku s okamžitou extrakcí akčních položek.
- **Formát „Lecture"** pro Audio Overviews — jeden přednášející, strukturovaný monolog (na rozdíl od dosavadního formátu dvou diskutujících hostitelů). V komunitě se objevily zmínky o kódu naznačujícím tuto funkci.
- **Monetizace přes Workspace** — Google pravděpodobně vidí NotebookLM primárně jako nástroj pro zvýšení hodnoty Workspace plánů, ne jako samostatný produkt. Free tier zůstane jako akvizice uživatelů, skutečné peníze se budou dělat na enterprise.

### Zajímavost na závěr

V únoru 2026 žurnalista David Greene žaloval Google s tvrzením, že Audio Overviews reprodukují jeho charakteristický hlas bez povolení. Google obvinění odmítl. Kauza ukazuje, jak rychle se NotebookLM dostal z experimentální hračky do mainstreamu — a jaké právní otázky to přináší.

---

## 14. Mini-timeline: co se kdy objevilo

| Období | Co se objevilo |
|--------|---------------|
| **Květen 2023** | Project Tailwind představen na Google I/O |
| **Říjen 2023** | Přejmenování na NotebookLM, zpřístupnění prvním uživatelům |
| **Září 2024** | Audio Overviews — funkce, která NotebookLM proslavila |
| **Říjen 2024** | Odstranění označení „experimentální" |
| **Prosinec 2024** | NotebookLM Plus (placená verze), interaktivní Audio Overviews |
| **Květen 2025** | Mobilní aplikace (offline a background poslech Audio Overviews) |
| **Červenec 2025** | Video Overviews + vylepšený Studio panel |
| **Září 2025** | Flashcards, Quizzes, Learning Guide, Enterprise API |
| **Prosinec 2025** | Slide Decks + Data Tables, integrace AI do Workspace plánů |
| **Leden 2026** | NotebookLM jako zdroj v Gemini app, materiály pro školství |

---

## 15. Cheat Sheet — vše na jedné stránce

### Základní pravidla
- **Jeden zápisník = jedno téma.** Nemíchejte nesouvisející dokumenty.
- **Kvalita zdrojů > kvantita.** Raději 10 čistých PDF než 40 nefiltrovaných.
- **Vždy vyžadujte citace.** Přidejte do promptu: *„Každé tvrzení podlož citací."*
- **Pojmenovávejte zdroje popisně.** AI vnímá názvy souborů.
- **Ukládejte cenné odpovědi.** Save to note → Convert to source.

### Configure Chat — tři režimy
- **Default** — univerzální
- **Learning Guide** — vede krok za krokem, klade kontrolní otázky
- **Custom** — vaše persona, pravidla, formát (až 10 000 znaků)

### Studio — co generovat kdy
| Potřebuji... | Generuji... |
|--------------|------------|
| Přehled | Report (Briefing / Study Guide / FAQ) |
| Procvičování | Quiz / Flashcards |
| Strukturovaná data | Data Table → Export to Sheets |
| Prezentaci | Slide Deck → Google Slides |
| Vizuální přehled | Mind Map / Infographic |
| Porozumění „na cestě" | Audio Overview (Deep Dive / Brief / Critique / Debate) |
| Video explainer | Video Overview (Pro/Ultra) |

### Univerzální prompt šablona
> [Role]: Jsi [typ experta].
> [Úkol]: Udělej [konkrétní artefakt] z [vybraných zdrojů].
> [Důkazy]: Každé tvrzení podlož citací. Pokud něco ve zdrojích není, napiš „nenalezeno".
> [Formát]: [Tabulka / odrážky / checklist / JSON].
> [Kontrola]: Na konci vyjmenuj nejasnosti a navrhni, jaké zdroje chybí.

### Pět pravidel, která vás ochrání před chybami
1. **Nekopírujte výstupy bez kontroly.** Vždy klikněte na citace.
2. **Nastavte Custom Instructions.** Bez nich dostáváte generické odpovědi.
3. **Batchujte prompty.** Jeden strukturovaný prompt = 3–5 jednoduchých otázek.
4. **Archivujte před mazáním.** Convert notes to source → pak teprve mažte originály.
5. **Prohledávání napříč zápisníky neexistuje.** Pojmenovávejte zápisníky popisně a dělejte je tematicky.

---

*Příručka sestavena v únoru 2026 na základě oficiální dokumentace Google, komunitních zdrojů, hloubkového online výzkumu a praktických zkušeností power uživatelů.*
