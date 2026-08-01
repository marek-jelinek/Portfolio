# Portfolio - Marek Jelínek

## Popis projektu
Osobní portfolio UX designera Marka Jelínka. One-page web s sekciemi: úvod, projekty, o mně, kontakt.

## Technologie
- HTML5
- CSS3
- JavaScript (na navigaci)
- Hostování: Cloudflare Pages

## Funkčnost
- Responzivní design (desktop, tablet, mobile)
- Fixní menu (roluje s uživatelem)
- Hamburger menu na mobilech (overlay v barvě primárního textu #1F1F1F)
- Tlačítko "Konzultace" v navigaci a v hamburger menu
- Kulaté tlačítko se šipkou nahoru v patičce (ve2 pravého rohu)
- SEO optimalizace
- AI zmínky

## Obrázky
- `Img/Marek.png` - Profilová fotka
- `Img/Donio-S.png` - Projekt Donio
- `Img/RegioJet-2S.png` - Projekt RegioJet (2x)
- `Img/RegioJet-S.png` - Projekt RegioJet
- `Img/Srovnavac-S.png` - Projekt Srovnávač dluhopisů

## Design systém

### Barvy
- **Primární text:** #1F1F1F (tmavě šedá)
- **Pozadí:** #F8F8F8 (velmi světlá šedá)
- **Černá:** #000000
- **Bílá:** #FFFFFF
- **Sekundární šedá:** #8C8C8C

### Písmo
- **Rodina:** Arial, Inter (fallback: sans-serif)
- **H1:** 76px, bold
- **H2:** 48px, bold
- **H3:** 32px, bold
- **Text:** 16px, regular

### Rozestupy
- **Mezera mezi prvky:** 30px (CSS proměnná: --container-gap)

### Layout
- **One-page design s fixní navigací**
- **Responsivní: desktop → tablet (hamburger) → mobile (hamburger)**

### Komponenty
- Tlačítko primární (černé s bílým textem) – pro použití na světlém pozadí
- Tlačítko na tmavém pozadí (bílé s tmavým textem) – pro použití na černé/tmavé ploše (např. tlačítko Konzultace v hamburger menu, nebo tlačítko scroll-to-top v kontaktní sekci). Pravidlo: kdykoliv je tlačítko umístěné na tmavém pozadí, použij tuto invertovanou variantu místo primární, aby zůstal dostatečný kontrast.
- Odkaz (interaktivní text)
- Kartičky projektů
- Hamburger menu (overlay, žádné zavírací tlačítko se nesmí animovat, aby nedocházelo k překryvu s hamburger ikonou)
- Tlačítko scroll-to-top (kruhové se šipkou, tmavé pozadí → bílá varianta tlačítka)

## Sekce webu
1. **Header/Navigace** - Logo + navigační menu + tlačítko Konzultace
2. **Úvod (Hero)** - Nadpis "Redesignér", podtext, profilová fotka
3. **Statistiky** - 10+ let, 2x konverze, 125 000+ Kč úspor
4. **Projekty** - Vybrané práce (Donio, RegioJet, Srovnávač dluhopisů)
5. **Ostatní projekty** - Text se jmény firem a agentur
6. **O mně** - Text o vzdělání a zkušenosti
7. **Kontakt** - Nadpis, email, lokace, LinkedIn odkaz
8. **Footer** - Tlačítko scroll-to-top

## Meta tagy (SEO)
- Title: "Marek Jelínek – UX & Product designer"
- Description: "Přes 10 let navrhuji a zlepšuji weby a aplikace..."
- OG tagy: title, description, image, url, type
- Twitter Card

## Budoucí rozšíření
- Podstránky (case studies)
- Kalkulačka
- Blog
- Dynamické obsahy
