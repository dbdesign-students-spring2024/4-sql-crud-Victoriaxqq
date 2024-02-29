# SQL CRUD

Here is my mock [dataset](https://github.com/dbdesign-students-spring2024/4-sql-crud-Victoriaxqq/blob/505a7b22c0b1ead9f2264ee719fc19b05ce28c5c/data/restaurants.csv)

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

Here is my mock [dataset](https://github.com/dbdesign-students-spring2024/4-sql-crud-Victoriaxqq/blob/505a7b22c0b1ead9f2264ee719fc19b05ce28c5c/data/user.csv) of user
Here is my mock [dataset](https://github.com/dbdesign-students-spring2024/4-sql-crud-Victoriaxqq/blob/8701877c713caac4149336f3b003fe261b0bd751/data/posts.csv) of post

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
    id INTEGER PRIMARY KEY,
    id_sender INTEGER NOT NULL,
    id_receiver INTEGER,
    content TEXT NOT NULL,
    post_type TEXT NOT NULL, 
    visibility BOOLEAN DEFAULT true,
    creation_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
.import '/Users/quanquanxie/Documents/GitHub/4-sql-crud-Victoriaxqq/data/posts.csv' posts

### Query 1 Register a new User
INSERT INTO users (email, password, handle)
VALUES ('AAA@user.com', 'AAAAA', 'userA'); 

### Query 2 Create a new Message sent by a particular User to a particular User (pick any two Users for example)
INSERT INTO posts (id_sender, id_receiver, content, post_type)
VALUES (1, 2, 'Hello, how are you?', 'message');

### Query 3 Create a new Story by a particular User (pick any User for example)
INSERT INTO posts (id_sender, content, post_type)
VALUES (1, 'This is my story for today!', 'story');

### Query 4 Show the 10 most recent visible Messages and Stories, in order of recency 
SELECT posts.id, posts.id_sender, posts.id_receiver, posts.content, posts.post_type, posts.visibility, posts.creation_time
FROM posts
JOIN users ON posts.id_sender = users.id
WHERE posts.visibility = true
ORDER BY posts.creation_time DESC
LIMIT 10;

### Query 5 Show the 10 most recent visible Messages sent by a particular User to a particular User (pick any two Users for example), in order of recency
SELECT posts.id, posts.id_sender, posts.id_receiver, posts.content, posts.post_type, posts.visibility, posts.creation_time
FROM posts
WHERE posts.visibility = true
  AND posts.post_type = 'message'
  AND posts.id_sender = 1
  AND posts.id_receiver = 2
ORDER BY posts.creation_time DESC
LIMIT 10;

### Query 6 Make all Stories that are more than 24 hours old invisible
UPDATE posts
SET visibility = false
WHERE post_type = 'story'
  AND TIMESTAMPDIFF(HOUR, creation_time, CURRENT_TIMESTAMP) > 24;

### Query 7 Show all invisible Messages and Stories, in order of recency
SELECT *
FROM posts
WHERE visibility = false
ORDER BY creation_time DESC;

### Query 8 Show the number of posts by each User
SELECT COALESCE(id_sender, id_receiver) AS user_id, COUNT(*) AS post_count
FROM posts
GROUP BY user_id;

### Query 9 Show the post text and email address of all posts and the User who made them within the last 24 hours
SELECT posts.content AS post_text, users.email, posts.creation_time
FROM posts
JOIN users ON posts.id_sender = users.id
WHERE posts.creation_time >= CURRENT_TIMESTAMP - INTERVAL 24 HOUR;

### Query 10 Show the email addresses of all Users who have not posted anything yet
SELECT users.email
FROM users
LEFT JOIN posts ON users.id = posts.id_sender
WHERE posts.post_id IS NULL;




