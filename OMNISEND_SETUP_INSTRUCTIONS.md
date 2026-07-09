# Omnisend Setup Instructions — Checkout Abandonment Email #1

## Kodėl blokais, o ne vienu HTML?

Omnisend canvas turi fiksuotą plotį (matote "Canvas width: 600" nustatymuose).
Vienas didelis HTML failas su savo fiksuotu 600px stalu + papildomu
paminkštinimu aplink jį pridėdavo papildomus pikselius ir turinys išlįsdavo
už canvas ribų (tiek desktop, tiek mobile peržiūroje). Kiekvienas blokas dabar
yra **fluid** (width:100%, be išorinio wrapper'io) — jis tiksliai atsitaiso
pagal canvas plotį, jokio persidengimo.

Taip pat išskaidymas leidžia tarp blokų įterpti **Omnisend natūralų
"Products" elementą** (matomas kairėje bibliotekoje, po "Content" →
"Products", pažymėtas žvaigždute), kuris automatiškai užpildo REALIAS
krepšelyje paliktas prekes — vietoj rankomis parašyto mockup.

## Surinkimo eiliškumas Omnisend redaktoriuje

Kiekvieną `.html` failą įdėkite kaip atskirą **"Custom HTML" content
bloką** (Content → HTML), tiksliai šia tvarka:

1. **`checkout-abandonment-email-1-block-1-header-hero.html`**
   Logotipas + hero nuotrauka. ŠIS BLOKAS TURI BŪTI PIRMAS — jame apibrėžti
   bendri mobile CSS class'ai (`.es-pad`, `.es-h1` ir t.t.), kuriuos naudoja
   ir kiti blokai.
   → Pakeiskite `[OMNISEND_IMAGE_URL_HERO]` į Omnisend įkeltos nuotraukos URL
   (failas: `large JPG_RGB-LTEssentialsGroupImage2_MC_1.jpg`).

2. **`checkout-abandonment-email-1-block-2-intro.html`**
   Antraštė „Jūsų prekės vis dar laukia" + įžanginis tekstas.

3. **Nemokamo pristatymo juosta – DVI ALTERNATYVOS per Conditional Content:**
   Pridėkite Omnisend **"Conditional Content"** bloką su taisykle pagal
   `Cart Total`:
   - Šaka **Cart Total < 40** → įdėkite
     `checkout-abandonment-email-1-block-3a-freeshipping-remaining.html`
   - Šaka **Cart Total >= 40** → įdėkite
     `checkout-abandonment-email-1-block-3b-freeshipping-achieved.html`

4. **Krepšelio prekės – NAUDOKITE OMNISEND NATŪRALŲ ELEMENTĄ:**
   Kairėje bibliotekoje eikite į **Products** (žvaigždutės ikona) ir
   įtempkite jį čia. Jis automatiškai susies su abandoned checkout eventu ir
   parodys realias krepšelyje esančias prekes (nuotrauka, pavadinimas,
   kiekis, kaina) — kartosis kiekvienai prekei automatiškai, nereikia jokio
   rankinio HTML.

5. **`checkout-abandonment-email-1-block-4-cta-sizeguide.html`**
   CTA mygtukas „Užbaigti užsakymą" + nuoroda į dydžių gidą.

6. **`checkout-abandonment-email-1-block-5-trust.html`**
   „Kodėl verta rinktis Sentiment" – 6 patikimumo argumentai.

7. **Poraštė – rekomenduojame Omnisend natūralų "Footer" elementą**
   (kairėje bibliotekoje, po "Content"), nes jis automatiškai sutvarko
   Unsubscribe/Preferences nuorodas pagal teisinius reikalavimus.
   Jei norite pilnai custom dizaino vietoj to — naudokite
   `checkout-abandonment-email-1-block-6-footer.html`.

## Merge tag'ai (patikrinti Omnisend Abandoned Checkout evente)

| Tag | Reikšmė |
|---|---|
| `{{ firstName }}` | Kontakto vardas |
| `{{ cartTotal }}` | Krepšelio suma |
| `{{ checkoutUrl }}` | Nuoroda atgal į checkout |
| `{{ currency }}` | Valiuta |
| `{{ unsubscribeUrl }}` | Atsisakymo nuoroda |

## Testavimas

Redaktoriaus canvas NEAPDOROJA merge tag'ų (matysite juos kaip raidinį
tekstą `{{ ... }}`) — tai normalu. Norėdami pamatyti realias reikšmes,
naudokite viršuje esantį **"Preview & test"** mygtuką arba išsisiųskite
testinį laišką sau.

## Nemokamo pristatymo skaičiavimas

`{{ 40 | minus: cartTotal | at_least: 0 | round: 2 }}` — apskaičiuoja, kiek
liko iki 40 € ribos, apkerpant neigiamas reikšmes iki 0.
`{{ cartTotal | times: 2.5 | at_least: 0 | at_most: 100 }}` — juostos
užpildymo procentas (100 / 40 = 2.5), apkarpytas 0–100 ribose.
Jei filtras `at_most` nepalaikomas, juostos konteineryje yra
`overflow:hidden` apsauga, kad procentas vizualiai neišlįstų.
