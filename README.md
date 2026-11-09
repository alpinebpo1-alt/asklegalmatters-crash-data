# Fatal motor vehicle crashes by U.S. place and road, 2020-2024 (derived from NHTSA FARS)

Version 1.1, built 2026-11-07 from the FARS files downloaded 2026-11-03. Creator: Ask Legal Matters (asklegalmatters.com).

Counts of fatal motor vehicle crashes and deaths from 2020 through 2024 for every city and town that has a guide page on asklegalmatters.com (11,699 places in 49 states), every county in those states, the roads with the most fatal crashes in each place, and state totals. All counts are derived from the National Highway Traffic Safety Administration's Fatality Analysis Reporting System (FARS), the census of fatal crashes on U.S. public roads. They are the same figures shown on the Ask Legal Matters injury guides; the files are also at https://asklegalmatters.com/injury-guides/data/.

Ask Legal Matters is a personal injury lead-generation service that collects free case review requests from injured people in the United States and shares them with independent attorneys and law firms. It is not a law firm and does not give legal advice. Nothing in this dataset is legal advice.

## Files

| File | Rows | One row is |
| --- | ---: | --- |
| places.csv | 11,699 | a city or town with a guide page (0 when no crash was coded to it) |
| roads.csv | 8,607 | one of the up to 10 roads with 2 or more fatal crashes in a place |
| counties.csv | 3,107 | a county (or parish, or independent city) in those states |
| states.csv | 49 | a state: every FARS crash in it, whatever its city or county coding |
| README.md | | this file |
| LICENSE.txt | | the license and credit lines |

All files are UTF-8 CSV with a header row and CRLF line ends. `page_url` in every file is the guide page that shows the row.

### places.csv

- `state`: two-letter postal code.
- `place`: the city or town as named on its guide page.
- `county`: the county, parish or independent city the guide places it in.
- `population`: the population figure in the place list behind the guides (GeoNames); for context only, not a Census estimate.
- `fatal_crashes_2020_2024`: fatal crashes, 2020 through 2024.
- `deaths_2020_2024`: people killed in those crashes (FARS counts a death within 30 days of the crash).
- `fatal_crashes_2020, fatal_crashes_2021, fatal_crashes_2022, fatal_crashes_2023, fatal_crashes_2024`: fatal crashes in each year.
- `deaths_2020, deaths_2021, deaths_2022, deaths_2023, deaths_2024`: deaths in each year.
- `after_dark_share`: share of the crashes recorded with dark lighting (dark - lighted, dark - not lighted, dark - unknown lighting), 0 to 1 with three decimals; blank when there were no crashes.
- `nonmotorist_crashes`: crashes in which at least one person not in a motor vehicle (a pedestrian, cyclist or other non-motorist) was involved.
- `intersection_crashes`: crashes recorded at or related to an intersection.
- `page_url`: the place's guide page.

### roads.csv

- `state`, `place`: as in places.csv.
- `rank`: 1 to 10, by fatal crashes, then deaths, then the latest year.
- `road`: the road name after the combining rules below, for example `Westheimer Rd (FM 1093)` or `State Route 8 (street name not recorded)`.
- `fatal_crashes_2020_2024`, `deaths_2020_2024`: fatal crashes recorded on that road in the place, and their deaths.
- `latest_year`: the most recent year with a fatal crash on that road.
- `page_url`: the place's guide page.

### counties.csv

- `state`, `county`: as above. The count columns are the same as in places.csv and include crashes with no city recorded.

### states.csv

- `state`, `state_name`: the state. The count columns are the same as in places.csv and include every crash in the state.
- `share_of_reports_with_no_city`: share of the state's FARS crash reports whose city was recorded as not reported or unknown, 0 to 1.

## Method

- Source: NHTSA FARS national CSV files (accident.csv) for 2020-2024, from https://static.nhtsa.gov/nhtsa/downloads/FARS/<year>/National/FARS<year>NationalCSV.zip, downloaded 2026-11-03. Releases (2020: Final File; 2021: Final File; 2022: Final File; 2023: Final File; 2024: Annual Report File (ARF)) as listed by NHTSA in "Overview of Motor Vehicle Traffic Crashes in 2024" (DOT HS 813 791, April 2026). FARS documentation: https://www.nhtsa.gov/research-data/fatality-analysis-reporting-system-fars
- Unit: one crash is one row of accident.csv; deaths are its FATALS count.
- States: every crash counts for its state.
- Counties: the crash's COUNTYNAME, matched to the county names in the place list.
- Places: the crash's CITYNAME, matched to a place in the same state's place list, ignoring case and punctuation and treating Saint as St, Mt as Mount, Ft as Fort and Hts as Heights; LEXINGTON-FAYETTE is Lexington, Kentucky; New York City crashes go to Brooklyn or Staten Island by their borough county; a name that still does not match is tried without a TOWNSHIP, VILLAGE, BOROUGH, TOWN or CITY suffix or a "TOWNSHIP OF" prefix, then by the first part of a hyphenated name when that part names one place. When several places share a name, the one in the crash's county is used, otherwise the most populous. A crash coded to a city in another county of the same state still counts for that city.
- After dark: LGT_CONDNAME dark - lighted, dark - not lighted or dark - unknown lighting. Non-motorist: PEDS greater than 0. Intersection: RELJCT2NAME intersection or intersection-related.
- Roads: the first trafficway recorded (TWAY_ID), cleaned to a readable name the same way on every guide page (for example SR-8 becomes State Route 8). The same street with and without a route number is one row, labelled with its route number or numbers (Westheimer Rd and Westheimer Rd (FM 1093) are one row); when a state's own number for a US route is recorded with it, the US number is shown (US-41 rather than US-41 / State Route 41). The same street with and without a leading direction (E Speedway Blvd and Speedway Blvd) is one row. Oregon's internal ODOT highway numbers are shown as the route they carry (Odot 9 (US-101) becomes US-101). BL and BV are read as Boulevard, and names that differ only by spaces (Base Line Rd, Baseline Rd) are one row. A route's ramps, frontage roads (also recorded as service, access or feeder roads) and business route (Texas BI, BU and BS codes included) are rows of their own (Ramp (I-10), Frontage Rd (I-45), US-70 Business), never the route's row, and road types (ramp, frontage, business, loop, spur) are never combined with a street. A route recorded without a street name is never combined with a named road, because the records do not show which stretch it was; when a named road carrying that route is in the same place's list, the bare route reads `<route> (street name not recorded)`. In one place, names that differ only by a missing street type or by Jr are one row (Dyer and Dyer St) unless the records use two street types for that name (Redwood St and Redwood Pkwy stay apart); M L King and MLK read as Martin Luther King. A route written with a space after its dash (SR- 29) is that route, and a city street name recorded after a route code and a slash keeps its name. Only roads with 2 or more fatal crashes are listed, at most 10 per place.
- Place list: the cities and towns with guide pages on asklegalmatters.com in 49 states (all except Alaska), with GeoNames population figures; counties as in that list, 23 corrected to the U.S. Census Bureau's 2020 place file on 2026-11-03.

## Limits

- Fatal crashes only: FARS covers crashes on public roads in which someone died within 30 days. These files do not count injury or property-damage crashes.
- City coding: a crash counts for a place only when the reporting agency recorded a city. 20.8% of Louisiana's and 16.8% of Georgia's 2020-2024 crash reports recorded no city, so place counts there are too low; in every other state the share is 0.7% or less (states.csv gives each state's share). A crash with no city counts only for its county and state.
- A place with 0 had no FARS crash coded to it; that does not mean none happened near it.
- New York City: FARS codes crashes in the five boroughs to New York City with the borough's county. The place list has Brooklyn (Kings County) and Staten Island (Richmond County) but no Bronx, Queens or Manhattan, so crashes there count only for Bronx, Queens and New York counties.
- Alaska is not included: the guides do not cover Alaska yet.
- 2024 is NHTSA's Annual Report File, its first release for that year; NHTSA revises it in the Final File, so 2024 figures can change. NHTSA had not published 2025 FARS data on 2026-11-03.
- Places are matched by name. A crash coded to a place name that is not in the list, or spelled differently, counts only for its county and state.
- A road row counts crashes recorded on that trafficway, not every crash near it; agencies may name the same road differently, and only the first trafficway is used.

## Changes from version 1

Version 1 (built 2026-11-03, on Zenodo since 2026-11-04) and version 1.1 use the same FARS files; no state total changes. What changed, so that the files match the guide pages again:

- Roads (2026-11-04 and 2026-11-07): a route's ramps, frontage roads and business route are rows of their own instead of being counted in the route's row (for example Houston: I-45 had 165 in version 1; in version 1.1 it has 127, and Frontage Rd (I-45), 23, and Ramp (I-45), 17, are rows of their own); names that differ only by a missing street type, Jr or MLK initials are one row; Bl/Bv read as Boulevard; Texas BI/BU/BS business-route codes; a space after a route's dash and city street names after a route code and a slash are read correctly.
- Places (2026-11-04): crashes the records code to a township, a New York town or Queens' Middle Village are no longer credited to a same-named city, village or neighborhood (they still count for the county and state); Jackson MS, Aurora CO, Salem OR, Broken Arrow OK and Rocky Mount NC moved to the county that holds most of each city.
- County names (2026-11-04): 47 names take the U.S. Census Bureau's 2020 capitals (Miami-Dade, DuPage, McHenry); page addresses and figures do not change.

## License

The compilation (the selection, matching, road cleaning and this documentation) is licensed CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/); credit "Ask Legal Matters (asklegalmatters.com)". FARS is a work of the U.S. government and is in the public domain. Population figures are from GeoNames (https://www.geonames.org/), licensed CC BY 4.0.

## Suggested citation

Ask Legal Matters (asklegalmatters.com). (2026). Fatal motor vehicle crashes by U.S. place and road, 2020-2024 (derived from NHTSA FARS) (Version 1.1) [Data set]. Zenodo. Derived from National Highway Traffic Safety Administration, Fatality Analysis Reporting System (FARS), 2020-2023 Final File and 2024 Annual Report File.

All versions: https://doi.org/10.5281/zenodo.23932869. Version 1: https://doi.org/10.5281/zenodo.23932870.

To cite a single figure, cite the guide page in its page_url.

## Contact

Corrections and questions: legal@asklegalmatters.com.
