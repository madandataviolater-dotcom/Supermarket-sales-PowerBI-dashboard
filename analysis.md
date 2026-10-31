# Dashboard analysis — problem statements and explanations

Each section states the question the visual was built to answer, how it was built, why that visual type fits, and what the data shows. Figures are computed from the source CSV (1,000 invoices, Jan–Mar 2019).

## Overall problem statement

A supermarket chain with three city branches wants a single performance tracker to monitor revenue, tax and quantity sold, and to see how sales break down by product line, city, gender and payment method, with slicers so a manager can drill into any branch or segment.

---

## 1. KPI cards — Sum of Total, Tax 5%, Quantity, Gross Income

**Question:** What are the headline numbers for the period?

**Build:** Four card/KPI visuals, each with a single implicit measure: `SUM(Total)`, `SUM(Tax 5%)`, `SUM(Quantity)`, `SUM(gross income)`. Display units set to auto, so Power BI shows 322.97K rather than 322,966.75.

**Why cards:** A card is the right visual for one headline number — no axis, no comparison, just the figure. Placing them at the top gives a manager the state of the business before reading any chart.

**Finding:** Total 322.97K, Tax 15.38K, Quantity 5,510 (displayed as 6K after rounding), Gross income 15.38K.

**Note:** In the original version the fourth card summed `cogs` (cost of goods sold, 307.59K) but was labelled "Sum of Profit". That has been corrected to `gross income`. Checking every card against the raw data is part of the build — a wrong label on a headline number is the most visible mistake a dashboard can make.

---

## 2. Trend Analysis by product line (area chart)

**Question:** Which product lines generate the most revenue, and how wide is the gap between the best and worst?

**Build:** `Product line` on the X-axis, `SUM(Total)` on the Y-axis, sorted descending, data labels on.

**Why an area chart:** With the categories sorted by value, the sloping area makes the drop-off from best to worst immediately visible. (A sorted bar chart would work equally well; the area was chosen for visual consistency with the tutorial.)

**Finding:** Food & beverages 56.1K, Sports & travel 55.1K, Electronic accessories 54.3K, Fashion accessories 54.3K, Home & lifestyle 53.9K, Health & beauty 49.2K. The spread is narrow — about 7K between top and bottom — so revenue is spread fairly evenly across lines. Health & beauty is the only line that stands out as lagging.

---

## 3. Total Sales vs Tax vs Unit price (clustered bar chart)

**Question:** How do the three city branches compare on revenue, tax collected and pricing?

**Build:** `City` on the Y-axis, three measures on the X-axis: `SUM(Total)`, `SUM(Tax 5%)`, `SUM(Unit price)`.

**Why clustered bars:** Three measures per category need to sit side by side for comparison, which is exactly what a clustered bar does. Horizontal orientation keeps the city names readable.

**Finding:** Naypyitaw 110.6K, Yangon 106.2K, Mandalay 106.2K. The branches are almost identical. Tax is exactly 5% of Total in this dataset, so it will always mirror revenue.

---

## 4. Mode of Payment (donut chart)

**Question:** Which payment methods do customers prefer?

**Build:** `Payment` on Legend, `SUM(Total)` on Values, data labels showing category and value.

**Why a donut:** Three categories, part-to-whole question. A donut works here because the slices are large and few; it would not with ten categories.

**Finding:** Cash 112.2K, Ewallet 110.0K, Credit card 100.8K. Near-even split; no single method dominates, so the chain cannot drop any payment option without affecting roughly a third of revenue.

---

## 5. Sum of Total by Gender (pie chart)

**Question:** Do male and female customers spend differently?

**Build:** `Gender` on Legend, `SUM(Total)` on Values.

**Why a pie:** Two categories, part-to-whole — the one case where a pie is the clearest choice.

**Finding:** Female 167.9K vs Male 155.1K. Invoice counts are almost equal (501 vs 499), so the difference comes from a higher average basket for female customers, not more visits.

---

## 6. Total sales by Product line (donut chart)

**Question:** What share of total revenue does each product line contribute?

**Build:** `Product line` on Legend, `SUM(Total)` on Values, data labels on.

**Why a donut:** Same data as the area chart, reframed as part-to-whole. Each line contributes roughly 15–17%.

**Note:** Showing the same measure twice is a known weakness of this layout. A planned revision replaces this donut with average `Rating` by product line (customer satisfaction) or sales by `Customer type` (Member vs Normal), both of which add new information.

---

## 7. Slicers — City, Gender, Product line

**Question:** Can a manager filter the whole page to one branch or segment?

**Build:** Three slicer visuals with checkbox lists. Default Power BI cross-filtering means every visual on the page responds to the selection.

**Why slicers:** This is what turns a static report into a tool. A branch manager can tick their own city and every card and chart updates to that branch alone.

---

## Planned improvements

- Add a date-axis line chart (`Date` by week) once a longer data window is available.
- Replace the duplicate product-line donut with average `Rating` by product line.
- Add a `Customer type` (Member vs Normal) comparison to assess the loyalty programme.
- Write explicit DAX measures (e.g. `Avg Basket = DIVIDE(SUM(Total), DISTINCTCOUNT([Invoice ID]))`) instead of relying on implicit sums.
