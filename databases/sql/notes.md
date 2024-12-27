# SQL notes

## Basics of SQL table creation

### Common data types
- INT
- DECIMAL(s,d) <-- size (digits, s) and decimal places (d)
- VARCHAR(c) <-- maximum length of strings (c)
- DATA (formatted as YYYY-MM-DD)
- TIMESTAMP (formatted as HH:MM:SS)

#### Creating tables
```
CREATE TABLE bond(
id INT PRIMARY KEY,
title VARCHAR(50) UNIQUE,
released INT,
actor VARCHAR(30),
director VARCHAR(30),
box_office DECIMAL(5,1) DEFAULT '0.0' <-- 5 characters long, 1 decimal place
);
```
#### Deleting tables
`DROP TABLE bond`

#### Constraints
- PRIMARY KEY
    - recommended for every database table
    - uniquely identifies each row in a table
    - cannot be null
    - can only be one primary key per table
- UNIQUE
    - does not allow duplicate values
    - can contain a single null
    - can have more than one in a table
- NOT NULL
    - does not allow null (empty) values
- DEFAULT
    - replaces null values with a specified default
    - 

Constraints
- help keep data clean
- ensures no duplicate or null values

#### Inserting into tables
```
INSERT INTO bond VALUES(
    1,'DR. NO',1962,'SEAN CONNERY','Terrence Young',59.5
)
```

If you only have data to insert for certain columns, you can specify that in the INSERT statement:
```
INSERT INTO bond(id,title,released) VALUES(
    2,'FROM RUSSIA WITH LOVE',1963
)
```

#### Describe a table
`describe bond`
this will print a list of the table columns, their types, whether they allow NULL, and their constraints

#### Adding columns
specify new column name and its data type
```
ALTER TABLE bond ADD studio VARCHAR(30)
```
#### Deleting columns
```
ALTER TABLE bond DROP studio
```
#### Modify data in columns
```
UPDATE bond
SET actor='Connery'
WHERE actor='SEAN CONNERY'
```
#### Delete certain rows
`DELETE FROM bond WHERE title='NEVER SAY NEVER AGAIN'`

## Basics of SQL querying (SELECTing, not creating / deleting tables etc.)
https://www.youtube.com/watch?v=kbKty5ZVKMY

#### SELECTing
star (*) selects all fields (columns) and all rows from 'player' table:
- `SELECT * FROM player` <-- 'player' is the name of a table in the db

select specific fields by writing out the field names:
```{sql}
SELECT player_name, birthday FROM player
```

#### you can rename fields when selecting them, by creating aliases
```{sql}
SELECT player_name as name, birthday as 'Birth day' FROM player
```
^ if you want a space in an alias, you need to use quotes

#### you can select specific rows using WHERE clause

e.g., any player with a weight greater than 90lbs
```{sql}
SELECT
    player_name, birthday
FROM
    player where weight>=90
```

#### you can use multiple arguments (using AND or OR)
```{sql}
SELECT
    player_name, birthday
FROM
    player WHERE weight>=90 AND height>190
```

#### you can check for text equality using =
```{sql}
SELECT
    player_name, birthday
FROM
    player WHERE player_name='Derek Andersen'
```
#### the 'LIKE' operator achieves the same goal as =
```{sql}
SELECT
    player_name, birthday
FROM
    player WHERE player_name LIKE 'Derek Andersen'
```

#### you can use `%` in text to find data that starts with something
```{sql}
SELECT
    player_name, birthday
FROM
    player WHERE player_name LIKE 'Derek%'
```
or ends with something
```{sql}
SELECT
    player_name, birthday
FROM
    player WHERE player_name LIKE '%Derek'
```
#### or check for any individual characters
```{sql}
SELECT
    player_name, birthday
FROM
    player WHERE player_name LIKE 'D_erek%'
```

#### the `in` operator lets you specify OR-groups to check for text

```{sql}
SELECT
    player_name, birthday
FROM
    player WHERE player_name IN ('Cristiano Ronaldo','Lionel Messi')
```

#### can use `between` operator for integers
```{sql}
SELECT
    player_name, birthday
FROM
    player WHERE weight between 180 and 190
```

#### can use `is null` or `is not null`
```{sql}
SELECT
    *
FROM
    match WHERE home_player_1 is null
```
```{sql}
SELECT
    *
FROM
    match WHERE home_player_1 is not null
```

#### you can order data retrieved using `order by` <field>. use `desc` for descending
```{sql}
SELECT
    *
FROM
    player order by weight desc
```

### Join data from different tables
the dot operator is saying you want to select the field after the dot, from the table before the dot. e.g.
- `table_name.field_name`
- so we can select multple field names from multiple tables at the same time
- then use a JOIN (inner join here)
- this query will return a view with those 4 fields, where the player_api_id matches across both
```{sql}
SELECT
    player_attributes.player_api_id,
    player.player_name,
    player_attributes.date,
    player_attributes.overall_rating
FROM
    player_attributes
    inner join player on player_attributes.player_api_id=player.player_api_id
```

you can also use aliases for tables in the FROM statements. this makes it cleaner. just write the alias right after the table like here:
```{sql}
SELECT
    a.player_api_id,
    b.player_name,
    a.date,
    a.overall_rating
FROM
    player_attributes a
    inner join player b on a.player_api_id=b.player_api_id
```

### Aggregate data with sum() aggregator
- here we can aggregate the data in the overall_rating field (sum it all)
- but then we can use the `group by` operator (specifying all the fields we want to group)
```{sql}
SELECT
    a.player_api_id,
    b.player_name,
    sum(a.overall_rating) as rating
FROM
    player_attributes a
    inner join player b on a.player_api_id=b.player_api_id
    group by a.player_api_id,
    b.player_name
    order by rating desc

```

there is also the average aggregator `avg()`

## SQL functions
- `LTRIM(string)` remove leading spaces from string
- `RTRIM(string)` remove trailing spaces from string
- `TRIM(string)` remove both leading and trailing spaces

- `LEFT(string, length)` return a specified number of characters from the left of the string
- `RIGHT(string, length)` return a specified number of characters from the right of the string

```
SELECT
    count ( * ) AS count,
    RTRIM(Segment) as segtrim,
    LEFT(order_id,2) as order_cat <-- gets the leftmost 2 chars of the string in order_id
FROM
    orders
GROUP BY
    segtrim,
    order_cat <-- need to add it here too
```

- `LPAD(str,len,padstr)` left pads a str with another str
- `RPAD(str,len,padstr)` right pads a str with another str

```
SELECT
    zip_code,
    LPAD (zip_code,5,'0') <-- pads on the left with zeroes
FROM
    orders
```

- `SUBSTRING(str,pos,length)` returns a specified number of characters from a particular starting position of a given string 

```
SELECT
    SUBSTRING(order_id,4,4) as order_num
FROM
    orders
```

- `LENGTH(str,pos,length)` commonly used with SUBSTRING. returns the length of a given string

```
SELECT
    customer_name,
    length(customer_name)
FROM
    orders
    WHERE
        length(customer_name) > 20
```

- `LOCATE(substr,str)` returns the position of the first occurrence of a string within a string
    - synonymous with `POSITION(substr IN str)`

```
SELECT customer_name,
    locate(' ', customer_name)
FROM
    orders
```

Small problem to get first and last names that have been concatenated with a space in between:
```
SELECT
    substr(customer_name,1,locate(' ',customer_name)-1) as first_name,
    substr(customer_name,locate(' ',customer_name)+1,length(customer_name)) as last_name
FROM
    orders
```

more functions:
- `UPPER(str)` makes all uppercase
- `LOWER(str)` makes all lowercase
- `CONCAT(str1,str2)` concatenates two strings

### Conditionals
this corrects all instances of "Ohios" in the state field to "Ohio"
```
SELECT
    state,
    CASE WHEN state LIKE 'Ohios' THEN 'Ohio' ELSE state END
FROM
    orders
```

can have multiple `WHEN` statements within a `CASE...END` block


### Difference between using a WHERE clause and a HAVING clause
- WHERE filters at the row level
    - it checks every row to see if some value is true
- HAVING means we've done some aggregation already, so it comes after a GROUP BY where we've done some grouping
    - HAVING checks by group rather than by row
    - for example you can sum up all of the value for some field which is already grouped by some other field