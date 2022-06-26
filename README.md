# **Amazon Vine Analysis**

## **INDEX**

**- [Overview](#overview)**

**- [Resources](#resources)**

**- [Results](#results)**

**- [Summary](#summary)**



## **Overview**

Since working with Jennifer on the SellBy project was so successful, we’ve been tasked with another, larger project:

- Analyzing Amazon reviews written by members of the paid Amazon Vine program.

The Amazon Vine program is a service that allows manufacturers and publishers to receive reviews for their products. Companies like SellBy pay a small fee to Amazon and provide products to Amazon Vine members, who are then required to publish a review.

For this project, we’ll have access to approximately 50 datasets. Each one contains reviews of a specific product, from clothing apparel to wireless products. We’ll need to pick one of these datasets and use PySpark to perform the ETL process to:

- extract the dataset,
- transform the data,
- connect to an AWS RDS instance,
- and load the transformed data into pgAdmin.

Next, we’ll use PySpark, Pandas, or SQL to determine if there is any bias toward favorable reviews from Vine members in the chosen dataset.

[:top: Go To Top](#index)

## **Resources**

### **Data**

- [Amazon Datasets](https://s3.amazonaws.com/amazon-reviews-pds/tsv/index.txt)
- [Video Game Dataset](https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Video_Games_v1_00.tsv.gz) Dataset chosen for this project

### **Tools**

- Google Colab Notebook
- PySpark
- PostgreSQL (pgAdmin)
- Amazon Web Sevices RDS

[:top: Go To Top](#index)

## **Results**

### **Deliverable 1: Performing ETL on Amazon Video Games Reviews**

✅ **[Deliverable 1 File](https://github.com/amonjaras/Amazon_Vine_Analysis/blob/main/Results/Amazon_Reviews_ETL.ipynb)**

As part of this project, we create a database on AWS RDS with tables in pgAdmin using the schema on **Fig 1**.

> *Fig 1: pgAdmin schema*

![pgAdmin_schema](https://github.com/amonjaras/Amazon_Vine_Analysis/blob/main/Images/postgresql_schema.png)

With the use of PySpark, we were able to extract the information and export it from AWS RDS to pgAdmin, **Fig 2 - Fig 5**, will prove that information was written into the tables.

> *Fig 2: customers_table*

![customers_table](https://github.com/amonjaras/Amazon_Vine_Analysis/blob/main/Images/d1_customers_table.png)

> *Fig 3: products_table*

![products_table](https://github.com/amonjaras/Amazon_Vine_Analysis/blob/main/Images/d1_products_table.png)

> *Fig 4: review_id_table*

![review_id_table](https://github.com/amonjaras/Amazon_Vine_Analysis/blob/main/Images/d1_review_id_table.png)

> *Fig 5: vine_table*

![vine_table](https://github.com/amonjaras/Amazon_Vine_Analysis/blob/main/Images/d1_vine_table.png)

### **Deliverable 2: Determine Bias of Vine Reviews**

✅ **[Deliverable 2 File](https://github.com/amonjaras/Amazon_Vine_Analysis/blob/main/Results/Vine_Review_Analysis.ipynb)**

**❗️ The following results were obtained by filtering the dataset by `total_votes > 20` and ``(helpful_votes / total_votes) > 50%``**

**1. How many Vine reviews and non-Vine reviews were there?**

- Total paid review = **94**

- Total unpaid review = **40471**

```
# Total number of paid reviews
total_paid_review = paid_df.count()
total_paid_review

# Total number of unpaid reviews
total_unpaid_review = unpaid_df.count()
total_unpaid_review
```
**2. How many Vine reviews were 5 stars? How many non-Vine reviews were 5 stars?**

- Total paid 5-star review = **48**

- Total unpaid 5-star review = **15663**

```
# Total number of 5-star paid reviews
total_paid_five_star_review = paid_df.filter(paid_df.star_rating == 5).count()
total_paid_five_star_review

# Total number of 5-star unpaid reviews
total_unpaid_five_star_review = unpaid_df.filter(unpaid_df.star_rating == 5).count()
total_unpaid_five_star_review
```

**3. What percentage of Vine reviews were 5 stars? What percentage of non-Vine reviews were 5 stars?**

- Percentage 5-star review paid = **51.06%**

- Percentage 5-star review unpaid = **38.7%**

```
# Percentage of 5-star paid reviews
percentage_five_star_paid = round((total_paid_five_star_review / total_paid_review) * 100, 2)
percentage_five_star_paid

# Percentage of 5-star unpaid reviews
percentage_five_star_unpaid = round((total_unpaid_five_star_review / total_unpaid_review) * 100, 2)
percentage_five_star_unpaid
```

[:top: Go To Top](#index)

## **Summary**

Based on the results , we can conclude that there is a positive bias for reviews in the Vine program. Where 51.06% of the reviews in the Vine program were 5 star, while the percentage for the non-Vine reviews is only 38.7%

Additionally, if we remove the filters on the dataframe for `total_votes>20` and `(helpful_votes / total_votes) >= 50%` to perform the analysis of percentages for paid vs unpaid 5 star reviews as a whole we have opposite results as shown on **Fig 6**.

> *Fig 6: Reviews as whole*

![d2_summary](https://github.com/amonjaras/Amazon_Vine_Analysis/blob/main/Images/d2_summary.png)


[:top: Go To Top](#index)
