# PART ONE: Restaurant finder
### Creating restaurants and reviews table 
practice data 
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
.import data/restaurants.csv restaurants --skip 1;

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
> **Results**: 

|               name               |
|----------------------------------|
| Kassulke LLC                     |
| Murray, Mayert and Stoltenberg   |
| Muller, Gorczany and Heidenreich |
| Pacocha, Mitchell and Robel      |
| Beer and Sons                    |
| Sporer Group                     |
| Turcotte, Bahringer and Hahn     |
| Kemmer Group                     |
| Schaden-Hoppe                    |
| Sawayn Inc                       |
| Jast and Sons                    |


2. Find all restaurants in a particular genre (pick any genre as an example) with 3 stars or more, ordered by the number of stars in descending order.
``` sql
SELECT name FROM restaurants WHERE category = "Peruvian" AND avg_ratings>=3.0 order by avg_ratings desc;
```
> **Results**: 

|              name               |
|---------------------------------|
| Will, Corwin and Beier          |
| Kulas Group                     |
| Mueller-Kautzer                 |
| Feil, Conroy and Harris         |
| Jacobson-Beatty                 |
| Bartoletti, Leannon and Shields |
| Watsica, Huel and Macejkovic    |
| Breitenberg-Sauer               |
| Daniel, Champlin and Russel     |
| Lesch-Rodriguez                 |
| Collier LLC                     |
| Schmidt, Schuster and Monahan   |
| Windler Group                   |
| Sawayn Inc                      |
| Stracke-Williamson              |
| Leannon Inc                     |
| O'Conner, Conn and Spencer      |
| Padberg, Kirlin and Collier     |
| Lynch, Schaefer and Schuster    |

3. Find all restaurants that are open now (see hint below).
``` sql
SELECT name from restaurants WHERE open_time <= strftime('%H:%M', 'now','localtime')< close_time ;
```
> **Results**: 

|                 name                  |
|---------------------------------------|
| Cole, Bruen and Collins               |
| Wilderman-Gusikowski                  |
| Green, Kirlin and Jacobs              |
| Vandervort-Lang                       |
| Lubowitz Inc                          |
| Macejkovic, Roberts and McDermott     |
| Hilpert-Pfannerstill                  |
| Champlin, Jacobi and Swaniawski       |
| Heller, Jaskolski and Daugherty       |



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
> **Results**: 

|     neighborhood     | numberOfRestaurants |
|----------------------|---------------------|
| Astoria              | 16                  |
| Battery Park City    | 21                  |
| Bedford-Stuyvesant   | 16                  |
| Boerum Hill          | 14                  |
| Carnegie Hill        | 20                  |
| Carroll Gardens      | 17                  |
| Chelsea              | 20                  |
| Chinatown            | 12                  |
| City Island          | 22                  |


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
user_id INTEGER NOT NULL,
recipient_id INTEGER NOT NULL,
category TEXT NOT NULL,
content TEXT NOT NULL,
date TEXT NOT NULL,
visible BOOLEAN NOT NULL,
FOREIGN KEY (user_id) REFERENCES users(user_id),
FOREIGN KEY (recipient_id) REFERENCES users(user_id)
);

.mode csv
.headers on
.separator ","
.import data/posts.csv posts --skip 1;
.import data/users.csv users --skip 1; 


```

 
### SQL queries
- the SQL code to create each of the required tables
- a link to each of the practice CSV data files in the data directory.
- the SQLite code to import the practice CSV data files into the tables.

1. Register a new User.
``` sql
INSERT INTO users (email,password,handle) values("dt2211@nyu.edu","123notrealpas!","daniellat");
```

2. Create a new Message sent by a particular User to a particular User (pick any two Users for example).
``` sql
INSERT INTO posts (user_id, recipient_id,category,content,date,visible)values(1,1001,"message","hi this is a message",strftime('%Y-%m-%d %H:%M:%S', 'now'), "TRUE");
```

3. Create a new Story by a particular User (pick any User for example).
``` sql
INSERT INTO posts (user_id, recipient_id,category,content,date,visible)values(1001,"public","story","hi this is a story",strftime('%Y-%m-%d %H:%M:%S', 'now'), "TRUE");
```

4. Show the 10 most recent visible Messages and Stories, in order of recency.
``` sql
SELECT * FROM posts WHERE visible="TRUE" order by date desc limit 10;

```
> **Results**: 

| post_id | user_id | recipient_id | category |                           content                            |        date         | visible |
|---------|---------|--------------|----------|--------------------------------------------------------------|---------------------|---------|
| 919     | 243     | 434          | message  | Proin leo odio, porttitor id, consequat in, consequat ut, nu | 2024-02-26 23:51:41 | TRUE    |
|         |         |              |          | lla.                                                         |                     |         |
| 618     | 927     | 559          | message  | Quisque erat eros, viverra eget, congue eget, semper rutrum, | 2024-02-26 23:37:45 | TRUE    |
|         |         |              |          |  nulla.                                                      |                     |         |
| 191     | 603     | 288          | message  | Donec vitae nisi.                                            | 2024-02-26 22:33:36 | TRUE    |
| 292     | 916     | 756          | message  | Donec vitae nisi.                                            | 2024-02-26 22:08:02 | TRUE    |
| 1521    | 69      | public       | story    | Proin risus.                                                 | 2024-02-26 21:32:45 | TRUE    |
| 1044    | 499     | public       | story    | Nunc rhoncus dui vel sem.                                    | 2024-02-26 21:25:47 | TRUE    |
| 196     | 190     | 167          | message  | Curabitur in libero ut massa volutpat convallis.             | 2024-02-26 21:20:09 | TRUE    |
| 684     | 768     | 733          | message  | Curabitur in libero ut massa volutpat convallis.             | 2024-02-26 20:53:55 | TRUE    |
| 1485    | 560     | public       | story    | Vestibulum rutrum rutrum neque.                              | 2024-02-26 20:42:08 | TRUE    |
| 1368    | 231     | public       | story    | Nullam orci pede, venenatis non, sodales sed, tincidunt eu,  | 2024-02-26 20:30:27 | TRUE    |
|         |         |              |          | felis.                                                       |                     |         |



5. Show the 10 most recent visible Messages sent by a particular User to a particular User (pick any two Users for example), in order of recency.
``` sql
SELECT * FROM posts WHERE user_id=243 AND recipient_id=434 AND category="message" order by date desc limit 10; 
```
> **Results**: 

| post_id | user_id | recipient_id | category |                           content                            |        date         | visible |
|---------|---------|--------------|----------|--------------------------------------------------------------|---------------------|---------|
| 919     | 243     | 434          | message  | Proin leo odio, porttitor id, consequat in, consequat ut, nu | 2024-02-26 23:51:41 | TRUE    |
|         |         |              |          | lla.                                                         |                     |         |

6. Make all Stories that are more than 24 hours old invisible.
``` sql
UPDATE posts SET visible="FALSE" where category="story" AND ROUND((JULIANDAY('now') - JULIANDAY(date)) * 24) >= 24;
```

7. Show all invisible Messages and Stories, in order of recency.
``` sql
SELECT * FROM posts WHERE visible="FALSE" order by date desc;
```
> **Results**: 

| post_id | user_id | recipient_id | category |                           content                            |        date         | visible |
|---------|---------|--------------|----------|--------------------------------------------------------------|---------------------|---------|
| 399     | 867     | 165          | message  | Nullam sit amet turpis elementum ligula vehicula consequat.  | 2024-02-26 23:58:53 | FALSE   |
| 711     | 398     | 557          | message  | Donec ut mauris eget massa tempor convallis.                 | 2024-02-26 23:51:16 | FALSE   |
| 1685    | 104     | public       | story    | Curabitur at ipsum ac tellus semper interdum.                | 2024-02-26 23:39:23 | FALSE   |
| 332     | 66      | 704          | message  | Proin eu mi.                                                 | 2024-02-26 23:17:13 | FALSE   |
| 686     | 250     | 823          | message  | Cras pellentesque volutpat dui.                              | 2024-02-26 23:04:05 | FALSE   |
| 952     | 983     | 965          | message  | Vivamus vestibulum sagittis sapien.                          | 2024-02-26 23:00:05 | FALSE   |
| 1699    | 355     | public       | story    | Vivamus metus arcu, adipiscing molestie, hendrerit at, vulpu | 2024-02-26 22:53:52 | FALSE   |
|         |         |              |          | tate vitae, nisl.                                            |                     |         |

8. Show the number of posts by each User.
``` sql
SELECT user_id,count(user_id) as NumberOfPosts FROM posts group by user_id;
```
> **Results**: 

| user_id | NumberOfPosts |
|---------|---------------|
| 1       | 3             |
| 2       | 1             |
| 3       | 1             |
| 4       | 3             |
| 5       | 2             |
| 6       | 2             |


9. Show the post text and email address of all posts and the User who made them within the last 24 hours.
``` sql
SELECT p.user_id, p.content AS post_text, u.email FROM posts p JOIN users u ON p.user_id = u.user_id WHERE ROUND((JULIANDAY('now') - JULIANDAY(date)) * 24) < 24;
```
> **Results**: 

| user_id |                          post_text                           |                 email                 |
|---------|--------------------------------------------------------------|---------------------------------------|
| 226     | Mauris enim leo, rhoncus sed, vestibulum sit amet, cursus id | tsimson69@wiley.com                   |
|         | , turpis.                                                    |                                       |
| 441     | Quisque arcu libero, rutrum ac, lobortis vel, dapibus at, di | celveyc8@microsoft.com                |
|         | am.                                                          |                                       |
| 868     | Morbi odio odio, elementum eu, interdum eu, tincidunt in, le | apledgeo3@pcworld.com                 |
|         | o.                                                           |                                       |
| 376     | Donec ut dolor.                                              | rbeiderbeckeaf@twitpic.com            |


10. Show the email addresses of all Users who have not posted anything yet.
``` sql
SELECT u.email FROM users u LEFT JOIN posts p ON p.user_id = u.user_id WHERE p.post_id is NULL;

```
> **Results**: 

	| uboagq8@vk.com                     |
	| pbodleighqf@yellowbook.com         |
	| mcardooqh@ycombinator.com          |
	| mruleqj@eepurl.com                 |
	| tcrollaqr@intel.com                |
	| cchoffinqv@aboutads.info           |
	| vkeetleyqx@tinypic.com             |
	| emieviller1@yelp.com               |
