# Záloha: animace "načítání" písmene v hero nadpisu

Použito na webu do 9. 9. 2026, kdy se nadpis změnil na „Product designer".
Nadpis byl „UI designér" a písmeno I se po načtení stránky během 0,9 s
prorolovalo abecedou až na X → „UX designér".

Původní commity: b6b6afe (přidání animace), a3cbb2b (úprava startu).

## 1. HTML — hero nadpis v index.html

```html
<h1>
  U<span class="letter-cycle" data-start="I" data-target="X"
    >I</span
  >
  designér
</h1>
```

`data-start` = písmeno, kterým animace začne; `data-target` = písmeno,
na kterém skončí. Animace jede abecedou od jednoho ke druhému.

## 2. JavaScript — na konci index.html, uvnitř posledního <script>

```js
// ANIMACE "NAČÍTÁNÍ" PÍSMENE V NADPISU - PROJEDE PÍSMENA OD POČÁTEČNÍHO PO CÍLOVÉ PÍSMENO
document.querySelectorAll(".letter-cycle").forEach((el) => {
  const target = el.dataset.target;
  const startCode = (el.dataset.start || "A").charCodeAt(0);
  const targetCode = target.charCodeAt(0);
  const totalSteps = targetCode - startCode;
  const duration = 900;
  const startTime = performance.now();

  function stepLetter(now) {
    const progress = Math.min((now - startTime) / duration, 1);
    const eased = 1 - Math.pow(1 - progress, 2);
    const index = Math.round(eased * totalSteps);
    el.textContent = String.fromCharCode(startCode + index);

    if (progress < 1) {
      requestAnimationFrame(stepLetter);
    } else {
      el.textContent = target;
    }
  }

  requestAnimationFrame(stepLetter);
});
```

## 3. CSS

Žádné. Třída `.letter-cycle` neměla vlastní styly, dědila vzhled z `h1`.

## Jak to vrátit zpět

1. Do `index.html` vložit zpět HTML nadpis (bod 1).
2. Do posledního `<script>` v `index.html` vložit zpět JS (bod 2).
