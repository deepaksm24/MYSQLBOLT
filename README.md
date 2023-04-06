# MYSQLBOLT

SQL Lesson 1: SELECT queries 101
Exercise 1 — Tasks
Find the title of each film 
Find the director of each film
Find the title and director of each film
Find the title and year of each film
Find all the information about each film

SELECT title FROM movies;

SELECT director FROM movies;

SELECT title,director FROM movies;

SELECT title,year FROM movies;

SELECT * FROM movies;



SQL Lesson 2: Queries with constraints (Pt. 1)
Exercise 2 — Tasks
Find the movie with a row id of 6
Find the movies released in the years between 2000 and 2010
Find the movies not released in the years between 2000 and 2010
Find the first 5 Pixar movies and their release year


SELECT * FROM movies where id=6

SELECT * FROM movies where year>=2000 and year<=2010

SELECT * FROM movies where year not between 2000 and 2010

SELECT * FROM movies where id between 1 and 5




SQL Lesson 3: Queries with constraints (Pt. 2)

Exercise 3 — Tasks
Find all the Toy Story movies
Find all the movies directed by John Lasseter
Find all the movies (and director) not directed by John Lasseter
Find all the WALL-* movies


SELECT * FROM movies where Title like "%Toy Story%"

SELECT * FROM movies where director like "%John lasseter%"


SELECT * FROM movies where director not like "%John lasseter%"


SELECT * FROM movies where title like "%Wall%"


SQL Lesson 4: Filtering and sorting Query results
Exercise 4 — Tasks
List all directors of Pixar movies (alphabetically), without duplicates
List the last four Pixar movies released (ordered from most recent to least)
List the first five Pixar movies sorted alphabetically
List the next five Pixar movies sorted alphabetically


SELECT distinct director FROM movies order by director asc


      SELECT * FROM movies ORDER BY year DESC LIMIT 4;



SELECT * FROM movies ORDER BY TITLE ASC LIMIT 5;


SELECT * FROM movies ORDER BY TITLE ASC LIMIT 5 OFFSET 5



SQL Review: Simple SELECT Queries
Review 1 — Tasks
List all the Canadian cities and their populations
Order all the cities in the United States by their latitude from north to south
List all the cities west of Chicago, ordered from west to east
List the two largest cities in Mexico (by population)
List the third and fourth largest cities (by population) in the United States and their population

SELECT * FROM north_american_cities WHERE country = "Canada"


SELECT * FROM north_american_cities WHERE country = "United States" ORDER BY latitude DESC



SELECT * FROM north_american_cities WHERE longitude < -87.629798
ORDER BY longitude


SELECT *
FROM north_american_cities
WHERE country = "Mexico"
ORDER BY population DESC
LIMIT 2




SELECT city
FROM north_american_cities
WHERE country = "United States"
ORDER BY population DESC
LIMIT 2 OFFSET 2;



SQL Lesson 6: Multi-table queries with JOINs

Exercise 6 — Tasks
Find the domestic and international sales for each movie ✓
Show the sales numbers for each movie that did better internationally rather than domestically
List all the movies by their ratings in descending order

SELECT title, domestic_sales, international_sales
FROM movies
INNER JOIN boxoffice
ON movies.id = boxoffice.movie_id



SELECT title, domestic_sales, international_sales
FROM movies
INNER JOIN boxoffice
 ON movies.id = boxoffice.movie_id
WHERE international_sales > domestic_sales;

SELECT title, rating
FROM movies
INNER JOIN boxoffice
ON movies.id = boxoffice.movie_id
ORDER BY rating DESC;


SQL Lesson 7: OUTER JOINs
Find the list of all buildings that have employees
Find the list of all buildings and their capacity
List all buildings and the distinct employee roles in each building (including empty buildings)
CT DISTINCT building FROM employees;


SELECT * FROM buildings


SELECT DISTINCT building_name, role
FROM buildings
LEFT JOIN employees
 ON building_name = employees.building;


SQL Lesson 8: A short note on NULLs
Find the name and role of all employees who have not been assigned to a building ✓
Find the names of the buildings that hold no employees

SELECT name, role FROM employees WHERE building IS NULL


SELECT DISTINCT building_name
FROM buildings
LEFT JOIN employees
    ON building_name = employees.building
WHERE employees.building IS NULL


SQL Lesson 9: Queries with expressions
Exercise 9 — Tasks
List all movies and their combined sales in millions of dollars ✓
List all movies and their ratings in percent
List all movies that were released on even number years

SELECT DISTINCT
    title,
    (domestic_sales + international_sales) / 1000000 AS sales
FROM movies
INNER JOIN boxoffice
    ON movies.id = boxoffice.movie_id;


SELECT DISTINCT
    title,
    (rating * 10) AS PERCENT
FROM movies
INNER JOIN boxoffice
    ON movies.id = boxoffice.movie_id;


    SELECT * FROM movies WHERE year % 2 = 0;

SQL Lesson 10: Queries with aggregates (Pt. 1)


RESET
Exercise 10 — Tasks
Find the longest time that an employee has been at the studio ✓
For each role, find the average number of years employed by employees in that role
Find the total number of employee years worked in each building

SELECT name, MAX(years_employed) FROM employees;

SELECT role, AVG(years_employed) as Avg
  FROM employees
  GROUP BY role;

SELECT building, SUM(years_employed) FROM employees GROUP BY building


Exercise 11 — Tasks
Find the number of Artists in the studio (without a HAVING clause) ✓
Find the number of Employees of each role in the studio
Find the total number of years employed by all Engineers
 SELECT COUNT(*) FROM employees WHERE role LIKE 'artist';



SELECT role, COUNT(name) FROM employees GROUP BY role;

  SELECT role, SUM(years_employed) FROM employees 
  GROUP BY role HAVING role LIKE 'engineer';

Exercise 12 — Tasks
Find the number of movies each director has directed ✓
Find the total domestic and international sales that can be attributed to each director



 SELECT director, COUNT(*) FROM movies GROUP BY director;



  SELECT director, SUM(domestic_sales) + SUM(international_sales) AS Total FROM movies 
 cross JOIN boxoffice ON movies.id = boxoffice.movie_id 
  GROUP BY director

