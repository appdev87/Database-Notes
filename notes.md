### COMMON DB CLASSES:
- DBMS: Database Management Systems
- DDL: Data Derived Language
- DML: Data Manipulation Language

### Database Modelling references:
- ERM: Entity Relationship Modelling
- ERD: Entity Relationship Diagram
- EER: Enhanced Entity Relationship
- Functional Dependency: 
    ▶ A functional dependency is a constraint between two sets of attributes in a relation R.
    ▶ If X and Y are two sets of attributes in the same relation R, then X → Y means that X functionally determines Y so that the values of the attributes in X uniquely determine the values of the attributes in Y

## Relationship Model
![RelationshipModelEmployee.jpg](resources/20EEE15BA9BEC51F66B5880211623E8A.jpg =446x316)

### Relational Algebra

\begin{align}
\pi_{Condition_1, .., Condition_n} (\ \sigma_{tuple_1, ..., tuple_n}\enspace Table ) \newline
E.g\thinspace1: \enspace
\pi_{GPA} (\pi_{sID,GPA,HS} \mathrm{Student}) \newline
E.g\thinspace2: \enspace
\sigma_{state=`CA'}(\sigma_{enr>8000} \mathrm{College})\newline \newline
Mathematically\enspace Join \enspace Statement:\newline
E.g\thinspace3:\enspace\pi_{sName, cName}(\sigma_{HS \gt enr}(\sigma_{state=`CA`}College \Join Student \Join \sigma_{major=`CS`}Apply))
\end{align}

#### Another example:
![RelationDBModel.png](resources/43D6B6046D8C3207D506A0D797DAA504.png =624x369)

### REFFERENCES:

- If data is not entered in the referred table, an error will come because the data has no reference to point to.

- If no specific column is referenced in the table, the `PRIMARY KEY` is taken as default.

  - E.g '`...residence VARCHAR REFERENCES Residence);`'

#### Referential Integrity:

• Puts `NULL` when the data is deleted

## Dangling Refence || Dead Link
When a references is made to an object but there is no values in that table to point to.

```sql
CREATE TABLE track (
    trackid INTEGER,
    trackname TEXT,
    trackartist INTEGER,
    FOREIGN KEY(trackartist) REFERENCES artist(artistid)
)
``` 

but artist(artistid) is empty.

# Normalization 
### Database issues due to redundancy:
1. Insertion Anomaly
  - When inserting a tuple but the tuple is repeated.
2. Deletion Anomaly
  - When removing a tuple, the tuple removes relevant data as well.
3. Updation Anomaly
  - Adding new tuples, creates data issues.

### Normalization of Database
Database Normalization is a technique of organizing the data in the database. Normalization is a systematic approach of decomposing tables to eliminate data redundancy(repetition) and undesirable characteristics like Insertion, Update and Deletion Anamolies. It is a multi-step process that puts data into tabular form, removing duplicated data from the relation tables.

Normalization is used for mainly two purposes,

Eliminating reduntant(useless) data.
Ensuring data dependencies make sense i.e data is logically stored.

#### Problems Without Normalization
If a table is not properly normalized and have data redundancy then it will not only eat up extra memory space but will also make it difficult to handle and update the database, without facing data loss. Insertion, Updation and Deletion Anamolies are very frequent if database is not normalized. To understand these anomalies let us take an example of a Student table.

|rollno	| name | branch | hod |	office_tel |
|:-------|:------:|:----:|:----:|----:|
|401|	Akon	|CSE  | Mr. X	|53337|
|402|	Bkon	|CSE  | Mr. X	|53337|
|403| Ckon	|CSE	| Mr. X	|53337|
|404|	Dkon	|CSE	| Mr. X	|53337|

In the table above, we have data of 4 Computer Sci. students. As we can see, data for the fields branch, hod(Head of Department) and office_tel is repeated for the students who are in the same branch in the college, this is Data Redundancy.

### Insertion Anomaly

Suppose for a new admission, until and unless a student opts for a branch, data of the student cannot be inserted, or else we will have to set the branch information as NULL.

Also, if we have to insert data of 100 students of same branch, then the branch information will be repeated for all those 100 students.

These scenarios are nothing but Insertion anomalies.

### Updation Anomaly

What if Mr. X leaves the college? or is no longer the HOD of computer science department? In that case all the student records will have to be updated, and if by mistake we miss any record, it will lead to data inconsistency. This is Updation anomaly.

### Deletion Anomaly

In our Student table, two different informations are kept together, Student information and Branch information. Hence, at the end of the academic year, if student records are deleted, we will also lose the branch information. This is Deletion anomaly.

### Normalization Rule
Normalization rules are divided into the following normal forms:

- First Normal Form
- Second Normal Form
- Third Normal Form
- BCNF
- Fourth Normal Form

### First Normal Form (1NF)
For a table to be in the First Normal Form, it should follow the following 4 rules:

- It should only have single(atomic) valued attributes/columns.
- Values stored in a column should be of the same domain
- All the columns in a table should have unique names.
- And the order in which data is stored, does not matter.

### Second Normal Form (2NF)
For a table to be in the Second Normal Form,

- It should be in the First Normal form.
- And, it should not have Partial Dependency.


### Third Normal Form (3NF)
A table is said to be in the Third Normal Form when,

- It is in the Second Normal form.
- And, it doesn't have Transitive Dependency.

### Boyce and Codd Normal Form (BCNF)
Boyce and Codd Normal Form is a higher version of the Third Normal form. This form deals with certain type of anomaly that is not handled by 3NF. A 3NF table which does not have multiple overlapping candidate keys is said to be in BCNF. For a table to be in BCNF, following conditions must be satisfied:

- R must be in 3rd Normal Form
and, for each functional dependency ( X → Y ), X should be a super Key.

### Fourth Normal Form (4NF)
A table is said to be in the Fourth Normal Form when,

- It is in the Boyce-Codd Normal Form.
- And, it doesn't have Multi-Valued Dependency.



# Joins:
- `INNER JOIN` only inner part of a subset.
- `WHERE` for a `Selection`.
  - e.g: 
  ```sql
  SELECT name, phone 
  FROM Contacts INNER JOIN Phone_numbers
  ON Contacts.contact_id = Phone_numbers.contact_id
  WHERE name = 'Emma';
  ```
- Use a condition as part of the `ON` clause.
  - e.g: 
  ```sql
  SELECT name, phone
  FROM Contacts INNER JOIN Phone_numbers
  ON Contacts.contact_id = Phone_numbers.contact_id AND name = 'Emma';
```

![JoinsSchema.png](resources/9C35865E61954602CCA1DAA541FA72D9.png =1024x724)

# Subqueries:

#### SELECT:
```sql
SELECT sID, sName
FROM Student 
WHERE sID IN (
  SELECT sID FROM Apply WHERE major = 'CS') 
  AND sID NOT IN (
  SELECT sID FROM Apply WHERE major = 'EE'
);
```

#### EXISTS:
**Eg.: Query searches from all colleges that are in the same state.**
```sql
SELECT cName, state
FROM College C1
WHERE EXISTS (
  SELECT * FROM College C2
  WHERE C2.state = C1.state AND C1.state <> C2.state
);
```

#### NOT EXISTS:
**Eg.: Query returns the students with the Highest GPA**
```sql
SELECT sName, GPA
FROM Student S1
WHERE NOT EXISTS (
  SELECT sName FROM Student S2 
  WHERE S2.GPA > S1.GPA 
);
```

#### ALL:
**Eg.: Query returns the students with the Highest GPA**
```sql
SELECT sName 
FROM Student 
WHERE GPA >= ALL ( SELECT GPA FROM Student);
```

#### IN:
**Eg.: Query returns the average GPA for 'CS' as major. 'IN'-clause returns _NO DUPLICATES_**
```sql
SELECT avg(GPA)
from Student
WHERE sID IN ( SELECT sID FROM Apply WHERE major = 'CS' );
```

#### UPDATE:
```sql
UPDATE neworder 
SET ord_date='15-JAN-10' 
WHERE ord_amount-advance_amount < (
  SELECT MIN(ord_amount) FROM orders
);
```

#### COUNT:
**Eg.: 'COUNT' and 'DISTINCT' can be use to avoid duplicated counts**
```sql
SELECT COUNT(DISTINCT sID)
FROM Apply 
WHERE major =  'CS';
```



**-- Eg.: Write a SELECT statement that compares the COUNT for UNION and UNION ALL for all Asian countries when combining name and full_name.**

#### UNION: (Provide a more accurate result because there are **_NO DUPLICATES_**).
```sql
SELECT COUNT(*)
FROM (
    SELECT full_name
    FROM countries WHERE continent_code='AS'
    UNION
    SELECT name
    FROM countries 
    WHERE continent_code='AS'
);
```

#### UNION ALL: (Includes duplicates and null values)
```sql
SELECT COUNT(*)
FROM (
    SELECT full_name
    FROM countries WHERE continent_code='AS'
    UNION ALL
    SELECT name
    FROM countries 
    WHERE continent_code='AS'
);
```

#### INTERSECT: (Almost the same as `INNER JOIN`)
```sql
SELECT COUNT(*)
FROM 
   (SELECT name
    FROM countries
    WHERE continent_code = 'EU'
    INTERSECT
    SELECT full_name FROM countries WHERE continent_code = 'EU'
    );
  ```
    

#### EXCEPT: (Almost the same as `LEFT JOIN`)
```sql
SELECT COUNT(*)
FROM 
   (SELECT name
    FROM countries
    WHERE continent_code = 'EU'
    EXCEPT
    SELECT full_name FROM countries WHERE continent_code = 'EU');
    ```

#### UPDATE:
```sql
UPDATE table
SET column_1 = new_value_1,
    column_2 = new_value_2
WHERE
    search_condition;
    ```
    

#### DELETE:
Eg 1: 
```sql
DELETE FROM films USING producers
  WHERE producer_id = producers.id AND producers.name = 'foo';
  ```
  
Eg 2: 
```sql
DELETE FROM films
  WHERE producer_id IN (SELECT id FROM producers WHERE name = 'foo');
  ```
### DELETE (PSQL-PostgreSQL)
Eg 3:
```sql
DELETE FROM table WHERE search_condition;
 ```

## Movie Join Examples
_Movie ( mID, title, year, director )_
English: There is a movie with ID number mID, a title, a release year, and a director. 

_Reviewer ( rID, name )_
English: The reviewer with ID number rID has a certain name. 

_Rating ( rID, mID, stars, ratingDate )_
English: The reviewer rID gave the movie mID a number of stars rating (1-5) on a certain ratingDate. 

**-- 1. Find the titles of all movies directed by Steven Spielberg.**

```sql
SELECT title
FROM Movie
WHERE director = 'Steven Spielberg';
```


**-- 2. Find all years that have a movie that received a rating of 4 or 5, and sort them in increasing order.**

```sql
SELECT DISTINCT year
FROM Movie, Rating
WHERE Movie.mId = Rating.mId AND stars IN (4, 5)
ORDER BY year;

SELECT DISTINCT year
FROM Movie
INNER JOIN Rating ON Movie.mId = Rating.mId
WHERE stars IN (4, 5)
ORDER BY year;

SELECT DISTINCT year
FROM Movie
INNER JOIN Rating USING(mId)
WHERE stars IN (4, 5)
ORDER BY year;

SELECT DISTINCT year
FROM Movie NATURAL JOIN Rating
WHERE stars IN (4, 5)
ORDER BY year;
```

**-- 3. Find the titles of all movies that have no ratings.**

```sql
SELECT title
FROM Movie
WHERE mId NOT IN (SELECT mID FROM Rating);
```

**-- 4. Some reviewers didn't provide a date with their rating. Find the names of all reviewers who have ratings with a NULL value for the date.**

```sql
SELECT name
FROM Reviewer
INNER JOIN Rating USING(rId)
WHERE ratingDate IS NULL;
```


**-- 5. Write a query to return the ratings data in a more readable format: reviewer name, movie title, stars, and ratingDate. Also, sort the data, first by reviewer name, then by movie title, and lastly by number of stars.**

```sql
SELECT name, title, stars, ratingDate
FROM Movie, Rating, Reviewer
WHERE Movie.mId = Rating.mId AND Reviewer.rId = Rating.rId
ORDER BY name, title, stars;

SELECT name, title, stars, ratingDate
FROM Movie
INNER JOIN Rating ON Movie.mId = Rating.mId
INNER JOIN Reviewer ON Reviewer.rId = Rating.rId
ORDER BY name, title, stars;

SELECT name, title, stars, ratingDate
FROM Movie
INNER JOIN Rating USING(mId)
INNER JOIN Reviewer USING(rId)
ORDER BY name, title, stars;

SELECT name, title, stars, ratingDate
FROM Movie NATURAL JOIN Rating NATURAL JOIN Reviewer
ORDER BY name, title, stars;
```

**-- 6. For all cases where the same reviewer rated the same movie twice and gave it a higher rating the second time, return the reviewer's name and the title of the movie.**

```sql
SELECT name, title
FROM Movie
INNER JOIN Rating R1 USING(mId)
INNER JOIN Rating R2 USING(rId)
INNER JOIN Reviewer USING(rId)
WHERE R1.mId = R2.mId AND R1.ratingDate < R2.ratingDate AND R1.stars < R2.stars;

SELECT name, title
FROM Movie
INNER JOIN Rating R1 USING(mId)
INNER JOIN Rating R2 USING(rId, mId)
INNER JOIN Reviewer USING(rId)
WHERE R1.ratingDate < R2.ratingDate AND R1.stars < R2.stars;
```


**-- 7. For each movie that has at least one rating, find the highest number of stars that movie received. Return the movie title and number of stars. Sort by movie title.**

```sql
SELECT title, MAX(stars)
FROM Movie
INNER JOIN Rating USING(mId)
GROUP BY mId
ORDER BY title;
```


**-- 8. For each movie, return the title and the 'rating spread', that is, the difference between highest and lowest ratings given to that movie. Sort by rating spread from highest to lowest, then by movie title.**

```sql
SELECT title, (MAX(stars) - MIN(stars)) AS rating_spread
FROM Movie
INNER JOIN Rating USING(mId)
GROUP BY mId
ORDER BY rating_spread DESC, title;
```


**-- 9. Find the difference between the average rating of movies released before 1980 and the average rating of movies released after 1980. (Make sure to calculate the average rating for each movie, then the average of those averages for movies before 1980 and movies after. Don't just calculate the overall average rating before and after 1980.)**

```sql
SELECT AVG(Before1980.avg) - AVG(After1980.avg)
FROM (
  SELECT AVG(stars) AS avg
  FROM Movie
  INNER JOIN Rating USING(mId)
  WHERE year < 1980
  GROUP BY mId
) AS Before1980, (
  SELECT AVG(stars) AS avg
  FROM Movie
  INNER JOIN Rating USING(mId)
  WHERE year > 1980
  GROUP BY mId
) AS After1980;
```

## Social Networking Examples
_Highschooler ( ID, name, grade )_
English: There is a high school student with unique ID and a given first name in a certain grade. 

_Friend ( ID1, ID2 )_
English: The student with ID1 is friends with the student with ID2. Friendship is mutual, so if (123, 456) is in the Friend table, so is (456, 123). 

_Likes ( ID1, ID2 )_
English: The student with ID1 likes the student with ID2. Liking someone is not necessarily mutual, so if (123, 456) is in the Likes table, there is no guarantee that (456, 123) is also present. 


**--Find the names of all students who are friends with someone named Gabriel.**

```sql
SELECT name FROM Highschooler
WHERE id IN (
  SELECT ID1 FROM Friend WHERE id1 = id AND
  id2 IN (SELECT id FROM Highschooler WHERE name="Gabriel")
)
```

**--For every student who likes someone 2 or more grades younger than themselves, return that student's name and grade, and the name and grade of the student they like.**

```sql
SELECT h1.name, h1.grade, h2.name, h2.grade
FROM Highschooler AS h1
JOIN Likes AS l on l.id1 = h1.id
JOIN Highschooler AS h2 ON h2.id = l.id2
WHERE h2.grade <= h1.grade - 2
```

**--For every pair of students who both like each other, return the name and grade of both students. Include each pair only once, with the two names in alphabetical order.**

```sql
SELECT n2, g2, n1, g1 FROM
( SELECT h1.id+h2.id AS mult, h1.name AS n1, h1.grade AS g1, h2.name Sn2, h2.grade AS g2
    FROM likes AS l
    JOIN highschooler AS h1 ON h1.id = l.id1
    JOIN highschooler AS h2 ON h2.id = l.id2
    WHERE exists (SELECT * FROM likes WHERE id1=h2.id AND id2=h1.id)
    ORDER BY  mult ASC , h1.name < h2.name DESC
) AS t
GROUP BY mult
```

**--Find names and grades of students who only have friends in the same grade. Return the result sorted by grade, then by name within each grade.**

```sql
SELECT  h1.name, h1.grade
FROM Highschooler AS h1
JOIN Friend Sf ON f.id1=h1.id
JOIN Highschooler AS h2 ON h2.id = f.id2
WHERE h1.grade = h2.grade
AND NOT EXISTS (
    SELECT id FROM Highschooler WHERE id IN (
      SELECT id2 FROM Friend WHERE id1 = h1.id) AND grade <> h1.grade
      )
    )
GROUP BY h1.name
ORDER BY h1.grade, h1.name
```

**--Find the name and grade of all students who are liked by more than one other student.**

```sql
SELECT name, grade FROM Highschooler
WHERE id IN (
  SELECT id2
  FROM Likes
  GROUP BY id2
  HAVING COUNT(id1) > 1
)
```

# SQL Movie-Rating Modification Exercises
_Movie ( mID, title, year, director )_
English: There is a movie with ID number mID, a title, a release year, and a director. 

_Reviewer ( rID, name )_
English: The reviewer with ID number rID has a certain name. 

_Rating ( rID, mID, stars, ratingDate )_
English: The reviewer rID gave the movie mID a number of stars rating (1-5) on a certain ratingDate. 


**--Add the reviewer Roger Ebert to your database, with an rID of 209.**

```sql
INSERT INTO Reviewer(rID, name) 
VALUES (209, 'Roger Ebert')
```

**--Insert 5-star ratings by James Cameron for all movies in the database. Leave the review date as NULL.**

```sql
INSERT INTO Rating  ( rID, mID, stars, ratingDate )
SELECT Reviewer.rID , Movie.mID, 5, null
FROM Movie
LEFT OUTER JOIN Reviewer
WHERE Reviewer.name = 'James Cameron';
```

**--For all movies that have an average rating of 4 stars or higher, add 25 to the release year. (Update the existing tuples; don't insert new tuples.)**

```sql
UPDATE Movie
SET year = year + 25
WHERE mID IN (
  SELECT mID FROM (
  SELECT AVG(stars) AS astar, mID FROM Rating
  WHERE mID = rating.mID
  GROUP BY mID
  HAVING astar >=4)
)
```

**--Remove all ratings where the movie's year is before 1970 or after 2000, and the rating is fewer than 4 stars.**

```sql
DELETE FROM rating
WHERE mID IN (
  SELECT mID 
  FROM Movie 
  WHERE year <1970 OR year > 2000
)
AND stars < 4
```

### INDEX: (Use for referencing)
```sql
CREATE INDEX index
 ON $TABLE $column;
 ```
 
 #####CREATE MULTIPLES INDEXES:
```sql
CREATE INDEX index
 ON TABLE (cloumn1, column2,.....);
 ```
 
 #####CREATE UNIQUE INDEXES: 
 ```sql
 CREATE UNIQUE INDEX index ON TABLE column;
 ```
 
 #####DROP INDEX: 
 ```sql
 DROP INDEX index;
 ```
 
 #####SHOW INDEXES: 
 ```sql
 SELECT * FROM USER_INDEXES;
 ```

### SUMMING TWO TABLES
```sql
SELECT coalesce(economy_free,0)+coalesce(business_free,0) FROM Flightreservation;
```
template1=# 
```sql 
SELECT * FROM yourtable ;
```
 |a | b |
|---|:--:|
 1 | 2
 4 | 5
(2 rows)

template1=# 
```sql
SELECT a + b FROM yourtable ;
```
 |?column?|
|:----------:|
  3 |
  9|
(2 rows)

template1=# 
```sql
SELECT SUM( a ), SUM( b ) FROM yourtable ;
```
 |sum | sum |
|-----|-----|
   5 |   7 |
(1 row)

template1=# 
```sql
SELECT SUM( a + b ) FROM yourtable ;
```
 |sum |
-----
  |12|
(1 row)

### OLAP vs OLTP
Online transaction processing (OLTP) –
Online transaction processing provides transaction-oriented applications in a 3-tier architecture. OLTP administers day to day transaction of an organization.

_Examples – Uses of OLTP are as follows:_

ATM center is an OLTP application.
OLTP handles the ACID properties during data transaction via the application.
It’s also used for Online banking, Online airline ticket booking, sending a text message, add a book to the shopping cart.

![IMAGE](resources/727852C11AE99EAA6F128138DA326337.jpg =761x483)

#### Comparisons of OLAP vs OLTP –
| OLAP (ONLINE ANALYTICAL PROCESSING) |	OLTP (ONLINE TRANSACTION PROCESSING) |
|-------------------------------------|:-------------------------------------:|
| Consists of historical data from various Databases.|	Consists only operational current data. |
|It is subject oriented. Used for Data Mining, Analytics, Decision making,etc.	| It is application oriented. Used for business tasks. |
| The data is used in planning, problem solving and decision making. | 	| The data is used to perform day to day fundamental operations.
It reveals a snapshot of present business tasks. | It provides a multi-dimensional view of different business tasks. |
| Large amount of data is stored typically in TB, PB |	The size of the data is relatively small as the historical data is archived. For ex MB, GB |
| Relatively slow as the amount of data involved is large. Queries may take hours.	| Very Fast as the queries operate on 5% of the data. |
| It only need backup from time to time as compared to OLTP. |	Backup and recovery process is maintained religiously |
| This data is generally managed by CEO, MD, GM. |	This data is managed by clerks, managers. |
| Only read and rarely write operation. |	Both read and write operations.

### TRANSACTION: (Use for testing before applying an actual use.)

##### BEGIN TRANSACTION;
```pgSQL
BEGIN TRANSACTION;
 
UPDATE accounts
   SET balance = balance - 5000
 WHERE account_no = 78;
 
UPDATE accounts
   SET balance = balance + 5000
 WHERE account_no = 153;
 
INSERT INTO account_changes(account_no,flag,amount,changed_at) 
VALUES (100,'-',1000, current_timestamp);
 
INSERT INTO account_changes(account_no,flag,amount,changed_at) 
VALUES (200,'+',1000, current_timestamp);

SELECT * FROM accounts;
```

#### ROLLBACK: (Use to disregard any other states) `ROLLBACK;`

#### COMMIT TRANSACTION: (Use to commit changes) `COMMIT TRANSACTION;`

#### END TRANSACTION: `END TRANSACTION;`

### Query Details:
##### TIME: `%%time` (At the top of the query)

##### EXPLAIN QUERY:
```sql
EXPLAIN QUERY PLAN
SELECT COUNT(*) 
FROM TRIPS
WHERE duration > 1
ORDER BY start_date;
```

### ISOLATION LEVELS IN TRANSACTIONS:
_SYNTAX:_  `BEGIN TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;`
_SYNTAX:_ `SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;`

|Isolation Level	| Dirty Read	| Nonrepeatable Read	| Phantom Read	| Serialization Anomaly |
|-----------------|:-------------:|:------------------------:|:-----------------------:|:------------:|
|Read uncommitted	| Allowed, but not in PG	|Possible | Possible | Possible |
|Read committed	|Not possible	| Possible	| Possible	| Possible |
|Repeatable read	| Not possible	| Not possible	| Allowed, but not in PG	| Possible |
|Serializable	| Not possible	| Not possible	| Not possible	| Not possible |

## Isolation levels
- per Transaction
- "In the eye of the beholder"

#### Dirty Reads
"Dirty" data item: written by an uncommited transaction.
- Another transaction reads an uncommitted Transaction.
  - T2 reads 'uncommited' transaction from T1.

### Isolation Level `READ UNCOMMITTED`
- A transaction may perform dirty reads.
- Telling the transaction that it's OK to read 'dirty values' from other transactions.

### Isolation Level `READ COMMITTED`
- Reads only committed transactions.
- A transaction may not perform dirty reads.
  - !!! STILL DOES NOT GUARANTEE GLOBAL SERIALIZABILITY.

### Isolation Level `REPEATABLE READ`
- A transaction may not perform dirty reads.
- An item read multiple times cannot change value.
  - !!! STILL DOES NOT GUARANTEE GLOBAL SERIALIZABILITY.
  - !!! BUT A REALATION CAN CHANGE: "PHANTOM" TUPLES.
    - PHANTOM TUPLES occur on `INSERTION` but doesn't allow `DELETION` of tuples.

### Isolation Level `SERIALIZABLE`
- It's the default transaction isolation level.

### `READ ONLY` transactions
- Helps system optimize performance.
- Independent of isolation level.
_SYNTAX_: `SET TRANSACTION READ ONLY;
SET TRANSACTION ISOLATION LEVEL REAPEATABLE READ;
SELECT AVG(GPA) FROM Student;
SELECT MAX(GPA) FROM Student;`

#### MVCC (Multiversion Concurrency Control)
Each SQL statement sees a snapshot of the Database as it was some time ago, regardless of the current state.

#### Advantage:
- Reading never blocks writing and writing never blocks reading. 
- PostgreSQL maintains this guarantee even when providing the strictest level of transaction isolation through the use of an innovative Serializable Snapshot Isolation (SSI) level.
- Usually provides better performance than locks.



