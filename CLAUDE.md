# Portfolio - Marek Jelínek

## Popis projektu
Osobní portfolio UX designera Marka Jelínka. One-page web, běží na **marekjelinek.cz**.

## Technologie a hosting
- Čisté HTML5 + CSS3 + JavaScript (žádný framework, žádný build krok)
- Styly jsou v samostatném souboru **`styles.css`**, napojeném v `index.html` přes `<link>`
  (dřív byly inline v `<style>` v hlavičce, vyčleněny kvůli přehlednosti)
- Hostováno na **Cloudflare** (Workers se statickými assety, ne klasické "Pages")
  - Konfigurace: `wrangler.toml` (assets.directory = "./")
  - Cloudflare projekt se jmenuje "portfolio", účet marker@seznam.cz
  - Nasazení: `npx wrangler deploy` (přihlášení přes `npx wrangler login` - jednorázově,
    musí to udělat uživatel sám, ne agent, protože jde o přihlášení k jeho účtu)
  - Doména přístupná i přes `https://portfolio.marker-b63.workers.dev` (worker subdoména)
  - **DŮLEŽITÉ:** `assets.directory = "./"` míří na kořen celého repa, takže Wrangler by bez
    dalšího opatření nahrál na veřejný web i `.git`, `.wrangler`, `.DS_Store` apod. Tomu
    brání soubor **`.assetsignore`** (syntaxe jako `.gitignore`) - musí zůstat aktuální při
    přidávání nových lokálních/citlivých souborů do repa
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
  - **`www.marekjelinek.cz` funguje přes Redirect Rule v Cloudflare** (Rules → Redirect Rules),
    ne přes kód webu. Pravidlo: hostname = `www.marekjelinek.cz` → dynamické přesměrování
    `concat("https://marekjelinek.cz", http.request.uri.path)`, kód **301**, zachování query
    stringu. Drží se tím jedna kanonická adresa (v `index.html` je i `<link rel="canonical">`).
    - **Proč to tak je:** Worker je zaregistrovaný jen na hostname `marekjelinek.cz`. Požadavek
      přicházející jako `www...` se netrefil do žádného workeru, Cloudflare pak hledal běžný
      webserver, žádný nenašel a vracel chybu **522**. Redirect Rule požadavek zachytí dřív.
    - **DNS záznam `www` (CNAME → `marekjelinek.cz`, proxovaný) musí zůstat.** Kdyby se smazal,
      `www` přestane existovat a přesměrování nemá co zachytit.
    - Pravidlo se **nedá vytvořit ani číst přes `wrangler`/API** - OAuth token z `wrangler login`
      má na zónu jen právo čtení. Změny pravidel a DNS dělá uživatel v Cloudflare dashboardu.
  - **Úklid DNS (září 2026):** smazány mrtvé záznamy `ftp`, `imap`, `pop3`, `smtp` po starém
    WEDOS hostingu (cílové servery už neexistovaly). Doména nemá žádný MX záznam, takže e-mail
    opravdu nikam nechodí. Smazání DNS záznamů v Cloudflare nemá vliv na registraci u WEDOSu.

## Analytika (kódy přenesené ze starého webu)
- **Google Analytics 4:** G-7DKJVE3JKQ
- **Google Tag Manager:** GTM-P3LGWML
- **Microsoft Clarity:** qmzphj8pcn
- Hotjar na starém webu byl, ale vědomě se NEpřenášel (uživatel nechtěl)

## Obrázky
- `Img/Marek.png` - Profilová fotka (600×755 px, zachovat poměr stran, nikdy neořezávat)
- `Img/Donio-porovnani.png`, `Img/RegioJet-new.png`, `Img/Srovnavac-new.png` - obrázky projektů
  (1160×825 px; staré `Donio-S.png`, `RegioJet-2S.png`, `Srovnavac-S.png` jsou smazané ze
  složky, ale dají se obnovit z historie v gitu)

## Design systém

### Barvy
- **Primární text:** #1F1F1F
- **Primární pozadí:** #FFFFFF (dřív #F8F8F8)
- **Pozadí sekce O mně:** #F4F4F4 (`--section-bg-alt`, odděluje ji od sekce Služby)
- **Černá:** #000000, **Bílá:** #FFFFFF
- **Šedá světlá:** #E6E6E6, **Šedá mezi světlou a střední:** #CCCCCC (`--gray-mid-light`,
  linky tagů služeb), **Šedá střední:** #8C8C8C

### Písmo
- **Rodina:** Arial, Inter (fallback sans-serif)
- Základní text (`p`): **20px na D i T, 16px na M** (dřív bylo omylem plochých 16px všude -
  opraveno, protože 16px má být jen ten nejmenší text v systému)
- Řádkování (line-height) u velkých/nadpisových stylů: **120 %**
- Řádkování u běžného textu (`p`): **150 %**
- Prostrkání (letter-spacing) u velkých textů: **-1px až -2px** (u H1 -2px)
- Styl `.subtitle` (centrovaný "podnadpisový" text): **20px na D, T i M**, řádkování 150 %
  (dřív 40/30/30 a řádkování 120 %; zmenšeno, když se do Služeb dal delší text)
  - Používá se: text pod "Jak vám pomůžu" (Služby); pod "Vybrané projekty" je zakomentovaný
- `.stat-number` (čísla ve statistikách): **60px na D**, 30px na T, 50px na M
- Původní velikost `.subtitle` (40/30/30) mají dál:
  - položky v rozbaleném hamburger menu (`.mobile-menu a`) - **vlastní třída**, ne přímo
    `.subtitle` (na přání: "stejná velikost, ale nepoužívej stejný styl")
- Velikost stylu `h3` (36px D, 30px T/M) má i text v sekci "Ostatní projekty"
  (`.other-projects p`)

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
- K dispozici i utility třídy `.mt-xxx` / `.mb-xxx` (a `-half` varianty) pro obecné použití.
  **Momentálně se nikde v HTML nepoužívají, ale uživatel je chce zachovat v kódu pro budoucí
  použití - nemazat jako "mrtvý kód" bez ptaní.**

**Kde se používá:**
- Mezi sekcemi obecně: 200px (2× xlarge, rozděleno 100+100 padding), KROMĚ přechodu
  "O mně → Kontakt", kde je záměrně 300px (ultra + ultra) - schválně větší pauza před
  kontaktní sekcí
- Mezi projekty (`.projects-grid` gap): ultra (150px)
- Mezi sekcí Projekty a seznamem "Ostatní projekty": taky ultra (150px) - `.other-projects-section`
  má vlastní `padding-top: var(--space-medium)`, který spolu se standardním spodním
  odsazením sekce Projekty dá dohromady přesně ultra
- Mezi kontaktní mřížkou a patičkou (`.kontakt-footer`): ultra (150px)
- Sekce Služby a O mně: na D mezera nahoře i dole ultra (150px), stejně jako začátek kontaktu
  (u Služeb je horní mezera na D 180px = spodní odsazení Projektů 100 + vlastních 80;
  zvětšeno o 20 % ze 150).
  Na T je to všude 100px (včetně začátku kontaktu), na M 70px.
- Statistiky (`#intro`) na D: fotka → čísla 136px (zmenšeno o 20 % ze 170),
  čísla → nadpis "Vybrané projekty" 192px (160 + 20 %). Na T/M beze změny.
- H1 margin-bottom: xsmall, H2: small, H3: xsmall

### Šířkové utility třídy
`.width-33`, `.width-50`, `.width-66`, `.width-75` (33,33 % / 50 % / 66,67 % / 75 % šířky
rodiče, na střed) - obecně použitelné na libovolný kontejner. **Stejně jako mezerové utility
třídy se momentálně nikde nepoužívají, ale mají v kódu zůstat pro budoucí použití.**

### Layout - hlavička a patička "na okraj"
Hlavička (`header`) a patička kontaktní sekce (`.kontakt-footer` - copyright + tlačítko
nahoru) se **záměrně roztahují až k pravému/levému okraji okna prohlížeče**, nezávisle na
tom, že hlavní obsah stránky je omezený na max-width 1400px na střed. Proto nejsou uvnitř
`.container`, ale mají vlastní odsazení od okraje okna.
**Všechny prvky v rozích (logo, menu, hamburger, křížek, copyright, tlačítko nahoru) mají
jednu společnou vzdálenost od okraje: `--edge-space` (15px na D, T i M).** V patičce platí
i pro spodní okraj. **Výjimka:** textové prvky u boků - logo (vlevo), odkazy menu (vpravo)
a copyright (vlevo) - mají od boku `--edge-space-text` (30px); svisle se drží společné osy / spodku. Logo, odkazy menu a hamburger leží v hlavičce na jedné středové ose.

### Menu a hlavička
- **Logo** (vlevo nahoře) **není fixní** - je součástí normálního toku stránky (`position:
  absolute` vůči dokumentu, ne vůči oknu) a při scrollování normálně odjede pryč se stránkou.
  Není součástí hamburger menu.
- **Tlačítko Konzultace v menu už není** - bylo v hlavičce na D i v rozbaleném hamburger menu,
  obojí je v `index.html` **zakomentované** (ne smazané), aby se dalo kdykoli vrátit. Roli CTA
  převzalo tlačítko "Chci konzultaci" v sekci O mně.
- **Desktop (D):** nahoře na stránce je vidět celé menu (jen odkazy).
  Jakmile uživatel začne scrollovat (`body.scrolled`), menu se plynule zmenší, posune a
  schová a místo něj naskočí kulatý hamburger (stejný, jaký je trvale vidět na T/M).
- **Tablet a mobil (T/M):** hamburger je vidět vždy, celé menu s odkazy se nezobrazuje nikdy.
- **Hamburger ikona:** černé kolečko 50×50px (stejná velikost jako `.button-round` dole v
  patičce), fixní pozice vpravo nahoře. Při otevření menu hamburger zmizí a **na přesně
  stejném místě** (stejné souřadnice) se objeví křížek pro zavření (`.close-btn`) - bez
  vlastního kolečka kolem sebe, jen ikona samotná.
- Otevřené menu (`.mobile-menu`) je overlay v barvě primárního textu (ne čistě černé).

### Komponenty (tlačítka)
**Na webu je momentálně jediné klasické tlačítko - CTA "Chci konzultaci" v sekci O mně -
a to je ve stylu `.button-primary`. Obrysové varianty se nikde nepoužívají.**

- **`.button-primary`** (černé s bílým textem) - jediný používaný styl tlačítka: CTA
  "Chci konzultaci" v sekci O mně. Dřív mělo vlastní hover (změna pozadí na šedou), ten byl
  zrušen ve prospěch jednotného hoveru (viz níže).
- **`.button-large`** - modifikátor velikosti, přidává se k základnímu tlačítku
  (`class="button button-primary button-large"`). Rozměry o 20 % větší než základní tlačítko
  (padding 14,4/24px) a **velikost písma podle běžného textu**, tedy 20px na D i T a 16px na M
  (základní tlačítka jdou na M na 14px). Používá se u CTA v sekci O mně.
- **`.button-outline`** - obrys (1px, barva primárního textu), průhledné pozadí, text stejnou
  barvou jako okolní texty. **Momentálně nepoužité** - zbylo jen v zakomentovaném tlačítku
  Konzultace v hlavičce. Zachovat, nemazat.
- **`.button-outline-on-dark`** - stejný princip, ale bílý obrys a bílý text pro tmavé pozadí.
  **Momentálně nepoužité** - zbylo jen v zakomentovaném tlačítku Konzultace v hamburger menu.
  Zachovat, nemazat.
- **`.button-round`** (kruhové) - tlačítko scroll-to-top v patičce kontaktní sekce, bílé na
  tmavém pozadí, zarovnané k pravému okraji stránky. Jediné další tlačítko na webu.
- Kartičky projektů - obrázek a text vedle sebe, **bez střídání stran** (žádný cik-cak),
  na tabletu obrázek nahoře a text pod ním, na mobilu text nahoře a obrázek (s popiskem) pod ním.

### Hover efekt
Všechny interaktivní prvky (odkazy, všechna tlačítka, logo, hamburger, křížek zavření)
mají **jednotný hover efekt** - jemné ztlumení na `opacity: 0.6` (přechod 0.3s). Sjednoceno
z dřívějších různých hoverů (barva textu, barva pozadí) do jednoho pravidla kvůli
konzistenci. Původně zkoušeno agresivnější `opacity: 0.25`, ale to působilo jako příliš
velká změna barvy - zmírněno na 0.6. Poslední zbytek starého hoveru (`.button-primary:hover`
měnil pozadí na šedou) byl odstraněn, když se `.button-primary` začal používat.

### Animace
- **Menu → hamburger:** při scrollování na desktopu se menu zmenší a odsune (`scale` +
  `translateX`, 0.35s) a hamburger se "vypruží" na místo (scale s `cubic-bezier` odrazem).
- **Počítání čísel ve statistikách:** čísla (10+, 2.0x, 125 000+) se při vjetí do viewportu
  (IntersectionObserver, spustí se jen jednou) načítají od nuly nahoru, cca 1,6s pro všechna
  tři čísla stejně (ease-out křivka, ne lineárně). Přesnost odpovídá výslednému číslu - celé
  jednotky pro 10+ a 125 000+, desetiny pro 2.0x (běží jako "0.3x, 0.7x..." a na konci zůstane
  přesně "2.0x", ne zaokrouhleno na "2x").

## Sekce webu (aktuální pořadí)
1. **Header** - Logo (mimo hlavičku, není fixní) + menu/hamburger (fixní, viz výše)
2. **Úvod (`#uvod`)** - H1 "Product designer", tagline, profilová fotka. Statistiky už tu NEJSOU
   (přesunuté do sekce Intro, viz níže).
3. **Intro a výsledky (`#intro`)** - H2 "Řeším složité problémy", pod ním `.subtitle` text,
   pod ním statistiky (10+, 2.0x, 125 000+) s počítací animací. Dřív tu byl seznam bolestivých
   bodů se šipkami ➔ (`.pain-points`) - ten byl zrušen a nahrazen jednou větou.
4. **Projekty (`#projekty`)** - H2 "Vybrané projekty" (dřív "Vybrané práce"). Podtext
   `.subtitle` ("Mám za sebou desítky projektů...") je **zakomentovaný** (skrytý, ne smazaný).
   3 projekty (Donio, Srovnávač dluhopisů, RegioJet), obrázky a texty v jednom pořadí
   (bez střídání stran). Na D je text 1/3 a obrázek 2/3 šířky (`grid-template-columns: 1fr 2fr`).
   Pod obrázkem může být šedý popisek `.project-caption` (15px na všech zařízeních,
   `--gray-medium`, 15px pod obrázkem, šířka jako obrázek, zarovnaný na střed) - u všech tří projektů.
   Donio používá obrázek `Img/Donio-porovnani.png`.
5. **Ostatní projekty (`.other-projects-section`)** - text se jmény firem a agentur, velikost
   textu jako `h3` (36/30/30).
6. **O mně (`#info`)** - text o vzdělání a zkušenosti, pod ním CTA tlačítko
   "Chci konzultaci" (`.button-primary .button-large`), které scrolluje na kontakt.
7. **Kontakt (`#kontakt`)** - tmavé pozadí, nadpis, email/lokace/LinkedIn, v patičce copyright
   "© 2008-teď Marek Jelínek" (vlevo, rok se píše jako "teď", ne pevné datum) a tlačítko
   scroll-to-top (vpravo).

## Meta tagy (SEO)
- Title: "Marek Jelínek – Product designer"
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
- Když uživatel řekne "nech to/ty" u něčeho označeného jako nepoužívané - znamená to zachovat
  v kódu i když je to momentálně "mrtvé", ne smazat (viz mezerové/šířkové utility, `.button-primary`)
- Nasazení (`wrangler deploy`) je citlivá akce viditelná navenek - před prvním nasazením v
  session ověřit, že existuje a funguje `.assetsignore`, ať se znovu nenahraje `.git` na web
