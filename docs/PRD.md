# FieldDynamics — Product Requirements Document

**Status:** Draft 0.1  
**Date:** 7 November 2026  
**Event:** NASA Space Apps Challenge 2026, Field Shift: Adapting Farms with NASA Data, 13–14 November  
**Team:** FieldDynamics Lab  
**Region:** Kaduna State, Nigeria  
**Stack:** FastAPI, HTMX, Alpine.js, Tailwind CSS

This draft is written from the product brief, the sources list, and the 1 November 2026 kickoff transcript. The judges’ Requirements tab (R1–R26, P1–P10, S1–S6, and Tier 2) was referenced by screen, and its full acceptance text was not in the materials used here.

## 1. Product

FieldDynamics is a light phone-browser tool for a Community-Based Advisor or extension agent in Kaduna. During a visit, the advisor enters the farmer’s local government area, field conditions, recent crops, and priorities. The tool compares three rotations — the farmer’s current one and two alternatives — using NASA’s long climate record for that LGA and named local rotation trials. The advisor leaves a summary on a channel the farmer already uses.

The farmer does not install an app. The tool supports the advisor’s judgment. It does not replace it.

**Promise:** The weather guide tells farmers when the rain comes. FieldDynamics tells them what to plant next, and why.

## 2. Problem

Kaduna farmers already receive season timing through people and channels they use today: Community-Based Advisors, the Kaduna State Farmers’ Weather Guide (NiMet, via HEDA), Hausa radio, voice calls, and USSD or SMS. Those channels say when rain is likely to start, stop, and break. They do not show, for a specific LGA, which rotation fits the field, what it costs in water, what the soil gains, or what the advisor should check before the farmer acts.

The pattern that has reached farmers in northern Nigeria puts the phone in a trusted person’s hand and sends the result back on WhatsApp, SMS, a printed card, or radio. FieldDynamics follows that pattern. It adds a crop-rotation layer to advice Kaduna already runs.

## 3. Users

| User | Who they are | Device | What they need |
| --- | --- | --- | --- |
| Community-Based Advisor or extension agent | Visits farmers in an LGA; trained through AGRA, NAERLS, or KADA programmes | Work smartphone, often on a weak network | A fast comparison they can do with the farmer, and something to leave behind |
| Smallholder farmer | Rain-fed farmer growing maize, soybean, sorghum, or cowpea; often Hausa-speaking | Basic phone, a shared smartphone, or no phone | Advice in plain words they can hear or keep |
| Radio presenter | Weekly farming segment, for example Alheri FM Kaduna | Laptop or phone | A short LGA-level script before planting season |

The advisor operates the tool. The farmer decides. The result travels on a channel the farmer already has.

The farmer profile comes from the challenge brief and the sources in this document. The farmer survey is meant to confirm or correct it. Assisted filling is required, because an online English form only reaches connected, English-reading farmers.

## 4. Principles

1. Phone first. One-handed use on a small Android screen. Targets are large enough for a thumb.
2. Light. No heavy map on first load. A map is optional and only after every Must requirement passes.
3. Pictures before words. Crop icons, a colour strip, and simple symbols. Text is short enough to read aloud.
4. Honest. Every recommendation shows its confidence and what to check locally.
5. The farmer’s current rotation is always shown first and is never labelled wrong.
6. Every screen can switch between English and Hausa.
7. Every screen ends with: “This supports your judgment. Check with your extension officer.”
8. Area climate data can live on the server. Farmer-specific answers stay on the phone until the advisor taps Share.
9. Same farm inputs and the same published LGA dataset always produce the same three cards.
10. Every evidence line names a study. Frost risk in Kaduna is shown as very low.

## 5. Scope

### Build for the event

- Six phone screens, plus card detail, in the browser. Nothing to install.
- All 23 Kaduna LGAs.
- Three rotation cards and a detail view.
- A 12-month season strip for the selected LGA.
- English, plus Hausa for key labels, and one recorded Hausa voice sample.
- Share actions the advisor starts: WhatsApp, SMS, and a one-page printed card.
- A text export of an LGA-level radio script.
- Preloaded NASA POWER monthly series for all 23 LGAs, plus MODIS greenness for the season strip.
- Evidence lines drawn only from named studies already in the brief.

### Demonstrate, and finish later

- Hausa voice: one recorded sample at the event. Automated voice comes later.
- Radio: a text script at the event. A partnership with a station such as Alheri FM comes later.

### Roadmap only

- USSD or automated voice calls, through partners who already run them (Crop2Cash, Precision Development).
- A new radio station, call centre, or short-code service.
- Tier 2 items in the judges’ checklist, including an optional map and sliders. These start only after every Must row passes.

### Out of scope

- A farmer app to download.
- Field sensors, scarecrow devices, or wearables.
- Member-only franchise software.
- Advice for a market outside Kaduna during this challenge.
- Sending farmer details anywhere before the advisor taps Share.

## 6. Journey

1. The advisor opens the tool on their phone. A return visit can restore the last saved farm.
2. They enter the LGA, soil, water, equipment, and the last two seasons of crops, with the farmer.
3. They pick up to three priorities, and where the crop is sold.
4. They look at the LGA’s year: rain, the usual dry spell, bare months, and the hottest months.
5. They compare the current rotation with two alternatives.
6. They open a card to see why it was suggested, the named evidence, the benefits, the gaps, and the risks to check.
7. The farmer chooses. If they are unsure, they check with an extension officer before anything is shared.
8. The advisor shares the chosen summary on WhatsApp, SMS, or a printed card. For a radio segment, they export an LGA script that contains no farmer identity.

## 7. Screens

| # | Screen | What the advisor does | States to handle | Requirement ids from the brief |
| --- | --- | --- | --- | --- |
| 0 | Start | Sees the name, the one-line promise, the language switch, and Start. A return visit shows “Last saved data”. | First visit, returning visit, offline | P1, P5 |
| 1 | Farm | Picks one of 23 LGAs. Soil: sandy, loamy, clay, or not sure. Water: rain only, irrigation, or both. Equipment: hand tools, animals, or tractor. Last two seasons from crop icons. | Missing field, “not sure”, LGA with thin data | R3, R13, R14, P6 |
| 2 | Priorities | Taps up to three: save water, better soil, extra income, feed animals, fewer pests, stop soil washing away. Says where they sell. | More than three, none picked | R5, R15, R16 |
| 3 | Your season | Sees a 12-month strip: rainy months, usual dry spell, bare-field months, hottest months. Each colour explains itself on tap. | Loading, unavailable, slow network | R17, R18, R20, R23 |
| 4 | Compare | Sees three cards, side by side on a wide screen and stacked on a phone: Current, Option A, Option B. Each shows crops by season, fit with priorities, water need, soil benefit, and confidence. | Current rotation not recognised, all three similar | R6, R7, R16, R19, R21 |
| 4a | Card detail | Reads why it was suggested, one line of named evidence, benefits, information gaps, and risks to check with an expert. | Low confidence shows a warning first | R8–R12, R22 |
| 5 | Share | Taps WhatsApp, SMS, Print card, Play Hausa sample, or Radio script. | Share fails, WhatsApp missing, SMS over 160 characters | P2, P3, P4, P7 |
| 6 | Learn | Reads a plain glossary: satellite versus ground data, how the tool decides, and the data sources. | — | R23, R24, R25, S3 |

## 8. Functional requirements

### Farm profile

- The LGA control lists all 23 Kaduna LGAs: Birnin Gwari, Chikun, Giwa, Igabi, Ikara, Jaba, Jema’a, Kachia, Kaduna North, Kaduna South, Kagarko, Kajuru, Kaura, Kauru, Kubau, Kudan, Lere, Makarfi, Sabon Gari, Sanga, Soba, Zangon Kataf, and Zaria. Free text is not enough.
- Soil, water, and equipment include an explicit “not sure” or equivalent only where the brief allows it. Soil has “not sure”. Water and equipment do not.
- Crop choices cover at least maize, soybean, sorghum, and cowpea, shown as icons.
- The advisor can save a farm on the device and resume it. The Start screen says when saved data is present.
- The tool still produces a season strip and cards when an answer is “not sure” or an LGA has a thin record, and it says that the confidence is lower.

### Priorities

- The advisor can select one, two, or three priorities. A fourth selection is refused with a visible reason.
- Continuing with none selected is blocked, with a short prompt to pick at least one.
- “Where they sell” is captured in plain language. It informs the extra-income and market reading. It is not sent anywhere by itself.

### Season strip

- Twelve months, for the selected LGA.
- Rainy months, the usual mid-season dry spell, bare-field months, and the hottest months are distinguishable without relying on colour alone. Each mark has a tap target and a text meaning.
- Expected shape for Kaduna, checked against the Weather Guide and NASA POWER:
  - Rain onset between early May and mid-June. The guide cites 9 May in Sanga and 10 June in Makarfi.
  - Rain ending mid to late October.
  - Little or no rain from about November through March.
  - A mid-season dry spell flagged around 10–25 July in central areas.
  - Northern LGAs show a shorter, drier season than southern LGAs. Makarfi and Kachia are the comparison pair.
- A frost warning for Kaduna is a defect. The strip shows frost risk as very low.
- If the series is missing, the screen says so. It does not invent a season.

### Comparison

- Exactly three cards: the farmer’s current rotation first, then two alternatives.
- The starting set, pending confirmation from the Data owner, is:
  - Current: continuous maize, until farmer history says otherwise.
  - Maize then soybean.
  - Maize then cowpea.
  - Sorghum then cowpea is the drier-LGA alternative, used where the LGA’s rainfall record supports it.
- Each card shows the crop sequence by season, how it meets the chosen priorities, relative water need, soil benefit, and a confidence level.
- If the last two seasons do not match a known rotation, the current card still appears, labelled as the farmer’s recent crops, and the two alternatives are still offered. The current card is never called wrong.
- If all three cards are similar, the screen says what they share and what still differs.
- Opening a card shows, in this order when confidence is low: the warning, why it was suggested, one evidence sentence naming a study, benefits, information gaps, and risks to check with an extension officer.
- Running the same farm twice yields the same cards.

### Learning

- Glossary entries are plain enough to read aloud.
- The page explains the difference between satellite records and a measurement taken in the field.
- The page explains, at a high level, how a card is chosen, and lists the datasets the running build actually uses.

## 9. Evidence the cards may cite

Cards cite a named study in one sentence. They do not paraphrase a source that is not in this list.

| Source | What a card may say it supports |
| --- | --- |
| Usman (2017), ABU Zaria, long-term rotation, nitrogen and tillage at IAR Samaru | Maize after soybean or cowpea, and the soil-quality effect |
| CIAT, crop rotation and nitrogen fertilization in maize, northern Guinea savanna of Nigeria | Yield after legumes, and nitrogen efficiency |
| Ebireri (2013), ABU Zaria, soil quality and maize yield in Samaru Alfisols | Cover crops and organic matter |

A new study can be added only after it is written into the Data & Evidence record and checked by QA. An evidence line with no named study is a High defect.

## 10. Data

### Must be in the event build

| Dataset | What the product uses it for | Access |
| --- | --- | --- |
| NASA POWER monthly series | Rainfall, temperature, and soil wetness for each of the 23 LGAs, over 20 or more years | POWER Data Access Viewer. No login. Pre-download before 13 November. |
| MODIS greenness (NDVI) | Bare-field months versus peak rainy months on the season strip | AppEEARS |

### Available if the Data owner confirms the link and the file

Giovanni (GPM rainfall), Earthdata Search (SMAP, GRACE-FO, Landsat, SRTM), and further AppEEARS extracts (ECOSTRESS, SMAP, SRTM). The brief lists these from general knowledge. Each link has to be opened and confirmed before the project page claims it.

### Local season reference

The Kaduna State Farmers’ Weather Guide 2025 is the check for onset, cessation, and the July dry spell. NASA POWER is the check for the long monthly record. The two can disagree. The screen shows the NASA record and can note the guide’s window. It does not silently average them into a third number.

### Units and privacy

- Every number on screen has a unit: mm, °C, or t/ha.
- LGA climate files are not farmer data.
- Farm answers stay in the browser. A network trace during ordinary use, before Share, shows no farmer payload leaving the phone.
- The radio script contains the LGA and the season advice. It contains no farmer name and no phone number.

### Modeling

The challenge checklist asks for a comparison of statistical or model approaches on historical climate and soil, including seasonal transitions, dry periods, and stress periods, and for rotation scenarios compared on nutrient cycling, erosion, organic matter, water, and market. The 1 November transcript assigns that modeling to Lenge D. Joshua.

The product requirement is narrower than the modeling method. Cards must be repeatable, cited, and understandable by an advisor. A trained model may inform the cards. If it is not ready on event day, the cards still ship from the preloaded POWER series, the MODIS strip, and the three named trials. The Learn screen describes the method the submitted build actually uses.

“SI” appears in the checklist as SI modeling and data fusion. The kickoff discussion treated it as statistical or model-based pattern finding. QA still needs the official expansion from the challenge statement before slides use the term.

## 11. Share outputs

| Channel | Reaches | Payload | Event scope | Pass condition |
| --- | --- | --- | --- | --- |
| WhatsApp | A household with a smartphone | Summary image if built, plus short text, via a WhatsApp share link | Build | The app opens with the text filled. Nothing is sent until the advisor sends it. If WhatsApp is missing, a clear fallback appears. |
| SMS | Any phone | One message of 160 characters or fewer: chosen rotation, plant window, one risk | Build | The SMS app opens under the character limit. |
| Printed card | A farmer with no phone | One page the advisor prints or photographs | Build | One page, readable in black and white, no cut-off text. |
| Hausa voice note | A farmer who cannot read | A recorded Hausa summary | One sample | Playback works. Automated recording is later. |
| Radio script | Listeners in an LGA | Area-level season advice | Text export | No farmer name or phone number. |
| USSD or voice call | Basic phones at scale | Short-code menu or automated call | Roadmap | Not in the event build. |

Shared text follows the language selected in the tool.

## 12. Language

- English is complete on every screen.
- Hausa covers key labels, and the share payload follows the selected language.
- One recorded Hausa sample is included for the voice action.
- Full Hausa copy is a later decision, not a promise of this draft.

The farmer survey still needs a language question: Hausa, English, or other. That question is open on the survey, not in the app.

## 13. Quality bar

We submit when all of the following are true:

- Every Must row in the Requirements tab is Pass.
- No Critical or High bug is open.
- At least one non-technical tester completes the usability script without help and can say, in their own words, why the chosen plan was suggested.
- The submission checklist is complete.

### Usability script

Tester uses a phone. The facilitator does not help. Record time and every hesitation.

1. “You farm maize every year in Zaria LGA, rain only, with hand tools. Enter your farm.”
2. “You want to save water and earn extra income. Which plan fits best, and why?”
3. “Send that plan to the farmer on WhatsApp.”

### Devices

- A low-end Android phone in Chrome, if the team has one: every screen and Share.
- Laptop Chrome, Slow 3G: Start, Season, and Compare load and respond.
- Laptop Chrome at 360 px wide: no sideways scrolling, buttons tappable.
- Offline after the first load: a clear message, and no silent failure.
- iPhone Safari: every screen.

### Defects that block submission

| Severity | Meaning here |
| --- | --- |
| Critical | A Must requirement or the submission is blocked. Example: fewer than three cards, or a private repository. |
| High | Wrong or misleading advice, or a broken core flow. Example: a frost warning for Kaduna, or evidence citing the wrong study. |
| Medium | The flow works and the meaning is unclear. Example: a colour on the season strip with no explanation. |
| Low | Cosmetic. |

Bugs use the shared sheet columns from the QA plan: Bug ID, Category, Title, Type, Preconditions, Steps to reproduce, Actual result, Expected result, Evidence, Status, Environment, QA comment, Business impact, Next steps, Assigned to, Retest comment, Reporter.

### Submission

- Demo video of 30 seconds or less, or a deck of 7 slides or fewer. Which one is still open until the dry run.
- Every link opens in a private window.
- The repository is public and opens while logged out.
- Datasets named on the project page are datasets the code uses.
- Required project-page fields are filled, and the terms box is ticked.

## 14. Technical shape

The interface is server-rendered HTML. FastAPI serves pages and small HTML fragments. HTMX swaps those fragments. Alpine.js handles local interaction that should not wait on the network: the language switch, the three-priority cap, tap-for-meaning on the season strip, and restoring the last farm. Tailwind styles a phone-first layout.

Suggested boundaries:

- Routes for the eight views, returning full pages on first load and fragments for HTMX updates.
- LGA climate JSON, prepared before the event, read by the server. The browser does not download raw NASA files.
- Recommendation code is a pure function of the farm profile plus the LGA file, so the repeatability test is a unit test.
- Share links are built on the client from the selected card and the selected language. The server does not receive the farm profile as part of sharing.
- Static assets are few: compiled CSS, crop icons, HTMX, Alpine, and one Hausa audio file. The first screen does not load a map.

This stack is the one agreed for the software build. It is separate from the Python modeling work, which can prepare the LGA files and the rotation notes the app reads.

## 15. Roles

| Person | Responsibility in this product |
| --- | --- |
| Goodness Mbakara | Software: the phone web tool |
| Lenge D. Joshua | Data and models: NASA series, rotation scenarios, SI or statistical comparison |
| Maureen Obiekwe | QA against the Requirements tab, the usability script, and the submission checklist |
| Mohamed Agnaou | Joined the kickoff late. Role is not assigned in the notes used here. |
| Business analyst named in the kickoff | Was expected and was absent. Role is not assigned in the notes used here. |

## 16. Open decisions

| Decision | Options | Where it stands in the materials | Owner |
| --- | --- | --- | --- |
| Market | Kaduna, or a US farm market | The 1 November call left this open. Maureen argued for northern Nigeria. The later brief is written entirely for Kaduna and is the basis of this PRD. | Whole team |
| Primary user | Farmer directly, or advisor with farmer | Brief recommends advisor with farmer, due 4 November. This PRD follows that. | Whole team |
| The three rotations | Continuous maize, maize–soybean, maize–cowpea, sorghum–cowpea in drier LGAs | Recommended from ABU and IAR Samaru trials. “Continuous maize” still needs confirmation. Due 5 November. | Data |
| Hausa coverage | Full Hausa, key labels, or later only | Brief recommends key labels plus one recorded sample. Due 7 November. | App and QA |
| Demo format | 30-second video or 7 slides | Decide after the dry run, by 12 November. | Whole team |
| Pre-event work | Planning only, or code as well, before 13 November | Read the Space Apps rules and keep a dated log. | QA / PM |
| Meaning of “SI” | Official challenge wording | Use the brief’s exact term in slides once confirmed. | QA / PM |
| Survey gaps | Language, water, equipment, two missing priorities, LGA dropdown, skip logic when the farmer has no phone | Listed as open in the QA plan. Survey: tally.so/r/OQaY7A | QA |

## 17. Risks

| Risk | What we do |
| --- | --- |
| The survey only reaches connected, English-reading farmers | Assisted filling with CBAs or ABU and IAR contacts, read in Hausa. State the limitation in the slides. |
| NASA downloads slip | Pre-pull POWER monthly data for all 23 LGAs before the event. |
| Tier 2 maps and sliders consume the event | Build them only after every Must row passes. |
| Evidence lines are vague | Every line cites a study from section 9. |
| The demo exceeds 30 seconds or 7 slides | QA times the video and counts the slides. |
| A link is private or broken | QA opens every link in a private window. |
| Frost is shown as a real Kaduna threat | Show “frost risk: very low in Kaduna”. |
| A model is unfinished on 13 November | Ship the deterministic cards from the preloaded series and the named trials. Describe that method on the Learn screen. |

## 18. Success for the challenge

A Kaduna advisor, on a cheap Android phone and a slow network, can enter a Zaria maize farm, compare three rotations, understand why one of them fits “save water” and “extra income”, and hand the farmer a WhatsApp message, an SMS of at most 160 characters, or a one-page card. The current rotation is still on screen. The evidence names a real trial. The season looks like Kaduna.

## 19. Gaps in this draft

- The full text of R1–R26, the product rows beyond the screen map, S1–S6, and the Tier 2 bonus rows.
- Confirmation that each NASA portal link loads, and the exact files the build will ship.
- A written scoring method for water need, soil benefit, and confidence. Section 14 requires the method to be deterministic. It does not invent the weights.
- Whether Space Apps allows code before 13 November.
- Final Hausa scope, final rotation list, and demo format.
