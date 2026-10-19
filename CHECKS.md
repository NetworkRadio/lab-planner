# Data checks

Every problem in an uploaded file ends in one of: drop the row, use a default, fix it automatically, or ask you with options. These rules are locked by automated tests; the app cannot be built if one fails.

| ID | File | Problem | What the app does |
|---|---|---|---|
| F01 | file | File has no data rows | **Explain and stop.** Nothing to analyse. The app explains what to export. |
| F02 | file | Text encoding is not UTF-8 | **Fix automatically.** UTF-16 and Windows-1252 files are decoded automatically. |
| F03 | file | Excel file (.xlsx or .xls) | **Fix automatically.** The first sheet is read as a table. |
| F04 | file | Header row is not the first line | **Fix automatically.** Title lines above the header are skipped. |
| F05 | file | Column names differ from the template | **Fix automatically.** Matched by name, then by keyword; you can still change the mapping. |
| F06 | file | Latitude and longitude are in one column | **Fix automatically.** Split into two columns. |
| F07 | file | Numbers use decimal commas, thousands separators, % signs or spaces | **Fix automatically.** Cleaned before reading. |
| F08 | file | No usable order rows after all checks | **Explain and stop.** Shows the top reasons and how to fix the export. |
| F09 | file | A required column cannot be found | **Explain and stop.** Names the missing column and the closest headers in the file. |
| F10 | file | More than 500,000 rows | **Note only.** Allowed; analysis may take a few seconds longer. |
| O01 | orders | Latitude or longitude blank, "null" or not a number | **Drop the row.** Row dropped. |
| O02 | orders | Latitude or longitude outside the valid range, or exactly 0,0 | **Drop the row.** Row dropped. |
| O03 | orders | Latitude and longitude swapped | **Fix automatically.** Swapped back when most rows look swapped. |
| O04 | orders | Lab ID blank | **Drop the row.** Row dropped. |
| O05 | orders | City, PIN or order date blank (strict mode only) | **Drop the row.** Row dropped when Strict mode is on. |
| O06 | orders | Order date unreadable | **Note only.** Row kept; it is left out only while a date range is applied. |
| O07 | orders | Order date is an Excel serial number | **Fix automatically.** Converted to a calendar date. |
| O08 | orders | Lab ID not found in the lab file | **Ask the user.** Asked when 1% of orders or 50 orders are affected (drop them, or assign to the nearest lab); below that they are dropped. |
| O09 | orders | No lab ID column in the order file | **Ask the user.** Asked: assign every order to its nearest lab. |
| O10 | orders | Repeated order IDs | **Ask the user.** Asked when 0.5% of rows are affected: keep all or keep the first only. |
| O11 | orders | Order dates mostly unreadable | **Ask the user.** Asked when 20% or more are unreadable: keep rows and skip the date filter, or leave dates out. |
| O12 | orders | Customer farther than the limit from the assigned lab | **Drop the row.** Left out of every figure (Parameters, default 50 km, 0 turns it off). |
| O13 | orders | City spelled differently (case, Bengaluru, Gurugram) | **Fix automatically.** Merged into one city. |
| O14 | orders | All orders beyond the distance limit | **Fix automatically.** The distance limit is switched off so the analysis can run. |
| L01 | labs | Lab ID blank | **Drop the row.** Row dropped. |
| L02 | labs | Lab latitude or longitude missing or invalid | **Drop the row.** Row dropped. |
| L03 | labs | Same lab ID on several rows | **Ask the user.** Identical rows are merged silently. If they disagree you choose which to keep. |
| L04 | labs | Assured / Non-Assured blank or not recognised | **Ask the user.** You choose: treat as Non-Assured, as Assured, or drop those labs. |
| L05 | labs | No Assured / Non-Assured column | **Ask the user.** You choose how to treat every lab. |
| L06 | labs | TAT breach blank or not a number | **Use a default.** Default value used (Parameters, 30%). Never used as grounds to close a lab. |
| L07 | labs | TAT breach given as a fraction (0 to 1) | **Fix automatically.** Multiplied by 100. |
| L08 | labs | TAT breach below 0 or above 100 | **Use a default.** Treated as missing; default used. |
| L09 | labs | NPS blank, "No responses" or outside -100 to 100 | **Use a default.** Default value used (Parameters, 40). Never used as grounds to close a lab. |
| L10 | labs | City or PIN blank (strict mode only) | **Drop the row.** Row dropped when Strict mode is on. |
| L11 | labs | Lab far from every other lab in its city | **Ask the user.** Asked: keep (coordinates may be right and the city tag wrong) or drop the lab. |
| L12 | labs | Optional numbers invalid (NPS responses, TAT orders, capacity) | **Fix automatically.** The field is ignored for that lab. |
| L13 | labs | PIN stored as a number such as 110092.0 | **Fix automatically.** Cleaned to 110092. |
| L14 | labs | Status written as Assured Lab, Non-Assured Lab, Y or N | **Fix automatically.** Mapped automatically. |
| J01 | join | Lab has no orders in the period | **Note only.** Kept; shown with zero orders. |
| J02 | join | Orders in a city with no lab | **Note only.** Kept; they appear as underserved. |
