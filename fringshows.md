## SQL Questions

First create a database called fringe_shows
```
  #terminal
  psql
  create database fringe_shows;
  \q
```

Populate the data using the script - fringeshows.sql
```
  #terminal
  psql -d fringe_shows -f fringeshows.sql
```

Using the SQL Database file given to you as the source of data to answer the questions.  Copy the SQL command you have used to get the answer, and paste it below the question, along with the result they gave.


## Section 1

  Revision of concepts that we've learnt in SQL today

  1. Select the names of all users.
    ```
    SELECT name FROM users;
    ```

  2. Select the names of all shows that cost less than £15.
    ```
    SELECT price FROM shows WHERE price <= 15;
    ```

  3. Insert a user with the name "Val Gibson" into the users table.
    ```
    INSERT INTO 'users' name VALUES ('Val Gibson');
    ```

  4. Select the id of the user with your name.
    ```
    SELECT name, id FROM users WHERE name = 'Alistair MacKay';
    ```

  5. Insert a record that Val Gibson wants to attend the show "Two girls, one cup of comedy".
    ```
    INSERT INTO "shows_users" (show_id, user_id) VALUES (12, 23);
    ```

  6. Updates the name of the "Val Gibson" user to be "Valerie Gibson".
    ```
    UPDATE users SET name = 'Val Gibson' WHERE name = 'Valerie Gibson';
    ```

  7. Deletes the user with the name 'Valerie Gibson'.
    ```
    DELETE FROM users WHERE name = 'Valerie Gibson';
    ```

  8. Deletes the shows for the user you just deleted.
    ```
    DELETE FROM shows_users WHERE show_id = 1 AND user_id = 1;
    ```


## Section 2

  This section involves more complex queries.  You will need to go and find out about aggregate funcions in SQL to answer some of the next questions.

  9. Select the names and prices of all shows, ordered by price in ascending order.
    ```
    SELECT name, price FROM shows ORDER BY price ASC;
    ```

  10. Select the average price of all shows.
    ```
    SELECT avg(price) FROM shows;
    ```

  11. Select the price of the least expensive show.
    ```
    SELECT min(price) FROM shows;   
    ```

  12. Select the sum of the price of all shows.
    ```
    SELECT sum(price) FROM shows;
    ```


  13. Select the sum of the price of all shows whose prices is less than £20.
    ```
    SELECT sum(price) FROM shows WHERE price <= 20;
    ```

  14. Select the name and price of the most expensive show.
    >The below answer works but it doesn't utilise max(price)
    ```
    SELECT name, price FROM shows WHERE price >= 32.99;
    ```
    >after reading the practice of combining ORDER BY DESC and LIMIT. http://www.techonthenet.com/sql/select_limit.php
    ```
    SELECT name, price FROM shows ORDER BY price DESC LIMIT 1;
    ```


  15. Select the name and price of the second from cheapest show.
    ```
    SELECT name, price FROM shows ORDER BY price ASC LIMIT 1 OFFSET 1;
    ```



  16. Select the names of all users whose names start with the letter "A".
    ```
    SELECT name FROM users WHERE name LIKE '%A%';
    ```

  17. Select the names of users whose names contain "el".
    ```
    SELECT name FROM users WHERE name LIKE '%el%';
    ```


## Section 3

  The following questions can be answered by using nested SQL statements but ideally you should learn about JOIN clauses to answer them.

  18. Select the time for the Edinburgh Royal Tattoo.

  show your working:
    ```
    SELECT id, name FROM shows;
      id |                  name                   
      ----+-----------------------------------------
      1 | Le Haggis
      2 | Shitfaced Shakespeare
      3 | Camille O'Sullivan
      4 | Game of Thrones - The Musical
      5 | Paul Dabek Mischief
      6 | Joe Stilgoe: Songs on Film – The Sequel
      7 | Aaabeduation – A Magic Show
      >>>8 | Edinburgh Royal Tattoo
      9 | Best of Burlesque
      10 | Two become One
      11 | Urinetown
      12 | Two girls, one cup of comedy
      13 | Balletronics
    ```

    ```
    SELECT id, time, show_id FROM times;

        id | time  | show_id
      ----+-------+---------
       1 | 13:30 |       1
       2 | 19:30 |       2
       3 | 17:15 |       3
       4 | 19:30 |       4
       5 | 12:45 |       5
       6 | 17:15 |       6
       7 | 12:45 |       7
       >>>8 | 22:00 |       8
       9 | 19:30 |       9
      10 | 14:15 |      10
      11 | 20:00 |      11
      12 | 12:45 |      12
      13 | 20:00 |      13
      (13 rows)
    ```

> Combined with my reading of the following site: http://www.tutorialspoint.com/sql/sql-using-joins.htm
> we get the following statement.
>but it's kind of cheating as it
    ```
    SELECT shows.id, name, time  FROM shows, times WHERE shows.id = times.show_id ORDER BY time DESC LIMIT 1;
    ```
>Attempt 2 - Using INNER JOIN (No venn diagrams needed)
    ```
    SELECT shows.id, name, time  FROM shows INNER JOIN times ON shows.id = times.show_id WHERE shows.id = 8;
    ```
>Attempt 2.5 - just messing around.
    ```
    fringe_shows=# SELECT shows.id, name, time  FROM shows INNER JOIN times ON shows.id = times.show_id WHERE name LIKE '%Edinburgh%';
    ```

  19. Select the number of users who want to see "Le Haggis".

>Note sure if correct as I would have ideally preferred to present the show name next to the count.
    ```
    SELECT COUNT(show_id) FROM shows_users INNER JOIN shows ON shows.id = shows_users.show_id;
    ```

  20. Select all of the user names and the count of shows they're going to see.
  

  21. SELECT all users who are going to a show at 13:30.


## Hints

  - As with anything, if you get stuck, move on, then go back if you have time.
  - Don't spend all night on it!
  - Use resources online to solve harder ones - there are solutions to these questions that work with what we've learnt today, but other tools exist in SQL that could make the queries 'better' or 'easier'.
