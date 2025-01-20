# **Telecommunication Customers Churn Prediction**

**By Muhammad Rizdky Maulady**

-   **Programming Language**

[![forthebadge
made-with-python](http://ForTheBadge.com/images/badges/made-with-python.svg)](https://www.python.org/)

-   **Git and Github**

Repository : [Telecommunication Customers Churn
Prediction](https://github.com/rizdkymaul/Telecommunication-Customers-Churn-Prediction.git)

-   **Dataset**

[Telecommunication
Customer](https://github.com/rizdkymaul/Telecommunication-Customers-Churn-Prediction/blob/main/Telecom_Customers_Churn.csv "Telecommunication Customer")
:::


`1. Which features should be used based on the EDA results?`

`2. Are there any additional supporting features??`

`3. Conduct model training & predict churn as the target variable`

`4. Mvaluate the model using Recall and ROC-AUC metrics`

:::

# Definitions of Each Column

● Customerid: the ID of the customer

● Gender: the gender of the customer

● Seniorcitizen: whether the customer is a senior citizen or not

● Partner: whether the customer has a partner or not

● Dependents: whether the customer has dependents such as children, etc.

● Tenure: the tenure of the customer's subscription

● PhoneService: whether the customer uses phone service or not

● MultipleLines: whether the customer uses multiple lines or not

● InternetService: the type of internet service used

● OnlineSecurity: whether the customer uses online security features

● OnlineBackup: whether the customer uses online backup features

● DeviceProtection: whether the customer uses device protection features

● TechSupport: whether the customer uses tech support features or not

● StreamingTV: whether the customer uses streaming TV features or not

● StreamingMovies: whether the customer uses streaming movie features or not

● Contract: the type of contract of the customer

● PaperlessBilling: whether the customer uses paperless billing features or not

● PaymentMethod: the payment type used by the customer

● MonthlyCharges: the total monthly charges paid

● TotalCharges: the total overall charges paid

● Churn: the target variable indicating whether the customer has churned or not




# **🎉Final Results🎉**
:::

:::
`1. Which features should be used based on the EDA results?`

Based on the correlation heatmap above, there are several features that have significant correlations with the target feature churn (according to the threshold value set at 0.15).

● These features are as follows:

- contract
- tenure_range
- internetservice
- totalcharges
- tenure_monthly_charge_interaction
- monthlycharges
- paperlessbilling
- onlinesecurity
- techsupport
- dependents
- Features to Consider

- seniorcitizen & partner - although there is a small positive correlation, senior customers and those with partners are slightly more at risk of churn.
- streamingtv and streamingmovies - although the correlation is very small, it is considered due to the impact of entertainment services on churn.
Features Not Used

- paymentmethod
- onlinebackup
- deviceprotection
- multiplelines
- phoneservice
- gender
- tenure (as it has been replaced by tenure_range to avoid multicollinearity)
:::

::: 
`2. Are there any additional supporting features?`

1. Adding a new feature for Subscription Duration in Range (tenure_range)
2. Adding an Interaction Feature between monthlycharges and tenure (tenure_monthly_charge_interaction)

**Proven by the final results, both of these features significantly support the model**
:::

`3. Conduct model training & predict churn as the target variable`

Here is the model training for predicting churn as the target variable based on business needs:

● XGBoost: Although it has a lower recall value, XGBoost shows the best ability to distinguish between classes in the training data (highest Train ROC AUC). However, the lower recall value can be a problem if the main goal is to capture all customers who churn.
● Random Forest: It has a good balance between ROC AUC and recall. With a high Test ROC AUC (0.860) and good recall (0.850), Random Forest is a solid choice for applications requiring effective churn detection.
● Decision Tree: Although it has the highest recall (0.860), the lower ROC AUC value indicates that this model may not be as strong as the others in distinguishing between classes.

:::

::: 
`4. Evaluate the model using Recall and ROC-AUC metrics.`

Here is the model evaluation using Recall and ROC-AUC metrics:

● Recall:
Decision Tree has the highest recall (0.860), meaning this model is most effective in capturing customers who churn. Random Forest also has a good recall (0.850), while XGBoost has the lowest recall (0.720).

● ROC AUC:
XGBoost has the highest Train ROC AUC (0.940), indicating the best ability to distinguish between classes in the training data.

**Random Forest has the highest Test ROC AUC (0.860), showing good performance on the test data**

# Business Recommendations Based on Final Model Evaluation and Feature Importance of the Random Forest Model:

**1. Fokus pada Kontrak**

Important Feature: `contract`

-   Recommendation: Offer more flexible contract options or incentives for customers who choose long-term contracts. This can help reduce churn by providing a sense of commitment  to customers.
-   Campaign Option: **Flexible Contracts**: Offer discounts for customers who choose long-term contracts. Provide an option to exit without penalty after a certain period.

**2. Optimize Monthly Costs**

Important Feature: `monthlycharges`

-   Recommendation: Review the monthly cost structure and consider offering more competitive packages or discounts for customers at risk of churn. Price adjustments can enhance customer satisfaction and reduce churn.
-   Campaign Option: **Savings Package**: Launch a new package with more competitive pricing and additional benefits. Offer promotions for customers at risk of churn.

1.  **Manfaatkan Tenure dan Total Charges**

Feature `Penting: tenure_range dan totalcharges`

-   Rekomendasi: Identifikasi pelanggan dengan tenure yang lebih pendek
    dan total charges yang tinggi. Tawarkan program loyalitas atau
    penghargaan untuk meningkatkan retensi pelanggan yang telah
    berinvestasi lebih banyak dalam layanan.
-   Opsi Campaign : `Loyalty Rewards`: Program penghargaan untuk
    pelanggan yang telah berlangganan lebih dari satu tahun. Berikan
    diskon atau bonus layanan untuk meningkatkan loyalitas.

1.  **Tingkatkan Layanan Internet**

Feature Penting: `internetservice`

-   Rekomendasi: Pastikan kualitas layanan internet yang ditawarkan
    memenuhi harapan pelanggan. Pertimbangkan untuk meningkatkan
    kecepatan atau menawarkan paket tambahan yang menarik untuk
    pelanggan yang menggunakan layanan internet.
-   Opsi Campaign : `Upgrade Gratis`: Tawarkan upgrade kecepatan
    internet gratis selama satu bulan untuk pelanggan yang berisiko
    churn.

1.  **Interaksi antara Tenure dan Biaya Bulanan**

Feature Penting: `tenure_monthly_charge_interaction`

-   Rekomendasi: Analisis interaksi antara lama berlangganan dan biaya
    bulanan untuk mengidentifikasi pola churn. Tawarkan penyesuaian
    harga atau promosi khusus untuk pelanggan yang telah lama
    berlangganan tetapi merasa biaya bulanan terlalu tinggi.
-   Opsi Campaign `Penyesuaian Harga`: Tawarkan penyesuaian harga untuk
    pelanggan yang telah lama berlangganan tetapi merasa biaya terlalu
    tinggi.

1.  **Tingkatkan Layanan Pelanggan**

Feature Penting: `techsupport, onlinesecurity, dan paperlessbilling`

-   Rekomendasi: Tingkatkan layanan pelanggan dengan menyediakan
    dukungan teknis yang lebih baik dan opsi keamanan online. Edukasi
    pelanggan tentang manfaat dari layanan ini untuk meningkatkan
    kepuasan dan loyalitas.
-   Opsi Campaign `Support Anytime`: Luncurkan layanan dukungan 24/7
    dengan pelatihan khusus untuk tim layanan pelanggan. Tawarkan sesi
    edukasi online tentang penggunaan layanan.

1.  **Segmentasi Pelanggan**

Feature Penting:
`dependents, streamingmovies, streamingtv, partner, dan seniorcitizen`

-   Rekomendasi: Lakukan segmentasi pelanggan berdasarkan fitur-fitur
    ini untuk menyesuaikan penawaran dan komunikasi. Misalnya, tawarkan
    paket khusus untuk keluarga, pelanggan senior, atau pengguna layanan
    streaming.
-   Opsi Campaign `Paket Keluarga`: Tawarkan paket khusus untuk keluarga
    dengan layanan streaming dan dukungan tambahan. `Senior Special`:
    Program khusus untuk pelanggan senior dengan diskon dan layanan yang
    disesuaikan.
:::
