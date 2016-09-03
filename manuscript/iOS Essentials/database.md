Retrieve all the information from the cd.facilities table?
```sql
SELECT facid,name,membercost,guestcost,initialoutlay,monthlymaintenance 
FROM cd.facilities;
```

You want to print out a list of all of the facilities and their cost to members. How would you retrieve a list of only facility names and costs?
```sql
SELECT name, membercost 
FROM cd.facilities;
```

How can you produce a list of facilities that charge a fee to members?
```sql
SELECT * FROM cd.facilities 
WHERE membercost > 0;
```
How can you produce a list of facilities that charge a fee to members, and that fee is less than 1/50th of the monthly maintenance cost? Return the facid, facility name, member cost, and monthly maintenance of the facilities in question.
```sql
SELECT facid, name, membercost, monthlymaintenance 
FROM cd.facilities 
WHERE membercost > 0 AND membercost < monthlymaintenance/50;
```

How can you produce a list of all facilities with the word 'Tennis' in their name?
Hint: % character matches any string, and _ matches any single character. Postgres supports regular expressions with the ~ operator, so we can also use that but LIKE operator is much more portable between systems.
```sql
SELECT * FROM cd.facilities 
WHERE name LIKE '%Tennis%';
```

How can you retrieve the details of facilities with ID 1 and 5? You would typically use the OR operator like the following.
```sql
SELECT * FROM cd.facilities
WHERE facid = 1 OR facid = 5;
```
Try to do it without using the OR operator.
```sql
SELECT * FROM cd.facilities
WHERE facid in (1,5);
```
The obvious answer to this question is to use a WHERE clause that looks like where facid = 1 or facid = 2. An alternative that is easier with large numbers of possible matches is the IN operator. The IN operator takes a list of possible values, and matches them against (in this case) the facid. If one of the values matches, the where clause is true for that row, and the row is returned.

The IN operator is a good early demonstrator of the elegance of the relational model. The argument it takes is not just a list of values - it's actually a table with a single column. Since queries also return tables, if you create a query that returns a single column, you can feed those results into an IN operator. To give a toy example:
```sql
SELECT * FROM cd.facilities
WHERE facid in (SELECT facid FROM cd.facilities WHERE facid = 1 OR facid = 5);
```
This above example shows you how to feed the results of one query into another. The inner query is called a subquery.

How can you produce a list of facilities, with each labelled as 'cheap' or 'expensive' depending on if their monthly maintenance cost is more than $100? Return the name and monthly maintenance of the facilities in question.
```sql
SELECT name,
CASE WHEN monthlymaintenance > 100 THEN
'expensive'
ELSE
'cheap'
END as cost 
FROM cd.facilities;
```

Before seeing the above query see this one. This is will return all the names and all rows of the message column will be hello.
```sql
SELECT name,'hello' AS message FROM cd.facilities;
```
So we are using the above query with the CASE statement to achieve the result. The second new concept is the CASE statement itself. CASE is effectively like if/switch statements in other languages, with a form as shown in the query. To add a 'middling' option, we would simply insert another when...then section. Finally, there's the AS operator. This is simply used to label columns or expressions, to make them display more nicely or to make them easier to reference when used as part of a subquery.

How can you produce a list of members who joined after the start of September 2012? Return the memid, surname, firstname, and joindate of the members in question.
```sql
SELECT memid,surname,firstname,joindate 
FROM cd.members
WHERE joindate > '2012-09-01';
```
This is our first look at SQL timestamps. They're formatted in descending order of magnitude: YYYY-MM-DD HH:MM:SS.nnnnnn. We can compare them just like we might a unix timestamp, although getting the differences between dates is a little more involved (and powerful!). In this case, we've just specified the date portion of the timestamp. This gets automatically cast by postgres into the full timestamp 2012-09-01 00:00:00.

How can you produce an ordered list of the first 10 surnames in the members table? The list must not contain duplicates.
```sql
SELECT DISTINCT surname
FROM cd.members
ORDER by surname
LIMIT 10;
```
There's three new concepts here, but they're all pretty simple.

Specifying DISTINCT after SELECT removes duplicate rows from the result set. Note that this applies to rows: if row A has multiple columns, row B is only equal to it if the values in all columns are the same. As a general rule, don't use DISTINCT in a willy-nilly fashion - it's not free to remove duplicates from large query result sets, so do it as-needed.
Specifying ORDER BY (after the FROM and WHERE clauses, near the end of the query) allows results to be ordered by a column or set of columns (comma separated).
The LIMIT keyword allows you to limit the number of results retrieved. This is useful for getting results a page at a time, and can be combined with the OFFSET keyword to get following pages. This is the same approach used by MySQL and is very convenient - you may, unfortunately, find that this process is a little more complicated in other DBs.

```sql
SELECT surname 
FROM cd.members
UNION
SELECT name
FROM cd.facilities;        
```
The UNION operator does what you might expect: combines the results of two SQL queries into a single table. The caveat is that both results from the two queries must have the same number of columns and compatible data types.
UNION removes duplicate rows, while UNION ALL does not. Use UNION ALL by default, unless you care about duplicate results.

You'd like to get the signup date of your last member. How can you retrieve this information?
```sql
SELECT joindate
FROM cd.members
ORDER BY joindate DESC
LIMIT 1;
```
Or you can also write the following.
```sql
SELECT max(joindate) AS latest
FROM cd.members;
This is our first foray into SQL's aggregate functions. They're used to extract information about whole groups of rows, and allow us to easily ask questions like:

- What's the most expensive facility to maintain on a monthly basis?
- Who has recommended the most new members?
- How much time has each member spent at our facilities?

The MAX aggregate function here is very simple: it receives all the possible values for joindate, and outputs the one that's biggest. There's a lot more power to aggregate functions.
```
You'd like to get the first and last name of the last member(s) who signed up - not just the date. How can you do that?
You can order by joindate and apply limit but note that this approach does not cover the extremely unlikely eventuality of two people joining at the exact same time. So instead use the normal where clause with sub query.
```sql
SELECT firstname, surname, joindate
FROM cd.members
WHERE joindate = (SELECT MAX(joindate) FROM cd.members);
```
In the suggested approach above, you use a subquery to find out what the most recent joindate is. This subquery returns a scalar table - that is, a table with a single column and a single row. Since we have just a single value, we can substitute the subquery anywhere we might put a single constant value. In this case, we use it to complete the WHERE clause of a query to find a given member.

You might hope that you'd be able to do something like below:
```sql
SELECT firstname, surname, max(joindate)
FROM cd.members;
```
Unfortunately, this doesn't work. The MAX function doesn't restrict rows like the WHERE clause does - it simply takes in a bunch of values and returns the biggest one. The database is then left wondering how to pair up a long list of names with the single join date that's come out of the max function, and fails. Instead, you're left having to say 'find me the row(s) which have a join date that's the same as the maximum join date'.

Let  be the number of CITY entries in STATION, and let  be the number of distinct CITY names in STATION; query the value of  from STATION. In other words, find the difference between the total number of CITY entries in the table and the number of distinct CITY entries in the table.
```sql
SELECT COUNT(CITY) - COUNT(DISTINCT CITY) FROM STATION;
```

Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.
```sql
(SELECT CITY, LENGTH(CITY) AS CITY_LENGTH FROM STATION ORDER BY CITY_LENGTH LIMIT 1)
UNION
(SELECT CITY, LENGTH(CITY) AS CITY_LENGTH FROM STATION ORDER BY CITY_LENGTH DESC LIMIT 1);
```
Query the list of CITY names starting with vowels (a, e, i, o, u) from STATION. Your result cannot contain duplicates.
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE CITY LIKE 'a%' 
OR CITY LIKE 'e%' 
OR CITY LIKE 'i%' 
OR CITY LIKE 'o%' 
OR CITY LIKE 'u%';
```

Or you can also do the following. MySQL SUBSTR() returns the specified number of characters from a particular position of a given string. 
```sql
SELECT DISTINCT(CITY) 
FROM STATION 
WHERE SUBSTR(CITY,LENGTH(CITY),1) IN ('A','E','I','O','U');
```

MySQL RIGHT() extracts a specified number of characters from the right side of a string. So you can also do the following.
```sql
SELECT DISTINCT(CITY)
FROM STATION 
WHERE RIGHT(CITY,1) IN ('A','E','I','O','U');
```

A regular expression is a powerful way of specifying a pattern for a complex search.The $ symbol marks the end of the string. So it matches strings that end with a,e,i,o, or u. So you could also do.
```sql
SELECT DISTINCT(CITY) 
FROM STATION
WHERE CITY REGEXP '[A,E,I,O,U]$';
```

Query the list of CITY names from STATION which have vowels (i.e., a, e, i, o, and u) as both their first and last characters. Your result cannot contain duplicates.
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE RIGHT(CITY,1) IN ('A', 'E', 'I', 'O', 'U')
AND LEFT(CITY,1) IN ('A', 'E', 'I', 'O', 'U');
```
Or you could use the following.
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE SUBSTR(CITY,1,1) IN ('A','E','I','O','U')
AND SUBSTR(CITY,LENGTH(CITY),1) IN ('A','E','I','O','U');
```
Or you could use regular expressions like the following.
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE CITY REGEXP '^[aeiou]'
AND CITY REGEXP '[aeiou]$';
```
Infact since you we using regular expression we can combine the pattern. ^ represents the start and $ represents end. [aeiou] means we can have any one of them in this set. . represents any character, so we are saying it should start with a vowel followed by an character and then end with a vowel.
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE CITY REGEXP '^[aeiou].*[aeiou]$'
```

Query the list of CITY names from STATION that do not start with vowels. Your result cannot contain duplicates. Equating the regexp to zero is how we negate the regular expression.
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE CITY REGEXP '^[aeiou]' = '0';
```
Or instead of regular expression if you are using the left or substr solution then you use the not in.
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE SUBSTR(CITY,1,1) NOT IN ('A','E','I','O','U');
```

Query the list of CITY names from STATION that do not end with vowels. Your result cannot contain duplicates.
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE SUBSTR(CITY,-1,1) NOT IN ('A','E','I','O','U');
```
Or you could do the following.
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE CITY REGEXP '[aeiou]$' = '0';
```

Query the list of CITY names from STATION that either do not start with vowels or do not end with vowels. Your result cannot contain duplicates.
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE CITY REGEXP '^[^aeiou]'
OR CITY REGEXP '[^aeiou]$';
```
Or you could do the following.
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE SUBSTR(CITY,1,1) NOT IN ('A','E','I','O','U')
OR SUBSTR(CITY,-1,1) NOT IN ('A','E','I','O','U');
```

Query the list of CITY names from STATION that do not start with vowels and do not end with vowels. Your result cannot contain duplicates.
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE SUBSTR(CITY,1,1) NOT IN ('A','E','I','O','U')
AND SUBSTR(CITY,-1,1) NOT IN ('A','E','I','O','U');
```
Or you could do the following.
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE CITY REGEXP '^[^aeiou].*[^aeiou]$';
```

Query the Name of any student in STUDENTS who scored higher than  Marks. Order your output by the last three characters of each name. If two or more students both have names ending in the same last three characters (i.e.: Bobby, Robby, etc.), secondary sort them by ascending ID.
```sql
SELECT DISTINCT Name
FROM STUDENTS
WHERE Marks > 75
ORDER BY SUBSTR(Name,-3,3) ASC, ID ASC;
```
Or you could also run the following.
```sql
SELECT DISTINCT Name
FROM STUDENTS
WHERE Marks > 75
ORDER BY RIGHT(Name,3) ASC, ID ASC;
```

Write a query that prints a list of employee names (i.e.: the name attribute) for employees in Employee having a salary greater than 2000 per month who have been employees for less than  months. Sort your result by ascending employee_id.
```sql
SELECT name
FROM Employee
WHERE salary > 2000 AND months < 10
ORDER BY employee_id ASC;
```

Write a query identifying the type of each record in the TRIANGLES table using its three side lengths. Output one of the following statements for each record in the table:

- Not A Triangle: The given values of A, B, and C don't form a triangle.
- Equilateral: It's a triangle with  sides of equal length.
- Isosceles: It's a triangle with  sides of equal length.
- Scalene: It's a triangle with  sides of differing lengths.

```sql
SELECT CASE WHEN (a+b <= c OR b+c <= a OR a+c <= b) THEN
'Not A Triangle'
WHEN (a = b AND b = c) THEN
'Equilateral'
WHEN (a = b OR b = c OR a = c) THEN
'Isosceles '
ELSE
'Scalene'
END FROM TRIANGLES;
```

Generate the following two result sets:

1.Query an alphabetically ordered list of all names in OCCUPATIONS, immediately followed by the first letter of each profession as a parenthetical (i.e.: enclosed in parentheses). For example: AnActorName(A), ADoctorName(D), AProfessorName(P), and ASingerName(S).
1. Query the number of occurrences of each occupation in OCCUPATIONS. Sort the occurrences in ascending order, and output them in the following format: 

There are total occupation_count occupations.
where occupationcount is the number of occurrences of an occupation in OCCUPATIONS and [occupation] is the lowercase occupation name. If more than one Occupation has the same occupationcount, they should be ordered alphabetically.

Note: There will be at least two entries in the table for each type of occupation.
```sql
SELECT CONCAT(name,'(',LEFT(occupation,1),')')
FROM OCCUPATIONS
ORDER BY name ASC;

SELECT CONCAT('There are total ',COUNT(occupation),' ',LOWER(occupation),'s.')
FROM OCCUPATIONS
GROUP BY occupation
ORDER BY COUNT(occupation) ASC, occupation ASC;
```

Query a count of the number of cities in CITY with populations larger than 100,000.
```sql
SELECT COUNT(NAME)
FROM CITY 
WHERE POPULATION > 100000;
```

Query the total population of all cities in CITY where District is California.
```sql
SELECT SUM(POPULATION)
FROM CITY
WHERE DISTRICT = 'California';
```

Query the average population of all cities in CITY where District is California.
```sql
SELECT AVG(POPULATION)
FROM CITY
WHERE DISTRICT = 'California';
```

Query the average population for all cities in CITY, rounded down to the nearest integer.
```sql
SELECT FLOOR(AVG(POPULATION)) FROM CITY;
```

Query the sum of the populations for all Japanese cities in CITY. The COUNTRYCODE for Japan is JPN.
```sql
SELECT SUM(POPULATION)
FROM CITY
WHERE COUNTRYCODE = 'JPN';
```

Query the difference between the maximum and minimum populations in CITY.
```sql
SELECT MAX(POPULATION) - MIN(POPULATION) FROM CITY;
```

Samantha was tasked with calculating the average monthly salaries for all employees in the EMPLOYEES table, but did not realize her keyboard's 0 key was broken until after completing the calculation. She wants your help finding the difference between her miscalculation (using salaries with any zeroes removed), and the actual average salary.

Write a query calculating the amount of error (i.e.: actually miscalculated average monthly salaries), and round it up to the next integer.
```sql
SELECT CEIL(AVG(Salary)-AVG(REPLACE(Salary,'0',''))) 
FROM EMPLOYEES;
```

We define an employee's total earnings to be their monthly  worked, and the maximum total earnings to be the maximum total earnings for any employee in the Employee table. Write a query to find the maximum total earnings for all employees as well as the total number of employees who have maximum total earnings. Then print these values as 2 space-separated integers.
```sql
SELECT SALARY*MONTHS, COUNT(*)
FROM EMPLOYEE
WHERE SALARY*MONTHS = (SELECT MAX(SALARY*MONTHS) FROM EMPLOYEE)
```

Query the sum of LAT_N, followed by the sum of LONG_W, from STATION. The two results should be separated by a space and rounded to 2 decimal places.
```sql
SELECT CONCAT(ROUND(SUM(LAT_N),2), ' ', ROUND(SUM(LONG_W),2))
FROM STATION;
```

Query the sum of Northern Latitudes (LAT_N) from STATION having values greater than 38.788 and less than 137.2345. Truncate your answer to 4 decimal places.
```sql
SELECT ROUND(SUM(LAT_N),4)
FROM STATION
WHERE LAT_N > 38.788 AND LAT_N < 137.2345
```

Query the greatest value of the Northern Latitudes (LAT_N) from STATION that is less than 137.2345. Truncate your answer to 4  decimal places.
```sql
SELECT ROUND(MAX(LAT_N),4)
FROM STATION
WHERE LAT_N < 137.2345;
```

Query the Western Longitude (LONG_W) for the largest Northern Latitude (LAT_N) in STATION that is less than 137.2345. Round your answer to 4 decimal places.
```sql
SELECT ROUND(LONG_W,4) 
FROM STATION 
WHERE LAT_N = (SELECT MAX(LAT_N) FROM STATION WHERE LAT_N < 137.2345);
```
Query the smallest Northern Latitude (LAT_N) from STATION that is greater than 38.7780. Round your answer to 4 decimal places.
```sql
SELECT ROUND(MIN(LAT_N),4) 
FROM STATION 
WHERE LAT_N > 38.7880;
```

Query the Western Longitude (LONG_W) for the smallest Northern Latitude (LAT_N) in STATION that is greater than 38.7780. Round your answer to 4 decimal places.
```sql
SELECT ROUND(LONG_W,4) 
FROM STATION 
WHERE LAT_N = (SELECT MIN(LAT_N) FROM STATION WHERE LAT_N > 38.778);
```

