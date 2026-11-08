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

## Guide A — kept

- **Field notebook.** Sun-bleached paper, ink, sorghum red, leaf green. Slab titles, ruled lines, stamp-like crop marks. Practical, not decorative.
- **Signal cloth.** Market-banner contrast: laterite orange, maize yellow, cowpea cream, cloth black, soybean green. Heavy short labels, full-width bands, icons large enough to read outdoors.
- **Extension card.** The printed card left with a farmer. Clay white, charcoal, a single blue-green, thin rules. Quiet form layout that still works in black and white.

## Guide B — instruments

A different system. No paper grain, no ruled stationery, no stamp marks, no wax-print bands, no clay-white form, no fingerprint button.

- **Field meter.** A rugged handheld. Matte charcoal ground, amber figures, pale gray words, one olive light for the selected choice. Small-caps labels, large amber action. Looks like equipment.
- **Chalk wall.** Writing on a dark earth wall. Thick chalk-white letters, millet-gold fill on the selected choice, no chrome and no rules. Crop marks are simple chalk drawings.
- **Broadcast.** A radio rundown. Black ground, white headlines, one signal-red action, gray cue numbers, hairlines only. No texture and no yellow.

## Guide C — borrowed from farm products

Sources, fetched 8 November 2026:

- [Plantix](https://plantix.net/en/) starts with one action and returns a treatment. [DesignRush](https://www.designrush.com/best-designs/apps/trends/best-agriculture-app-designs) describes that screen as stacked cards, and describes Farm.One as white space where the crop photo supplies the color.
- The same DesignRush roundup describes Agrilo as neon lime on near-black panels, with a different fill pattern on each soil reading so the meaning is not color alone.
- [OneSoil Scouting](https://play.google.com/store/apps/details?id=io.onesoil.scouting&hl=en_US) is a field check: see growth, mark a problem, add a note. Reviewers on the [App Store listing](https://apps.apple.com/us/app/onesoil-scouting-farming-tool/id1458134040) say each feature is finished and easy, rather than crowded.
- [xFarm](https://www.xfarm.ag/en/app) splits climate from fieldwork, and can hand a result to WhatsApp. The season screen stays separate from the comparison, and Share stays a handoff.

Directions:

- **Crop cards.** Off-white. A real crop photo carries the color. One primary button. Results are stacked cards, the way a diagnosis comes back.
- **Pattern bars.** Near-black panels and one neon-lime action. Water, soil, and the season are bars with a fill pattern plus a word: solid, stripes, or dots.
- **Scout note.** Pale gray, a white note pinned to a simple greenness legend. No basemap. The note names the field and the last crops. One calm green action.

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
design/screens/00-start/{field-meter,chalk-wall,broadcast}.png
design/screens/01-farm/{field-meter,chalk-wall,broadcast}.png
design/screens/02-priorities/{field-meter,chalk-wall,broadcast}.png
design/screens/03-season/{field-meter,chalk-wall,broadcast}.png
design/screens/04-compare/{field-meter,chalk-wall,broadcast}.png
design/screens/04a-card-detail/{field-meter,chalk-wall,broadcast}.png
design/screens/05-share/{field-meter,chalk-wall,broadcast}.png
design/screens/06-learn/{field-meter,chalk-wall,broadcast}.png
design/screens/00-start/{crop-cards,pattern-bars,scout-note}.png
design/screens/01-farm/{crop-cards,pattern-bars,scout-note}.png
design/screens/02-priorities/{crop-cards,pattern-bars,scout-note}.png
design/screens/03-season/{crop-cards,pattern-bars,scout-note}.png
design/screens/04-compare/{crop-cards,pattern-bars,scout-note}.png
design/screens/04a-card-detail/{crop-cards,pattern-bars,scout-note}.png
design/screens/05-share/{crop-cards,pattern-bars,scout-note}.png
design/screens/06-learn/{crop-cards,pattern-bars,scout-note}.png
```

## States written here, not drawn

These states are specified for the build and are not extra mock images: empty required fields, “not sure” soil, thin LGA data, a fourth priority refused, no priority selected, season data loading or unavailable, current rotation not recognised, all three cards similar, share failure, WhatsApp missing, SMS over 160 characters, and offline after first load.
