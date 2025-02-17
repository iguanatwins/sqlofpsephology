# Unknown Pleasures - The Joy of Finding Things Out 
# Or How I Learned To Appreciate Object Permance of The Database

Micheal has a very nice relational database for the election results website we will use.

We can query it here: https://psephology-datasette-f3e7b1b7eb77.herokuapp.com/psephology
This database used postgres sql, sql comes in many flavours. postgres is a good one to learn it is similar to sql server (microsoft) or oracle, but a little more flexiable. Mysql is a bit more weird, but same general concepts.

## 1. What is a Database? Why Would I use one?

### 1.1 They organise information to make searching fast and summarising easy!
__Indexes and unqiue keys__

Databases organise information into tables. These have 'Keys' or 'IDs' which enable the tables to be indexed, these are unique to each row.

Lets query a table using a __select all__ statement. In sql statement refers to the full command you are making to the database. When the command returns values this is also called a query. We will only be covering queries in this course.

```
select *         -- we have chosen to select all (in sql 'select *') columns from the table
from candidacies -- 'candidacies' is the table name,  
limit 100 --> this sets how many rows I would like to return, you can change to number to how many you would like
```

We can see the ID in the fist column which as per convention is unique for each row in the table thus giving the unique identfier.

We can check this across the whole table, we will use the __count__ function and the __distinct__ clause within a select statement. 
Clauses in sql make limits on on the data to be returned (i.e. we only want the distinct values here to be returned not all). Functions use the data as an input to give another output.

```
select count(*) as 'count_rows', 
count(distinct ID) AS 'count_index' -- count rows and count the number of distinct ids
from candidacies
/*
The AS just provides a name to the column on the output
The counts you receive should be identical as every row must have a unique id
 */
```

You can use this count of unique values or the count all to count grouped values.

```
/* What are the number of candidacies in the database where a candidate was a sitting MP or not? */
select candidate_is_sitting_mp, count(distinct ID) 
from candidacies
group by candidate_is_sitting_mp 
--the group by should include all the different categories you want to be able to see
--here the candidancies are set up as binary 0 = not a sitting mp, 1 = is a sitting mp
```

We might want to know about only a subset of the data for this we can use the __where__ clause

```
/* What are the number of candidacies where a candidate was a sitting MP or not, and was standing as an independent */
select candidate_is_sitting_mp, count(distinct ID) 
from candidacies
where is_standing_as_independent = 1
--again is_standing_as_independent is binary 1 = is standing as independent, 0 = is not standing as independent
/*output shows it more unusual for independent candidancies to be a sitting mp */
,,,

Because the categories are relatively simple you might want to put candidate_is_sitting_mp and is_standing_as_independent in the output.

You can also sum
```
select candidate_family_name, SUM(vote_count)AS 'total_family_name_vote_count'
from candidacies
group by candidate_family_name

```

we can also divide

```
/*vote count by family and average family name vote count by candidacy */
select candidate_family_name, SUM(vote_count)AS 'total_family_name_vote_count', SUM(vote_count)/count(distinct id) AS 'average_cadidancy_vote_count'
from candidacies
group by candidate_family_name
order by SUM(vote_count)/count(distinct id) desc

```

Here we can see the use of the __order by__ clause. this orders the output. The letters 'desc' stand for desc-ending so we can see the highest value at the top.
side note the ability in postgres to refer to calculated values directly in the order by is far more flexible than sql server sql.

We may instead decide instead of seeing the whole range of values we just want to see our top value or top 5 values using the limit clause. (try to add the clause into the query)

Or may want to use the having clause to only we family names which only had over 10% of the vote share. We can use the having clause for this

```
/* */
select candidate_family_name, SUM(vote_count)AS 'total_family_name_vote_count', MIN(vote_share)
from candidacies
group by candidate_family_name
WHERE MIN(vote_share) > 0.1
```

We have covered lots of clauses and a function. We'll come back to all these later. 
Next we will move onto how different tables can relate to one another to give more context to our outputs or add new information.


### 1.2 Databases are used to define different concepts and how they relate to one another, making the data a bit more flexiable.

In 1.1 we focused on one entity 'candidacies'. 

An entity is just another name for a table, but the idea is that in also conveys entities are considered concepts within the subject area of the dataset. 

They can be treated almost as an object which has a set of attributes. 

**Attributes** are also just another name for a column. But again it is to relate that the columns must be a subdomain of the entity.

Lets start again with candidacies and just have a look at the data held in the table.

```
select *         -- we have chosen to select all (in sql 'select *') columns from the table
from candidacies -- 'candidacies' is the table name,  
limit 25 --> this sets how many rows I would like to return, you can change to number to how many you would like
```




### 1.31 We can draw out these relationships in diagrams so they look a bit more physical
### In fact what we are about to look at can also be called a __physical__ model

We talked about relationships and joins. Let's look at the election websites Entity-Relationship Diagram or ERD.

https://electionresults.parliament.uk/schema.svg

It's very big as it should be, to cover the full dataset and how people would want to understand and question the data.

It has lots of relationships between entities.

First there's some basic notation, this is known as UML.


### 1.32 We can use these relationships to make our outputs a bit more complex



### 1.4 When a computer is executing a sql statement the clauses are applied in a set order

This is known as order of execution, it's a bit boring but can help with writing statements and where you might not be getting the number you think you should.


### 1.5 We can chose data through more complex ways and relate things to themselves
 

over(partition by)

subqueries

### 1.6 the data is always typed this saves space and makes calculations easier




## 2.0 Lets make our own Entity Relationship Diagrams

Lets work out some solid entities and their relationships.

[This will require some data... or we could think of a problem set]


