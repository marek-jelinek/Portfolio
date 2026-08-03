# Portfolio - Marek Jelínek

## Popis projektu
Osobní portfolio UX designera Marka Jelínka. One-page web, běží na **marekjelinek.cz**.

## Technologie a hosting
- Čisté HTML5 + CSS3 + JavaScript (žádný framework, žádný build krok)
- Hostováno na **Cloudflare** (Workers se statickými assety, ne klasické "Pages")
  - Konfigurace: `wrangler.toml` (assets.directory = "./")
  - Cloudflare projekt se jmenuje "portfolio"
- Kód uložený na GitHubu: **marek-jelinek/Portfolio** (větev main)
  - Přihlášení přes GitHub CLI (`gh`), credential helper nastavený v Gitu
- Doména **marekjelinek.cz**:
  - Registrátor zůstává **WEDOS** (tam se doména kupovala/prodlužuje)
  - DNS (kam doména ukazuje) spravuje **Cloudflare** (nameservery přepnuty z WEDOSu)
  - U `.cz` domén WEDOS používá tzv. NSSET místo přímého zadání nameserverů
  - **E-mail na doméně momentálně nefunguje** (starý hosting/mail byl zrušen) - pokud bude
    potřeba e-mail (např. jméno@marekjelinek.cz), je potřeba buď znovu aktivovat mail hosting
    u WEDOSu, nebo použít externí službu (Google Workspace, Zoho Mail...)
  - Google Search Console ověření běží přes DNS TXT záznam (nezávislé na obsahu webu)

## Analytika (kódy přenesené ze starého webu)
- **Google Analytics 4:** G-7DKJVE3JKQ
- **Google Tag Manager:** GTM-P3LGWML
- **Microsoft Clarity:** qmzphj8pcn
- Hotjar na starém webu byl, ale vědomě se NEpřenášel (uživatel nechtěl)

## Obrázky
- `Img/Marek.png` - Profilová fotka (600×755 px, zachovat poměr stran, nikdy neořezávat)
- `Img/Donio-S.png`, `Img/RegioJet-2S.png`, `Img/Srovnavac-S.png` - obrázky projektů

## Design systém

### Barvy
- **Primární text:** #1F1F1F
- **Primární pozadí:** #F8F8F8
- **Černá:** #000000, **Bílá:** #FFFFFF
- **Šedá světlá:** #E6E6E6, **Šedá střední:** #8C8C8C

### Písmo
- **Rodina:** Arial, Inter (fallback sans-serif)
- Řádkování (line-height) u velkých/nadpisových stylů: **120 %**
- Řádkování u běžného textu (`p`): **150 %**
- Prostrkání (letter-spacing) u velkých textů: **-1px až -2px** (u H1 -2px)

### Breakpointy (zkratky používané v zadáních)
- **D (desktop):** výchozí styl, žádný media query, > 768px
- **T (tablet):** `@media (max-width: 768px)`
- **M (mobil):** `@media (max-width: 480px)`

### Systém mezer (spacing)
CSS proměnné v `:root`, používat vždy tyto, nezadávat mezery napevno v px:
- `--space-xsmall: 15px`
- `--space-small: 30px`
- `--space-medium: 50px`
- `--space-large: 70px`
- `--space-xlarge: 100px`
- `--space-ultra: 150px`
- Ke každé existuje i poloviční varianta (`--space-xxx-half`) pro rozdělení mezery mezi
  dva sousední prvky (např. spodní okraj jednoho + horní okraj druhého)
- K dispozici i utility třídy `.mt-xxx` / `.mb-xxx` (a `-half` varianty) pro obecné použití

**Kde se používá:**
- Mezi sekcemi: 200px (2× xlarge, rozděleno 100+100 padding), KROMĚ přechodu
  "O mně → Kontakt", kde je záměrně 300px (ultra + ultra)
- Mezi projekty (`.projects-grid` gap): ultra (150px)
- H1 margin-bottom: xsmall, H2: small (kromě `#intro h2` → medium), H3: xsmall

### Šířkové utility třídy
`.width-33`, `.width-50`, `.width-66`, `.width-75` (33,33 % / 50 % / 66,67 % / 75 % šířky
rodiče, na střed) - obecně použitelné na libovolný kontejner.

### Layout - hlavička a patička "na okraj"
Hlavička (`header`) a patička kontaktní sekce (`.kontakt-footer` - copyright + tlačítko
nahoru) se **záměrně roztahují až k pravému/levému okraji okna prohlížeče**, nezávisle na
tom, že hlavní obsah stránky je omezený na max-width 1400px na střed. Proto nejsou uvnitř
`.container`, ale mají vlastní horizontální padding (30px D/T, 20px M).

### Komponenty
- **Tlačítko primární** (černé s bílým textem) - pro světlé pozadí
- **Tlačítko na tmavém pozadí** (bílé s tmavým textem, třída `.button-on-dark`) - kdykoliv
  je tlačítko na tmavé/černé ploše, vždy použít tuto invertovanou variantu kvůli kontrastu
- Kartičky projektů - obrázek a text vedle sebe, **bez střídání stran** (žádný cik-cak),
  na tabletu/mobilu obrázek nahoře a text pod ním
- Hamburger menu (overlay v barvě primárního textu, ne čistě černé) - zavírací křížek se
  **nesmí animovat** (dřív docházelo k překryvu s hamburger ikonou, teď se hamburger ikona
  při otevření jen schová)
- Tlačítko scroll-to-top (kruhové, `.button-round`) - v kontaktní sekci, zarovnané k
  pravému okraji stránky (stejná úroveň jako tlačítko Konzultace v hlavičce)

## Sekce webu (aktuální pořadí)
1. **Header** - Logo + menu + tlačítko Konzultace (roztažené k okrajům okna)
2. **Úvod (#uvod)** - H1 "Redesignér", tagline, profilová fotka, hned pod tím statistiky
   (10+, 2x, 125 000+) - přesunuté sem z vlastní sekce
3. **Intro a problémy (#intro)** - H2 "Řeším složité problémy" + seznam bolestivých bodů
   (5 vět se šipkou ➔, styl `.subtitle`, zarovnané na levý praporek, blok na střed podle
   nejdelšího řádku)
4. **Projekty (#projekty)** - H2 "Vybrané práce" + podtext, 3 projekty (Donio, RegioJet,
   Srovnávač dluhopisů), obrázky a texty v jednom pořadí (bez střídání)
5. **Další projekty** - text se jmény firem a agentur (styl stejný jako `.subtitle`, 40px)
6. **O mně (#info)** - text o vzdělání a zkušenosti
7. **Kontakt (#kontakt)** - tmavé pozadí, nadpis, email/lokace/LinkedIn, v patičce copyright
   "© 2008-2026 Marek Jelínek" (vlevo) a tlačítko scroll-to-top (vpravo)

## Styl "subtitle" (dřív "intro-text")
Sdílený styl pro velké centrované "podnadpisové" texty (40px, letter-spacing -1px,
line-height 1.2). Používá se na:
- Úvodní větu v `#uvod` ("Pomáhám firmám...")
- Podtext u "Vybrané práce"
- Seznam bolestivých bodů v `#intro` (s doplňkovými pravidly `.pain-points .subtitle`,
  které mění zarovnání na levé a šířku na auto/fit-content, protože kontext je jiný -
  seznam, ne centrovaná věta)

## Meta tagy (SEO)
- Title: "Marek Jelínek – UX & Product designer"
- Description: "Přes 10 let navrhuji a zlepšuji weby a aplikace..."
- OG tagy: title, description, image, url, type
- Twitter Card

## Budoucí rozšíření
- Podstránky (case studies)
- Kalkulačka
- Blog
- E-mail na doméně (viz sekce Hosting výše)

## Poznámky k workflow
- Uživatel není programátor - u úprav vzhledu chce vidět náhled/screenshot, ne popis kódu
- Před riskantnějšími vizuálními experimenty (které se nemusí líbit) udělat git commit jako
  záchranný bod, pak zkusit variantu, a nabídnout snadný návrat
- Po dokončení nějakého uceleného kroku nabídnout git commit se srozumitelnou českou zprávou
- Nedávat po každé úpravě dlouhé shrnutí (viz obecné pravidlo v `/Users/marek/LAB/CLAUDE.md`)
