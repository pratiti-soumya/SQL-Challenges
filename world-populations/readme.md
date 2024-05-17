# Case Study: World Populations

## Introduction
This case study focuses on analyzing global population data by continent and country using SQL queries. The dataset consists of two tables: `countries`, which contains information about different countries and their respective continents, and `population_years`, which records the population of each country for various years.

## Problem Statement
The goal of this analysis is to extract meaningful insights from the population data, such as the number of countries in Africa, the total population of Oceania in 2005, and other relevant population statistics.

## Data Analysis

### Step 1: Preview the Data
To understand the structure of the data, we preview the tables.
```sql
SELECT * FROM countries;
SELECT * FROM population_years;
```
### Step 2: Analyze Population Data
### 1. How many entries in the countries table are from Africa?
```sql
SELECT COUNT(DISTINCT name) AS 'countries from Africa'
FROM countries
WHERE continent = 'Africa';
```
Insight: This query counts the number of unique countries in Africa.

### 2. What was the total population of the continent of Oceania in 2005?
```sql
WITH Population AS
(SELECT country_id, population 
FROM population_years
WHERE year = 2005)

SELECT SUM(Population.population) AS 'Total Population', 
countries.continent AS 'Continent'
FROM Population 
JOIN countries ON Population.country_id = countries.id
WHERE countries.continent = 'Oceania'
GROUP BY countries.continent;
```
Insight: This query calculates the total population of Oceania in 2005.

### 3. What is the average population of countries in South America in 2003?
```sql
WITH Population AS
(SELECT country_id, population 
FROM population_years
WHERE year = 2003)

SELECT AVG(Population.population) AS 'Average Population', 
countries.continent AS 'Continent'
FROM Population 
JOIN countries ON Population.country_id = countries.id
WHERE countries.continent = 'South America'
GROUP BY countries.continent;
```
Insight: This query calculates the average population of South American countries in 2003.

### 4. What country had the smallest population in 2007?
```sql
WITH Population AS
(SELECT country_id, population 
FROM population_years
WHERE year = 2007)

SELECT MIN(Population.population) AS 'Minimum Population', 
countries.name AS 'Country Name'
FROM Population 
JOIN countries ON Population.country_id = countries.id;
```
Insight: This query identifies the country with the smallest population in 2007.

### 5. What is the average population of Poland during the time period covered by this dataset?
```sql
WITH Population AS
(SELECT country_id, population 
FROM population_years)

SELECT AVG(Population.population) AS 'Average Population', 
countries.name AS 'Country Name'
FROM Population 
JOIN countries ON Population.country_id = countries.id
WHERE countries.name = 'Poland';
```
Insight: This query calculates the average population of Poland over the entire period covered by the dataset.

### 6. How many countries have the word “The” in their name?
```sql
SELECT COUNT(*) AS 'Total Countries with The'
FROM countries 
WHERE name LIKE '%The%';
```
Insight: This query counts the number of countries with the word "The" in their name.

### 7. What was the total population of each continent in 2010?
```sql
WITH Population AS
(SELECT country_id, population 
FROM population_years
WHERE year = 2010)

SELECT SUM(Population.population) AS 'Total Population', 
countries.continent AS 'Continent'
FROM Population 
JOIN countries ON Population.country_id = countries.id
GROUP BY countries.continent;
```
Insight: This query calculates the total population of each continent in 2010.

### Insights
- Number of African Countries: The dataset includes a certain number of countries from Africa.
- Population of Oceania in 2005: The total population of Oceania in 2005 is quantified.
- Average Population of South America in 2003: The average population of South American countries in 2003 is determined.
- Smallest Population in 2007: The country with the smallest population in 2007 is identified.
- Average Population of Poland: The average population of Poland over the covered period is calculated.
- Countries with 'The' in Their Name: The number of countries with "The" in their name is counted.
- Total Population by Continent in 2010: The total population of each continent in 2010 is computed.

### Conclusion
This analysis provides valuable insights into global population distributions and trends. By understanding these patterns, policymakers and researchers can make more informed decisions regarding resource allocation, development planning, and other critical areas.

