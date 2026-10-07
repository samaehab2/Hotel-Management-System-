🏨 Hotel Management System – Power BI Dashboard:
An interactive, 5-page Power BI report that analyses **87,210 hotel bookings** (City Hotel & Resort Hotel, July 2015 – August 2017) to answer one business question:

> **Where is the hotel losing money, and which guests and booking channels drive revenue?**
The report covers booking performance, cancellations, revenue leakage and guest behaviour, with a navigation home page and global slicers (Month, Market Segment, Country) on every analysis page.


📌 Headline Numbers (no filters applied)

 KPI | Value :
 Total Bookings | 87,210 |
 Canceled Bookings | 24,008 |
 **Cancellation Rate** | **27.5%** |
 Total Revenue (ADR × stay nights) | ≈ 34.4M |
 **Lost Revenue** (canceled bookings) | **≈ 11.5M (about 33% of total revenue)** |
 Average ADR | 106.5 |
 Average Lead Time | 80 days |
 Average Length of Stay | 3.6 nights |
 Repeat Guest Rate | 3.9% |

🔍 Key Insights

1. **One in three revenue units is lost to cancellations.** Lost revenue is ≈ 11.5M against ≈ 34.4M total revenue (33%).
2. **Online Travel Agents drive volume and risk.** Online TA is 59% of bookings with a 35.4% cancellation rate. Corporate (12.1%), Direct (14.8%) and Offline TA/TO (14.8%) are far more reliable.
3. **City Hotel cancels more than Resort Hotel.** 30.1% vs 23.5%, and City Hotel also carries more lost revenue (6.6M vs 4.9M).
4. **Longer lead time means higher cancellation risk.** Bookings made 0–30 days ahead cancel at 16.4%, while 181–365 days ahead cancel at 39.7%.
5. **Engaged guests cancel less.** Bookings with special requests cancel at 21.8% against 33.3% for those without.
6. **Non-refundable deposits show a 94.7% cancellation rate.** This is counter-intuitive, but it applies to only about 1,000 bookings (1.2%), so it deserves a data-quality check before any policy decision.
7. **Strong seasonality.** Average ADR rises from about 70 in January to about 151 in August, and August is also the busiest month (11,242 bookings) and the month with the highest lost revenue (≈ 2.6M).
8. **Meal plan lifts rate.** Full Board averages about 143 ADR against about 104 for Bed & Breakfast, although it represents under 1% of bookings.
9. **Loyalty is a gap.** Only 3.9% of bookings come from repeat guests, and 82% of customers are Transient.
10. **Portugal is the biggest market** (≈ 9.2M revenue) but also the largest source of lost revenue (≈ 4.2M, 46% of its revenue).

💡 Recommendations

- Tighten cancellation policies or require deposits for **Online TA** and long lead-time bookings.
- Overbook strategically in **peak months (Jul–Aug)** where cancellation losses are highest.
- Encourage **special requests and pre-arrival engagement** (upsells, pre-check-in) since they correlate with fewer cancellations.
- Promote **Half/Full Board packages** to lift ADR.
- Launch a **loyalty programme** to grow the 3.9% repeat-guest base.
- Investigate the **Non Refund deposit** records for data-quality or policy issues.


 🧱 Data Model

- **Fact table:** `hotel_bookings_final` (87,210 rows, 36 source columns + calculated columns)
- **Date table:** `DateTable` (with `Month` label and `MonthSort` for correct chronological ordering), related to `hotel_bookings_final[arrival_date]`
- **Source:** CSV file loaded with Power Query

Calculated columns
| `arrival_date` | Builds a real date from day, month name and year columns |
| `Has Special Requests` | "Has requests" / "No requests" based on `total_of_special_requests` |
| `Lead Time Bucket` | Groups lead time into 0-30, 31-90, 91-180, 181-365, 365+ |

DAX measures
| `Total Bookings` | `COUNTROWS(hotel_bookings_final)` |
| `Canceled Bookings` | Count of rows where `is_canceled = 1` |
| `Net Bookings` | Count of rows where `is_canceled = 0` |
| `Cancellation Rate` | `DIVIDE(Canceled Bookings, Total Bookings, 0)` |
| `Total Revenue` | `SUMX(table, adr * total_stay)` |
| `Lost Revenue` | `SUMX(FILTER(canceled), adr * (weekend nights + week nights))` |
| `Average ADR` | `AVERAGE(adr)` |
| `Average Lead Time` | `AVERAGE(lead_time)` |
| `Average Length of Stay` | `AVERAGE(total_stay)` |
| `Repeat Guest Rate` | Share of bookings where `is_repeated_guest = 1` |
| `Revenue MoM %` | `(Revenue − Revenue previous month) / Revenue previous month` using `DATEADD` |
| `Bookings with Adults / Children / Babies` | Bookings where each guest count is greater than 0 |


🛠️ Tools & Skills Demonstrated

- **Power BI Desktop** – report design, custom navigation, slicers, cross-filtering
- **Power Query** – data loading and cleaning
- **DAX** – measures, calculated columns, time intelligence, `CALCULATE`, `SUMX`, `FILTER`, `DIVIDE`, `DATEADD`
- **Data modelling** – date table and relationships
- **Data storytelling** – KPI cards, trend, funnel, scatter, donut, pie and stacked-area visuals
- **Data Preprocessing & EDA**
        -Cleaning & Handling Missing Data:** Handled missing values, formatted data types, and removed duplicates using `pandas` and `numpy`.
        -Exploratory Data Analysis:** Performed in-depth data analysis and visualization with `matplotlib` and `seaborn` to uncover statistical distributions and patterns.
