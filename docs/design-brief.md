# FieldDynamics screen mock brief

Phone-browser tool for a Kaduna extension advisor. One Android phone per image, portrait 9:16, UI filling a 360-wide screen. Large thumb targets, short text, crop pictures before paragraphs. No map, no install screen, no desktop layout.

Eight screens, three visual directions. Layout, content, and components stay the same. Only colour, type, texture, and density change.

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

| Folder | Screen | What is on it |
| --- | --- | --- |
| `00-start` | Start | Name, promise, EN/HA switch, Start, “Last saved data” for a returning Zaria visit |
| `01-farm` | Farm | LGA dropdown on Zaria, soil, water, equipment, last two seasons as crop icons |
| `02-priorities` | Priorities | Up to 3 large taps, 2 of 3 selected, market field |
| `03-season` | Your season | 12-month strip for Zaria: rain, July dry spell, bare months, hottest months, tap-for-meaning |
| `04-compare` | Compare | Three stacked cards: Current, Option A, Option B |
| `04a-card-detail` | Card detail | Option A opened: why, evidence, benefits, gaps, risks |
| `05-share` | Share | WhatsApp, SMS, Print card, Play Hausa sample, Radio script |
| `06-learn` | Learn | Glossary, satellite vs ground, how a card is chosen, data sources |

## Shared components

- Header with FieldDynamics, step, and EN | HA
- Primary thumb button and a quiet secondary action
- Choice chips, crop-icon buttons, priority tiles with a 2-of-3 count
- Season strip with an icon plus a label, so meaning does not depend on colour alone
- Rotation card: sequence, priority fit, water, soil, confidence
- Low-confidence mark on Option B only
- Share row of five actions
- Saved-data line on Start only

## Directions

- **Field notebook.** Sun-bleached paper, ink, sorghum red, leaf green. Slab titles, ruled lines, stamp-like crop marks. Practical, not decorative.
- **Signal cloth.** Market-banner contrast: laterite orange, maize yellow, cowpea cream, cloth black, soybean green. Heavy short labels, full-width bands, icons large enough to read outdoors.
- **Extension card.** The printed card left with a farmer. Clay white, charcoal, a single blue-green, thin rules. Quiet form layout that still works in black and white.

Avoid a generic SaaS look: no Inter or Roboto, no purple-to-blue gradient, no glass panels, no rounded shadow cards on everything, and no three marketing feature cards.

## Files

```text
design/screens/00-start/{field-notebook,signal-cloth,extension-card}.png
design/screens/01-farm/{field-notebook,signal-cloth,extension-card}.png
design/screens/02-priorities/{field-notebook,signal-cloth,extension-card}.png
design/screens/03-season/{field-notebook,signal-cloth,extension-card}.png
design/screens/04-compare/{field-notebook,signal-cloth,extension-card}.png
design/screens/04a-card-detail/{field-notebook,signal-cloth,extension-card}.png
design/screens/05-share/{field-notebook,signal-cloth,extension-card}.png
design/screens/06-learn/{field-notebook,signal-cloth,extension-card}.png
```

## States written here, not drawn

These states are specified for the build and are not extra mock images: empty required fields, “not sure” soil, thin LGA data, a fourth priority refused, no priority selected, season data loading or unavailable, current rotation not recognised, all three cards similar, share failure, WhatsApp missing, SMS over 160 characters, and offline after first load.
