# Gaming Database and Analytics (MySQL)

## Project Overview

Gaming Database and Analytics is a MySQL database project that stores and analyzes information about gamers, video games, genres, gaming platforms, reviews, and achievements.

The project uses SQL queries to identify highly rated games, analyze player reviews, filter games by genre and platform, summarize achievements earned by gamers, and compare game ratings based on available review data.

**Database:** `GamingDatabase` *(change this if your actual database name is different)*  
**Technology:** MySQL 8.0+  
**Tool:** MySQL Workbench  
**Core tables:** 8

## Project Aim

The main aim of Gaming Database and Analytics is to organize gaming-related information using MySQL and analyze data about games, gamers, genres, platforms, reviews, and achievements. The system helps identify highly rated games, explore game categories and supported platforms, analyze player feedback, and track achievements earned by gamers.

## Objectives

- Store and manage gamer information.
- Maintain video game details, including developers, release dates, prices, and age rates.
- Organize games by genre.
- Identify the platforms on which games are available.
- Record and analyze gamer reviews and ratings.
- Calculate average ratings and total reviews for games.
- Track achievements earned by gamers.
- Demonstrate SQL concepts such as joins, indexes, views, stored procedures, and triggers.

## Database Tables

| **Table** | **Purpose** |
|---|---|
| `gamers` | Stores gamer names, usernames, email addresses, countries, and registration dates. |
| `games` | Stores game titles, developer names, release dates, prices, and age rates. |
| `genres` | Stores genre names and descriptions. |
| `platforms` | Stores gaming platform names and manufacturers. |
| `game_reviews` | Stores gamer reviews, ratings, review text, and review dates. |
| `game_genres` | Connects games with their associated genres. |
| `game_platforms` | Connects games with their supported gaming platforms. |
| `achievements` | Stores achievements earned by gamers in particular games. |

## Important Attributes

### 1. gamers

- `gamer_id` — Primary Key (PK)
- `gamer_name`
- `gamer_username` — Unique
- `gamer_email` — Unique
- `country`
- `registration_date`

### 2. games

- `game_id` — PK
- `game_title`
- `developer_name`
- `release_date`
- `price`
- `age_rate`

### 3. genres

- `genre_id` — PK
- `genre_name` — Unique
- `genre_description`

### 4. platforms

- `platform_id` — PK
- `platform_name` — Unique
- `manufacturer`

### 5. game_reviews

- `review_id` — PK
- `gamer_id` — Foreign Key (FK)
- `game_id` — FK
- `rating`
- `review_text`
- `review_date`

### 6. game_genres

- `game_id` — FK
- `genre_id` — FK
- Composite Primary Key: (`game_id`, `genre_id`)

### 7. game_platforms

- `game_id` — FK
- `platform_id` — FK
- Composite Primary Key: (`game_id`, `platform_id`)

### 8. achievements

- `achievement_id` — PK
- `gamer_id` — FK
- `game_id` — FK
- `achievement_name`

**Note:** The `game_genres` and `game_platforms` tables implement many-to-many relationships. The `achievements` table records achievements earned by gamers rather than maintaining a separate catalogue of all possible achievements.

## Entity Relationships

- `gamers` → `game_reviews`: One-to-many (1:N)
- `games` → `game_reviews`: One-to-many (1:N)
- `games` → `game_genres`: One-to-many (1:N)
- `genres` → `game_genres`: One-to-many (1:N)
- `games` → `game_platforms`: One-to-many (1:N)
- `platforms` → `game_platforms`: One-to-many (1:N)
- `gamers` → `achievements`: One-to-many (1:N)
- `games` → `achievements`: One-to-many (1:N)

PK means Primary Key, FK means Foreign Key, and 1:N means one-to-many.

## Main SQL Analyses

- Display gamer names and usernames.
- Identify games priced above a specified amount.
- Sort games by release date.
- Calculate the average rating and total number of reviews for each game.
- Display games alongside their genres.
- Identify the platforms associated with each game.
- Count achievements earned by each gamer.
- Find the top three games by average rating.
- Filter games with average ratings above a specified value.
- Retrieve detailed review information, including gamer names, game titles, ratings, and review dates.

The rating analyses depend on the reviews available in the database. Games without reviews can be included in summary queries using a `LEFT JOIN`, but they do not have an average rating when no ratings are recorded.

## SQL Concepts Used

- `CREATE DATABASE` and `CREATE TABLE`
- Primary keys, foreign keys, and constraints
- `INSERT`, `UPDATE`, and `DELETE`
- `SELECT`, `WHERE`, and `ORDER BY`
- `ALTER TABLE`
- `INNER JOIN` and `LEFT JOIN`
- `GROUP BY` and `HAVING`
- `COUNT()`, `SUM()`, and `AVG()`
- `ROUND()` and `LIMIT`
- Indexes
- Views
- Stored procedures
- Triggers and audit records

## Indexes

The project includes indexes on selected columns to support common filtering and lookup operations.

- `idx_gamers_country` — `gamers.country`
- `idx_games_title` — `games.game_title`
- `idx_genres_name` — `genres.genre_name`
- `idx_platforms_name` — `platforms.platform_name`
- `idx_reviews_rating` — `game_reviews.rating`
- `idx_game_genres_genre` — `game_genres.genre_id`
- `idx_game_platforms_platform` — `game_platforms.platform_id`
- `idx_achievements_gamer` — `achievements.gamer_id`

**Note:** The `genre_name` and `platform_name` columns are already declared `UNIQUE`. MySQL creates indexes for these constraints, so separate indexes on the same columns may be redundant.

## Views

The project includes the following views for retrieving and summarizing gaming data.

| **View** | **Purpose** |
|---|---|
| `view_game_details` | Displays game details, including title, developer, release date, price, and age rate. |
| `view_gamer_reviews` | Displays gamer names, game titles, ratings, review text, and review dates. |
| `view_game_rating_summary` | Calculates the total reviews and average rating for each game. |
| `view_gamer_achievement_summary` | Displays the number of achievements earned by each gamer. |

Example:

```sql
SELECT * FROM view_game_details;

SELECT * FROM view_gamer_reviews;

SELECT * FROM view_game_rating_summary;

SELECT * FROM view_gamer_achievement_summary;
```

## Stored Procedures

The project includes four stored procedures.

| **Procedure** | **Purpose** |
|---|---|
| `sp_games_by_genre` | Retrieves games belonging to a specified genre. |
| `sp_game_rating_summary` | Calculates the number of reviews and average rating for a specified game. |
| `sp_add_game_review` | Adds a game review after validating that the rating is between 1 and 10. |
| `sp_achievements_by_gamer` | Retrieves achievements earned by a specified gamer. |

Example calls:

```sql
CALL sp_games_by_genre(1);

CALL sp_game_rating_summary(101);

CALL sp_achievements_by_gamer(1);
```

To add a review:

```sql
CALL sp_add_game_review(
    7,
    1,
    101,
    9,
    'Excellent gameplay and graphics',
    '2026-10-25'
);
```

**Note:** The `sp_add_game_review` procedure accepts a review ID because `review_id` is a primary key without `AUTO_INCREMENT`. Ensure that the specified ID is unused before calling the procedure.

## Triggers and Audit Records

The project uses triggers to record selected changes to game reviews in an audit table.

### Audit Table: `game_review_audit`

The audit table stores details about review changes.

- `audit_id` — Primary Key with `AUTO_INCREMENT`
- `review_id` — ID of the affected review
- `action_type` — Type of operation, such as `UPDATE` or `DELETE`
- `old_rating` — Previous rating, when applicable
- `new_rating` — Updated rating, when applicable
- `changed_at` — Date and time of the audit record

### Triggers

- `trg_audit_review_update` — An `AFTER UPDATE` trigger that records rating changes when the old and new ratings differ.
- `trg_audit_review_delete` — An `AFTER DELETE` trigger that records the rating of a deleted review.

These triggers demonstrate how MySQL can automatically record selected database changes for auditing purposes.

## How to Run the Project

1. Install MySQL Server 8.0 or later.
2. Open MySQL Workbench and connect to your MySQL server.
3. Open your SQL script containing the database and table definitions.
4. Create the database and select it using `USE`.
5. Execute the `CREATE TABLE` statements in the correct order.
6. Insert the sample data into the tables.
7. Create the indexes.
8. Create the views.
9. Create the stored procedures.
10. Create the audit table and triggers.
11. Execute the SQL queries and test the views, procedures, and triggers.

### Recommended Table Creation Order

1. `gamers`
2. `games`
3. `genres`
4. `platforms`
5. `game_reviews`
6. `game_genres`
7. `game_platforms`
8. `achievements`

Create the referenced tables before creating tables that depend on their primary keys.

For the `games` table, use `game_id` as the primary key from the beginning. This avoids the need to rename the original `gamer_id` column after table creation.

To verify the database and tables, execute:

```sql
USE GamingDatabase;

SHOW TABLES;
```

Replace `GamingDatabase` if you chose a different database name.

## Limitations

- The sample data is illustrative and does not represent a complete gaming industry dataset.
- The analysis depends on the reviews, games, and achievements available in the database.
- Average ratings may be less representative when a game has very few reviews.
- The schema is simplified for a student database project.
- The achievements table records earned achievements and does not contain a separate catalogue of every possible achievement.
- The project analyzes stored records and does not predict game popularity or future ratings.

## Future Enhancements

- Add more games, gamers, genres, and platforms.
- Expand the dataset with additional reviews and achievements.
- Add queries for the highest-priced games and most-reviewed games.
- Analyze ratings by genre and platform.
- Add review validation constraints to the database schema.
- Create a dashboard to display game ratings, review counts, and player statistics.
- Develop an application interface to interact with the MySQL database.

## Conclusion

Gaming Database and Analytics demonstrates how MySQL can be used to organize and analyze gaming-related data. It combines relational database design with practical SQL queries, joins, aggregate functions, views, stored procedures, indexes, and triggers.

The project provides hands-on experience with database management and SQL analytics while creating a foundation for further database development.
