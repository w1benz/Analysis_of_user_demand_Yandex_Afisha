# Analysis of User Demand — Yandex Afisha

Exploratory data analysis and hypothesis testing on ticket order data for
Yandex Afisha (event ticketing platform), aimed at understanding event
popularity, seasonal demand patterns, and behavioral differences between
mobile and desktop users.

> **Note on data:** this project uses a training dataset provided by
> **Yandex Practicum** (Data Analyst program) as part of a learning
> simulation. The dataset and project brief were provided by Yandex
> Practicum; all data cleaning, analysis, statistical testing, and
> conclusions below were built and written independently.

**Notebook:** [`Analysis_of_user_demand_Yandex_Afisha_eng.ipynb`](./Analysis_of_user_demand_Yandex_Afisha_eng.ipynb)

---

## 1. Business Context

Yandex Afisha wants to better understand user activity and ticket sales
patterns to inform product and marketing decisions — including which
event types perform best, how demand shifts seasonally, and whether
mobile and desktop users behave differently enough to warrant separate
product/marketing strategies.

**Goals:**
- Conduct exploratory data analysis (EDA) to identify patterns in user activity and sales
- Test statistical hypotheses related to user behavior across device types
- Prepare data-driven recommendations for the product team

**Tasks:**
- Load and review the source data
- Prepare the data for research analysis (types, missing values, duplicates, outliers)
- Analyze the distribution of orders and revenue
- Compare mobile vs. desktop users using statistical hypothesis testing
- Prepare visualizations and a final summary with recommendations

---

## 2. Tech Stack

- **Python** (pandas, numpy)
- **Matplotlib / Seaborn** — visualizations
- **SciPy** — statistical hypothesis testing (Mann–Whitney U test)
- **Jupyter Notebook**

---

## 3. Dataset

Two core datasets plus a supporting currency-exchange table.

### `final_tickets_orders_df` (orders_df) — ticket order records
| Field | Description |
|---|---|
| `order_id` | Unique order ID |
| `user_id` | Unique user ID |
| `created_dt_msk` / `created_ts_msk` | Order creation date / datetime (Moscow time) |
| `event_id` | Event ID, links to the events table |
| `cinema_circuit` | Cinema chain (or "no" if not applicable) |
| `age_limit` | Age restriction for the event |
| `currency_code` | Payment currency (e.g. `rub`) |
| `device_type_canonical` | Device used to place the order (`mobile` / `desktop`) |
| `revenue` | Revenue from the order |
| `service_name` | Ticket operator name |
| `tickets_count` | Number of tickets purchased |
| `total` | Total order amount |
| `days_since_prev` | Days since the user's previous purchase |

### `final_tickets_events_df` (events_df) — event metadata
| Field | Description |
|---|---|
| `event_id` | Unique event ID |
| `event_name` | Event name |
| `event_type_description` | Event type description |
| `event_type_main` | Main event category (theater, concert, etc.) |
| `organizers` | Event organizers |
| `region_name` / `city_name` | Region / city |
| `venue_id` / `venue_name` / `venue_address` | Venue details |

### `final_tickets_tenge_df` — supporting currency data
KZT (tenge) → RUB exchange rate for 2024 (`nominal`, `data`, `curs`, `cdx`),
used to convert tenge-denominated orders into rubles for consistent
revenue analysis.

**Combined scope:** 25 columns, 290,849 rows after merging.

---

## 4. Methodology

1. **Data loading & review** — examined structure and types of both datasets
2. **Preprocessing** — optimized `age_limit` to `int8`, checked and
   handled missing values, removed explicit and implicit duplicates,
   filtered outliers in `revenue` and `tickets_count`, converted tenge
   revenue to rubles, and engineered new columns: `revenue_rub`,
   `one_ticket_revenue_rub`, `month`, `season`
3. **Exploratory analysis** — examined event popularity by type and
   season, order volume trends by month, average ticket price (check)
   by season and category, regional and operator-level performance,
   daily active users (DAU), and orders-per-user patterns by device type
4. **Statistical hypothesis testing** — used the **Mann–Whitney U test**
   (α = 0.05) to compare mobile vs. desktop users on two dimensions:
   number of orders placed, and time between orders
5. **Conclusions & recommendations** — synthesized findings into
   actionable product/marketing takeaways

---

## 5. Key Insights

**Event popularity**
**Concerts** were the most frequently attended event type, while
**theater** events were less popular. Demand showed a seasonal skew:
children's events (ages 6–12) peaked in summer, while events with no age
restriction peaked in autumn.

**Seasonal order growth**
Order volume grew steadily from **~34,000 orders in June to nearly
100,000 in November**, with a particularly sharp +35% jump between
September and October — autumn is clearly the platform's peak demand
period.

**Average check (ticket price)**
Average revenue per ticket was higher in **summer** than autumn overall
(**270 RUB vs. 250 RUB**), especially within the concerts category —
despite autumn having higher order *volume*.

**Regional & operator leaders**
The **Kamenevsky region** led across all key indicators — number of
events, orders, and revenue. Among ticket operators, **Loviticket!** and
**Tickets Without Problems** stood out as the main partners by both
offering variety and financial performance.

**Mobile vs. desktop behavior (hypothesis testing)**
Both Mann–Whitney U tests returned a p-value of 0.000 (highly
significant, α = 0.05):
- **Mobile users place more orders** than desktop users
- **Mobile users also have longer intervals between orders**

This combination — more frequent purchasing overall, but with more time
between individual orders — suggests mobile is the primary engagement
channel, but with room to shorten the re-purchase cycle.

---

## 6. Recommendations

1. **Prioritize the mobile experience** — mobile already drives more
   orders per user; continued investment here has the highest leverage
   for retention and new-user acquisition.
2. **Target push notifications and timed campaigns at mobile users**
   specifically to shorten the gap between orders and increase purchase
   frequency.
3. **Plan inventory and marketing around the autumn demand peak**
   (especially the September–October surge), while using the higher
   average summer ticket price to optimize revenue during the
   lower-volume season.
4. **Deepen partnerships with top-performing regions and operators**
   (Kamenevsky region; Loviticket!, Tickets Without Problems) as
   reference points for expansion elsewhere.

---

## 7. Repository Structure

```
.
├── README.md
└── Analysis_of_user_demand_Yandex_Afisha_eng.ipynb
```

---

## Author

**Vladislav Wiesner**
[LinkedIn](#) · [GitHub](https://github.com/w1benz)
