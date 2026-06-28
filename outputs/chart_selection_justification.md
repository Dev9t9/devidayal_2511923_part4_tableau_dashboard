# Chart Selection Justification

For each major view in the dashboard: the business question it answers, why the chart type fits,
the fields used, the design principle applied, and the mistake deliberately avoided.

---

## 1. Sales Trend View — Line chart
- **Question answered:** How are sales (and profit) changing over time?
- **Why this chart:** Time is continuous, so a line chart shows direction, momentum, and seasonality far better than bars. Two lines (sales + profit) let the reader see whether profit tracks sales.
- **Fields:** `order_date` (continuous month) on Columns; `SUM(Sales)` and `SUM(Profit)` on Rows (dual axis).
- **Design principle:** Continuous encoding for continuous data; clear axis starting at a sensible baseline.
- **Mistake avoided:** Not using a pie or stacked bar for time, and not truncating the y-axis to exaggerate small movements.

## 2. Regional Performance View — Map (or bar) + highlight
- **Question answered:** Which regions/states perform best on sales and profit?
- **Why this chart:** Geography is spatial, so a filled map reads instantly; a sorted bar chart is the honest fallback when exact comparison matters more than location.
- **Fields:** `region`/`state` on Detail/Columns; `SUM(Sales)` for size/length; `Profit Margin` for color.
- **Design principle:** Encode magnitude (sales) and quality (margin) together so a big-but-thin region is visible; sort bars descending.
- **Mistake avoided:** Not colouring a map by raw profit alone (which just mirrors size); using margin for colour adds real information.

## 3. Category Profitability View — Bar chart (sales) with profit/margin
- **Question answered:** Which categories and sub-categories drive profit or losses?
- **Why this chart:** Categories are discrete, so length-based bars give the clearest comparison; a diverging colour for margin instantly separates winners from drags.
- **Fields:** `category` / `sub_category` on Rows; `SUM(Profit)` on Columns; `Profit Margin` on Color (diverging).
- **Design principle:** Sort by profit, colour by margin so Furniture's low margin pops even where sales look healthy.
- **Mistake avoided:** Not using a treemap sized by sales only, which would hide the fact that Furniture's revenue is low-quality.

## 4. Discount vs Profit View — Scatter plot
- **Question answered:** How does discounting relate to profit?
- **Why this chart:** Two continuous measures across many orders → a scatter plot reveals the relationship and the point where orders cross into losses; a trend line quantifies it.
- **Fields:** `discount` on Columns; `profit` on Rows; `category` on Color; a reference line at profit = 0 and a linear trend line.
- **Design principle:** Show the full distribution (not an average) so the loss zone above ~20% discount is visible; reference line at zero anchors interpretation.
- **Mistake avoided:** Not reducing this to a single average bar, which would hide that the relationship is non-linear and turns negative.

## 5. Customer Segment View — Bar / highlight table
- **Question answered:** How do customer segments compare on sales, margin, and returns?
- **Why this chart:** A small number of segments with several metrics each → a highlight table or grouped bars is compact and scannable.
- **Fields:** `customer_segment` on Rows; `SUM(Sales)`, `Profit Margin`, `Return Rate` as columns/colour.
- **Design principle:** Put the differentiating metric (return rate) in colour so Home Office's higher returns stand out despite similar sales.
- **Mistake avoided:** Not showing only sales (which look identical) and missing the real story in returns.

## 6. Shipping Performance View — Bar chart
- **Question answered:** Which shipping modes cause delay (and how do returns differ)?
- **Why this chart:** Discrete ship modes compared on a numeric measure (avg delivery days) → sorted bars are the cleanest, least ambiguous choice.
- **Fields:** `ship_mode` on Rows; `AVG(delivery_days)` on Columns; optional `Return Rate` on colour.
- **Design principle:** Sort by delay; label values so the ~4.7-day Standard average is explicit.
- **Mistake avoided:** Not using a line chart (ship modes are not a sequence) and not implying a false trend between modes.

## 7. Return Analysis View — Bar chart / highlight table
- **Question answered:** Where are returns concentrated (category, segment, region)?
- **Why this chart:** Comparing a rate across discrete groups → bars/highlight table make the outliers (Furniture 7.7%) obvious.
- **Fields:** `category` (or `customer_segment`) on Rows; `Return Rate` on Columns/Color.
- **Design principle:** Express as a rate, not a raw count, so groups of different sizes compare fairly.
- **Mistake avoided:** Not plotting raw return counts, which would just track group size instead of risk.

---

### Cross-cutting design choices
- **Consistent colour language:** one diverging palette for margin/profit (red = bad, green = good) reused across views so the reader learns it once.
- **Sorting:** every categorical view sorted by the metric that matters, never left alphabetical.
- **Minimal clutter:** no 3-D, no chart-junk, gridlines kept light, values labelled only where they aid reading.
- **Honest scales:** axes start at zero for bars; the scatter shows the full point cloud rather than a summarised average.
