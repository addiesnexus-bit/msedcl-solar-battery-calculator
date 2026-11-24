# Solar + Battery Calculator – HT Industrial (MSEDCL)

A single-file, browser-based calculator that sizes solar and battery storage for an HT Industrial consumer in Maharashtra (MSEDCL). It compares two battery strategies side by side and shows the output in Hinglish.

- **Full Backup:** the battery covers all non-solar hours (Zone A + B + D).
- **Smart Saving / Peak Shaving:** the battery covers Zone D and part of Zone B.

No build step, no dependencies, no backend. All calculation runs in the browser. The only external request is Google Fonts, and the page falls back to system fonts without it.

## Files

| File | Purpose |
|---|---|
| `solar_battery_calculator_msedcl.html` | The complete calculator (HTML + CSS + JS) |
| `README.md` | This file |

## Run locally

Open `solar_battery_calculator_msedcl.html` in any modern browser.

## Host on GitHub Pages

1. Rename the HTML file to `index.html` (or keep the name and open it by full path).
2. Push it to a GitHub repository.
3. Go to **Settings → Pages**, choose the `main` branch and the `/ (root)` folder, then save.
4. The site goes live at `https://<username>.github.io/<repo-name>/`.

## Inputs

| Group | Fields |
|---|---|
| Connection | Sanctioned Load (kW), Contract Demand (kVA), Billing Days |
| Monthly units (kWh / kVAh) | Total units, Zone A (00–06), Zone B (06–09), Zone C (09–17), Zone D (17–24) |
| Existing system & bill | Existing Solar (kW), Monthly Bill (Rs.) |
| Rates (ex-GST) | Solar Rs./kW, Battery Rs./MWh, PCS Rs./kW, Civil/EMS % |
| Battery pack | Volt, Ah, chemistry (LiFePO4 90% / Lead Acid 50%), apply DoD to pack count |
| Assumptions | New-bill % for each mode, PCS peak factor, Year-12 battery replacement %, MWh and Sq.Ft per container |
| Demand capping (optional) | Old billed demand, new capped demand, demand charge Rs./kVA |

Default values are sample data. Replace them with the customer's actual figures and your current quotes.

## Formulas

```
Battery kWh          = V × Ah / 1000
Daily units          = Monthly units / Billing days
Solar hours daily    = Zone C daily
Non-solar daily      = Zone A + B + D daily

Required solar (kW)  = (Total daily units × 1.15) / 4.2      # Nagpur: 4.2 units/kW/day
Additional solar     = Required solar − Existing solar
Solar area           = Additional solar × 80 Sq.Ft
Solar generation/day = Total solar kW × 4.2

Full Backup battery  = Non-solar daily × 1.15
Smart Saving battery = Zone D daily × 1.15 + Zone B daily × 0.5

New bill (Full)      = Current bill × 15%
New bill (Smart)     = Current bill × 32%
Investment           = (Additional solar × rate + Battery MWh × rate + PCS kW × rate) × (1 + Civil/EMS %)
Payback (years)      = Investment / Yearly saving
25-year saving       = Yearly saving × 25 − Battery replacement (Year 12, one time)
Demand capping saving = (Old billed demand − New capped demand) × Demand charge   # optional
```

The tool recommends **Smart Saving first** when its payback is shorter than Full Backup's, and says so otherwise.

## Assumptions and limitations

Read these before showing numbers to a customer.

- **Bill factors are thumb rules.** The 15% and 32% new-bill figures are approximations. The real MSEDCL bill has energy charge, fixed charge, wheeling, FAC, duty and TOD components. These factors drive the payback more than anything else, so treat the output as indicative.
- **Solar size is the same in both modes.** Solar is sized to the full daily load. In Smart Saving mode, where the battery is smaller, this can oversize solar and overstate investment.
- **Investment counts only additional solar.** Existing solar is treated as already installed.
- **PCS sizing is an assumption.** Full Backup: non-solar daily ÷ 16 h × peak factor. Smart Saving: Zone D daily ÷ 7 h × peak factor.
- **Battery pack count follows the required kWh directly.** Tick "Pack count me DoD lagao" to divide by usable depth of discharge.
- **Demand capping is off by default.** Fixed charges are already inside the new-bill percentages, so enabling it can double-count.
- **TOD zones are user-defined.** They may not match the current MSEDCL tariff slabs exactly. Check against the latest tariff order.
- **Container capacity and footprint are defaults.** Edit them to match your supplier.

## Download as PDF

The **Download as PDF** button opens the browser's print dialog. Choose **Save as PDF**. The input panel is hidden in the printout. Use the downloaded HTML file or GitHub Pages for this; it may not work inside embedded or sandboxed previews.

## Disclaimer

Prices are indicative, ex-GST. Final pricing and sizing are confirmed only after a site survey.
