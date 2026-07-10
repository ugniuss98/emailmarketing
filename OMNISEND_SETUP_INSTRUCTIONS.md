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
   jūsų automation event lauką `event.value` (tai jūsų event'e yra
   krepšelio suma):
   - Šaka **event.value < 40** → įdėkite
     `checkout-abandonment-email-1-block-3a-freeshipping-remaining.html`
   - Šaka **event.value >= 40** → įdėkite
     `checkout-abandonment-email-1-block-3b-freeshipping-achieved.html`

4. **Krepšelio prekės:**
   Jūsų event'e prekių laukai (`event.lineItems[0].productTitle`,
   `productImageURL`, `productURL`, `productSKU`, `productDiscount`,
   `productStrikeThroughPrice`, `productVariantID` ir t.t.) yra pasiekiami
   TIK per fiksuotą indeksą `lineItems[0]` — tai reiškia, kad ranka rašytas
   HTML su šiais laukais parodys **tik pirmą** krepšelio prekę, ne visas.
   ⚠️ Jei norite, kad automatiškai rodytųsi VISOS krepšelio prekės (kai jų
   daugiau nei viena), naudokite Omnisend natūralų **"Products"** elementą
   (kairėje bibliotekoje, po "Content" → "Products", pažymėtas žvaigždute)
   — jis susieja su abandoned checkout eventu ir kartojasi kiekvienai
   prekei automatiškai. Custom HTML su `lineItems[0]` tinka tik jei
   žinote, kad krepšelyje visada yra 1 prekė, arba norite rodyti tik
   pirmąją prekę kaip akcentą.

5. **`checkout-abandonment-email-1-block-4-cta-sizeguide.html`**
   CTA mygtukas „Užbaigti užsakymą" + nuoroda į dydžių gidą.

6. **`checkout-abandonment-email-1-block-5-trust.html`**
   „Kodėl verta rinktis Sentiment" – 6 patikimumo argumentai.

7. **Poraštė – rekomenduojame Omnisend natūralų "Footer" elementą**
   (kairėje bibliotekoje, po "Content"), nes jis automatiškai sutvarko
   Unsubscribe/Preferences nuorodas pagal teisinius reikalavimus.
   Jei norite pilnai custom dizaino vietoj to — naudokite
   `checkout-abandonment-email-1-block-6-footer.html`.

## Merge tag'ai

Yra DVI skirtingos sintaksės, priklausomai nuo lauko tipo:

**Kontakto lygio laukai** — naudoja `{{ }}`:

| Tag | Reikšmė |
|---|---|
| `{{ firstName }}` | Kontakto vardas |
| `{{ unsubscribeUrl }}` | Atsisakymo nuoroda |

**Jūsų automation event laukai** — naudoja `[[event.field]]` (pagal jūsų
rastą lauko sąrašą):

| Tag | Reikšmė |
|---|---|
| `[[event.value]]` | Krepšelio suma |
| `[[event.abandonedCheckoutURL]]` | Nuoroda atgal į checkout |
| `[[event.cartID]]` | Krepšelio ID |
| `[[event.lineItems[0].productTitle]]` | Pirmos prekės pavadinimas |
| `[[event.lineItems[0].productImageURL]]` | Pirmos prekės nuotrauka |
| `[[event.lineItems[0].productURL]]` | Nuoroda į pirmą prekę |
| `[[event.lineItems[0].productSKU]]` | Pirmos prekės SKU |
| `[[event.lineItems[0].productDiscount]]` | Nuolaida |
| `[[event.lineItems[0].productStrikeThroughPrice]]` | Kaina prieš nuolaidą |
| `[[event.lineItems[0].productVariantID]]` | Varianto ID |
| `[[event.lineItems[0].productVariantImageURL]]` | Varianto nuotrauka |

Visada pridėkite `|default:"..."` (kaip jūs jau darėte), kad tuščias laukas
netaptų matomas kaip klaida.

## Testavimas

Redaktoriaus canvas NEAPDOROJA merge tag'ų (matysite juos kaip raidinį
tekstą `{{ ... }}` arba `[[ ... ]]`) — tai normalu. Norėdami pamatyti
realias reikšmes, naudokite viršuje esantį **"Preview & test"** mygtuką
arba išsisiųskite testinį laišką sau.

## Nemokamo pristatymo skaičiavimas

`[[40 | minus: event.value | at_least: 0 | round: 2 | default: "0"]]` —
apskaičiuoja, kiek liko iki 40 € ribos, apkerpant neigiamas reikšmes iki 0.
`[[event.value | times: 2.5 | at_least: 0 | at_most: 100 | default: "0"]]`
— juostos užpildymo procentas (100 / 40 = 2.5), apkarpytas 0–100 ribose.
Jei filtras `at_most` nepalaikomas, juostos konteineryje yra
`overflow:hidden` apsauga, kad procentas vizualiai neišlįstų.
