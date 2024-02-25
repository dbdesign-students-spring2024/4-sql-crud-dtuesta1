# PART ONE: Restaurant finder
### Creating restaurants and reviews table 
``` sql
CREATE TABLE restaurants (
	rest_id INTEGER PRIMARY KEY,
	name TEXT,
	category TEXT, 
	price_tier TEXT,
	neighborhood TEXT, 
	open_time TEXT,
	close_time TEXT, 
	avg_ratings REAL,
	good_for_kids TEXT 
);

.mode csv 
.headers on
.import /Users/tuesti/DatabaseNYU/4-sql-crud-dtuesta1/data/mockdata.csv restaurants;

CREATE TABLE reviews (
    review_id INTEGER PRIMARY KEY, 
    rest_name TEXT,
    rating REAL,
    user_name TEXT
);
```
### SQL queries

1. find all cheap restaurants in a particular neighborhood (pick any neighborhood as an example).

``` sql
SELECT name FROM restaurants WHERE price_tier = "cheap" AND neighborhood="SoHo";
```

2. Find all restaurants in a particular genre (pick any genre as an example) with 3 stars or more, ordered by the number of stars in descending order.
``` sql
SELECT name FROM restaurants WHERE price_tier = "cheap" AND neighborhood="SoHo";
```

3. Find all restaurants that are open now (see hint below).
``` sql
SELECT name from restaurants WHERE open_time <= strftime('%H:%M', 'now','localtime')< close_time ;

```

4. Leave a review for a restaurant (pick any restaurant as an example; note that leaving a review has no automatic effect on the average rating of the restaurant).

``` sql
 insert into reviews (rest_name,rating,user_name) values ("Kozey Inc", 3.0, "dani");
```

5. Delete all restaurants that are not good for kids.
``` sql
 delete from restaurants where good_for_kids="false";
```

6. Find the number of restaurants in each NYC neighborhood.
``` sql
select neighborhood,count(rest_id)as numberOfRestaurants from restaurants group by neighborhood;
```


# PART TWO: Social media app
### creating tables
``` sql
CREATE TABLE users (
user_id INTEGER PRIMARY KEY,
email TEXT,
password TEXT,
handle TEXT
);

CREATE TABLE posts (
post_id INTEGER PRIMARY KEY,
user_handle TEXT,
receiver_handle TEXT,
category TEXT,
content TEXT,
date TEXT,
visible TEXT
);
```


### SQL queries
- the SQL code to create each of the required tables
- a link to each of the practice CSV data files in the data directory.
- the SQLite code to import the practice CSV data files into the tables.

1. Register a new User.
``` sql

```

2. Create a new Message sent by a particular User to a particular User (pick any two Users for example).
``` sql

```

3. Create a new Story by a particular User (pick any User for example).
``` sql

```

4. Show the 10 most recent visible Messages and Stories, in order of recency.
``` sql

```

5. Show the 10 most recent visible Messages sent by a particular User to a particular User (pick any two Users for example), in order of recency.
``` sql

```

6. Make all Stories that are more than 24 hours old invisible.
``` sql

```

7. Show all invisible Messages and Stories, in order of recency.
``` sql

```

8. Show the number of posts by each User.
``` sql

```

9. Show the post text and email address of all posts and the User who made them within the last 24 hours.
``` sql

```

10. Show the email addresses of all Users who have not posted anything yet.
``` sql

```
