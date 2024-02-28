# SQL CRUD

Here is my mock dataset["]

## SQL code
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