# FieldDynamics screen mock brief

Phone-browser tool for a Kaduna extension advisor. One Android phone per image, portrait 9:16, UI filling a 360-wide screen. Large thumb targets, short text, crop pictures before paragraphs. No map, no install screen, no desktop layout.

Eight screens. Three guides, three directions each. The screen list, the Zaria sample, and the shared components stay the same. Earlier guides stay in the folders. Guide C borrows interaction ideas from other farm products and does not copy their names, logos, or screens.

## Locked sample

Every screen tells this story:

- LGA: Zaria
- Soil: loamy
- Water: rain only
- Equipment: hand tools
- Last two seasons: maize, then maize
- Priorities: save water, extra income (2 of 3)
- Market: Zaria grain market
- Current rotation: maize then maize
- Option A: maize then soybean
- Option B: maize then cowpea, marked lower confidence
- Evidence on Option A: Usman (2017), IAR Samaru
- Language: English selected on an EN | HA switch. No invented Hausa sentences.
- Footer on every screen: “This supports your judgment. Check with your extension officer.”

Frost is not shown as a threat.

## Screens

The locked screen system covers 11 screens representing the complete user journey and PRD specifications:

| Folder | Screen | What is on it |
| --- | --- | --- |
| `00-start` | F0. Start | Name, promise, EN/HA switch, Start, “Last saved data” for a returning Zaria visit |
| `01-farm` | F1. Farm | LGA dropdown on Zaria, soil (with satellite default), water, equipment, past 2 seasons with real crop photos |
| `02-priorities` | F2. Priorities | 7 priority buttons (2 of 3 chosen cap), where crop is sold |
| `03-season` | F3. Your season | 12-month strip for Zaria: rain, July dry spell, bare months, hottest months, NASA POWER trend line, scenario selector |
| `03a-crop-check` | F3d. Crop check | Maize monthly water demand (ETc) vs rainfall curve, highlighting pollination stage risk during July dry spell |
| `04-compare` | F4. Compare | Three stacked cards: Current (continuous maize), Option A (maize then soybean), Option B (maize then cowpea) |
| `04a-card-detail` | F5. Card detail | Option A opened: why, scientific evidence (Franke 2008 & Usman 2017), benefits, gaps, risks to check |
| `05-share` | F6. Share | Summary card, WhatsApp, Print card, Play Hausa sample, Radio script (SMS removed) |
| `05a-print-card` | P4. Extension card | Physical 1-page A5 summary card designed to be left with the smallholder |
| `06-learn` | F7. Learn | Plain glossary, satellite vs ground data (POWER vs GPM), how the tool decides, model comparison |
| `07-trend-map` | F8. LGA trend map | Interactive map of Kaduna's 23 LGAs colored by onset shift or erosion risk, with Zaria popover |

## Shared components

- Header with FieldDynamics, step indicator, and EN | HA switch
- Primary thumb button (min 44px) and quiet secondary actions
- Choice chips, real crop-photo buttons, 7 priority tiles with a 2-of-3 count
- Dual-encoded season strip (color + pattern + text label)
- Rotation cards with confidence badges, opportunity tags, and water demand in mm
- Share channels tailored to smallholders (WhatsApp, Print, Audio, Radio) without SMS
- Offline cached-data indicators

## Design System: Crop Cards

The team evaluated 9 initial design directions across Guides A, B, and C. The **Crop Cards** system was selected as the unified direction and upgraded with principles borrowed from high-craft agtech benchmarks:

- **From Klim (Justified Studio):** Confident typography, warm off-white canvas (`#FAF8F5`), clean card borders (`#E2DED5`), and digestible multi-season rotation summaries that eliminate bureaucratic fatigue.
- **From FarmRise (Lollypop / Bayer):** High-contrast outdoor legibility under direct sunlight, oversized thumb targets, custom illustrative crop iconography, and an intuitive card stream.
- **From Arable:** Tactical data indicators, modular card stacks, and explicit units on every reading (mm, °C).

Avoid a generic SaaS look: no Inter or Roboto, no purple-to-blue gradient, no glass panels, no rounded shadow cards on everything, and no three marketing feature cards.

## Future design inspiration & benchmarks

High-craft benchmarks scouted for future design iterations:

1. **[Klim](https://klim.eco/) (Justified Studio)** — Modern regenerative agriculture companion. Bold typography and editorial card layouts. Its "Crop Mix Composer" turns multi-season crop rotation planning and soil impact into clean, consumer-grade mobile cards without administrative fatigue.
2. **[FarmRise](https://www.lollypop.design/work/farmrise/) (Lollypop Design Studio / Bayer)** — A' Design Award (Interface & UX) and C2A winner. Built specifically for smallholders on budget Android devices. Engineered for direct midday sun and rough hands: oversized thumb targets, custom illustrative crop iconography, and an intuitive card-stream layout that replaces nested menus.
3. **[Arable](https://www.arable.com/) (Arable Mobile)** — Industrial microclimate elegance and IoT Breakthrough winner. Pairs daily canopy ground-truth photos directly with moisture and temperature charts. Features modular card stacks for water demand and crop stress using an earthy, high-contrast palette with purposeful alert accents.

## Files

```text
design/screens/00-start/crop-cards.png
design/screens/01-farm/crop-cards.png
design/screens/02-priorities/crop-cards.png
design/screens/03-season/crop-cards.png
design/screens/03a-crop-check/crop-cards.png
design/screens/04-compare/crop-cards.png
design/screens/04a-card-detail/crop-cards.png
design/screens/05-share/crop-cards.png
design/screens/05a-print-card/crop-cards.png
design/screens/06-learn/crop-cards.png
design/screens/07-trend-map/crop-cards.png
```

## States written here, not drawn

These states are specified for the build and are not extra mock images: empty required fields, “not sure” soil, thin LGA data, a fourth priority refused, no priority selected, season data loading or unavailable, current rotation not recognised, all three cards similar, share failure, WhatsApp missing, SMS over 160 characters, and offline after first load.
