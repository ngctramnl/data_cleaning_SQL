# Cleaning Data in SQL
- The changelog can be found in the notebook uploaded. The notebook was downloaded from a [workspace](https://app.datacamp.com/workspace/w/b0bcb8ab-dc5b-45f8-a660-5c52890c5bec/edit) on Datacamp.
## Missing values
I filter for records where the `unemployment_rate` is `NULL` and then `COUNT()` the number of rows. **163 missing values recorded**.
  
```-- Identifying missing data --
SELECT COUNT(*) AS number_missing_unemployment_rates
FROM world.economies
WHERE unemployment_rate IS NULL
```
### Filling missing values
I use `COALESCE()` to replace NULL values with the average unemployment rate (which is accessed via a subquery). `COALESCE()` works by returning the first argument if it is not null. If it is null, it returns the second argument, and so forth. If the `unemployment_rate` column is `NULL`, it returns the second argument, which is the average unemployment I calculate with a subquery.
```-- Filling missing values --
WITH clean AS (
SELECT
	code,
	year,
	gross_savings,
	income_group,
	gdp_percapita,
	inflation_rate,
	total_investment,
	COALESCE(unemployment_rate, (SELECT AVG(unemployment_rate) FROM world.economies WHERE unemployment_rate IS NOT NULL)) AS filled_unemployment_rate,
	ROW_NUMBER() OVER(PARTITION BY code, unemployment_rate) AS row_number
FROM world.economies)

-- Discarding duplicate rows --
SELECT *
FROM clean
WHERE row_number = 1;
```
## Duplicate rows
To identify duplicate rows, I use `ROW_NUMBER()` to assign numbers to rows based on identical combinations. By choosing the `PARTITION` of the window function, I specify over code, unemployment_rate I want to look for duplicates.

I use `PARTITION BY` to assign row numbers based on the combination of country code and unemployment rate. Duplicate rows have a value of 2 or greater.
### Discarding duplicate rows
Removing duplicate rows is just as simple as identifying them. To do so, you simply need to change your filter to select `row_number`s with a value of 1.

## Invalid data
I search for rows where the `indep_year` contains a negative value. To do so, I convert the column to text using `::TEXT`, and then use `LIKE` and the pattern. The pattern I use searches for a minus sign (`-`), followed by any other characters (using the wildcard `%`). I also use pattern matching to find rows with similar variants. I use a pattern to identify all rows with `Monarchy` in the `gov_form` column, useing the `%` wildcard characters to allow for words/whitespace on either side of the word.
```
SELECT indep_year
FROM world.countries
WHERE indep_year::TEXT LIKE '-%'
```
```
SELECT DISTINCT name, gov_form
FROM world.countries
WHERE gov_form LIKE '%Monarchy%'
```
### Fixing invalid data
One way is to use a `CASE` statement to recategorize the data. I convert all `gov_form` rows that contain "Monarchy" to "Monarchy". The remaining entries are left as they are.
```
SELECT DISTINCT 
	name, 
    	gov_form,
CASE WHEN gov_form LIKE '%Monarchy%' THEN 'Monarchy' 
ELSE gov_form END AS fixed_gov_form
FROM world.countries
WHERE gov_form LIKE '%Monarchy%'
```
## Data types
In the query below, I retrieve each column and the data type for the `rental` table in the `dvdrentals` schema.
```
SELECT 
	column_name,
	data_type
FROM information_schema.columns
WHERE table_name = 'rental'
```
### Converting data types
I convert two strings and two integers to different data types. The latter two columns produce identical results. The column `integer_to_text` converts the integer 16 to text using `CAST()`. The column `integer_to_text_with_operator` does the same with the cast operator `::`.
```
SELECT
	CAST('42' AS INTEGER) AS string_to_integer,
	CAST('2022-06-01' AS DATE) AS string_to_date,
	16::TEXT AS integer_to_text_with_operator
```
### Converting date formats
Using `TO_CHAR()` to convert a given date to a provided format.

I use the short name of the month and the last two digits of the year to convert the precise rental date to a month_year column.

_Note: In Workspace, SQL queries are converted to pandas DataFrames. As a result, some formatting strings may result in Python automatically interpreting the result as a datetime and converting the date back to the original format._
```
SELECT 
  rental_id, 
  rental_date, 
  TO_CHAR(rental_date, 'Mon-YY') AS month_year
FROM dvdrentals.rental
```



# Visualizing findings on Tableau
<div class='tableauPlaceholder' id='viz1713451852391' style='position: relative'><noscript><a href='#'><img alt='Dashboard 1 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Ec&#47;Economies_17134501777760&#47;Dashboard1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='Economies_17134501777760&#47;Dashboard1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Ec&#47;Economies_17134501777760&#47;Dashboard1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-GB' /></object></div>                

