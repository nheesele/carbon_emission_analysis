# CARBON EMISSION ANALYSIS

![](https://github.com/nheesele/carbon_emission_analysis/blob/main/big_emission-_shutterstock_1871428867.jpg)
Souce: https://green-glossary.com/en/terim/66-emisyon

This report aims to analyze carbon emissions to examine the carbon footprint across various industries, identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

## Data Souce:

Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

## Data Structure
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.

![](https://github.com/nheesele/carbon_emission_analysis/blob/main/Database%20diagram.png)

Table 1: product_emissions
```sql
SELECT * FROM product_emissions LIMIT 5;
```
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|

Table 2: industry_groups
```sql
SELECT * FROM industry_groups LIMIT 5;
```
|id|industry_group|
|--|--------------|
|1|"Consumer Durables, Household and Personal Products"|
|2|"Food, Beverage & Tobacco"|
|3|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|
|4|"Mining - Iron, Aluminum, Other Metals"|
|5|"Pharmaceuticals, Biotechnology & Life Sciences"|

Table 3: companies
```sql
SELECT * FROM companies LIMIT 5;
```
|id|company_name|
|--|------------|
|1|"Autodesk, Inc."|
|2|"Casio Computer Co., Ltd."|
|3|"Cisco Systems, Inc."|
|4|"CNX Coal Resources, LP"|
|5|"Coca-Cola Enterprises, Inc."|

Table 4: countries
```sql
SELECT * FROM countries LIMIT 5;
```
|id|country_name|
|--|------------|
|1|Australia|
|2|Belgium|
|3|Brazil|
|4|Canada|
|5|Chile|

## Data analysis
#### Which products contribute the most to carbon emissions?

Here are the Top 10 products with the highest average PCF
```SQL
SELECT 
		product_name,
		ROUND(AVG(carbon_footprint_pcf),2) AS "Total PCF"
FROM product_emissions
GROUP BY product_name
ORDER BY AVG(carbon_footprint_pcf) DESC
LIMIT 10;
```
|product_name|Total PCF|
|------------|---------|
|Wind Turbine G128 5 Megawats|3718044.00|
|Wind Turbine G132 5 Megawats|3276187.00|
|Wind Turbine G114 2 Megawats|1532608.00|
|Wind Turbine G90 2 Megawats|1251625.00|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.00|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000.00|
|TCDE|99075.00|
|Mercedes-Benz GLE (GLE 500 4MATIC)|91000.00|
|Mercedes-Benz S-Class (S 500)|85000.00|
|Mercedes-Benz SL (SL 350)|72000.00|

#### What are the industry groups of these products?

Let's see which industry do the top 10 above belongs to:
```SQL
SELECT 	product_emissions.id,
		product_emissions.product_name,
		product_emissions.carbon_footprint_pcf,
		industry_groups.industry_group
FROM product_emissions
LEFT JOIN industry_groups
    ON industry_groups.id = product_emissions.industry_group_id
GROUP BY product_name
ORDER BY AVG(carbon_footprint_pcf) DESC
LIMIT 10;
```

|id|product_name|carbon_footprint_pcf|industry_group|
|--|------------|--------------------|--------------|
|22917-4-2015|Wind Turbine G128 5 Megawats|3718044|Electrical Equipment and Machinery|
|22917-5-2015|Wind Turbine G132 5 Megawats|3276187|Electrical Equipment and Machinery|
|22917-3-2015|Wind Turbine G114 2 Megawats|1532608|Electrical Equipment and Machinery|
|22917-2-2015|Wind Turbine G90 2 Megawats|1251625|Electrical Equipment and Machinery|
|8362-1-2016|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687|Automobiles & Components|
|904-2-2013|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000|Materials|
|12134-8-2017|TCDE|99075|Materials|
|4235-30-2016|Mercedes-Benz GLE (GLE 500 4MATIC)|91000|Automobiles & Components|
|4235-32-2016|Mercedes-Benz S-Class (S 500)|85000|Automobiles & Components|
|4235-36-2016|Mercedes-Benz SL (SL 350)|72000|Automobiles & Components|

#### What are the industries with the highest contribution to carbon emissions?

Here are the top 10 industries with the highest total PCF:

```SQL
SELECT 	industry_groups.industry_group,
		ROUND(SUM(product_emissions.carbon_footprint_pcf),2) AS "Total PCF"
FROM product_emissions
LEFT JOIN industry_groups
    ON industry_groups.id = product_emissions.industry_group_id
GROUP BY industry_groups.industry_group
ORDER BY SUM(carbon_footprint_pcf) DESC
LIMIT 10;
```
|industry_group|Total PCF|
|--------------|---------|
|Electrical Equipment and Machinery|9801558.00|
|Automobiles & Components|2582264.00|
|Materials|577595.00|
|Technology Hardware & Equipment|363776.00|
|Capital Goods|258712.00|
|"Food, Beverage & Tobacco"|111131.00|
|"Pharmaceuticals, Biotechnology & Life Sciences"|72486.00|
|Chemicals|62369.00|
|Software & Services|46544.00|
|Media|23017.00|


#### What are the companies with the highest contribution to carbon emissions?

Top 10 companies with highest PCF

```SQL
SELECT 
    companies.company_name,
    ROUND(AVG(product_emissions.carbon_footprint_pcf),2) AS "Total PCF"
FROM product_emissions
JOIN companies
    ON companies.id = product_emissions.company_id
GROUP BY companies.company_name
ORDER BY AVG(product_emissions.carbon_footprint_pcf) DESC
LIMIT 10;
```
|company_name|Total PCF|
|------------|---------|
|"Gamesa Corporación Tecnológica, S.A."|2444616.00|
|"Hino Motors, Ltd."|191687.00|
|Arcelor Mittal|83503.50|
|Weg S/A|53551.67|
|Daimler AG|43089.19|
|General Motors Company|34251.75|
|Volkswagen AG|26238.40|
|Waters Corporation|24162.00|
|"Daikin Industries, Ltd."|17600.00|
|CJ Cheiljedang|15802.83|

#### What are the countries with the highest contribution to carbon emissions?


#### What is the trend of carbon footprints (PCFs) over the years?


#### Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?



