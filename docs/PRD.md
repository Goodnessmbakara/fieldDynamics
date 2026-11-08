# FieldDynamics PRD: Field Shift Rotation Advisor

**Date:** Nov 8, 2026 · @Someone  
**Status:** Ready for Build  
**Event:** NASA Space Apps Challenge 2026, local event November 14 to 15  
**Challenge:** Field Shift: Adapting Farms with NASA Data  
**Team:** FieldDynamics Lab  
**Region:** Kaduna State, Nigeria, all 23 LGAs  

FieldDynamics is a phone-first decision-support tool that helps Kaduna extension advisors compare a farmer's current crop rotation with two evidence-backed alternatives, using NASA Earth observations, local soil data and the farmer's own priorities. This PRD specifies every feature to the level needed to build, test and judge it, and maps every line of the NASA Space Apps 2026 "Field Shift" challenge statement to a feature.

---

## 1. Summary, goals and non-goals

**Product promise:** Kaduna's advisors already tell farmers when the rain is coming. FieldDynamics tells them what to plant next, and why, using NASA's long-term view from space. It supports their judgment; it does not replace it.

**Event:** NASA Space Apps Challenge 2026, local event November 14 to 15.  
**Challenge:** Field Shift: Adapting Farms with NASA Data.  
**Team:** FieldDynamics Lab.  
**Region:** Kaduna State, Nigeria, all 23 LGAs.

### What we ship
A light web tool, opened in a phone browser, that takes a farmer's situation as input (LGA, soil, water, equipment, past crops, priorities, market) and returns:
1. **A season profile** for that LGA from 20+ years of NASA data: rainy months, usual dry spell, bare-field months, hottest months, and how the season is shifting.
2. **Three rotation cards:** the farmer's current rotation plus two evidence-backed alternatives, ranked by the farmer's priorities, each scored on the same seven dimensions and carrying a confidence level.
3. **A detail view per card:** why it was suggested, evidence, benefits, information gaps, and risks to check locally.
4. **A shareable summary** on channels the farmer already uses (WhatsApp, printed card, Hausa voice note, LGA radio script).
5. **A Learn screen** that explains the science in plain words.

### Goals

| Goal | Measure of success | Checked by |
| :--- | :--- | :--- |
| **Cover every line of the challenge statement** | Every row of the traceability table in section 12 maps to a built feature | QA, before submission |
| **Real NASA data behind every number** | No hardcoded climate values in the app; all come from the data contract | Code review plus S3 |
| **Explainable SI layer** | Two trained models, each beaten against a simple baseline on held-out years and LGAs | README evaluation table |
| **Usable by an advisor without help** | A non-technical tester completes Start to Share in under 3 minutes, unaided | Usability script (R26) |
| **Works where advisors work** | Start, Season and Compare load under Chrome Slow 3G at 360 px wide | QA device matrix |
| **Judges can see it all** | Every Must feature appears in the video, project page or README | S7 mapping |

### Non-goals for the two-day build
- **No SMS:** Removed from scope; the share screen offers WhatsApp, print, Hausa voice note and radio script.
- **No farmer-installed app:** Advisors open a web page; farmers receive the result.
- **No weather forecast:** NiMet already forecasts. We describe long-term patterns and risk, not next season's rain.
- **No sensors or field hardware:** Satellites observe the field; nothing is installed.
- **No USSD, call centre or radio station:** These are roadmap items through partners (Crop2Cash, PxD, Alheri FM).
- **No per-crop satellite detection:** At this scale satellites cannot tell maize from soybean; crop specificity comes from crop rules and water balance (stated on Learn).
- **No user accounts or stored farmer data:** Nothing is saved server-side about a farmer.

### Priority legend used in this PRD
- **Must:** required to submit. Built and passing QA before feature freeze (Day 2, hour 6).
- **Should:** built if every Must passes.
- **Bonus:** only if every Must and Should passes.

Requirement IDs (R, P, S, T) match the Requirements v2 tab in the team's Field Dynamics Lab sheet. Rows this PRD adds are marked new and numbered from R39.

---

## 2. Users, context and journey

The advisor operates the tool, the farmer decides, and the result travels back to the farmer on a channel they already have. We add a rotation layer to Kaduna's existing advice system; we do not replace AGRA's CBAs, NiMet's weather guide, HarvestPlus radio, PxD, Crop2Cash or Babban Gona.

### Users

| User | Who they are | Device and connection | What they need from FieldDynamics |
| :--- | :--- | :--- | :--- |
| **Primary: Community-Based Advisor (CBA) or extension agent** | Visits farmers in their LGA; trained under AGRA, NAERLS or KADA programmes | Work smartphone, often on 2G or weak 3G | A fast way to compare rotations with a farmer on the spot, and something to leave behind |
| **Beneficiary: smallholder farmer** | Rain-fed, often Hausa-speaking; grows maize, sorghum, soybean, cowpea, groundnut, millet | Basic phone, shared smartphone, or none | Advice in plain words or Hausa that they can hear, keep or show |
| **Secondary: radio presenter (e.g. Alheri FM Kaduna)** | Runs a weekly farming segment | Laptop or phone | A short LGA-level script before planting season, with no individual farmer data |
| **Reviewer: NASA Space Apps judge** | Earth science and software expert | Laptop | Clear proof of NASA data, sound methods, honest uncertainty, real users |

### Context that shapes the design
- Farmers plan the next season in the dry months (November to April), which is when advisors visit and fields are bare.
- Many farmers intercrop (for example maize with cowpea), so the current-rotation input must accept an intercrop.
- Kaduna has three agro-zones: the drier North (millet, sorghum, groundnut, cowpea, sesame), the Central maize and soybean belt, and the wetter South (ginger, yam, cassava). Candidate rotations must come from what each LGA actually grows.
- Kaduna North and Kaduna South LGAs are mostly urban with little cropland; the tool says so instead of showing thin cards.

### Journey, step by step
1. Advisor opens the link on their phone. The Start screen loads fast, even offline from last use.
2. **Farm screen:** advisor picks the LGA, confirms or changes the soil type (pre-filled from iSDAsoil), sets water access and equipment, and taps the crops grown in the last two seasons.
3. **Priorities screen:** farmer chooses up to 3 goals (for example add nitrogen, extra income) and where they sell.
4. **Your season screen:** advisor shows the farmer the 12-month strip for their LGA: when rain comes, the usual dry spell, when fields are bare, the hottest months, and how the season has shifted over 20+ years. Optional: tap a crop to see its water need against local rain, or pick a season type (normal, late start, dry spell year) to see how cards change.
5. **Compare screen:** three cards side by side (stacked on a phone): Current, Option A, Option B, with the same seven rows and a confidence badge each.
6. **Card detail:** advisor reads the why, evidence, benefits, gaps and risks with the farmer. Low-confidence cards show "Check with your extension officer" first.
7. **Farmer decides.** The tool never picks for them; the current rotation is never labelled wrong.
8. **Share:** advisor sends the chosen summary on WhatsApp, prints or photographs the card, plays the Hausa voice sample, or exports the LGA radio script.
9. **Learn (any time):** plain-language glossary, satellite vs ground data, how the tool decides, and data sources.

---

## 3. System overview

Everything heavy runs before anyone opens the app: data is pulled, harmonised and modelled offline, and the results are written as precomputed files that follow one locked data contract. The app only reads those files and runs the rule-based engine on the advisor's inputs, so it stays fast on a weak network and never waits on a live satellite query.

---

## 4. Data sources

Five NASA datasets do the real work, joined with soil, land cover and crop data from outside NASA. Everything is aggregated to 23 LGAs x months (and cropland-only where it matters) before any model sees it: that is our common analysis grid (R29).

### NASA datasets (used)

| Dataset | What it gives us | Native resolution | Period pulled | Feeds | Priority |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **NASA POWER (daily, agroclimatology)** | Rainfall, min/max temperature, humidity, solar radiation, wind, surface and root-zone soil wetness | About 0.5 degrees | 1981 to latest | Onset and dry spells, heat days, frost check, long-term trend, FAO-56 ET0 inputs | Must |
| **MODIS NDVI (MOD13Q1)** | Greenness every 16 days | 250 m | 2000 to latest | Bare-field months, crop stress model target | Must |
| **GPM IMERG (daily)** | Satellite rainfall including intense events | About 0.1 degrees | 2000 to latest | Extreme rain days, rain intensity for erosion, cross-check of POWER rain | Must |
| **SMAP soil moisture (L3/L4)** | Top-layer soil wetness | About 9 km | 2015 to latest | Soil moisture anomalies, stress model feature | Must |
| **SRTM elevation** | Height and slope | 30 m | Static | Slope for the erosion and infiltration index | Must |

### NASA datasets considered and not used (shown on Learn, R34)

| Dataset | Reason not used |
| :--- | :--- |
| **GRACE / GRACE-FO** | About 300 km footprint: Kaduna is only a handful of pixels. Long-term water trend comes from the 40-year POWER rainfall record instead |
| **Landsat** | Field-level detail, but cloudy in the rainy season and heavy to process in 48 hours. Roadmap for field-level zones |
| **ECOSTRESS** | Patchy coverage over Nigeria. Evapotranspiration is computed with FAO-56 instead |
| **GEDI** | Measures forest structure and biomass, which does not describe annual crops |

### Non-NASA data

| Dataset | What it gives us | Resolution | Feeds | Priority |
| :--- | :--- | :--- | :--- | :--- |
| **iSDAsoil v1 (all 21 layers, Earth Engine)** | Texture, pH, organic and total carbon, nitrogen, P, K, Ca, Mg, S, Zn, Fe, Al, CEC, bulk density, stone, bedrock depth, texture class, fertility constraints, plus uncertainty | 30 m, 0 to 20 and 20 to 50 cm | Soil default per LGA, nutrient and organic matter rows, infiltration index, risk lines, confidence | Must |
| **ESA WorldCover v200** | Cropland mask (class 40) | 10 m | Restricts NDVI and soil statistics to farmland | Must |
| **Crop baseline per LGA (NAERLS Agricultural Performance Survey, Kaduna State Bureau of Statistics, 2025 zone study)** | Top crops and planted area per LGA or zone | LGA or senatorial zone | Which crops may appear on each LGA's cards | Must |
| **Rotation evidence (Franke et al. 2008 Zaria trials; Usman 2017; Ebireri 2013; CIAT savanna study; Ghana savanna rotation trials)** | Measured effects of rotations on yield, soil, pests | Study sites | Evidence lines, rules file, pest-cycle claims | Must |
| **FAO-56 crop tables** | Crop coefficients (Kc), stage lengths, rooting depths | Per crop | Water need per crop | Must |
| **Kaduna Farmers' Weather Guide 2025 (NiMet, HEDA)** | Onset dates per LGA, dry spell window | LGA | Sanity checks on our onset detection | Should |
| **Market price data (NAERLS or state market surveys)** | Relative crop prices | State | Market row on cards | Should |
| **FieldDynamics farmer survey (Tally)** | Current rotations, problems, digital access, preferred channels | Respondent | Default candidates, pitch evidence | Should |

### Data rules
- Every dataset is logged in `docs/sources.md` with product version, date range, access date and link (S3).
- Values that are interpolated or filled (cloud gaps in NDVI, SMAP gaps) are flagged and lower confidence.
- Sand, silt and clay from iSDAsoil are plain percentages; all other iSDA properties use the back-transform on their catalog page.
- Every number shown to a user traces to a dataset or a cited study. No invented values.

---

## 5. Analytics layer

Twelve components turn raw data into what the cards say: two trained models, each compared against a simpler method, plus statistical indicators, physics and rules. All of them run offline before the event demo; their outputs are precomputed files the API serves (section 11). The rotation ranking itself stays rule-based because no outcome labels exist to train it on.

| ID | Component | Type | Requirement rows |
| :--- | :--- | :--- | :--- |
| **A1** | Harmonisation and cropland mask | Data processing | R29, R2 |
| **A2** | Rain onset, cessation and dry spells | Agronomic rule | R18, R20, R32 |
| **A3** | Climate indicators | Statistical | R20 |
| **A4** | Bare-field detection | Statistical | R17, R27 |
| **A5** | Season-type clustering | Trained (unsupervised) | R27, R28, R18 |
| **A6** | Crop stress model | Trained (supervised) | R27, R28, R21, R33 |
| **A7** | Long-term trend | Statistical | R20, R32 |
| **A8** | Soil water properties | Pedotransfer | R3, R4 |
| **A9** | Crop water balance | Physics (FAO-56) | R4, R13, R20 |
| **A10** | Erosion and infiltration index | Rule-based index | R19, R31 |
| **A11** | Crop rules knowledge base | Knowledge base | R4, R16, R19, R31 |
| **A12** | Confidence aggregator | Rule | R21, R22 |

### A1. Harmonisation and cropland mask
- **Inputs:** every gridded dataset in section 4, LGA boundaries (GAUL level 2, 23 LGAs checked), ESA WorldCover.
- **Method:** clip to Kaduna; mask water; aggregate to LGA mean per day (POWER, GPM), per 16-day composite (NDVI), per month (SMAP); compute NDVI and soil statistics twice, over all land and over cropland only; resample with bilinear for continuous values, nearest for classes, mean when coarsening.
- **Output:** one tidy table per source keyed by `lga`, `date`, and `lga_month_features.parquet` (all sources joined, monthly).
- **Done when:** one command rebuilds every table; no LGA is missing; LGAs under 10% cropland are flagged `low_cropland`.

### A2. Rain onset, cessation and dry spells
- **Inputs:** daily rainfall (POWER, cross-checked with GPM).
- **Method:** onset = first day after 1 April with at least 20 mm over 3 days and no dry spell of 10 or more days in the next 30 days (the standard West African agronomic definition). Cessation = first day after 1 September when a 70 mm soil water bucket, drained by daily evaporation, empties. Dry spell = run of consecutive days under 1 mm inside the season.
- **Output per LGA-year:** onset date, cessation date, season length, longest dry spell, number of dry spells of 10+ days, and their calendar window.
- **Checked against:** NiMet 2025 onset dates per LGA (May 9 in Sanga to June 10 in Makarfi) and the July 10 to 25 dry spell window.

### A3. Climate indicators
- **Heat stress:** days with Tmax at or above 35 °C per month (maize pollination risk), from POWER.
- **Frost risk:** days with Tmin at or below 2 °C. Expected to be zero for Kaduna; shown as "very low" rather than hidden.
- **Extreme precipitation:** GPM days above the 95th percentile of wet days and days above 50 mm, per month.
- **Soil moisture anomaly:** SMAP monthly value minus its 2015 to latest monthly mean, divided by the standard deviation (z-score).
- **Evaporation:** reference evapotranspiration ET0 per month from A9.
- **Output:** `lga_month_indicators` with a long-term mean and the share of years above each threshold.

### A4. Bare-field detection
- **Inputs:** cropland NDVI per LGA (A1).
- **Method:** Savitzky-Golay smoothing (window 5 composites, order 2) to remove cloud noise, then months with smoothed NDVI below a bare threshold (start at 0.25, tuned per zone) are marked bare.
- **Compared against:** the same threshold on raw NDVI. Reported: number of false flips per year and agreement with the known dry season (November to April).
- **Output:** per LGA, a 12-month bare probability (share of years bare in that month).

### A5. Season-type clustering (trained, unsupervised)
- **Unit:** one row per LGA-year (23 LGAs x about 24 years since NDVI starts in 2000).
- **Features:** onset day, cessation day, season length, total rain, longest dry spell, count of 10+ day dry spells, heat days, peak cropland NDVI. Standardised.
- **Main model:** K-means, k chosen between 2 and 6 by silhouette score.
- **Compared against:** Gaussian mixture model (BIC to pick components), which also gives each year a membership probability.
- **Output:** named season types from centroids (for example normal, late start, mid-season drought, wet), the share of years each LGA spent in each type, and a monthly rain and NDVI profile per type. These profiles drive the season-type selector (F4b).
- **Evaluated by:** silhouette and Davies-Bouldin scores, stability across 20 seeds, and whether documented bad years fall in a stress cluster.

### A6. Crop stress model (trained, supervised)
- **Target:** cropland NDVI anomaly in the peak months (August and September) per LGA-year, as a z-score against the LGA's own mean. Negative = stressed season.
- **Features:** onset day, dry spell count and length, rainfall per growth window, heat days, extreme rain days, SMAP anomaly (2015 onward, missing earlier), static soil (sand, organic carbon, available water capacity).
- **Baseline:** Ridge regression.
- **Main model:** LightGBM, kept shallow because the table is small (about 550 rows): at most 8 leaves, at least 20 rows per leaf, early stopping.
- **Uncertainty:** LightGBM quantile models at the 10th and 90th percentile give a range per prediction.
- **Validation:** leave-one-year-out and grouped folds by senatorial zone. Never a random split.
- **Metrics:** RMSE, MAE, R² on held-out folds; interval coverage (the 80% range should contain about 80% of results).
- **Explainability:** SHAP values show which conditions drive stress in each LGA; the top drivers feed the card "Why" lines.
- **Honesty rule:** if LightGBM does not beat Ridge, we say so and ship Ridge.
- **Output:** expected stress and its range per LGA and season type; feature importance table for Learn and README.

### A7. Long-term trend
- **Inputs:** annual series 1981 to latest per LGA: total rain, onset day, dry spell count, heat days.
- **Method:** Mann-Kendall test with Sen's slope.
- **Compared against:** ordinary least-squares trend.
- **Output:** change per decade with a significance flag, for example "onset 4 days later per decade (significant)". Feeds the trend line on Season and each card's "copes with this trend" line (R32).

### A8. Soil water properties
- **Inputs:** iSDAsoil sand, clay, organic carbon, bulk density per LGA (cropland topsoil).
- **Method:** Saxton and Rawls (2006) pedotransfer equations.
- **Output:** field capacity, wilting point, available water capacity (mm per metre), saturated hydraulic conductivity (used for infiltration).

### A9. Crop water balance (FAO-56)
- **ET0:** FAO-56 Penman-Monteith from POWER temperature, humidity, radiation and wind; Hargreaves computed alongside as a check.
- **Crop need:** ETc = Kc x ET0 per growth stage, with planting at the LGA's median onset and FAO-56 stage lengths.
- **Deficit:** ETc minus effective rainfall per stage, over 20+ years, giving how often each crop runs short in each LGA.
- **Fallback:** if the full balance is not ready, cards use FAO table water need per crop. The crop water demand study (R38) upgrades this.
- **Output:** per crop and LGA: seasonal water need (mm), deficit frequency, riskiest stage.

### A10. Erosion and infiltration index
- **Erosion risk (per LGA):** slope from SRTM x rain intensity (GPM 95th percentile) x erodibility proxy (higher silt and fine sand, lower organic carbon), scaled 0 to 1.
- **Infiltration potential (per LGA):** saturated conductivity from A8, adjusted for bulk density (compaction) and organic carbon.
- **Crop effect (in the engine):** rotations with longer ground cover, legumes and deep roots score better on both.

### A11. Crop rules knowledge base
A single file, `config/crops.yaml`, one entry per crop, every value with a source note:
- botanical family and pest or disease group (for example Striga host, stem borer host, Solanaceae)
- nitrogen fixing (yes or no, typical kg N per ha left)
- rooting depth, drought tolerance, heat tolerance
- FAO-56 Kc per stage and season length
- ground cover months (for erosion) and residue amount (for organic matter)
- forage value for livestock
- market class (staple, cash, both) and relative price band
- labour and equipment needs
- suitable pH and texture range

*Crops in scope:* maize, sorghum, millet, rice, soybean, cowpea, groundnut, sesame, plus ginger, yam and cassava for southern LGAs.

### A12. Confidence aggregator
- **Inputs:** A6 range width, share of filled or missing data for the LGA, iSDAsoil uncertainty width, cropland share, and evidence strength (local Kaduna trial, same savanna zone, or general).
- **Method:** each input scores 0 to 2; total maps to High, Medium or Low.
- **Output:** a confidence level and a plain reason, for example "Medium: soil data is uncertain here and SMAP covers only 10 years."

---

## 6. User inputs and the insights they unlock

The statement asks that farmers can "input their own priorities or constraints, such as water availability, equipment limitations, or market preferences." Every input below changes something the farmer can see. An input that changes nothing is a bug.

### Input fields

| ID | Input | Screen | Options | Default | Validation | What it changes | Row |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **I1** | LGA | Farm | All 23 Kaduna LGAs, one consistent spelling, grouped by senatorial zone | Last used, else empty | Required; Next disabled until chosen | Loads the LGA's season, soil default, crop baseline and candidate rotations. Low-cropland LGAs (Kaduna North, Kaduna South) show a notice instead of thin cards | P6, R37 |
| **I2** | Soil type | Farm | Sandy, Loamy, Clay, Not sure | Pre-filled from iSDAsoil texture for the LGA's cropland, labelled "from satellite soil map" | Optional override | Water holding, infiltration and crop suitability; at least one card's fit or risk text changes. "Not sure" keeps the iSDA value and lowers confidence one step | R3 |
| **I3** | Water access | Farm | Rain only, Irrigation, Both | Rain only | Required | Rain only: high-water and dry-season options are removed or penalised. Irrigation: dry-season options allowed, water row reweighted | R13 |
| **I4** | Equipment | Farm | Hand tools, Animal traction, Tractor | Hand tools | Required | Labour line and risk text on each card; high-stone soils add a workability warning for hand tools | R14 |
| **I5** | Past two seasons | Farm | Crop icons per season; one crop or two (intercrop) per season; "Other" with free text | Empty | Required, at least one season | Becomes Card 1 "Current". Drives pest-cycle scoring (same family repeated) and nitrogen status (no legume in two seasons) | R7 |
| **I6** | Priorities | Priorities | Pick up to 3 of 7: Save water; Add nitrogen to soil; Build organic matter; Stop soil washing away; Feed animals; Extra income; Fewer pests | None | 0 to 3; a 4th tap shows "Pick up to 3" | Weights in the ranking: chosen priorities count 3 times, others once. None chosen: equal weights with a notice | R5, R16 |
| **I7** | Where you sell | Priorities | Local market; Buyer, processor or feed mill; Mostly home use | Local market | Required | Market row and the income benefit; "home use" lowers the weight of cash crops | R15 |
| **I8** | Season type | Your season | Typical year; Late-start year; Dry-spell year; Wet year (from A5 clusters) | Typical year | Optional | Swaps the season strip and re-scores all cards under that season's profile. This is our scenario slider for rainfall patterns | R39 (new), T4 |
| **I9** | Crop check | Your season | Any crop in the LGA's list | None | Optional | Overlays that crop's monthly water need against the LGA's usual rain and marks its riskiest stage | R40 (new) |
| **I10** | Language | Every screen | English, Hausa | Phone language, else English | Always available | All labels, card headings and the share text | P1 |
| **I11** | Farmer name and phone for sharing | Share | Free text, optional | Empty | Never stored; used only to address the share | Personalises the WhatsApp message and printed card | P9 |

### Seven priorities, seven card rows
Each priority button maps to one of the seven comparison dimensions the statement lists, so a farmer's choice visibly moves the matching row and the ranking:

| Priority button | Card row | Statement wording it answers |
| :--- | :--- | :--- |
| **Save water** | Water use | water use; reduce water demand; conserving water |
| **Add nitrogen to soil** | Nutrients | nitrogen-fixing potential; nutrient cycling |
| **Build organic matter** | Organic matter | organic matter improvement; improving soil structure |
| **Stop soil washing away** | Erosion and infiltration | erosion reduction; erosion risk; improve infiltration |
| **Feed animals** | Forage | forage value; forage potential |
| **Extra income** | Market | market suitability; supplemental income; market considerations |
| **Fewer pests** | Pests | interrupt pest cycles; pest-cycle disruption |

### Questions the tool answers from these inputs

| The advisor or farmer asks | Inputs used | Where the answer shows |
| :--- | :--- | :--- |
| **When does rain usually start here, and is it getting later?** | I1 | Season strip and trend line |
| **Which months are my fields usually bare?** | I1 | Season strip, bare band |
| **When is my current crop most at risk?** | I1, I5 | Season strip, risk markers for current crops |
| **Does maize get enough rain here?** | I1, I9 | Crop check overlay |
| **Which rotation adds nitrogen and still fits my water?** | I1, I3, I6 | Compare, ranking and Nutrients and Water rows |
| **What if the rain starts late this year?** | I1, I8 | Season strip and re-scored cards |
| **Will this rotation sell where I sell?** | I7 | Market row and income benefit |
| **Can I do this with hand tools?** | I4 | Labour line and risks |
| **How sure is the tool?** | All | Confidence badge and reason |

---

## 7. Rotation engine

The crop baseline decides what is possible, the evidence decides what is proven, the data decides what fits, and the priorities decide the order. The engine is rule-based and fully explainable: every score keeps the reason that produced it. Requirement rows: R5 to R7, R16, R19, R31, R35.

### Step 1. Candidate rotations from evidence
Candidates live in `config/rotations.yaml`. A template is offered only if a cited study supports it. Rows marked "to verify" are checked against the papers by November 10, or dropped.

| Template | Sequence | Evidence | Best fit | Status |
| :--- | :--- | :--- | :--- | :--- |
| **RT01** | Continuous maize | Usman 2017 (low soil quality baseline) | Used as Current only | Confirmed |
| **RT02** | Maize then soybean | Franke et al. 2008 (Zaria, highest maize yield after dual-purpose soybean); Usman 2017; CIAT savanna study | Central zone | Confirmed |
| **RT03** | Maize then cowpea | Usman 2017; CIAT savanna study | Central and North | Confirmed |
| **RT04** | Maize then groundnut | Savanna rotation trials (Ghana) | Central and North | To verify |
| **RT05** | Sorghum then cowpea | Kaduna Weather Guide advice; savanna trials | North | To verify |
| **RT06** | Sorghum or millet then groundnut | Savanna rotation trials | North | To verify |
| **RT07** | Maize with cowpea intercrop, then soybean | Samaru intercrop trials; Franke et al. 2008 | Central | To verify |
| **RT08** | Forage legume (Stylosanthes or lablab) then maize | Franke et al. 2008 (forage legume plus manure); Ebireri 2013 (legume cover raised organic carbon) | Mixed crop and livestock farms | Confirmed |
| **RT09** | Southern templates (yam, cassava or ginger with a legume) | To source | South | Open |

### Step 2. Filter to what fits this farm
A template is removed if any rule fails:
1. **Crop baseline:** every crop in it must be in the LGA's top-crop list (R37). This keeps southern LGAs from getting northern rotations.
2. **Water:** with Rain only (I3), a crop whose deficit frequency (A9) is above 50% under the chosen season type (I8) is removed. *Season length:* a crop whose growing period is longer than the LGA's usual season (onset to cessation, A2) is removed, or kept with a risk line if it fits in most years (the statement warns of crops that "fall outside expected growing seasons").
3. **Soil:** a crop far outside its pH or texture range (A11) is removed; a near miss stays with a risk line.
4. **Equipment:** a template needing a tractor is removed for Hand tools only if no hand-tool practice exists for it.
5. **Not the same as Current:** alternatives must differ from Card 1 in at least one crop.

*Fewer than two survivors:* soft rules (2 to 4) are relaxed one at a time, the card is marked Low confidence, and its risk line says which rule was relaxed. Three cards always show (R6).

### Step 3. Score seven dimensions (0 to 1 each)

| Dimension | How it is scored | Data used |
| :--- | :--- | :--- |
| **Nutrients** | Share of seasons with a nitrogen-fixing crop, adjusted by the legume's typical N left behind; P and S limits from iSDAsoil add a risk line (Franke found Zaria soils need P and S, not N alone) | A11, iSDAsoil N, P, S |
| **Organic matter** | Residue returned plus cover months, scaled; legume cover crops score highest | A11, iSDAsoil organic carbon |
| **Erosion and infiltration** | Ground-cover months and rooting depth, scaled by the LGA's erosion risk and infiltration potential | A10, A11 |
| **Pests** | 1 if the crop family changes every season and no Striga host repeats; 0 if the same family repeats; legumes that suppress Striga add credit (to verify) | A11, I5 |
| **Water use** | 1 minus the weighted deficit frequency of the rotation's crops under the chosen season type; irrigation relaxes it | A9, I3, I8 |
| **Forage** | Forage value of residues and haulms (cowpea and groundnut haulm, sorghum stover) | A11 |
| **Market** | Price band times fit with where the farmer sells | A11, I7 |

On the card each score shows as Low, Medium or High, with an arrow for better, same or worse than Current.

### Step 4. Weight by the farmer's priorities
Total = sum of (weight x score) divided by sum of weights. A chosen priority (I6) has weight 3; every other dimension has weight 1. The two highest-scoring survivors become Option A and Option B. Ties go to the template with stronger local evidence.

### Step 5. Mark opportunities (R19)
Each card states which of the statement's four opportunities apply, with one line of reason:
- **Interrupts pest cycles:** Pests score higher than Current.
- **Reduces water demand:** Water use score higher than Current.
- **Improves infiltration:** Erosion and infiltration score higher than Current.
- **Adds supplemental income:** Market or Forage score higher than Current.

### Step 6. Confidence and robustness
- Confidence comes from A12 plus evidence strength for the template.
- **Sensitivity check:** weights are nudged by up to 20% in 200 runs. If Option A and B swap places in more than 30% of runs, both cards show "Close call: discuss with your extension officer."

### Step 7. Explanations
Every rule and score emits a short reason fragment (for example "soybean fixes nitrogen", "July dry spells in 6 of 20 years"). The card text in section 9 is assembled from these fragments plus the cited study, so no explanation is written by hand per farmer.

### Engine acceptance criteria
- Changing any input in I1 to I8 changes at least one card's score, ranking or text.
- The same inputs always give the same cards (deterministic).
- Scoring a request takes under 200 ms on the server.
- Unit tests cover each filter rule, each dimension, the weighting, the fewer-than-two fallback and the tie-break.

---

## 8. Screens and features

Seven screens plus one optional map view, opened in a phone browser with nothing to install. Built with FastAPI, Jinja, HTMX, Alpine, Tailwind, Leaflet and Chart.js, first on mock data shaped exactly like the data contract, then on real outputs.

### On every screen (global)
- **Header:** tool name, step indicator (1 of 6), English/Hausa switch (P1).
- **Footer:** "This supports your judgment. Check with your extension officer." (R25).
- Back always keeps entered values; nothing is lost moving between screens.
- **Layout:** works at 360 px wide with no sideways scroll (P10); tap targets at least 44 px; text readable aloud.
- **Network:** core screens work under Chrome Slow 3G; last loaded LGA data is cached on the phone for offline reuse (P5).
- **Privacy:** no farmer data leaves the phone unless the advisor taps Share (P9).

### F0. Start
- **Purpose:** orient the advisor in five seconds.
- **Elements:** name, one-line promise, language switch, Start button, Learn link, and "Last saved: Zaria, 2 days ago" when cached data exists.
- **States:** first visit; returning visit (resume or start new); offline (cached LGAs listed, others greyed).
- **Accept when:** Start loads in under 3 seconds on Slow 3G; offline state shows without errors.

### F1. Farm
- **Purpose:** capture the farm's situation (I1 to I5).
- **Elements:** LGA dropdown grouped by zone; soil chips with the iSDA default pre-selected and labelled; water chips; equipment chips; two season rows of crop icons, each allowing one or two crops (intercrop) and "Other".
- **States:** LGA not chosen (Next disabled); low-cropland LGA (notice: "Little farmland is mapped here; results are less certain"); data loading; unrecognised crop typed under Other (accepted, flagged).
- **Accept when:** the advisor completes it in under 60 seconds; changing soil changes at least one card later (R3).

### F2. Priorities
- **Purpose:** capture what the farmer wants (I6, I7).
- **Elements:** seven large icon buttons (section 6), counter "2 of 3 chosen", where-you-sell chips.
- **States:** none chosen (allowed, equal weights notice); fourth tap blocked with message.
- **Accept when:** each priority button changes the ranking for at least one test farm (R16).

### F3. Your season
- **Purpose:** show the LGA's long-term season so the farmer sees why the cards say what they say (R17, R18, R20, R27, R32).
- **Season strip (F3a):** 12 months, colour bands for rainy months, usual dry spell window, bare-field months, hottest months; each band has a tap-for-meaning icon (R23). Below it, frequencies: "Dry spell in July in 6 of 20 years".
- **Trend line (F3b):** one sentence plus a small chart: "Rain now starts about X days later than in the 1980s" (A7), with significance stated plainly.
- **Season type selector (F3c, I8):** Typical, Late start, Dry spell, Wet. Changing it redraws the strip and re-scores the cards.
- **Crop check (F3d, I9):** pick a crop to see its monthly water need against usual rain, with its riskiest stage marked.
- **Pattern panel (F3e):** collapsed list of all nine statement patterns for the LGA: temperature, heat stress days, rainfall, soil moisture anomaly, evaporation, frost (very low), extreme rain days, vegetation stress, long-term water trend. Each shows one number and a one-line meaning (R20).
- **States:** loading skeleton; data unavailable for a source (pattern shows "no data" and confidence drops); slow network (strip first, chart after).
- **Accept when:** every value on the screen comes from the API, none hardcoded; bare months for Kaduna fall roughly November to April.

### F4. Compare
- **Purpose:** the core decision view (R6, R7, R21, R31).
- **Elements:** three cards (stacked on phone, side by side on wide screens): Card 1 "Current" (never labelled wrong), Option A, Option B. Each card shows the crop sequence by season with icons, the seven dimension rows (Low/Medium/High plus arrow against Current), the opportunity tags (R19), water need in plain words, a confidence badge with its reason, and "See details".
- **States:** current rotation not recognised (card shows crops, rows marked "not enough information", gap stated); all three similar (message: "These options score close; your judgment matters most here"); close call from the sensitivity check; low confidence (badge plus warning first).
- **Accept when:** three cards always render; changing priorities reorders A and B or changes fit labels (R5).

### F5. Card detail
- **Purpose:** the reasoning behind one card (R8 to R12, R22).
- **Elements:** Why (1 to 2 sentences), Evidence (named study with link), Benefits (tied to chosen priorities), Information gaps, Risks to check (each with who to check with), how it copes with the season trend (R32), and "Choose this for sharing".
- **States:** low confidence or thin data: "Check with your extension officer" block shows first (R22).
- **Accept when:** every card has at least one benefit, gap and risk; every evidence line links to a study in the sources list.

### F6. Share
- **Purpose:** get the result back to the farmer (section 10).
- **Elements:** summary preview; buttons for WhatsApp, Print card, Play Hausa sample, Radio script (LGA level); optional farmer name field.
- **States:** WhatsApp not installed (fallback: copy text and save image); share failed (retry plus copy); print unavailable (save as image).
- **Accept when:** each button produces its output on a test phone; no SMS option appears.

### F7. Learn
- **Purpose:** explain the science in plain words (section 10).
- **Elements:** glossary, satellite vs ground data, how the tool decides, how sure the tool is, crop-type explainers, datasets used and not used, model comparison table.
- **Accept when:** reachable from every screen in one tap; readable without network after first visit.

### F8. LGA trend map (Should, T1)
- **Purpose:** let users explore trends across Kaduna (statement: interactive maps of long-term trends).
- **Elements:** Leaflet map of the 23 LGAs coloured by one chosen trend (onset shift, rainfall trend, heat days, erosion risk); tap an LGA to jump into its Season screen.
- **Rules:** built only after every Must passes; precomputed GeoJSON with values per LGA; the map container sits outside every HTMX swap target.
- **Bonus extension (T2):** zones of erosion risk and water stress inside an LGA from SRTM, GPM and iSDA rasters.

---

## 9. Strategy card content

Every card, including Current, carries the same fields in the same order, so cards can be compared line by line. Text is assembled from engine reason fragments and the evidence file, written at a level that reads aloud well.

| Field | Rule | Length | Example (Option A, Zaria, dummy values) | Row |
| :--- | :--- | :--- | :--- | :--- |
| **Title** | Crop sequence with season labels | One line | Maize, then Soybean | R6 |
| **Badge** | Current, Option A or Option B | One word | Option A | R7 |
| **Seven rows** | Low, Medium, High plus arrow against Current | One line each | Nutrients: High, up from Low | R31 |
| **Opportunity tags** | Which of the four apply (Step 5, section 7) | Tags | Breaks pest cycle, Adds income | R19 |
| **Water need** | Seasonal need in mm and plain words, plus how often it ran short | One line | Needs about 450 mm; rain fell short in 3 of 20 seasons | R4, R13 |
| **Confidence** | High, Medium or Low plus reason | One line | Medium: soil data is uncertain here | R21 |
| **Why** | Top 2 reasons from the engine, tied to the farmer's priorities and the LGA's conditions | 1 to 2 sentences | Soybean adds nitrogen your maize can use next season and breaks the maize pest cycle; it fits Zaria's rainfall in most years | R8 |
| **Evidence** | Named study, place, one-sentence finding, link | 1 sentence | Trials in Zaria found maize after dual-purpose soybean gave the highest yields (Franke et al., 2008) | R9 |
| **Benefits** | At least one per chosen priority that the card improves | 1 to 3 bullets | More nitrogen for next season's maize | R10 |
| **Information gaps** | What the tool does not know | 1 to 2 bullets | Soil data here is a satellite estimate, not a soil test | R11 |
| **Risks to check** | Each with who to check with | 1 to 3 bullets | Soils around Zaria may also need phosphorus and sulphur: ask your extension officer about a soil test | R12 |
| **Copes with the trend** | How the rotation handles the LGA's long-term shift | 1 sentence | Soybean's shorter season suits the later rain starts seen since the 1980s | R32 |
| **Human judgment flag** | Shown first when confidence is Low or data is thin | 1 sentence | Check with your extension officer before deciding | R22 |

### Writing rules for card text
- Plain words a farmer would use; no jargon on the card (jargon lives in Learn, linked by a tap icon).
- Never say a rotation "will" do something; say "may" or "in most years".
- Never label the current rotation wrong or bad; describe it with the same rows.
- No number appears that is not in the data contract or a cited study.
- Hausa versions exist for titles, row names, badges, tags and the human judgment flag at minimum (P1), checked by a native speaker.

---

## 10. Share outputs and Learn content

### Share channels (F6)
The result reaches the farmer through what they already use. SMS is out of scope for this build.

| ID | Channel | Who it reaches | What is produced | How it works | Priority |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **P2** | WhatsApp | Households with a smartphone | Summary image of the chosen card plus short text: LGA, rotation, best planting window, one benefit, one risk, the support line | Web Share API with the image where the phone supports it; otherwise a WhatsApp link with the text and a Save image button | Must |
| **P4** | Printed card | Farmers with no phone | One page (A5 or A4) with the same content, crop icons, and the advisor's name | Print stylesheet; Save as image fallback | Must |
| **P8** | Hausa voice sample | Farmers who cannot read | One recorded Hausa summary for the demo LGA | Pre-recorded audio file played from Share; automation is roadmap | Should |
| **P7** | Radio script | Everyone listening in an LGA | About 150 words (60 seconds read): the LGA's season pattern, the usual dry spell, and the two best-evidenced rotations for typical farms there. No individual farmer data | Generated per LGA from precomputed outputs; copy or download as text | Should |
| **P3** | SMS | Basic phones | Removed | Out of scope for the two-day build | Removed |

*Share acceptance:* each output contains only the chosen card's content; no farmer name or phone is stored on the server; the text matches the selected language.

### Learn screen content (F7)
Every item below answers a line of the statement.

| Section | Content | Statement line it answers | Row |
| :--- | :--- | :--- | :--- |
| **Glossary** | Evapotranspiration, soil moisture anomaly, NDVI (vegetation index), frost risk, water balance; each with plain meaning and why a farmer should care | Consider explaining scientific terms such as evapotranspiration, soil moisture anomalies, vegetation indices, frost risk, and water-balance indicators | R23 |
| **Satellite vs ground data** | What satellites see well (wide areas, every few days, decades of history) and badly (small fields, exact values); what ground data adds (rain gauges, soil tests, the farmer's memory); why combining improves accuracy; the POWER vs GPM rainfall comparison as a real example | Clarify how satellite observations differ from ground-based measurements, and how combining both may improve accuracy | R24 |
| **How conditions affect crops** | How temperature, soil moisture, rainfall, evaporation and vegetation stress relate to crop risk and soil health, using the LGA's own numbers | Explore how changes in temperature, soil moisture, rainfall, evaporation, and vegetation stress relate to crop vulnerability and soil health | R20 |
| **How crops build soil** | Nitrogen-fixing crops, cover crops, deep-rooting crops, drought-tolerant crops: one short note each with a Kaduna crop example | Include brief explanations of how nitrogen-fixing, cover, deep-rooting, or drought-tolerant crops contribute to long-term soil health | R30 |
| **How the tool decides** | The four steps (possible, proven, fits, ranked), the seven dimensions, and how priorities change weights | Explainable rotation comparisons; mapping rotations to priorities | R16, R31 |
| **How sure is the tool** | What High, Medium and Low mean; what lowers confidence (model range, missing data, soil uncertainty, distant evidence); where local expertise must check results | Explain how the model interprets uncertainty and where local expertise would be needed to validate results | R21, R22 |
| **What the models do** | Season types (K-means vs GMM), stress model (Ridge vs LightGBM) with the comparison table and held-out scores; bare-field detection; trend test | Compare different SI or statistical approaches; SI for seasonal transitions, bare-field periods and crop stress | R27, R28 |
| **One common grid** | All data aggregated to LGA and month, cropland only where it matters, and why | Harmonising datasets to a common analysis grid | R29 |
| **Backtest** | Chart showing the engine flags risk in a documented bad Kaduna season | Validity of the approach | R33 |
| **Limits** | Satellites cannot tell maize from soybean at this scale; the tool describes the LGA, not a single field | Honest limits; where human judgment matters | R11, R22 |
| **Data sources** | NASA datasets used, what each was used for, and those considered but not used, with reasons | Uses NASA Earth observations; named datasets | R2, R34 |

---

## 11. Non-functional requirements, API and data contract

### Non-functional requirements

| Area | Requirement | How it is tested | Row |
| :--- | :--- | :--- | :--- |
| **Speed** | Start, Season and Compare usable within 5 seconds on Chrome Slow 3G; first page under 1 MB excluding map tiles | Chrome throttling on a mid-range Android | P5 |
| **Scoring time** | `/partials/cards` responds in under 200 ms server-side | API timing test | R35 |
| **Offline** | Last viewed LGA's season and cards reopen with no network | Airplane-mode test | P5 |
| **Layout** | No sideways scroll at 360 px; tap targets at least 44 px | Device matrix | P10 |
| **Accessibility** | Text contrast at least 4.5 to 1; every icon has a text label; works with screen zoom at 200% | Manual check | R26 |
| **Language** | English and Hausa for all labels, card headings, tags and the support line | Native speaker review | P1 |
| **Privacy** | No farmer name, phone or location stored or logged server-side | Code review, log check | P9 |
| **Determinism** | Same inputs give the same cards | Repeat-call test | R35 |
| **Reproducibility** | One command rebuilds every data file from raw pulls | Clean-machine run | R29, S4 |
| **Reliability** | No live calls to Earth Engine or NASA during a user session; all outputs precomputed | Architecture review | R35 |
| **Hosting** | One FastAPI service on Render or Railway, public URL, repo public | Incognito check | S2, S4 |

### Endpoints
HTML routes render full pages; partial routes return fragments for HTMX; JSON routes serve data and tests.

| Method and path | Returns | Used by |
| :--- | :--- | :--- |
| `GET /, /farm, /priorities, /season, /compare, /card/{id}, /share, /learn` | Full pages | Browser |
| `POST /partials/cards` | Three card fragments for the given inputs (I1 to I8) | Compare, season type change |
| `GET /partials/season?lga=&season_type=` | Season strip, trend and pattern panel | Your season |
| `GET /partials/crop-check?lga=&crop=` | Crop water need overlay | Crop check |
| `GET /partials/explain/{term}` | Plain-language explainer | Tap-for-meaning icons |
| `GET /api/lgas` | 23 LGAs with zone, cropland share, low-cropland flag | Farm dropdown |
| `GET /api/lga/{lga}/profile` | Soil default, crop baseline, season summary | Farm, tests |
| `POST /api/score` | The same result as `/partials/cards`, as JSON | Tests, radio script |
| `GET /api/share/{kind}` | WhatsApp text and image, print card, radio script | Share |
| `GET /api/map/trends.geojson` | LGA polygons with trend values | Trend map (F8) |
| `GET /health` | Status and contract version | Monitoring |

### Data contract (R36)
The contract is the only bridge between the models and the app. It is locked before the build starts and changed only with agreement from both the data side and the app side. Every file carries `contract_version` and `source` (model or fallback).

POWER monthly averages with `source: fallback`. The app works end to end on it, and the trained outputs replace it without any app change. Rows R2, R20, R27, R28 and R33 pass QA only after `source: model`.

---

## 12. Challenge traceability

Every line of the challenge statement, in order, mapped to where FieldDynamics answers it. 43 items: 39 covered by Must features, 2 by Should features (36, 39), 1 split between Must and Bonus (40), and 1 answered partly now with the rest as Bonus (37). This table is re-checked against the live build before submission.

### Opening summary and background

| # | Statement says | FieldDynamics answer | Rows | Coverage |
| :--- | :--- | :--- | :--- | :--- |
| **1** | Create a decision-support tool | Seven-screen web tool, F0 to F7 | R1 | Must |
| **2** | Uses NASA Earth observations | POWER, MODIS, GPM, SMAP, SRTM (section 4) behind every number | R2 | Must |
| **3** | Local soil information | iSDAsoil soil default per LGA, override on Farm (I2) | R3 | Must |
| **4** | Crop characteristics | Crop rules file A11; water need and N-fixing on every card | R4 | Must |
| **5** | Farmer priorities | Seven priority buttons (I6) weight the ranking | R5, R16 | Must |
| **6** | Explore rotation strategies that strengthen soil health | Compare and Card detail screens (F4, F5) | R6 | Must |
| **7** | Adapt farms to changing conditions | Long-term trend A7, trend line F3b, "copes with the trend" line on each card | R32 | Must |
| **8** | Pressures: shifting temperatures, changing rainfall, water shortages, extreme weather, declining soil health | Heat days, onset shift, water deficit, extreme rain days, soil nutrient and organic matter rows | R20 | Must |
| **9** | Protect fields, conserve water, long-term resilience | Erosion and infiltration row, water row, trend line | R31, R32 | Must |
| **10** | A crop may consume too much water | Water filter and water row under the chosen season type | R13, R31 | Must |
| **11** | May introduce disease | Pest and disease group per crop; Pests row penalises repeats | R19, R31 | Must |
| **12** | May conflict with local markets | Where-you-sell input (I7) and Market row | R15, R31 | Must |
| **13** | May fall outside expected growing seasons | Season-length filter against the LGA's onset and cessation (section 7, Step 2) | R41 (new) | Must |
| **14** | SI techniques turn data into farmer-friendly insights | Season types (A5) and stress model (A6) feed plain card text | R27 | Must |

### Objectives

| # | Statement says | FieldDynamics answer | Rows | Coverage |
| :--- | :--- | :--- | :--- | :--- |
| **15** | App, model, interactive map, or other tool | App (F0 to F7), two trained models (A5, A6), LGA trend map (F8) | R1, R27, T1 | Must; map Should |
| **16** | Improve soil health and resilience to weather and water conditions | Seven rows plus season type re-scoring | R31, R39 | Must |
| **17** | Named datasets: SMAP, ECOSTRESS, GPM, Landsat, MODIS, NASA POWER, GRACE/GRACE-FO, SRTM | Five used; four listed as considered and not used, with reasons, on Learn | R2, R34 | Must |
| **18** | Patterns: temperature, heat stress, rainfall, soil moisture, evaporation, frost risk, extreme precipitation, vegetation stress, long-term water trends | All nine computed (A2, A3, A4, A7, A9) and shown in the pattern panel F3e | R20 | Must |
| **19** | Priorities: water use, forage value, nitrogen-fixing potential, erosion reduction, organic matter improvement, market suitability | One priority button and one card row each (section 6) | R16, R31 | Must |
| **20** | Highlight times when fields are typically bare | Bare-field detection A4, bare band on the season strip | R17 | Must |
| **21** | Seasons when current crops appear more vulnerable | Risk markers for the current crops on the strip; stress model A6 | R18 | Must |
| **22** | Opportunities: interrupt pest cycles, reduce water demand, improve infiltration, add supplemental income | Opportunity tags with one-line reasons on every card | R19 | Must |
| **23** | Compare at least three rotation strategies, including the existing one | Three cards always; Card 1 is Current | R6, R7 | Must |
| **24** | For each: why suggested, evidence, benefits, information gaps, risks for local review | Card detail F5, fields in section 9 | R8 to R12 | Must |
| **25** | Supportive tool; does not replace farmer judgment or local expertise | Footer on every screen; human judgment flag on low-confidence cards | R22, R25 | Must |

### Potential considerations

| # | Statement says | FieldDynamics answer | Rows | Coverage |
| :--- | :--- | :--- | :--- | :--- |
| **26** | Relate temperature, soil moisture, rainfall, evaporation, vegetation stress to crop vulnerability and soil health | Pattern panel and Learn section "How conditions affect crops" | R20 | Must |
| **27** | Explain evapotranspiration, soil moisture anomalies, NDVI, frost risk, water-balance indicators | Glossary and tap-for-meaning icons | R23 | Must |
| **28** | Satellite vs ground data, and how combining improves accuracy | Learn section with POWER vs GPM example | R24 | Must |
| **29** | Compare different SI or statistical approaches | K-means vs GMM; Ridge vs LightGBM; smoothed vs raw NDVI; Mann-Kendall vs linear; Penman-Monteith vs Hargreaves | R28 | Must |
| **30** | SI highlights seasonal transitions, bare-field periods, crop stress times | A5 season types, A4 bare months, A6 stress | R27 | Must |
| **31** | Explain how the model interprets uncertainty and where local expertise must validate | Quantile ranges, confidence aggregator A12, Learn "How sure is the tool" | R21, R22 | Must |
| **32** | Harmonise datasets to a common analysis grid (SMAP, ECOSTRESS LST, GPM, Landsat/MODIS VI, GEDI, soil) | LGA by month grid (A1); temperature from POWER replaces ECOSTRESS LST; GEDI noted as not relevant to annual crops | R29, R34 | Must |
| **33** | Compare scenarios on nutrient cycling, erosion risk, organic matter, pest-cycle disruption, water use, forage potential, market | The seven card rows | R31 | Must |
| **34** | Map rotations to priorities such as conserving water, supplemental income, soil structure | Priority weighting, Step 4 | R5, R16 | Must |
| **35** | Explain nitrogen-fixing, cover, deep-rooting, drought-tolerant crops | Learn "How crops build soil" | R30 | Must |
| **36** | Interactive maps of trends: long-term soil moisture, heat waves, water-storage declines | LGA trend map F8; water storage shown through the 40-year rainfall trend because GRACE is too coarse | T1 | Should |
| **37** | Field-level vulnerabilities: erosion zones, water stress hotspots, frost-prone areas | LGA-level erosion and water stress on cards now; field-level zones are T2; frost shown as very low | T2 | Bonus, partial |
| **38** | Add-on: timelines | 12-month season strip | T3 | Must |
| **39** | Add-on: overlays comparing datasets | POWER vs GPM rainfall comparison on Learn; crop need vs rain overlay | R24, R40 | Should |
| **40** | Add-on: scenario sliders for rainfall or temperature | Season type selector re-scores cards (rainfall patterns; a hot-year type if the clusters produce one). A free temperature slider is Bonus T4 | R39, T4 | Must for rainfall, Bonus for temperature |
| **41** | Easy for farmers with limited technical experience | Advisor-led flow, icons, plain words, Hausa, usability test | R26, P1 | Must |
| **42** | Farmers input priorities or constraints: water, equipment, market | I3, I4, I6, I7 | R13 to R16 | Must |
| **43** | Remind that the tool supports, not replaces, local expertise; highlight where human judgment matters | Footer, human judgment flag, risks naming who to check with | R12, R22, R25 | Must |

*Note on Resources tab:* The statement also says more resources may be added to the Resources tab before the event: it is checked on November 10 and November 12, and any change is logged.

### New requirement rows from this PRD

| ID | Requirement | Acceptance criteria | Priority |
| :--- | :--- | :--- | :--- |
| **R39** | Season type selector | Choosing Late start, Dry spell or Wet redraws the strip and changes at least one card's score | Must |
| **R40** | Crop check overlay | Choosing a crop shows its monthly water need against usual rain and its riskiest stage | Should |
| **R41** | Season-length filter | A crop longer than the LGA's usual season is removed or carries a risk line | Must |
| **R42** | Separate priorities for nitrogen and organic matter | Seven priority buttons, one per card row | Must |
| **R43** | Cropland mask | NDVI and soil statistics computed on cropland only; low-cropland LGAs flagged | Must |

---

## 13. Validation, demo and submission

### How we prove it works

| Check | Method | Pass condition | Row |
| :--- | :--- | :--- | :--- |
| **Season types** | Silhouette, Davies-Bouldin, stability over 20 seeds; K-means vs GMM | Clusters stable; documented bad years fall in a stress type | R27, R28 |
| **Stress model** | Leave-one-year-out and zone-grouped folds; Ridge vs LightGBM; RMSE, MAE, R²; interval coverage | Scores reported honestly; the better model ships; 80% range covers 70 to 90% of results | R28, R21 |
| **Backtest** | Hold out a documented bad Kaduna season (the year is taken from records, not memory); run the engine under that year's conditions | Current continuous maize shows higher risk and the water or stress rows worsen | R33 |
| **Onset sanity** | Compare A2 onset dates with the NiMet 2025 guide per LGA | Same order of LGAs; gaps explained | R18 |
| **Bare months** | Compare A4 output with the known dry season | Kaduna bare roughly November to April | R17 |
| **Rain sources** | POWER vs GPM monthly rain per LGA | Agreement reported; used on Learn | R24 |
| **Ranking robustness** | 200 weight perturbations of up to 20% | Close calls flagged on cards | R31 |
| **Usability** | Three tasks by a non-technical tester, ideally a real CBA: find the LGA's dry spell month; choose a rotation; share it on WhatsApp | All three done unaided in under 3 minutes | R26 |
| **Engine and API** | pytest for every rule and endpoint; Playwright end-to-end run of Start to Share | Green in CI on every pull request | R1, R35 |
| **Devices and network** | 360 px Android, Slow 3G, offline reopen, Hausa labels | All pass | P1, P5, P10 |

### Demo (S1)
Format decided after the November 12 dry run: a video of 30 seconds or less, or 7 slides or fewer. Draft shot list for the video:
- **0 to 4 s:** a Kaduna field and one line on the problem (shifting rain, tired soils).
- **4 to 10 s:** an advisor picks Zaria, soil and priorities on the phone.
- **10 to 17 s:** the season strip and the trend line, with "NASA data" visible.
- **17 to 24 s:** three cards, then one card's Why and Evidence.
- **24 to 28 s:** Share to WhatsApp, the farmer receives it.
- **28 to 30 s:** "Supports, not replaces, your judgment" and the team name.

### Project page and README
- **Project page:** problem, users, how it works, NASA datasets with what each was used for (S3), the SI comparison in one table, how uncertainty is handled, survey findings, links to the live app, video and repo.
- **README:** setup and one-command rebuild, data sources, harmonisation method, model evaluation tables, backtest chart, data contract, limits (S4).
- **Visibility map (S7):** every Must row listed with where a judge sees it: video, project page or README.

### Submission checklist
- **S1:** demo within limits (30 seconds or 7 slides)
- **S2:** every link opens in an incognito window
- **S3:** datasets on the project page match what the code uses
- **S4:** repo public and opens while logged out; README complete
- **S5:** every required field on the project page filled
- **S6:** terms box ticked
- **S7:** visibility map complete
- **S8:** pre-event work log kept in the repo, within Space Apps rules
- *Submitted with at least one hour to spare.*

---

## 14. Milestones, risks and open questions

### Milestones
`[embed: node/7ac85c33-45e0]`

The app is built on dummy data first, so the event days are spent swapping in real outputs and polishing, not starting from scratch. If a gate is missed, scope shrinks to the Must rows; the dates do not move.

### Risks

| Risk | Effect | Prevention |
| :--- | :--- | :--- |
| **Models late** | App has no real numbers on event day | Fallback contract files from POWER averages; app built on them first; models swap in |
| **Small training table (about 550 LGA-years)** | LightGBM overfits | Shallow trees, grouped validation, Ridge as baseline; ship whichever wins |
| **NDVI mixes crops with other vegetation** | "Crop stress" is really vegetation stress | Cropland mask (R43); limit stated on Learn |
| **Rotation evidence thin for some templates** | Judges doubt the cards | Only templates with a cited study ship; "to verify" rows confirmed against the papers or dropped |
| **Urban LGAs have little cropland** | Meaningless cards | Low-cropland notice instead of cards |
| **Single point of failure on data and models** | Whole app waits on one person | Contract plus fallback; a second person paired on preprocessing; independent review of model outputs |
| **Scope creep into map and sliders** | Musts unfinished | F8 and T2 to T4 only after every Must passes; feature freeze at Day 2 hour 6 |
| **Weak network at the venue** | Demo fails live | Recorded video; offline cache; local copy of the app |
| **Hausa text inaccurate** | Loses trust with users and judges | Native speaker review before November 12 |
| **Demo over 30 seconds or 7 slides** | Disqualified from global judging | Timed and counted before submission |

### Open questions

| Question | Needed by |
| :--- | :--- |
| **Exact local submission deadline and time zone** | Nov 9, 2026 |
| **What pre-event work Space Apps allows (code or planning only)** | Nov 9, 2026 |
| **Which documented bad season to backtest, from records** | Nov 10, 2026 |
| **Source for market prices per crop** | Nov 10, 2026 |
| **Evidence for southern templates (yam, cassava, ginger) or drop them** | Nov 10, 2026 |
| **Whether clustering yields a hot-year season type (for a temperature scenario)** | Nov 11, 2026 |
| **Native Hausa reviewer and voice** | Nov 11, 2026 |
| **Video or slides** | Nov 12, 2026 |

---

## Sources

- [NASA Space Apps 2026: Field Shift challenge statement](https://www.spaceappschallenge.org/2026/challenges/adapting-farms-with-nasa-data/?tab=details)
- [NASA POWER Data Access Viewer](https://power.larc.nasa.gov/data-access-viewer/)
- [AppEEARS (MODIS, SMAP extraction)](https://appeears.earthdatacloud.nasa.gov)
- [Earthdata Search (GPM, SMAP, SRTM)](https://search.earthdata.nasa.gov)
- [iSDAsoil datasets in the Earth Engine catalog](https://developers.google.com/earth-engine/datasets/tags/isda)
- [iSDAsoil metadata (SoilHive)](https://www.soilhive.ag/datasets/17)
- [Kaduna State Farmers' Weather Guide 2025 (NiMet, HEDA)](https://ejsdprogram.hedang.org/?p=2450)
- [Usman (2017), long-term rotation and maize yield, ABU Zaria](https://kubanni.abu.edu.ng/items/c9eabdda-8da7-4635-ad62-91ab58a1988a/full)
- [Ebireri (2013), soil quality under legume management, ABU Zaria](https://kubanni.abu.edu.ng/handle/123456789/1144)
- [CIAT: crop rotation and nitrogen in the northern Guinea savanna](https://alliancebioversityciat.org/publications-data/effect-crop-rotation-and-nitrogen-fertilization-yield-and-nitrogen-efficiency)
- [AGRA Kaduna Consortium with NAERLS](https://naerls.gov.ng/2025/07/agra-monitoring-team-visits-naerls-to-assess-the-impact-of-agra-kaduna-consortium-2021-2024-achievements/)
- Franke et al. (2008), Nutrient Cycling in Agroecosystems 82: 117 to 135 (Zaria rotation trials): full paper to be pulled before quoting yields
