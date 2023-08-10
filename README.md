# sql_cheatcodes
sql examples
##############################INTRO TO SQL DATACAMP

### Databases
#relational databases:
	#define relationships between tables of data inside the database

#SQL
SELECT *
FROM patrons
LIMIT 30


### Tables
#table rows and columns are referred as records and fields


## Our very own table


### Data 


### Introducing queries

SELECT name
FROM patrons;

#selecting multiple fields
SELECT card_num, name
FROM patrons;

#selecting all fields
SELECT *
FROM patrons;


##Querying the books table


### Querying the books table

#aliasing : to rename columns
SELECT name AS first_name, year_hired
FROM employees;

#selecting distinct records
SELECT DISTINCT year_hired
FROM employees;

#DISTINCT with multiple fields
SELECT DISTINCT dept_id, year_hired
FROM employees;

# views: a view is a virtual table that is the result of a saved SQL SELECT statement; when accessed, views automatically update in response to updates in the underlying data

CREATE VIEW employee_hire_years AS
SELECT id, name, year_hired
FROM employees;


## Making queries DISTINCT

-- Select unique authors and genre combinations from the books table
SELECT DISTINCT author, genre
FROM books;

## aliasing
-- Alias author so that it becomes unique_author
SELECT DISTINCT author AS unique_author
FROM books;

-- Your code to create the view:
CREATE VIEW library_authors AS
SELECT DISTINCT author AS unique_author
FROM books;

-- Select all columns from library_authors
SELECT *
FROM library_authors


### SQL flavors

#PostgreSQL
	#free and open source relational database system 
#SQL server
	#has free and paid versions; created by microsoft


#  comparing postgreSQL and SQL server

# postgreSQL
SELECT id, name
FROM employees
LIMIT 2;

#SQL server
SELECT id, name
FROM employees
TOP 2;


## Limiting results

-- Select the first 10 genres from books using PostgreSQL
SELECT genre
FROM books
LIMIT 10;



#############################
###########################
###########################
### INTERMEDIATE SQL


### querying a database

#COUNT(): counts the # of records with a value in a field
SELECT COUNT (birthdate) AS count_brithdates
FROM people;


#count multiple fields
SELECT COUNT(name) AS count_names, COUNT(birthdate) AS count_birthdates
FROM people;


#using * with COUNT(): COUNT(field_name) counts values in a field ; COUNT(*) counts records in a table
SELECT COUNT(*) AS total_records
FROM people;


#DISTINCT
SELECT DISTINCT language
FROM films;


#COUNT() with DISTINCT
SELECT COUNT(DISTINCT birthdate) AS count_distinct_birthdates
FROM people;



## Learning to COUNT()
-- Count the number of records in the people table
SELECT COUNT(*) AS count_records
FROM people;


-- Count the number of records in the people table
SELECT COUNT(language) AS count_languages, COUNT(country) AS count_countries
FROM films;



## SELECT DISTINCT

-- Return the unique countries from the films table
SELECT DISTINCT country 
FROM films;

-- Count the distinct countries from the films table
SELECT COUNT(DISTINCT country)  AS count_distinct_countries
FROM films;



### Query execution
#SQL is not processed in its written order


## debuggin errors


### SQL style

#put non-standard field names in double quotes 
SELECT title, "release year", country
FROM films
LIMIT 3;
 


### Filtering numbers

SELECT title 
FROM films
WHERE release_year >= 1960;

SELECT title 
FROM films
WHERE release_year <> 1960; #this symbol means not equal to 1960


#WHERE with strings
SELECT title
FROM films
WHERE country = 'Japan';


## Using WHERE with numbers

-- Select film_ids and imdb_score with an imdb_score over 7.0
SELECT film_id, imdb_score
FROM reviews
WHERE imdb_score > 7.0


-- Select film_ids and facebook_likes for ten records with less than 1000 likes 
SELECT film_id, facebook_likes
FROM reviews
WHERE facebook_likes < 1000
LIMIT 10;

-- Count the records with at least 100,000 votes
SELECT COUNT(*) AS films_over_100K_votes
FROM reviews
WHERE num_votes>=100000


## Using WHERE with text

-- Count the Spanish-language films
SELECT COUNT(language) AS count_spanish
FROM films
WHERE language = 'Spanish';



### Multiple criteria

#OR, AND, BETWEEN
SELECT *
FROM coats
WHERE color = 'yellow' OR length = 'short';


SELECT *
FROM coats
WHERE color = 'yellow' AND length = 'short';


SELECT *
FROM coats
WHERE buttons BETWEEN 1 AND 5;


SELECT title 
FROM films
WHERE (release_year = 1994 OR release_year = 1995)
	AND (certification = 'PG' OR certification = 'R');


SELECT title
FROM films
WHERE release_year
BETWEEN 1994 AND 2000 AND country='UK';



## using AND
-- Select the title and release_year for all German-language films released before 2000
SELECT title, release_year
FROM films
WHERE language='German' AND release_year<2000


-- Select all records for German-language films released after 2000 and before 2010
SELECT *
FROM films
WHERE language='German' AND release_year>2000 AND release_year<2010



## using OR
SELECT title, release_year
FROM films
WHERE (release_year = 1990 OR release_year = 1999)
	AND (language = 'English' OR language = 'Spanish')
-- Filter films with more than $2,000,000 gross
	AND gross>2000000;


## using BETWEEN

-- Select the title and release_year for films released between 1990 and 2000
SELECT title, release_year
FROM films
WHERE release_year BETWEEN 1990 AND 2000
	AND budget > 100000000
-- Amend the query to include Spanish or French-language films
	AND (language='Spanish' OR language='French');



###Filtering text

#LIKE: used to search for a pattern in a field; % match zero, one or many characters
SELECT name 
FROM people
WHERE name LIKE 'Ade%'; #this gives all names that start with Ade


# _ match a single character
SELECT name
FROM people
WHERE name LIKE 'Ev_';

# NOT LIKE
SELECT name
FROM people
WHERE name NOT LIKE 'A.%'; #yields all names thad DO NOT start with A.


#Wildcardposition
SELECT name
FROM people
WHERE name LIKE '%r'; #selects names with last letter being r

SELECT name
FROM people
WHERE name LIKE '__t%';


# WHERE, OR
SELECT title
FROM films
WHERE release_year = 1920
OR release_year = 1930
OR release_year = 1940;


# WHERE, IN
SELECT title
FROM films
WHERE release_year IN (1920, 1930, 1940);

SELECT title
FROM films
WHERE country IN ('Germany', 'France');



## LIKE and NOT LIKE
-- Select the names that start with B
SELECT name
FROM people
WHERE name LIKE 'B%';


SELECT name
FROM people
-- Select the names that have r as the second letter
WHERE name LIKE '_r%'


SELECT name
FROM people
-- Select names that don't start with A
WHERE name NOT LIKE 'A%';


## WHERE IN

-- Find the title and release_year for all films over two hours in length released in 1990 and 2000
SELECT title, release_year
FROM films
WHERE release_year IN (1990, 2000) AND duration>120; 


-- Find the title and language of all films in English, Spanish, and French
SELECT title, language
FROM films
WHERE language IN ('English', 'Spanish', 'French');


-- Find the title, certification, and language all films certified NC-17 or R that are in English, Italian, or Greek
SELECT title, certification, language
FROM films
WHERE (certification IN ('NC-17', 'R')) AND (language IN ('English', 'Italian', 'Greek'));


## Combining filtering and selecting

-- Count the unique titles
SELECT COUNT(DISTINCT title) AS nineties_english_films_for_teens
FROM films
-- Filter to release_years to between 1990 and 1999
WHERE release_year BETWEEN 1990 AND 1999
-- Filter to English-language films
	AND language='English'
-- Narrow it down to G, PG, and PG-13 certifications
	AND certification IN ('G', 'PG', 'PG-13');


### NULL values


#COUNT(field_name) includes only non-missing values
#COUNT(*) includes missing values

# IS NULL
SELECT name
FROM people
WHERE birthdate IS NULL;

#count # of nulls
SELECT COUNT(*) AS no_birthdates
FROM people
WHERE birthdate IS NULL;

#count # of NOT nulls
SELECT COUNT(name) AS count_birthdates
FROM people
WHERE birthdate IS NOT NULL;


SELECT COUNT(certification) AS count_certification
FROM films;


SELECT COUNT(certification) AS count_certification
FROM films
WHERE certification IS NOT NULL;


## Practice with NULLs

-- List all film titles with missing budgets
SELECT title AS no_budget_info
FROM films
WHERE budget IS NULL


-- Count the number of films we have language data for
SELECT COUNT(films) AS count_language_known
FROM films
WHERE language IS NOT NULL;


### Summarizing data

#aggregate functions
SELECT AVG(budget)
FROM films;

SELECT SUM(budget)
FROM films;

SELECT MIN(budget)
FROM films;

SELECT MAX(budget)
FROM films;


#non-numerical data
SELECT MIN(country) #selects the first name by alphabet
FROM films;


## Practice with aggregate functions

-- Query the sum of film durations
SELECT SUM(duration) AS total_duration
FROM films;

-- Calculate the average duration of all films
SELECT AVG(duration) AS average_duration
FROM films;

-- Find the latest release_year
SELECT MAX(release_year) AS latest_year
FROM films;

-- Find the duration of the shortest film
SELECT MIN(duration) AS shortest_film
FROM films;


### Summarizing subsets

#using WHERE with aggregate functions
SELECT AVG(budget) AS avg_budget
FROM films
WHERE release_year >=2010;


SELECT COUNT(budget) AS count_budget
FROM films
WHERE release_year >=2010;


# ROUND(): round a number to a specificed decimal


SELECT ROUND(AVG(budget), 2) AS avg_budget #leaves 2 places after dot
FROM films
WHERE release_year >=2010;

#ROUND() using a negative parameter
SELECT ROUND(AVG(budget), -5) AS avg_budget #ads 0's if there was less than 5 numbers
FROM films
WHERE release_year >=2010;


## combining aggregate function with WHERE

-- Calculate the sum of gross from the year 2000 or later
SELECT SUM(gross) AS total_gross
FROM films
WHERE release_year>=2000


-- Calculate the average gross of films that start with A
SELECT AVG(gross) AS avg_gross_A
FROM films
WHERE title LIKE 'A_%';


-- Calculate the lowest gross film in 1994
SELECT MIN(gross) AS lowest_gross
FROM films
WHERE release_year=1994;


-- Calculate the highest gross film released between 2000-2012
SELECT MAX(gross) AS highest_gross
FROM films
WHERE release_year BETWEEN 2000 AND 2012;


##using ROUND()

-- Round the average number of facebook_likes to one decimal place
SELECT ROUND(AVG(facebook_likes),1) AS avg_facebook_likes
FROM reviews;


## ROUND() with a negative parameter
-- Calculate the average budget rounded to the thousands
SELECT ROUND(AVG(budget), -3) AS avg_budget_thousands
FROM films;


### Aliasing and arithmetic

SELECT (gross-budget) AS profit
FROM films;

#aliasing with functions
SELECT MAX(budget) AS max_budget, MAX(duration) AS max_duration
FROM films;


#order of execution: FROM, WHERE, SELECT, LIMIT


## aliasing with functions

-- Calculate the title and duration_hours from films
SELECT title, duration/60.0 AS duration_hours
FROM films;



-- Calculate the percentage of people who are no longer alive
SELECT count(deathdate) * 100.0 / COUNT(*) AS percentage_dead
FROM people;


-- Find the number of decades in the films table
SELECT (MAX(release_year) - MIN(release_year)) / 10.0 AS number_of_decades
FROM films;


## rounding results

-- Round duration_hours to two decimal places
SELECT title, ROUND(duration / 60.0,2) AS duration_hours
FROM films;


### Sorting results

#ORDER BY
SELECT title,budget
FROM films
ORDER BY budget; #in decreasing order


SELECT title,budget
FROM films
ORDER BY budget DESC;

SELECT title,budget
FROM films
WHERE budget IS NOT NULL
ORDER BY budget DESC;


#ORDER BY multiple fields
SELECT title, wins, imbd_score
FROM best_movies
ORDER BY wins DESC, imbd_score DESC;


## sorting single fields
-- Select name from people and sort alphabetically
SELECT name
FROM people
ORDER BY name;


-- Select the title and duration from longest to shortest film
SELECT title, duration
FROM films
ORDER BY duration DESC;


##sorting multiple fields

-- Select the release year, duration, and title sorted by release year and duration
SELECT release_year, duration, title
FROM films
ORDER BY release_year, duration;

-- Select the certification, release year, and title sorted by certification and release year
SELECT certification, release_year, title
FROM films
ORDER BY certification, release_year ASC;


#grouping data

SELECT certification, COUNT(title) AS title_count
FROM films
GROUP BY certification;

#GROUP BY multiple fields
SELECT certification, language, COUNT(title) AS title_count
FROM films
GROUP BY certification, language;


#GROUP BY with ORDER BY
SELECT certification, language, COUNT(title) AS title_count
FROM films
GROUP BY certification
ORDER BY title_count DESC;


## GROUP BY single fields

-- Find the release_year and film_count of each year
SELECT release_year, COUNT(*) AS film_count
FROM films
GROUP BY release_year; 


-- Find the release_year and average duration of films for each year
SELECT release_year, AVG(duration) AS avg_duration
FROM films
GROUP BY release_year;


## GROUP BY multiple fields

-- Find the release_year, country, and max_budget, then group and order by release_year and country
SELECT release_year, country, MAX(budget) AS max_budget
FROM films
GROUP BY release_year, country
ORDER BY release_year, country;


#counting different languages spoken by year
SELECT release_year, COUNT(DISTINCT language) AS dist_lang
FROM films
GROUP BY release_year
ORDER BY dist_lang DESC;


### Filtering grouped data

#HAVING
SELECT release_year, COUNT(title) AS title_count
FROM films
GROUP BY release_year
HAVING COUNT(title)>10;


# HAVING vs WHERE
	#WHERE filters individual record, HAVING filters grouped records


#in what years was the avg film duration higher than 2 hours
SELECT release_year
FROM films
GROUP BY release_year
HAVING AVG(duration) > 120;



## Filter with HAVING

-- Select the country and distinct count of certification as certification_count
SELECT country, COUNT(DISTINCT certification) AS certification_count
FROM  films
-- Group by country
GROUP BY country
-- Filter results to countries with more than 10 different certifications
HAVING COUNT(DISTINCT certification)>10;


## HAVING and sorting
-- Select the country and average_budget from films
SELECT country, ROUND(AVG(budget),2) AS average_budget
FROM films
-- Group by country
GROUP BY country
-- Filter to countries with an average_budget of more than one billion
HAVING AVG(budget)>1000000000
-- Order by descending order of the aggregated budget
ORDER BY average_budget DESC;


## All together now

-- Modify the query to also list the average budget and average gross
SELECT release_year, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM films
WHERE release_year > 1990
GROUP BY release_year;

SELECT release_year, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM films
WHERE release_year > 1990
GROUP BY release_year
-- Modify the query to see only years with an avg_budget of more than 60 million
HAVING AVG(budget)>60000000;

SELECT release_year, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM films
WHERE release_year > 1990
GROUP BY release_year
HAVING AVG(budget) > 60000000
-- Order the results from highest to lowest average gross and limit to one
ORDER BY avg_gross DESC
LIMIT 1;


######### SQL for joining data

### Introduction to INNER JOIN

#presidents table
SELECT *
FROM presidents;

#INNER JOIN in SQL
SELECT p1.country, p1.content,
	prime_minister, president
FROM prime_ministers AS p1
INNER JOIN presidents AS p2
ON p1.country = p2.country;


## Inner join

SELECT * 
FROM cities
  -- Inner join to countries
  INNER JOIN countries
    -- Match on the country codes
    ON cities.country_code = countries.code;



-- Select name fields (with alias) and region 
SELECT cities.name AS city , countries.name AS country, region
FROM cities
  INNER JOIN countries
    ON cities.country_code = countries.code;


## Inner join (2)

-- Select fields with aliases
SELECT c.code AS country_code, name, year, inflation_rate
FROM countries AS c
  -- Join to economies (alias e)
  INNER JOIN economies AS e
    -- Match on code
    ON c.code = e.code;


## Inner join (3)

-- Select fields
SELECT c.code, c.name, c.region, p.year, p.fertility_rate
  -- From countries (alias as c)
  FROM countries AS c
  -- Join with populations (as p)
  INNER JOIN populations AS p
    -- Match on country code
    ON c.code = p.country_code;


-- Select fields
SELECT c.code, name, region, e.year, fertility_rate, unemployment_rate
  -- From countries (alias as c)
  FROM countries AS c
  -- Join to populations (as p)
  INNER JOIN populations AS p
    -- Match on country code
    ON c.code = p.country_code
  -- Join to economies (as e)
  INNER JOIN economies AS e
    -- Match on country code
    ON c.code = e.code;


-- Select fields
SELECT c.code, name, region, e.year, fertility_rate, unemployment_rate
  -- From countries (alias as c)
  FROM countries AS c
  -- Join to populations (as p)
  INNER JOIN populations AS p
    -- Match on country code
    ON c.code = p.country_code
  -- Join to economies (as e)
  INNER JOIN economies AS e
    -- Match on country code and year
    ON c.code = e.code AND p.year = e.year;


### INNER JOIN via USING

SELECT left_table.id AS L_id
	left_table.val AS L_val
	right_table.val AS R_val
FROM left_table
INNER JOIN right_table
USING (id);


-- Select fields
SELECT c.name AS country, continent, l.name AS language, official
  -- From countries (alias as c)
  FROM countries AS c
  -- Join to languages (as l)
  INNER JOIN languages AS l
    -- Match using code
    USING(code);


###Self-ish joins, just in CASE

SELECT name, continent, indep_year
	CASE WHEN indep_year < 1908 THEN 'before 1908'
		WHEN indep_year <= 1930 THEN 'between 1908 and 1930'
	ELSE 'after 1930' END
	SA indep_year_group
FROM states
ORDER BY indep_year_group;


## Self-join
-- Select fields with aliases
SELECT p1.country_code, p1.size AS size2010, p2.size AS size2015
-- From populations (alias as p1)
FROM populations AS p1
  -- Join to itself (alias as p2)
  INNER JOIN populations AS p2
    -- Match on country code
    ON p1.country_code=p2.country_code;		


-- Select fields with aliases
SELECT p1.country_code,
       p1.size AS size2010,
       p2.size AS size2015
-- From populations (alias as p1)
FROM populations AS p1
  -- Join to itself (alias as p2)
  INNER JOIN populations AS p2
    -- Match on country code
    ON p1.country_code = p2.country_code
        -- and year (with calculation)
        AND p1.year = p2.year - 5;



-- Select fields with aliases
SELECT p1.country_code,
       p1.size AS size2010, 
       p2.size AS size2015,
       -- Calculate growth_perc
       ((p2.size - p1.size)/p1.size * 100.0) AS growth_perc
-- From populations (alias as p1)
FROM populations AS p1
  -- Join to itself (alias as p2)
  INNER JOIN populations AS p2
    -- Match on country code
    ON p1.country_code = p2.country_code
        -- and year (with calculation)
        AND p1.year = p2.year - 5;


## Case when and then

SELECT name, continent, code, surface_area,
    -- First case
    CASE WHEN surface_area > 2000000 THEN 'large'
        -- Second case
        WHEN surface_area > 350000 AND surface_area<2000000  THEN 'medium'
        -- Else clause + end
        ELSE 'small' END
        -- Alias name
        AS geosize_group
-- From table
FROM countries;


## Inner challenge


SELECT country_code, size,
    -- First case
    CASE WHEN size > 50000000 THEN 'large'
        -- Second case
        WHEN size > 1000000 AND size < 50000000 THEN 'medium'
        -- Else clause + end
        ELSE 'small' END
        -- Alias name (popsize_group)
        AS popsize_group
-- From table
FROM populations
-- Focus on 2015
WHERE year = 2015;


SELECT country_code, size,
    CASE WHEN size > 50000000 THEN 'large'
        WHEN size > 1000000 THEN 'medium'
        ELSE 'small' END
        AS popsize_group
-- Into table
INTO pop_plus
FROM populations
WHERE year = 2015;

-- Select all columns of pop_plus
SELECT *
FROM pop_plus;



SELECT country_code, size,
  CASE WHEN size > 50000000
            THEN 'large'
       WHEN size > 1000000
            THEN 'medium'
       ELSE 'small' END
       AS popsize_group
INTO pop_plus       
FROM populations
WHERE year = 2015;

-- Select fields
SELECT name, continent, geosize_group, popsize_group
-- From countries_plus (alias as c)
FROM countries_plus AS c
  -- Join to pop_plus (alias as p)
  INNER JOIN pop_plus AS p
    -- Match on country code
    ON c.code = p.country_code
-- Order the table    
ORDER BY geosize_group;



### LEFT and RIGHT JOINs


#inner join just keep the records that are in BOTH tables; left join keeps all the records in the left table

SELECT p1.country, prime_minister, president
FROM prime_ministers AS p1
LEFT JOIN presidents AS p2
ON p1.country = p2.country;


## Left join

-- Select the city name (with alias), the country code,
-- the country name (with alias), the region,
-- and the city proper population
SELECT c1.name AS city, code, c2.name AS country,
       region, city_proper_pop
-- From left table (with alias)
FROM cities AS c1
  -- Join to right table (with alias)
  INNER JOIN countries AS c2
    -- Match on country code?
    ON c1.country_code = c2.code
-- Order based on descending country code
ORDER BY code DESC;


SELECT c1.name AS city, code, c2.name AS country,
       region, city_proper_pop
FROM cities AS c1
  -- Join right table (with alias)
  LEFT JOIN countries AS c2
    -- Match on country code
    ON c1.country_code = c2.code
-- Order by descending country code
ORDER BY code DESC;



## Left join(2)

/*
Select country name AS country, the country's local name,
the language name AS language, and
the percent of the language spoken in the country
*/
SELECT c.name AS country, local_name, l.name AS language, percent
-- From left table (alias as c)
FROM countries AS c
  -- Join to right table (alias as l)
  INNER JOIN languages AS l
    -- Match on fields
    ON c.code = l.code
-- Order by descending country
ORDER BY country DESC;



/*
Select country name AS country, the country's local name,
the language name AS language, and
the percent of the language spoken in the country
*/
SELECT c.name AS country, local_name, l.name AS language, percent
-- From left table (alias as c)
FROM countries AS c
  -- Join to right table (alias as l)
  LEFT  JOIN languages AS l
    -- Match on fields
    ON c.code = l.code
-- Order by descending country
ORDER BY c.name DESC;


## Left join(3)

-- Select name, region, and gdp_percapita
SELECT name, region, gdp_percapita
-- From countries (alias as c)
FROM countries AS c
  -- Left join with economies (alias as e)
  LEFT JOIN economies AS e
    -- Match on code fields
    ON c.code = e.code
-- Focus on 2010
WHERE year = 2010;

-- Select fields
SELECT region, AVG(gdp_percapita) AS avg_gdp
-- From countries (alias as c)
FROM countries AS c
  -- Left join with economies (alias as e)
  LEFT JOIN economies AS e
    -- Match on code fields
    ON c.code = e.code
-- Focus on 2010
WHERE year = 2010
-- Group by region
GROUP BY region;


-- Select fields
SELECT region, AVG(gdp_percapita) AS avg_gdp
-- From countries (alias as c)
FROM countries AS c
  -- Left join with economies (alias as e)
  LEFT JOIN economies AS e
    -- Match on code fields
    ON c.code=e.code
-- Focus on 2010
WHERE year = 2010
-- Group by region
GROUP BY region
-- Order by descending avg_gdp
ORDER BY avg_gdp DESC;


## Right join

-- convert this code to use RIGHT JOINs instead of LEFT JOINs
/*
SELECT cities.name AS city, urbanarea_pop, countries.name AS country,
       indep_year, languages.name AS language, percent
FROM cities
  LEFT JOIN countries
    ON cities.country_code = countries.code
  LEFT JOIN languages
    ON countries.code = languages.code
ORDER BY city, language;
*/

SELECT cities.name AS city, urbanarea_pop, countries.name AS country,
       indep_year, languages.name AS language, percent
FROM languages
  RIGHT JOIN countries
    ON countries.code = languages.code
  RIGHT JOIN cities
    ON cities.country_code = countries.code
ORDER BY city, language;



### FULL JOINs

SELECT p1.country as pm_co, p2.country AS pres_co
	prime_minister, president
FROM prime_ministers AS p1
FULL JOIN presidents AS p2
ON p1.country = p2.country;


## full join

SELECT name AS country, code, region, basic_unit
-- From countries
FROM countries
  -- Join to currencies
  FULL JOIN currencies
    -- Match on code
    USING (code)
-- Where region is North America or null
WHERE region = 'North America' OR region IS NULL
-- Order by region
ORDER BY region;

SELECT name AS country, code, region, basic_unit
-- From countries
FROM countries
  -- Join to currencies
  LEFT JOIN currencies
    -- Match on code
    USING (code)
-- Where region is North America or null
WHERE region = 'North America' OR region IS NULL
-- Order by region
ORDER BY region;

SELECT name AS country, code, region, basic_unit
-- From countries
FROM countries
  -- Join to currencies
  INNER JOIN currencies
    -- Match on code
    USING (code)
-- Where region is North America or null
WHERE region = 'North America' OR region IS NULL
-- Order by region
ORDER BY region;


## Full join (2)

SELECT countries.name, code, languages.name AS language
-- From languages
FROM languages
  -- Join to countries
  FULL JOIN countries
    -- Match on code
    USING (code)
-- Where countries.name starts with V or is null
WHERE countries.name LIKE 'V%' OR countries.name IS NULL
-- Order by ascending countries.name
ORDER BY countries.name;


SELECT countries.name, code, languages.name AS language
-- From languages
FROM languages
  -- Join to countries
  INNER JOIN countries
    -- Match using code
    USING (code)
-- Where countries.name starts with V or is null
WHERE countries.name LIKE 'V%' OR countries.name IS NULL
-- Order by ascending countries.name
ORDER BY countries.name;



## Full join (3)

-- Select fields (with aliases)
SELECT c1.name AS country, region, l.name AS language,
       basic_unit, frac_unit
-- From countries (alias as c1)
FROM countries AS c1
  -- Join with languages (alias as l)
  FULL JOIN languages AS l
    -- Match on code
    USING (code)
  -- Join with currencies (alias as c2)
  FULL JOIN currencies AS c2
    -- Match on code
    USING (code)
-- Where region like Melanesia and Micronesia
WHERE region LIKE 'M%esia';


###CROSSing the rubicon

SELECT prime_minister, president
FROM prime_ministers AS p1
CROSS JOIN presidents AS p2
WHERE p1.continent IN ("North America", "Oceania");


##A table of two cities
-- Select fields
SELECT c.name AS city, l.name AS language
-- From cities (alias as c)
FROM cities AS c        
  -- Join to languages (alias as l)
  CROSS JOIN languages AS l
-- Where c.name like Hyderabad
WHERE c.name LIKE 'Hyder%';



-- Select fields
SELECT c.name AS city, l.name AS language
-- From cities (alias as c)
FROM cities AS c        
  -- Join to languages (alias as l)
  INNER JOIN languages AS l
    -- Match on country code
    ON c.country_code = l.code
-- Where c.name like Hyderabad
WHERE c.name LIKE 'Hyder%';


##Outer challenge

-- Select fields
SELECT c.name AS country,
       region,
       life_expectancy AS life_exp
-- From countries (alias as c)
FROM countries AS c
  -- Join to populations (alias as p)
  LEFT JOIN populations AS p
    -- Match on country code
    ON c.code = p.country_code
-- Focus on 2010
WHERE year = 2010
-- Order by life_exp
ORDER BY life_exp
-- Limit to 5 records
LIMIT 5;


### State of the UNION

SELECT prime_minister AS leader, country
FROM prime_ministers
UNION
SELECT monarch, country
FROM monarchs
ORDER BY country;


-- Select fields from 2010 table
SELECT *
  -- From 2010 table
  FROM economies2010
	-- Set theory clause
	UNION
-- Select fields from 2015 table
SELECT *
  -- From 2015 table
  FROM economies2015
-- Order by code and year
ORDER BY code, year;


-- Select field
SELECT country_code
  -- From cities
  FROM cities
	-- Set theory clause
	UNION
-- Select field
SELECT code
  -- From currencies
  FROM currencies
-- Order by country_code
ORDER BY country_code;


##Union all
#To include duplicates, you can use UNION ALL

-- Select fields
SELECT code, year
  -- From economies
  FROM economies
	-- Set theory clause
	UNION ALL
-- Select fields
SELECT country_code, year
  -- From populations
  FROM populations
-- Order by code, year
ORDER BY code, year;


###INTERSECTional data science

SELECT id
FROM left_one
INTERSECT
SELECT id
FROM right_one;


SELECT country
FROM prime_ministers
INTERSECT
SELECT country
FROM presidents;


#INTERSECT on two fields
SELECT country, prime_minister AS leader
FROM prime_ministers
INTERSECT
SELECT country, president
FROM presidents;


## Intersect

#UNION ALL will extract all records from two tables, while INTERSECT will only return records that both tables have in common
-- Select fields
SELECT code, year
  -- From economies
  FROM economies
	-- Set theory clause
	INTERSECT
-- Select fields
SELECT country_code, year
  -- From populations
  FROM populations
-- Order by code and year
ORDER BY code, year;


## Intersect(2)
-- Select fields
SELECT name
  -- From countries
  FROM countries
	-- Set theory clause
	INTERSECT
-- Select fields
SELECT name
  -- From cities
  FROM cities;



### EXCEPTional

#monarchs that aren't prime ministers
SELECT monarch, country
FROM monarchs
EXCEPT
SELECT prime_minister, country
FROM prime_ministers;


## Except
#selecting cities that are no capitals
-- Select field
SELECT name
  -- From cities
  FROM cities
	-- Set theory clause
	EXCEPT
-- Select field
SELECT capital
  -- From countries
  FROM countries
-- Order by result
ORDER BY name ASC;


## Except (2)

-- Select field
SELECT capital
  -- From countries
  FROM countries
	-- Set theory clause
	EXCEPT
-- Select field
SELECT name
  -- From cities
  FROM cities
-- Order by ascending capital
ORDER BY capital ASC;


### Semi-joins and Anti-joins

#intro to subqueries
SELECT president, country, continent
FROM presidents
WHERE country IN
	(SELECT name 
	 FROM states
	 WHERE indep_year < 1800);

#anti-join
SELECT president, country, continent
FROM presidents
WHERE continent LIKE '%America'
     AND country NOT IN
	(SELECT name
	 FROM states
	 WHERE indep_year < 1800);	


## Semi-join

-- Select code
SELECT code
  -- From countries
  FROM countries
-- Where region is Middle East
WHERE region = 'Middle East';



-- Select field
SELECT DISTINCT(name)
  -- From languages
  FROM languages
-- Order by name
ORDER BY name ASC;


-- Query from step 2
SELECT DISTINCT name
  FROM languages
-- Where in statement
WHERE code IN
  -- Query from step 1
  -- Subquery
  (SELECT code
   FROM countries
   WHERE region = 'Middle East')
-- Order by name
ORDER BY name;



## Diagnosing problems using anti-join

-- Select statement
SELECT COUNT(*)
  -- From countries
  FROM countries
-- Where continent is Oceania
WHERE continent = 'Oceania';



-- Select fields (with aliases)
SELECT c1.code, name, basic_unit AS currency
  -- From countries (alias as c1)
  FROM countries AS c1
  	-- Join with currencies (alias as c2)
  	INNER JOIN currencies AS c2
    -- Match on code
    ON c1.code = c2.code
-- Where continent is Oceania
WHERE c1.continent = 'Oceania';


-- Select fields
SELECT code, name
  -- From Countries
  FROM countries
  -- Where continent is Oceania
  WHERE continent = 'Oceania'
  	-- And code not in
  	AND code NOT IN
  	-- Subquery
  	(SELECT code
  	 FROM currencies);


## Set theory challenge

-- Select the city name
SELECT name
  -- Alias the table where city name resides
  FROM cities AS c1
  -- Choose only records matching the result of multiple set theory clauses
  WHERE country_code IN
(
    -- Select appropriate field from economies AS e
    SELECT e.code
    FROM economies AS e
    -- Get all additional (unique) values of the field from currencies AS c2   
    UNION
    SELECT c2.code
    FROM currencies AS c2
    -- Exclude those appearing in populations AS p  
    EXCEPT
    SELECT p.country_code
    FROM populations AS p
);



### Subqueries inside WHERE and SELECT clauses

#Asian countries below average 'fert_rate'
SELECT name, fert_rate
FROM states
WHERE continent = 'Asia'
    AND fert_rate <
	(SELECT AVG(fert_rate)
	 FROM states);


#subqueries inside SELECT clauses-setup
SELECT DISTINCT continent
FROM prime_ministers;


#subquery inside SELECT clause - complete
SELECT DISTINCT continent,
	(SELECT COUNT(*)
	 FROM states
	 WHERE prime_ministers.continent=states.continent) AS countries_num
FROM prime_ministers;


## subquery inside where

-- Select average life_expectancy
SELECT AVG(life_expectancy)
  -- From populations
  FROM populations
-- Where year is 2015
WHERE year = 2015;


-- Select fields
SELECT *
  -- From populations
  FROM populations
-- Where life_expectancy is greater than
WHERE life_expectancy >
  -- 1.15 * subquery
  1.15 * (SELECT AVG(life_expectancy)
   FROM populations
   WHERE year = 2015) AND
  year = 2015;


## Subquery inside where (2)

-- Select fields
SELECT name, country_code, urbanarea_pop
  -- From cities
  FROM cities
-- Where city name in the field of capital cities
WHERE name IN
  -- Subquery
  (SELECT capital
   FROM countries)
ORDER BY urbanarea_pop DESC;


## Subquery inside select

SELECT countries.name AS country, COUNT(*) AS cities_num
  FROM cities
    INNER JOIN countries
    ON countries.code = cities.country_code
GROUP BY country
ORDER BY cities_num DESC, country
LIMIT 9;

SELECT countries.name AS country,
  -- Subquery
  (SELECT COUNT(*)
   FROM cities
   WHERE countries.code = cities.country_code) AS cities_num
FROM countries
ORDER BY cities_num DESC, country
LIMIT 9;


## Subquery inside FROM clause

SELECT DISTINCT monarchs.continent, subquery.max_perc
FROM monarchs,
    (SELECT continent, MAX(women_parli_pec) AS max_perc
     FROM states
     GROUP BY continent) AS subquery
WHERE monarchs.continent = subquery.continent
ORDER BY continent;


## Subquery inside from

-- Select fields (with aliases): determining for each country code how many languages are listed
SELECT code, COUNT(*) AS lang_num 
  -- From languages
  FROM languages
-- Group by code
GROUP BY code;


SELECT local_name, subquery.lang_num
  FROM countries,
  	(SELECT code, COUNT(*) AS lang_num
  	 FROM languages
  	 GROUP BY code) AS subquery
  WHERE countries.code = subquery.code
ORDER BY lang_num DESC;


-- Select fields
SELECT name, continent, inflation_rate
  -- From countries
  FROM countries
  	-- Join to economies
  	INNER JOIN economies
    -- Match on code
    USING (code)
-- Where year is 2015
WHERE year = 2015;



-- Select the maximum inflation rate as max_inf
SELECT MAX(inflation_rate) AS max_inf
  -- Subquery using FROM (alias as subquery)
  FROM (
      SELECT name, continent, inflation_rate
      FROM countries
      INNER JOIN economies
      USING (code)
      WHERE year = 2015) AS subquery
-- Group by continent
GROUP BY continent;


-- Select fields
SELECT name, continent, inflation_rate
  -- From countries
  FROM countries
	-- Join to economies
	INNER JOIN economies
	-- Match on code
	ON countries.code = economies.code
  -- Where year is 2015
  WHERE year = 2015
    -- And inflation rate in subquery (alias as subquery)
    AND inflation_rate IN (
        SELECT MAX(inflation_rate) AS max_inf
        FROM (
             SELECT name, continent, inflation_rate
             FROM countries
             INNER JOIN economies
             ON countries.code = economies.code
             WHERE year = 2015) AS subquery
      -- Group by continent
        GROUP BY continent);



-- Select fields
SELECT code, inflation_rate, unemployment_rate
  -- From economies
  FROM economies
  -- Where year is 2015 and code is not in
  WHERE year = 2015 AND code NOT IN
  	-- Subquery
  	(SELECT code
  	 FROM countries
  	 WHERE (gov_form = 'Constitutional Monarchy' OR gov_form LIKE '%Republic%'))
-- Order by inflation rate
ORDER BY inflation_rate;



-- Select fields
SELECT DISTINCT name, total_investment, imports
  -- From table (with alias)
  FROM countries AS c
    -- Join with table (with alias)
    LEFT JOIN economies AS e
      -- Match on code
      ON (c.code = e.code
        -- and code in Subquery
        AND c.code IN (
          SELECT l.code
          FROM languages AS l
          WHERE official = 'true'
        ) )
  -- Where region and year are correct
  WHERE region = 'Central America' AND year = 2015
-- Order by field
ORDER BY name;



-- Select fields
SELECT region, continent, AVG(fertility_rate) AS avg_fert_rate
  -- From left table
  FROM countries AS c
    -- Join to right table
    INNER JOIN populations AS p
      -- Match on join condition
      ON c.code = p.country_code
  -- Where specific records matching some condition
  WHERE year = 2015
-- Group appropriately?
GROUP BY region, continent
-- Order appropriately
ORDER BY avg_fert_rate;


-- Select fields
SELECT name, country_code, city_proper_pop, metroarea_pop,
	  -- Calculate city_perc
      city_proper_pop / metroarea_pop * 100 AS city_perc
  -- From appropriate table    
  FROM cities
  -- Where
  WHERE name IN
    -- Subquery
    (SELECT capital
     FROM countries
     WHERE (continent = 'Europe'
        OR continent LIKE '%America'))
       AND metroarea_pop IS NOT NULL
-- Order appropriately
ORDER BY city_perc DESC
-- Limit amount
LIMIT 10;



###############
############################
############### JOINING DATA IN SQL
############################
###############

#INNER JOIN: looks for records in BOTH tables
SELECT *
FROM presidents;

--Inner join of presidents and pprime ministers, joining on country
SELECT prime_ministers.country, prime_ministers.continent, prime_minister, president
FROM prime_ministers
INNER JOIN presidents
ON prime_ministers.country = presidents.country;

#Aliasing tables
SELECT p1.country, p1.continent, prime_minister, president
FROM prime_ministers AS p1
INNER JOIN presidents AS p2
ON p1.country = p2.country;


# using USING
SELECT p1.country, p1.continent, prime_minister, president
FROM prime_ministers AS p1
INNER JOIN presidents AS p2
USING(country);


### your first join
-- Select all columns from cities
SELECT *
FROM cities;

SELECT * 
FROM cities
-- Inner join to countries
INNER JOIN countries
-- Match on country codes
ON cities.country_code = countries.code;


-- Select name fields (with alias) and region 
SELECT cities.name AS city, countries.name AS country, countries.region
FROM cities
INNER JOIN countries
ON cities.country_code = countries.code;


-- Select fields with aliases
SELECT c.code AS country_code, c.name, e.year,e.inflation_rate
FROM countries AS c
-- Join to economies (alias e)
INNER JOIN economies AS e
-- Match on code field using table aliases
ON c.code = e.code;


## Using in action
SELECT c.name AS country, l.name AS language, official
FROM countries AS c
INNER JOIN languages AS l
-- Match using the code column
USING(code);


### Defining relationship

#one-to-many relationships
#one-to-one relationship
#many-to-many relationship


## inspecting a relationship

-- Select country and language names, aliased
SELECT c.name AS country, l.name AS language
-- From countries (aliased)
FROM countries AS c 
-- Join to languages (aliased)
INNER JOIN languages AS l
-- Use code as the joining field with the USING keyword
using(code);


-- Rearrange SELECT statement, keeping aliases
SELECT c.name AS country, l.name AS language
FROM countries AS c
INNER JOIN languages AS l
USING(code)
-- Order the results by language
ORDER BY language;


### Multiple joins

#joins on joins
SELECT *
FROM left_table 
INNER JOIN right_table
ON left_table.id = right_table.id
INNER JOIN another_table
ON left_table.id = another_table.id;

#what to join first
SELECT p1.country, p1.continent, president, prime_minister
FROM prime_ministers AS p1
INNER JOIN presidents AS p2
USING(country);


#chaining joins
SELECT p1.country, p1.continent, president, prime_minister, pm_start
FROM prime_ministers AS p1
INNER JOIN presidents AS p2
USING(country)
INNER JOIN prime_minister_terms AS p3
using(prime_minister);


#joining on multiple keys
SELECT *
FROM left_table
INNER JOIN right_table
ON left_table.id = right_table.id
	AND left_table.date = right_table.date;

#joining multiple tables
-- Select relevant fields
SELECT name, year, fertility_rate
-- Inner join countries and populations, aliased, on code
FROM countries AS c
INNER JOIN populations AS p
ON c.code = p.country_code;

-- Select fields
SELECT name,  fertility_rate, e.year, e.unemployment_rate
FROM countries AS c
INNER JOIN populations AS p
ON c.code = p.country_code
-- Join to economies (as e)
INNER JOIN economies as e 
-- Match on country code
USING(code);


## checking multi-table joins
SELECT name, e.year, fertility_rate, unemployment_rate
FROM countries AS c
INNER JOIN populations AS p
ON c.code = p.country_code
INNER JOIN economies AS e
ON c.code = e.code
-- Add an additional joining condition such that you are also joining on year
	AND p.year = e.year;


### Left and right joins
#left join will fill with null the observations that do not contain values for a variables of the "right" table

#LEFT JOIN syntax
SELECT p1.country, prime_minister, president
FROM prime_ministers AS p1
LEFT JOIN presidents AS p2
USING(country);


#RIGHT JOIN syntax
SELECT *
FROM left_table
RIGHT JOIN right_table
ON left_table.id = right_table.id;


#RIGHT JOIN with presidents and prime ministers
SELECT p1.country, prime_minister, president
FROM prime_ministers AS p1
RIGHT JOIN presidents AS p2
USING(country);


## this is a LEFT JOIN, right?
SELECT 
    c1.name AS city,
    code,
    c2.name AS country,
    region,
    city_proper_pop
FROM cities AS c1
-- Perform an inner join with cities as c1 and countries as c2 on country code
INNER JOIN countries AS c2 
ON c1.country_code=c2.code
ORDER BY code DESC;


SELECT 
	c1.name AS city, 
    code, 
    c2.name AS country,
    region, 
    city_proper_pop
FROM cities AS c1
-- Join right table (with alias)
LEFT JOIN countries AS c2
ON c1.country_code = c2.code
ORDER BY code DESC;


## Building on your LEFT JOIN
SELECT name, region, gdp_percapita
FROM countries AS c
LEFT JOIN economies AS e
-- Match on code fields
ON c.code = e.code
-- Filter for the year 2010
WHERE year=2010;

-- Select region, and average gdp_percapita as avg_gdp
SELECT AVG(gdp_percapita) AS avg_gdp, region
FROM countries AS c
LEFT JOIN economies AS e
USING(code)
WHERE year = 2010
-- Group by region
GROUP BY region;


SELECT region, AVG(gdp_percapita) AS avg_gdp
FROM countries AS c
LEFT JOIN economies AS e
USING(code)
WHERE year = 2010
GROUP BY region
-- Order by descending avg_gdp
ORDER BY avg_gdp DESC
-- Return only first 10 records
LIMIT 10;


## Is this RIGHT?
-- Modify this query to use RIGHT JOIN instead of LEFT JOIN
SELECT countries.name AS country, languages.name AS language, percent
FROM languages
RIGHT JOIN countries
USING(code)
ORDER BY language;


### FULL JOINs (combines a LEFT and RIGHT join)

#FULL JOIN syntax
SELECT left_table.id AS L_id, right_table.id AS R_id, 	left_table.val AS L_val, right_table.val AS R_val
FROM left_table
FULL JOIN right_table
USING (id);

#example
SELECT p1.country AS country, prime_minister,president
FROM prime_ministers AS p1
FULL JOIN presidents AS p2
ON p1.country = p2.country
LIMIT 10;


## comparing joins
SELECT name AS country, code, region, basic_unit
FROM countries
-- Join to currencies
FULL JOIN currencies
USING (code)
-- Where region is North America or null
WHERE region = 'North America'
	OR name IS NULL
ORDER BY region;

SELECT name AS country, code, region, basic_unit
FROM countries
-- Join to currencies
LEFT JOIN currencies 
USING (code)
WHERE region = 'North America' 
	OR name IS NULL
ORDER BY region;

SELECT name AS country, code, region, basic_unit
FROM countries
-- Join to currencies
INNER JOIN currencies 
USING (code)
WHERE region = 'North America' 
	OR name IS NULL
ORDER BY region;


## Chaining FULL JOINs

SELECT 
	c1.name AS country, 
    region, 
    l.name AS language,
	basic_unit, 
    frac_unit
FROM countries as c1 
-- Full join with languages (alias as l)
FULL JOIN languages AS l  
USING(code)
-- Full join with currencies (alias as c2)
FULL JOIN currencies AS c2
USING(code)
WHERE region LIKE 'M%esia';


### Crossing into CROSS JOIN

#CROSS JOIN creates all possible combinations of two tables
#CROSS JOIN syntax
SELECT id1, id2
FROM table1
CROSS JOIN table2;

SELECT prime_minister, president
FROM prime_ministers AS p1
CROSS JOIN presidents AS p2
WHERE p1.continent IN ('Asia')
    AND p2.continent IN ('South America');


##Histories and languages
SELECT c.name AS country, l.name AS language
-- Inner join countries as c with languages as l on code
FROM countries as c
INNER JOIN languages as l 
USING(code)
WHERE c.code IN ('PAK','IND')
	AND l.code in ('PAK','IND');


SELECT c.name AS country, l.name AS language
FROM countries AS c        
-- Perform a cross join to languages (alias as l)
CROSS JOIN languages as l 
WHERE c.code in ('PAK','IND')
	AND l.code in ('PAK','IND');


##Choosing your join
SELECT 
	c.name AS country,
    region,
    life_expectancy AS life_exp
FROM countries AS c
-- Join to populations (alias as p) using an appropriate join
INNER JOIN populations as p 
ON c.code = p.country_code
-- Filter for only results in the year 2010
WHERE p.year =2010
-- Sort by life_exp
ORDER BY life_exp
-- Limit to five records
LIMIT 5;



### Self joins (tables that join with themselves)
SELECT p1.country as country1, p2.country as country2, p1.continent
FROM prime_ministers as p1
INNER JOIN prime_ministers as p2
ON p1.continent = p2.continent
LIMIT 10;

SELECT p1.country as country1, p2.country as country2, p1.continent
FROM prime_ministers as p1
INNER JOIN prim_ministers as p2
ON p1.continent = p2.continent
   AND p1.country <> p2.country; #<> is not equal

-- Select aliased fields from populations as p1
SELECT 
	p1.country_code, 
    p1.size AS size2010, 
    p2.size AS size2015
-- Join populations as p1 to itself, alias as p2, on country code
FROM populations AS p1
INNER JOIN populations AS p2
USING(country_code);


SELECT 
	p1.country_code, 
    p1.size AS size2010, 
    p2.size AS size2015
FROM populations AS p1
INNER JOIN populations AS p2
ON p1.country_code = p2.country_code
WHERE p1.year = 2010
-- Filter such that p1.year is always five years before p2.year
    AND p1.year = p2.year - 5;


### Set theory for SQL joins

#UNION 
#UNION ALL :returns all records in both tables, including duplicates

#UNION syntax
SELECT *
FROM left_table
UNION
SELECT *
FROM right_table;

SELECT *
FROM left_table
UNION ALL
SELECT *
FROM right_table;


SELECT monarch as leader, country
FROM monarchs
UNION 
SELECT prime_minister, country
FROM prime_ministers
ORDER BY country, leader
LIMIT 10;


SELECT monarch as leader, country
FROM monarchs
UNION ALL
SELECT prime_minister, country
FROM prime_ministers
ORDER BY country, leader
LIMIT 10;


## Comparing global economies
-- Select all fields from economies2015
SELECT *
FROM economies2015
-- Set operation
UNION
-- Select all fields from economies2019
SELECT *
FROM economies2019
ORDER BY code, year;

## Comparing two set operations
-- Query that determines all pairs of code and year from economies and populations, without duplicates
SELECT code, year
FROM economies
UNION 
SELECT country_code, year
FROM populations
ORDER BY code, year;


SELECT code, year
FROM economies
-- Set theory clause
UNION ALL
SELECT country_code, year
FROM populations
ORDER BY code, year;


#INTERSECT syntax
SELECT id, val
FROM left_table
INTERSECT
SELECT id, val
FROM right_table;

#INNER JOIN returns duplicate values

SELECT country as intersect_country
FROM prime_ministers
INTERSECT 
SELECT country
FROM presidents;

#INTERSECT on two fields
SELECT country, prime_minister AS leader
FROM prime_ministers
INTERSECT 
SELECT country, president
FROM presidents;


-- Return all cities with the same name as a country
SELECT name
FROM countries
INTERSECT
SELECT name
FROM cities;


### EXCEPT
SELECT monarch, country
FROM monarchs
EXCEPT
SELECT prime_minister, country
FROM prime_ministers;

-- Return all cities that do not have the same name as a country
SELECT name 
FROM cities
EXCEPT
SELECT name
FROM countries
ORDER BY name;


### Subqurying with semi joins and anti joins

#additive joins 
SELECT *
FROM left_table 
INNER JOIN right_table
ON left_table.id = right_table.id;

#semi join: chooses records in the first table where a condition is met in the second table

#kicking off your semi join
SELECT country, continent, president
FROM presidents;

#building on your semi join
SELECT country
FROM states
WHERE indep_years <1800;


#finish the semi join (an intro to subqueries)
SELECT president, country, continent
FROM presidents
WHERE country IN 
    (SELECT country
     FROM states
     WHERE indep_year<1800);


#an anti join with the presidents
SELECT country, president
FROM presidents
WHERE continent LIKE '%America'
    AND country NOT IN
       (SELECT country
        FROM states
        WHERE indep_year < 1800);

## semi join
-- Select country code for countries in the Middle East
SELECT code
FROM countries
WHERE region = 'Middle East';

-- Select unique language names
SELECT DISTINCT(name)
FROM languages
-- Order by the name of the language
ORDER BY name;


SELECT DISTINCT name
FROM languages
-- Add syntax to use bracketed subquery below as a filter
WHERE code IN
    (SELECT code
    FROM countries
    WHERE region = 'Middle East')
ORDER BY name;


##Diagnosing problems using anti join

SELECT code, name
FROM countries
WHERE continent = 'Oceania'
-- Filter for countries not included in the bracketed subquery
  AND code NOT IN
    (SELECT code
    FROM currencies);

###Subqueries inside WHERE and SELECT 

#syntax for query using WHERE IN statement
SELECT *
FROM some_table 
WHERE some_numeric_field IN (4,8,12);

#syntax for querying inside WHERE
SELECT * 
FROM some_table
WHERE some_field IN
    (SELECT som_numeric_field
     FROM another_table
     WHERE field2 = some_condition);

#subsqueries inside SELECT
SELECT DISTINCT continent
FROM states;

#subqueries inside SELECT
SELECT DISTINT continent
     (SELECT COUNT(*)
      FROM monarchs
      WHERE states.continent = monarch.continent) AS monarch_count
FROM states;

