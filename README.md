# Fashion e-commerce data analysis
I analysed the fashion e-commerce dataset for the period from June 30, 2016, to July 31, 2022, to identify key trends and insights. 

My results:
- [Dashboard](https://github.com/uzhegovaelena/ecommerce_analysis/blob/main/Fashion_e-commerce_analysis.pdf)
- [Presentation](https://github.com/uzhegovaelena/ecommerce_analysis/blob/main/presentation_fashion_ecommerce_analysis.pdf)
- [Report](https://github.com/uzhegovaelena/ecommerce_analysis/blob/main/Report_ecommerce_analysis.pdf)
- [Data analysis using colab notebook](https://github.com/uzhegovaelena/ecommerce_analysis/blob/main/e_commerce.ipynb)

Stack: Python, Jupyter notebook, SQL, Bigquery, Looker Studio.

**Overview:**

The top-selling product categories are Women's Clothing, Women's Shoes, and Men's Clothing, while the majority of the user base is between the ages of 20-32 years old, and females constitute more than 60% of the user base. Jakarta, Jawa Barat (Bandung), and Jawa Tengah (Semarang) are the top-performing cities, with an average purchase frequency of 9.2 times per year and an average order value of IDR 366,800. The popular payment methods on the platform are e-wallets like OVO and Gopay, and credit cards.

The RFM (Recency, Frequency, Monetary) analysis was conducted to identify high-value customers and develop effective marketing campaigns. Overall, the RFM analysis highlights the importance of personalization and customer retention for the company's growth and success.



SQL code example RFM score calculation:
```
...
rfm_percentiles AS (
SELECT
a.*,
--All percentiles for MONETARY
b.percentiles[offset(25)] AS m25,
b.percentiles[offset(50)] AS m50,
b.percentiles[offset(75)] AS m75,
b.percentiles[offset(100)] AS m100,
--All percentiles for FREQUENCY
c.percentiles[offset(25)] AS f25,
c.percentiles[offset(50)] AS f50,
c.percentiles[offset(75)] AS f75,
c.percentiles[offset(100)] AS f100,
--All percentiles for RECENCY
d.percentiles[offset(25)] AS r25,
d.percentiles[offset(50)] AS r50,
d.percentiles[offset(75)] AS r75,
d.percentiles[offset(100)] AS r100
FROM
data_for_rfm a,
(SELECT APPROX_QUANTILES(monetary, 100) percentiles FROM data_for_rfm) b,
(SELECT APPROX_QUANTILES(frequency, 100) percentiles FROM data_for_rfm) c,
(SELECT APPROX_QUANTILES(recency, 100) percentiles FROM data_for_rfm) d
),

rfm_scores AS (
SELECT *,
CONCAT(r_score, f_score, ROUND(m_score)) AS rfm_score
FROM (
SELECT *,
CASE WHEN monetary <= m25 THEN 1
WHEN monetary <= m50 AND monetary > m25 THEN 2
WHEN monetary <= m75 AND monetary > m50 THEN 3
WHEN monetary <= m100 AND monetary > m75 THEN 4
END AS m_score,
CASE WHEN frequency <= f25 THEN 1
WHEN frequency <= f50 AND frequency > f25 THEN 2
WHEN frequency <= f75 AND frequency > f50 THEN 3
WHEN frequency <= f100 AND frequency > f75 THEN 4
END AS f_score,
--Recency scoring is reversed
CASE WHEN recency <= r25 THEN 4
WHEN recency <= r50 AND recency > r25 THEN 3
WHEN recency <= r75 AND recency > r50 THEN 2
WHEN recency <= r100 AND recency > r75 THEN 1
END AS r_score,
FROM rfm_percentiles
))
...

```
### RFM visualisation
![Link](https://github.com/uzhegovaelena/ecommerce_analysis/blob/main/RFM%20table.png)
![Link](https://github.com/uzhegovaelena/ecommerce_analysis/blob/main/RFM%20analysis.png)

