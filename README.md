# Home Sale Analysis

For this assignment, I am using PySpark and SQL to query a large data set regarding home sales for 2019-2022. To execute this assignment, I used Google Colab. 
</br>
</br>
I start by reading in the CSV with the sales data with PySpark and creating a DataFrame.

```
spark.sparkContext.addFile(url)
df = spark.read.csv(SparkFiles.get("home_sales_revised.csv"), sep=",", header=True)
df.show()
```

![df](https://github.com/acosta109/Home_Sales/assets/119609975/c6727432-33be-413c-8333-1ebe4fd0e21e)

From here, I create my temporary view of the data set `df.createOrReplaceTempView('home_sales')` and start executing the assigned queries. First, I am asked to find the average price of four bedroom houses sold by year. I executed the following query.

```
SELECT year(date) AS year , ROUND(avg(price), 2) AS average_price 
FROM home_sales 
WHERE bedrooms = 4 
GROUP BY year(date) 
ORDER BY year(date)
```
![avg 4 bedroom house sold by year](https://github.com/acosta109/Home_Sales/assets/119609975/e3cf25c0-1eea-4eb0-9887-cf5f118f2cdc)

Additionally, I query to find the average price by year of 3 bed 3 bath houses and 3 bed 3 bath with 2 floors and 2,000sqft. 

```
SELECT year(date) AS year, ROUND(avg(price), 2) AS average_price 
FROM home_sales WHERE bedrooms = 3 AND bathrooms = 3 
GROUP BY year(date) 
ORDER BY year(date)
```
![3bed](https://github.com/acosta109/Home_Sales/assets/119609975/5832e73e-86e8-4f39-b03e-cf932b7a1c73)

```
SELECT year(date) AS year, ROUND(avg(price), 2) AS average_price 
FROM home_sales 
WHERE bedrooms = 3 AND bathrooms = 3 AND floors = 2 AND sqft_living >= 2000 
GROUP BY year(date) 
ORDER BY year(date)
```
![3 bed 3 bath 2 floors](https://github.com/acosta109/Home_Sales/assets/119609975/8c0639cf-7112-4b15-8a94-105c1de0ac3f)

Lastly, I am tasked with performing the following query with the data set uncached, cached, and paritioned by `date_built` using parquet. 
```
SELECT ROUND(avg(view), 2) AS view_rating 
FROM home_sales 
WHERE price >= 350000
```
### Uncached
![noncached](https://github.com/acosta109/Home_Sales/assets/119609975/35dd52ed-9ef4-4ac3-b80a-4a22c179ebac)

### Cached
![cached](https://github.com/acosta109/Home_Sales/assets/119609975/946c2601-36ee-4253-b383-7dd11d6c4764)

### Partitioned
![partitioned](https://github.com/acosta109/Home_Sales/assets/119609975/dcbc887e-d8bf-437c-bd43-1834a85058bc)

## Results of 3 Trials

As I expected, the uncached data was the slowest way to query to data. I expected the partition to be the fastest, but 
it's possible this data set isn't large enough for it to make a noticeable difference between the cached and partitioned 
data.
