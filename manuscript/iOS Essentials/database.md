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