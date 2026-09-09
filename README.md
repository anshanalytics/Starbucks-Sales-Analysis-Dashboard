# ☕ Starbucks Sales Analysis Dashboard

An end-to-end Power BI project that analyzes Starbucks sales transactions across multiple months and turns raw data into an interactive dashboard for tracking revenue, customer behavior, and store performance.

---

## 📌 Business Problem

Starbucks stores across multiple locations generate thousands of transactions every day, spanning different items, payment modes, order channels (walk-in vs. mobile app), and customer segments. All of this activity lives as raw, disconnected data across separate systems: sales logs, customer records, and item/menu details.

Without a centralized reporting layer, store and regional managers face real operational blind spots:

- Which hours of the day drive the highest revenue and order volume, and are stores staffed correctly for them?
- Are stores hitting their revenue, order, and customer targets?
- How do customers prefer to pay: Cash, Card, or UPI, and does that shift over time?
- Is order volume coming more from walk-in customers or the mobile app, and what does that mean for channel investment?
- When a specific transaction needs to be traced (a refund, a complaint, an audit), how quickly can it be found?

Manually pulling this from raw CSV exports is slow, difficult to maintain, and hard to keep refreshed and consistent across reports.

---

## 🎯 Dashboard Goal

The goal of this project is to build a self-service Power BI dashboard that lets stakeholders:

1. Track performance against revenue, quantity, order, and customer targets through an interactive dashboard.
2. Identify peak sales hours to guide staffing and inventory decisions.
3. Understand payment mode and customer-channel (walk-in vs. mobile-app) trends.
4. Search and analyze individual transactions for support, auditing, or customer service.
5. See when the underlying data was last refreshed via a dynamic "Last Update" timestamp.

---

## 🗂️ Dataset Overview

This project is built on three linked source tables:

| File | Rows | Description |
|---|---|---|
| sales.csv | 10,000 transactions | Core fact table: transaction ID, store ID, datetime, customer ID, item ID, quantity, price, total amount, payment mode, and customer type (walk-in / mobile-app) |
| customers.csv | 500 customers | Customer ID, name, email, phone, age, and gender |
| items.csv | 77 menu items | Item name, calories, fat, carbs, fiber, protein, and category (bakery, hot breakfast, sandwich, salad, parfait, petite, bistro box) |

**Key facts about the data:**

- Transactions span October 10, 2025 to April 7, 2026, across 3 store locations (101, 102, 103).
- Payment is fairly evenly split: Card (3,357), Cash (3,338), UPI (3,305) transactions.
- Orders are split almost 50/50 between walk-in (5,021) and mobile-app (4,979) customers.
- Total recorded revenue across the dataset: $78,401.49, with 16,466 total items sold.
- The dataset contains 500 customer records and 10,000 transactions. Where a dashboard view shows a different order count (e.g., 9,936 vs. 10,000), this reflects an active filter or slicer selection at the time of that view, not a difference in the source data.
- The menu is bakery-heavy (41 of 77 items), followed by petite items, bistro boxes, hot breakfast, sandwiches, parfaits, and salads, giving room for category-level performance analysis.

---

## 🗂️ Dashboard Structure

The Power BI report is built as a 3-page navigation experience:

- **Home** — Branded landing/navigation page
- **Overview** — High-level KPIs and hourly performance trends
- **Details** — Transaction-level table for search and review

---

## 📊 Key Visuals & Highlights

### 1. Overview Page

- KPI radial indicators for Order_Count, Customer_Count, Total_Amount, and Total_Quantity, each shown as a progress ring against a defined target (e.g., $100K revenue target, 750 customer target).
- Avg Order Amount by Hour, Total Amount by Hour, and Total Quantity by Hour: three synchronized bar charts covering store operating hours (8 AM–7 PM).
- Dynamic peak-hour highlighting: the single best-performing hour is automatically colored a darker green using conditional DAX measures, so the peak stands out without manual formatting.
- Date range and payment mode slicers to filter the whole page interactively.
- Dynamic "Last Update" timestamp: reflects the latest transaction datetime after the report is refreshed.

### 2. Details Page

- A searchable, sortable transaction-level table (transaction_id, customer_id, item, payment_mode, Total_Amount, Total_Quantity, Avg_Order_Amount, Order_Count).
- A totals row that automatically aggregates the currently filtered view (the exact totals shown depend on which slicers/filters are applied at that time, e.g., a filtered view may show 9,936 rather than the full 10,000 transactions).
- Built for support or audit teams that need to search for a specific customer or transaction quickly.

---

## 🧮 Data Model & DAX

The report is built on a clean star schema:

- **Fact table:** sales — transaction_id, store_id, datetime, customer_id, item_id, quantity, price, total_amount, payment_mode, customer_type
- **Dimension tables:** customers (500 rows), items (77 rows, including full nutrition data), and a dedicated Calendar date/time table for time intelligence
- **Central measures table (_Measure)** keeping all DAX logic organized in one place, including:
  - Core aggregations: Total_Amount, Total_Quantity, Order_Count, Customer_Count, Avg_Order_Amount
  - Order_Count is defined as DISTINCTCOUNT(transaction_id), since the dataset has no separate order_id field, one transaction is treated as one order. Customer_Count is DISTINCTCOUNT(customer_id).
  - Targets for each core KPI to support goal-tracking
  - Month-over-month growth % using DATEADD time intelligence
  - Conditional formatting measures that dynamically recolor the highest-performing hour on every chart

---

## 💡 Insights Uncovered

- 9 AM is the strongest hour of the day for both revenue and order volume, a clear signal for aligning morning staffing and inventory ahead of the rush.
- Revenue and order volume show a noticeable decline after 1 PM, indicating a potential early-afternoon demand lull that could be addressed with a targeted promotion (e.g., a lunchtime discount).
- Average order value stays fairly stable (~$7.50–$8.50) across all hours, which means the swing in total revenue through the day is driven mainly by foot traffic, not basket size, reinforcing that staffing and traffic management matter more than upselling during off-peak hours.
- Payment methods are almost evenly distributed (Card, Cash, UPI each ~33% of transactions), so no single payment channel dominates. This indicates that payment infrastructure and operational support should accommodate all three payment methods effectively.
- Walk-in and mobile-app orders are nearly split 50/50, indicating the mobile channel has matured into an equally important ordering path, not a secondary one.
- The dataset contains 500 customer records and 10,000 transactions, indicating potential repeat visitation. A distinct-customer transaction analysis can be used to quantify customer repeat behavior and support loyalty-focused strategies.

---

## 📈 Business Impact

This dashboard enables Starbucks store and operations teams to:

- Optimize staffing schedules around clearly identified peak hours instead of guesswork.
- Design targeted promotions for low-traffic periods (like early-to-mid afternoon) to smooth out demand across the day.
- Track performance against revenue, order, and customer targets through an interactive dashboard, instead of waiting on end-of-month reports.
- Invest confidently in the mobile-app channel, backed by data showing it drives nearly half of all orders.
- Resolve customer and payment disputes faster using the transaction-level Details page.
- Cut manual reporting effort, since the dashboard can simply be refreshed from the source CSV files, updating all KPIs and the "Last Update" timestamp in one step.

---

## 🛠️ Tools Used

- **Power Query** — data cleaning and transformation
- **DAX** — KPI measures, targets, time intelligence, and conditional formatting
- **Data Modeling** — star schema with fact and dimension tables
- **Power BI** — interactive dashboards, slicers, navigation, and data visualization

---

## 📂 Project Structure

Starbucks-Analysis-Dashboard/
│
├── README.md
├── Starbucks_Analysis_Dashboard.pbit # Power BI template file
├── dataset/
│ ├── sales.csv # 10,000 transaction records
│ ├── customers.csv # 500 customer records
│ └── items.csv # 77 menu items with nutrition data
├── details_page.png
├── home_page.png
└── overview_page.png


---

## 📸 Dashboard Preview

### Home
![Home Page](https://raw.githubusercontent.com/anshanalytics/Starbucks-Analysis-Dashboard/main/home_page.png)

### Overview
![Overview Page](https://raw.githubusercontent.com/anshanalytics/Starbucks-Analysis-Dashboard/main/overview_page.png)

### Details
![Details Page](https://raw.githubusercontent.com/anshanalytics/Starbucks-Analysis-Dashboard/main/details_page.png)

---

## 🚀 How to Use

1. Clone or download this repository.
2. Open Starbucks_Analysis_Dashboard.pbit in Power BI Desktop.
3. When prompted, point it to the dataset/ folder (sales.csv, customers.csv, items.csv) included in this repo.
4. Let the data load: KPIs and charts will populate, and the "Last Update" timestamp will reflect the latest transaction datetime after the refresh.
5. Navigate between Home → Overview → Details using the top navigation bar.

---

## 🔮 Future Enhancements

- Add a menu/item performance page using the existing nutrition data (calories, fat, protein) to compare healthy vs. indulgent item sales.
- Surface the built-in target and growth % measures visually (KPI cards with target lines, MoM growth trend charts).
- Add a store-level comparison view using store_id to benchmark the 3 locations against each other.
- Add a customer-type (walk-in vs. mobile-app) breakdown to quantify channel performance directly on the dashboard.

---

## 👤 Author

Built as a personal data analytics project to demonstrate Power BI dashboard design, DAX measure writing, and end-to-end data storytelling from raw transactional data to business insight.
