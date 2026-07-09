# Omnisend Email Setup Instructions

## Checkout Abandonment Email #1

### 1. Nuotraukų įkėlimas
1. Prisijunkite prie Omnisend'o
2. Eikite į **Media Library** → **Upload**
3. Įkelkite failą: `large JPG_RGB-LTEssentialsGroupImage2_MC_1.jpg`
4. Omnisend suteiks URL, pvz.: `https://cdn.omnisend.com/...`
5. Nukopijuokite šį URL

### 2. HTML šablono paruošimas
1. Atidarykite `checkout-abandonment-email-1.html`
2. Raskite: `[OMNISEND_IMAGE_URL_HERO]`
3. Pakeiskite jį gautuu Omnisend CDN URL

### 3. Dinaminiai blokai (Omnisend merge tags)

| Vieta | Ką daryti |
|------|----------|
| `{{firstName}}` | Automatinis klientės vardas (jei ne – rodys "malonu Jus matyti") |
| `{{abandonedCheckoutUrl}}` | Automatiška nuoroda į nebaigtą krepšelį |
| `{{unsubscribeUrl}}` | Automatiška atsisakymo nuoroda |

### 4. Krepšelio prekės (SVARBU)
**Šablone** yra maketo pavyzdys su `{{product.title}}` ir `{{product.price}}`.

**Omnisend'e** turėtumėte:
1. Raskite komentarą `<!-- MAKETO PAVYZDYS – Omnisend... -->`
2. Pakeiskite jį **Omnisend "Abandoned Checkout Products" bloku** – tai rodys faktines prekes iš nebaigtai sesijos
3. Arba naudokite Omnisend'o *Email Block* → *Products from abandoned checkout*

### 5. Finalizavimas
✓ Visas tekstas jau lietuvių kalba  
✓ Nėra "buy now" agresyvumo (1-o laiškas)  
✓ Dydžių gidas prilinkotas  
✓ CTA aiškus: "Užbaigti užsakymą"  
✓ Patikimumo argumentai įtraukti  

---

## Pastabos

- **Šriftai**: HTML naudoja serif (Georgia) ir sans-serif (Helvetica/Arial) dėl email kliento suderinamumo. The Seasons / Gilroy gali būti tik mockup'ams.
- **Fono spalva**: `#EFEAE3` (švelnias kreminis tonas per brand'ą)
- **Tekstas**: Šiltas, elegantiškas, be spaudimo – pasitikėjimo fokusas

---

## Nuotrauka šaltinis
- Failas iš repo: `large JPG_RGB-LTEssentialsGroupImage2_MC_1.jpg`
- 3 moterys, maudymosi / namų drabužio tema
- Puikiai atitinka Triumph + Sentiment brand'o vizualinį stilių

