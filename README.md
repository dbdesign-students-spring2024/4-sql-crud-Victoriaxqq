# SQL CRUD

Here is my mock [dataset]()

## SQL code of Restaurant finder
### Create restaurants table
.mode csv
CREATE TABLE restaurant (
    id INTEGER PRIMARY KEY,
    category TEXT,
    price_tier TEXT,
    neighborhood TEXT,
    opening_hours TEXT,
    average_rating INTEGER,
    good_for_kids BOOLEAN
);
.import '/Users/quanquanxie/Documents/GitHub/4-sql-crud-Victoriaxqq/data/restaurants_no_header.csv' restaurant
### Create reviews table
CREATE TABLE reviews (
    id INTEGER PRIMARY KEY,
    restaurant_id INTEGER,
    comment TEXT,  
    rating INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (restaurant_id) REFERENCES restaurant(id)
); 
### Query 1 Find all cheap restaurants in a particular neighborhood (pick any neighborhood as an example
SELECT *
FROM restaurant
WHERE neighborhood = 'Oakwood' AND price_tier = 'cheap';
### Query 2 Find all restaurants in a particular genre (pick any genre as an example) with 3 stars or more, ordered by the number of stars in descending order
SELECT *
FROM restaurant
WHERE category = 'Greek' AND average_rating >= 3
ORDER BY average_rating DESC;
### Query 3 Find all restaurants that are open now (see hint below)
SELECT strftime('%H:%M', 'now', 'utc') AS current_time_utc;
SELECT *
FROM restaurant
WHERE strftime('%H:%M', 'now', 'utc') BETWEEN substr(opening_hours, 1, 5) AND substr(opening_hours, 6, 5)
  AND average_rating >= 3;
### Query 4 Leave a review for a restaurant (pick any restaurant as an example; note that leaving a review has no automatic effect on the average rating of the restaurant)
INSERT INTO reviews (restaurant_id, comment, rating)
VALUES (1, 'Great experience!', 5);
### Query 5 Delete all restaurants that are not good for kids
DELETE FROM restaurant
WHERE good_for_kids = FALSE;
### Query 6 Find the number of restaurants in each NYC neighborhood
SELECT neighborhood, COUNT(*) AS restaurant_count
FROM restaurant
GROUP BY neighborhood;

## SQL code of Social media app

Here is my mock [dataset]() of user
Here is my mock [dataset]() of post

### Create users table
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    email TEXT UNIQUE,
    password TEXT,
    handle TEXT UNIQUE
);
.import '/Users/quanquanxie/Documents/GitHub/4-sql-crud-Victoriaxqq/data/user.csv' user

### Create posts table
CREATE TABLE posts (
    post_id INT PRIMARY KEY,
    user_id_sender INT NOT NULL,
    user_id_receiver INT,
    content TEXT NOT NULL,
    post_type VARCHAR(10) NOT NULL, -- Change the size as needed
    visibility BOOLEAN DEFAULT true,
    creation_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

### Query 1 Register a new User
INSERT INTO users (email, password, handle)
VALUES ('AAA@user.com', 'AAAAA', 'userA'); 

###
INSERT INTO Posts (UserFrom, UserTo, ContentType, Content)
VALUES ('User1', 'User2', 'Message', 'This is a new message.');