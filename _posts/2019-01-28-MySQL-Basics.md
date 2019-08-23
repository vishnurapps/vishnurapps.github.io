The USE command is used to load a database
eg:		USE imdb;

To see the tables present in the database we can use the SHOW TABLES command.
```sql
mysql> SHOW TABLES;
+------------------+
| Tables_in_imdb   |
+------------------+
| actors           |
| directors        |
| directors_genres |
| movies           |
| movies_directors |
| movies_genres    |
| roles            |
+------------------+
7 rows in set (0.00 sec)
```

To see the datatypes in one table, we can use the DESCRIBE keyword.

DESCRIBE movies;

mysql> DESCRIBE movies;
+-----------+--------------+------+-----+---------+-------+
| Field     | Type         | Null | Key | Default | Extra |
+-----------+--------------+------+-----+---------+-------+
| id        | int(11)      | NO   | PRI | 0       |       |
| name      | varchar(100) | YES  | MUL | NULL    |       |
| year      | int(11)      | YES  |     | NULL    |       |
| rankscore | float        | YES  |     | NULL    |       |
+-----------+--------------+------+-----+---------+-------+
4 rows in set (0.00 sec)

Here the above data means the movies table has 4 columns (id, name, year and rankscore). id is integer having maximum size 11 bytes name is char having 100 bytes year is interger having 4 bytes and rankscore is a floating point number.

We can use the SELECT command to select the columns of a database.

mysql> SELECT *from movies;

+--------+------------------------------------------------------------------------------------------------------+------+-----------+
| id     | name                                                                                                 | year | rankscore |
+--------+------------------------------------------------------------------------------------------------------+------+-----------+
|      0 | #28                                                                                                  | 2002 |      NULL |
|      1 | #7 Train: An Immigrant Journey, The                                                                  | 2000 |      NULL |
|      2 | $                                                                                                    | 1971 |       6.4 |
|      3 | $1,000 Reward                                                                                        | 1913 |      NULL |

.....................................

| 412317 | "rgammk"                                                                                             | 1995 |      NULL |
| 412318 | "zgnm Leyla"                                                                                         | 2002 |      NULL |
| 412319 | " Istanbul"                                                                                          | 1983 |      NULL |
| 412320 | "sterreich"                                                                                          | 1958 |      NULL |
+--------+------------------------------------------------------------------------------------------------------+------+-----------+
388269 rows in set (0.25 sec)

The above command have shown all the columns of the movie table. In reallity there will be 100s of columns in a database. So the SELECT * is not that useful. We can specify the name of the columns that we want to see.

mysql> SELECT name,year FROM movies;
+------------------------------------------------------------------------------------------------------+------+
| name                                                                                                 | year |
+------------------------------------------------------------------------------------------------------+------+
| #28                                                                                                  | 2002 |
| #7 Train: An Immigrant Journey, The                                                                  | 2000 |
| $                                                                                                    | 1971 |
| $1,000 Reward                                                                                        | 1913 |

........................

| "zgnm Leyla"                                                                                         | 2002 |
| " Istanbul"                                                                                          | 1983 |
| "sterreich"                                                                                          | 1958 |
+------------------------------------------------------------------------------------------------------+------+
388269 rows in set (0.19 sec)


While executing the SELECT command, the row order in the database is not changed.

In a realworld dataset, there will be tens of thousands of rows and there is no point in listing all of them like we have done above. To see some rows, we can use the LIMIT command.

mysql> SELECT name,rankscore FROM movies LIMIT 10;
+-------------------------------------+-----------+
| name                                | rankscore |
+-------------------------------------+-----------+
| #28                                 |      NULL |
| #7 Train: An Immigrant Journey, The |      NULL |
| $                                   |       6.4 |
| $1,000 Reward                       |      NULL |
| $1,000 Reward                       |      NULL |
| $1,000 Reward                       |      NULL |
| $1,000,000 Duck                     |         5 |
| $1,000,000 Reward, The              |      NULL |
| $10,000 Under a Pillow              |      NULL |
| $100,000                            |      NULL |
+-------------------------------------+-----------+
10 rows in set (0.00 sec)

If we want to see 10 movies after this, we have to use the OFFSET command.

mysql> SELECT name,rankscore FROM movies LIMIT 10 OFFSET 10;
+------------------------+-----------+
| name                   | rankscore |
+------------------------+-----------+
| $100,000 Pyramid, The  |      NULL |
| $1000 a Touchdown      |       6.7 |
| $20,000 Carat, The     |      NULL |
| $21 a Day Once a Month |      NULL |
| $2500 Bride, The       |      NULL |
| $30                    |       7.5 |
| $30,000                |      NULL |
| $300 y tickets         |      NULL |
| $40,000                |       9.6 |
| $5,000 Reward          |      NULL |
+------------------------+-----------+
10 rows in set (0.00 sec)

The number of rows specified after the OFFSET is ingnored. If I say OFFSET 15, then the first 15 rows are not considered. If the LIMIT is 5 and OFFSET is 15, the 5 from after 15th row is shown.

If we want to sort the data in the database, we have to use the ODERBY command.

mysql> SELECT name,rankscore,year FROM movies ORDER BY year DESC LIMIT 10;
+-------------------------------------------+-----------+------+
| name                                      | rankscore | year |
+-------------------------------------------+-----------+------+
| Harry Potter and the Half-Blood Prince    |      NULL | 2008 |
| Tripoli                                   |      NULL | 2007 |
| War of the Red Cliff, The                 |      NULL | 2007 |
| Rapunzel Unbraided                        |      NULL | 2007 |
| Spider-Man 3                              |      NULL | 2007 |
| Untitled Star Trek Prequel                |      NULL | 2007 |
| DragonBall Z                              |      NULL | 2007 |
| Harry Potter and the Order of the Phoenix |      NULL | 2007 |
| Andrew Henry's Meadow                     |      NULL | 2006 |
| American Rain                             |      NULL | 2006 |
+-------------------------------------------+-----------+------+
10 rows in set (0.15 sec)

The above command will show us three columns (name, rankscore and year) from movies table. It will order the data in the descending order of year and show the first 10 in that.

If we dont specify anything after the ORDER BY command, then the sorting will be done in ascending order.

mysql> SELECT name,rankscore,year FROM movies ORDER BY year LIMIT 10;
+--------------------------------------+-----------+------+
| name                                 | rankscore | year |
+--------------------------------------+-----------+------+
| Roundhay Garden Scene                |      NULL | 1888 |
| Traffic Crossing Leeds Bridge        |      NULL | 1888 |
| Monkeyshines, No. 2                  |      NULL | 1890 |
| Monkeyshines, No. 1                  |       7.3 | 1890 |
| Monkeyshines, No. 3                  |      NULL | 1890 |
| Duncan Smoking                       |       3.6 | 1891 |
| Newark Athlete                       |       4.3 | 1891 |
| Duncan or Devonald with Muslin Cloud |       3.5 | 1891 |
| Monkey and Another, Boxing           |       3.2 | 1891 |
| Duncan and Another, Blacksmith Shop  |       3.5 | 1891 |
+--------------------------------------+-----------+------+
10 rows in set (0.15 sec)


The ORDER BY will not preserve the order in the database table.

If we want to print the unique value types in a column, we can use the DISTINCT keyword.

mysql> SELECT DISTINCT genre FROM movies_genres;
+-------------+
| genre       |
+-------------+
| Documentary |
| Short       |
| Comedy      |
| Crime       |
| Western     |
| Family      |
| Animation   |
| Drama       |
| Romance     |
| Mystery     |
| Thriller    |
| Adult       |
| Music       |
| Action      |
| Fantasy     |
| Sci-Fi      |
| Horror      |
| War         |
| Musical     |
| Adventure   |
| Film-Noir   |
+-------------+
21 rows in set (0.81 sec)

Here the list of genere  type availabe is printed.

mysql> SELECT DISTINCT first_name, last_name FROM directors;
+--------------------------------+-----------------------------+
| first_name                     | last_name                   |
+--------------------------------+-----------------------------+
| 'Chico'                        | Hernandez                   |
| 'Philthy' Phil                 | Phillips                    |
| 'Weird Al'                     | Yankovic                    |
| A.                             | Aleksandrov                 |
| A.                             | Babes                       |

..............

| Aaron                          | Blaise                      |
| Aaron                          | Crozier                     |
| Aaron                          | Downing                     |
| Aaron                          | Estrada                     |
| Aaron                          | Fishman                     |

.............. 

| Þór Elís                       | Pálsson                     |
| Þorsteinn                      | Jónsson                     |
| Þorvaldur                      | Kristinsson                 |
| Þráinn                         | Bertelsson                  |
+--------------------------------+-----------------------------+
86844 rows in set (0.85 sec)

Here the combination of firstname and lastname is taken for distinct command.

If we want to apply some logical coditions to the data in the table, we have to use the WHERE keyword.

mysql> SELECT name,year,rankscore FROM movies WHERE rankscore>9 ;
+------------------------------------------------------------------------+------+-----------+
| name                                                                   | year | rankscore |
+------------------------------------------------------------------------+------+-----------+
| $40,000                                                                | 1996 |       9.6 |
| +1 -1                                                                  | 1987 |       9.6 |
| 12 (2003/II)                                                           | 2003 |       9.8 |
| 12 stulyev                                                             | 1971 |       9.3 |

....................

| tat critique                                                           | 1992 |       9.1 |
| tre avec                                                               | 1996 |       9.4 |
| lm savasisi                                                            | 1984 |       9.7 |
| ltima llamada, La (1996/I)                                             | 1996 |       9.5 |
+------------------------------------------------------------------------+------+-----------+
1069 rows in set (0.12 sec)

The above command shows the movies which have rankscore greater than 9

mysql> SELECT name,year,rankscore FROM movies WHERE rankscore>9.5 ORDER BY rankscore DESC LIMIT 10;
+-----------------------------------------+------+-----------+
| name                                    | year | rankscore |
+-----------------------------------------+------+-----------+
| Clearing, The                           | 2001 |       9.9 |
| Dawn of the Friend                      | 2004 |       9.9 |
| Duminica la ora 6                       | 1965 |       9.9 |
| Distinto amanecer                       | 1943 |       9.9 |
| Duck Soup                               | 1942 |       9.9 |
| Devil's Circus, The                     | 1926 |       9.9 |
| Blow Job                                | 2002 |       9.9 |
| Atunci i-am condamnat pe toti la moarte | 1971 |       9.9 |
| Complex Sessions, The                   | 1994 |       9.9 |
| Dosti                                   | 1964 |       9.9 |
+-----------------------------------------+------+-----------+
10 rows in set (0.14 sec)

The above command shows 10 movies having rankscore greater than 9.5 in descending order.

mysql> SELECT * FROM movies_genres WHERE genre = 'Comedy' LIMIT 10;
+----------+--------+
| movie_id | genre  |
+----------+--------+
|        2 | Comedy |
|        6 | Comedy |
|        8 | Comedy |
|       11 | Comedy |
|       15 | Comedy |
|       18 | Comedy |
|       21 | Comedy |
|       28 | Comedy |
|       29 | Comedy |
|       35 | Comedy |
+----------+--------+
10 rows in set (0.00 sec)


mysql> SELECT * FROM movies_genres WHERE genre <> 'Horror' LIMIT 10;
+----------+-------------+
| movie_id | genre       |
+----------+-------------+
|        1 | Documentary |
|        1 | Short       |
|        2 | Comedy      |
|        2 | Crime       |
|        5 | Western     |
|        6 | Comedy      |
|        6 | Family      |
|        8 | Animation   |
|        8 | Comedy      |
|        8 | Short       |
+----------+-------------+
10 rows in set (0.00 sec)

In SQL, <> means not equal. So the above query was to show 10 movies whose genre is not horror.

Another important point is the = wont work with NULL.

mysql> SELECT name,year,rankscore FROM movies WHERE rankscore = NULL;
Empty set (0.11 sec)

Here the output was empty eventhough there are many movies having rankscore as NULL. To get the output in these situations we neeed to use the IS NULL operator.

mysql> SELECT name,year,rankscore FROM movies WHERE rankscore IS NULL LIMIT 10;
+-------------------------------------+------+-----------+
| name                                | year | rankscore |
+-------------------------------------+------+-----------+
| #28                                 | 2002 |      NULL |
| #7 Train: An Immigrant Journey, The | 2000 |      NULL |
| $1,000 Reward                       | 1913 |      NULL |
| $1,000 Reward                       | 1915 |      NULL |
| $1,000 Reward                       | 1923 |      NULL |
| $1,000,000 Reward, The              | 1920 |      NULL |
| $10,000 Under a Pillow              | 1921 |      NULL |
| $100,000                            | 1915 |      NULL |
| $100,000 Pyramid, The               | 2001 |      NULL |
| $20,000 Carat, The                  | 1913 |      NULL |
+-------------------------------------+------+-----------+
10 rows in set (0.00 sec)


mysql> SELECT name,year,rankscore FROM movies WHERE rankscore IS NOT NULL LIMIT 10;
+--------------------------+------+-----------+
| name                     | year | rankscore |
+--------------------------+------+-----------+
| $                        | 1971 |       6.4 |
| $1,000,000 Duck          | 1971 |         5 |
| $1000 a Touchdown        | 1939 |       6.7 |
| $30                      | 1999 |       7.5 |
| $40,000                  | 1996 |       9.6 |
| $50,000 Climax Show, The | 1975 |       2.6 |
| $pent                    | 2000 |       4.3 |
| $windle                  | 2002 |       5.4 |
| '15'                     | 2002 |       6.8 |
| '38                      | 1987 |       6.7 |
+--------------------------+------+-----------+
10 rows in set (0.00 sec)

mysql> SELECT name,year,rankscore FROM movies WHERE rankscore>9 AND year>2000 LIMIT 10;
+----------------------------+------+-----------+
| name                       | year | rankscore |
+----------------------------+------+-----------+
| 12 (2003/II)               | 2003 |       9.8 |
| 14 Million Dreams          | 2003 |       9.5 |
| 2+1                        | 2004 |       9.2 |
| 2wks, 1yr                  | 2002 |       9.4 |
| 5 Card Stud                | 2002 |       9.4 |
| Able's House Is Green, The | 2003 |       9.4 |
| Accordon                   | 2004 |       9.7 |
| Ai-Fak                     | 2004 |       9.4 |
| American Beer              | 2004 |       9.1 |
| American Exile             | 2001 |       9.1 |
+----------------------------+------+-----------+
10 rows in set (0.02 sec)


mysql> SELECT name,year,rankscore FROM movies WHERE NOT year<=2000 LIMIT 10;
+--------------------------------+------+-----------+
| name                           | year | rankscore |
+--------------------------------+------+-----------+
| #28                            | 2002 |      NULL |
| $100,000 Pyramid, The          | 2001 |      NULL |
| $300 y tickets                 | 2002 |      NULL |
| $5.15/Hr.                      | 2004 |      NULL |
| $windle                        | 2002 |       5.4 |
| '15'                           | 2002 |       6.8 |
| '60s Pop Rock Reunion          | 2004 |      NULL |
| '88 Dodge Aries                | 2002 |      NULL |
| 'Ang Galing galing mo, Babes'  | 2002 |      NULL |
| 'As se hizo' - Torremolinos 73 | 2003 |      NULL |
+--------------------------------+------+-----------+
10 rows in set (0.00 sec)


mysql> SELECT name,year,rankscore FROM movies WHERE rankscore>9 OR year>2007 LIMIT 10;
+-------------------+------+-----------+
| name              | year | rankscore |
+-------------------+------+-----------+
| $40,000           | 1996 |       9.6 |
| +1 -1             | 1987 |       9.6 |
| 12 (2003/II)      | 2003 |       9.8 |
| 12 stulyev        | 1971 |       9.3 |
| 14 Million Dreams | 2003 |       9.5 |
| 2+1               | 2004 |       9.2 |
| 2wks, 1yr         | 2002 |       9.4 |
| 36K               | 2000 |       9.5 |
| 5 Card Stud       | 2002 |       9.4 |
| 8  $              | 1999 |       9.2 |
+-------------------+------+-----------+
10 rows in set (0.00 sec)

BETWEEN keyword is used when we want to see things between two values. Make site that you give lower limit first and then the upper limit.

mysql> SELECT name,year,rankscore FROM movies WHERE year BETWEEN 1999 AND 2000 LIMIT 10;
+---------------------------------------------------------------+------+-----------+
| name                                                          | year | rankscore |
+---------------------------------------------------------------+------+-----------+
| #7 Train: An Immigrant Journey, The                           | 2000 |      NULL |
| $30                                                           | 1999 |       7.5 |
| $pent                                                         | 2000 |       4.3 |
| & frres                                                       | 2000 |      NULL |
| '60s, The                                                     | 1999 |      NULL |
| '70s: The Decade That Changed Television, The                 | 2000 |      NULL |
| 'Bats' Abound                                                 | 1999 |      NULL |
| 'Betty Bee' (sopravvivere d'arte)                             | 1999 |      NULL |
| 'Halloween' Unmasked 2000                                     | 1999 |      NULL |
| 'N Sync & Britney Spears: Your #1 Video Requests... And More! | 2000 |      NULL |
+---------------------------------------------------------------+------+-----------+
10 rows in set (0.00 sec)

If we give upper limit first and lower limit second, then the query returns empty.

mysql> SELECT name,year,rankscore FROM movies WHERE year BETWEEN 2000 AND 1999;
Empty set (0.12 sec)

mysql> SELECT director_id, genre FROM directors_genres WHERE genre IN ('Comedy','Horror') LIMIT 10;
+-------------+--------+
| director_id | genre  |
+-------------+--------+
|           8 | Comedy |
|          10 | Comedy |
|          12 | Comedy |
|          18 | Comedy |
|          22 | Comedy |
|          23 | Horror |
|          31 | Comedy |
|          37 | Comedy |
|          41 | Comedy |
|          42 | Comedy |
+-------------+--------+
10 rows in set (0.14 sec)


There is wildcard support in SQL. If we want to see all the movies starting with Tis, we can use the below command.

mysql> SELECT name,year,rankscore FROM movies WHERE name LIKE 'Tis%' LIMIT 10;
+-------------------------------------+------+-----------+
| name                                | year | rankscore |
+-------------------------------------+------+-----------+
| Tis a Gift to Be Simple             | 1994 |       8.2 |
| Tis an Ill Wind That Blows No Good  | 1909 |      NULL |
| Tis an Till Wind That Blows No Good | 1912 |      NULL |
| Tis kakomoiras                      | 1963 |       9.5 |
| Tis mias drakmis ta giasemia        | 1960 |      NULL |
| Tis moiras t' apopaidi              | 1925 |      NULL |
| Tis nyhtas ta kamomata              | 1957 |      NULL |
| Tis the Season                      | 1994 |      NULL |
| Tis the Season                      | 1998 |         9 |
| Tis tyhis ta grammena               | 1957 |      NULL |
+-------------------------------------+------+-----------+
10 rows in set (0.22 sec)

Here % means any number of letters after Tis. The below command shows first names that ends in es.

mysql> SELECT first_name, last_name FROM actors WHERE first_name LIKE '%es' LIMIT 10;
+--------------+-----------+
| first_name   | last_name |
+--------------+-----------+
| James        | 52X       |
| Tørres       | Aadland   |
| Charles      | Aaron     |
| Reyes        | Abades    |
| Jean-Jacques | Abadie    |
| James        | Abbott    |
| Georges      | Abe       |
| Jacques      | Abellira  |
| Artashes     | Abelyan   |
| Hovhannes    | Abelyan   |
+--------------+-----------+
10 rows in set (0.14 sec)

List of first names having es in them.

mysql> SELECT first_name, last_name FROM actors WHERE first_name LIKE '%es%' LIMIT 10;
+------------+--------------+
| first_name | last_name    |
+------------+--------------+
| Néstor     | 'Kick Boxer' |
| James      | 52X          |
| Tørres     | Aadland      |
| Jesper     | Aagaard      |
| Vesa       | Aaltonen     |
| Charles    | Aaron        |
| James (I)  | Aaron        |
| James (II) | Aaron        |
| César      | Abades       |
| Reyes      | Abades       |
+------------+--------------+
10 rows in set (0.00 sec)

The _ is used to match one character. 

mysql> SELECT first_name, last_name FROM actors WHERE first_name LIKE 'Agn_s' LIMIT 10;
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| Agnès      | Bouloche  |
| Agnes      | Wilke     |
| Agnes      | Adams     |
| Agnes      | Aker      |
| Agnès      | Akopian   |
| Agnès      | Alberny   |
| Ágnes      | Almády    |
| Agnès      | Andersen  |
| Agnes      | Anderson  |
| Agnès      | Aranis    |
+------------+-----------+
10 rows in set (0.14 sec)

mysql> SELECT first_name, last_name FROM actors WHERE first_name LIKE 'L%' AND first_name NOT LIKE 'Li%' LIMIT 10;
+------------+----------------+
| first_name | last_name      |
+------------+----------------+
| L          | Lover          |
| L'abbé     | Bernès         |
| L'Abbé     | Simon          |
| L'Ami      | Francis        |
| L'Carole   | Caffery        |
| L'nelle    | Hamanaka       |
| L'Oreal    | Bibbs          |
| L'Ronald   | Smith          |
| L'Tanya    | Van Hamersveld |
| L.         | Alaverdyan     |
+------------+----------------+
10 rows in set (0.00 sec)

If we want to use % and _ in the string we should use escape characters.

The Aggregate functions are used to calculate a single values from all the rows. The examples are COUNT, MIN, MAX, SUM, AVG.

mysql> SELECT MIN(year) FROM movies;
+-----------+
| MIN(year) |
+-----------+
|      1888 |
+-----------+
1 row in set (1.16 sec)


mysql> SELECT MAX(year) FROM movies;
+-----------+
| MAX(year) |
+-----------+
|      2008 |
+-----------+
1 row in set (0.09 sec)

Getting the number of rows. If we dont know the column names.

mysql> SELECT COUNT(*) FROM movies;
+----------+
| COUNT(*) |
+----------+
|   388269 |
+----------+
1 row in set (1.74 sec)

Instead of * we can use a column name.

mysql> SELECT COUNT(year) FROM movies;
+-------------+
| COUNT(year) |
+-------------+
|      388269 |
+-------------+
1 row in set (0.08 sec)

We can see that the number of rows returned is same in both cases.

mysql> SELECT COUNT(*) FROM movies WHERE year>2000;
+----------+
| COUNT(*) |
+----------+
|    46006 |
+----------+
1 row in set (0.08 sec)

The above command returns the list of movies released after 2000.

mysql> SELECT year, COUNT(year) FROM movies GROUP BY year;
+------+-------------+
| year | COUNT(year) |
+------+-------------+
| 1888 |           2 |
| 1890 |           3 |
| 1891 |           6 |
| 1892 |           9 |
| 1893 |           2 |
| 1894 |          59 |

.......

| 2004 |        8718 |
| 2005 |        1449 |
| 2006 |         195 |
| 2007 |           7 |
| 2008 |           1 |
+------+-------------+
120 rows in set (0.21 sec)


Here we are specifying that we want to see year and COUNT(years) after grouping the movies by year. This means that 2 movies were released in 1888 3 in 1890 etc.

Same output is obtained using the below command.

mysql> SELECT year, COUNT(year) FROM movies GROUP BY year ORDER BY year;
+------+-------------+
| year | COUNT(year) |
+------+-------------+
| 1888 |           2 |
| 1890 |           3 |
| 1891 |           6 |

.........

| 2005 |        1449 |
| 2006 |         195 |
| 2007 |           7 |
| 2008 |           1 |
+------+-------------+
120 rows in set (0.42 sec)


Suppose we are intrested in sorting the data according to the number of movies per year, we cannot use COUNT(year) as the input to ORDER BY, instead we can use the alias feature. We are using year_count as alias for COUNT(year).

mysql> SELECT year, COUNT(year) FROM movies GROUP BY year ORDER BY year;
+------+-------------+
| year | COUNT(year) |
+------+-------------+
| 1888 |           2 |
| 1890 |           3 |
| 1891 |           6 |
| 1892 |           9 |

.....

| 1999 |      10976 |
| 2000 |      11643 |
| 2001 |      11690 |
| 2003 |      11890 |
| 2002 |      12056 |
+------+------------+
120 rows in set (0.21 sec)


If grouping contains rows having NULL value, they will be grouped together.

HAVING keyword is used along with the GROUP BY to achieve similar functionality as that of WHERE.

Here the order of execution is as follows.
1) Execution of the GROUP BY
2) Execution of aggreegate function
3) Execution of HAVING

If we dont use GROUP BY, then HAVING has similar meaning as WHERE.

mysql> SELECT name, year FROM movies HAVING year>2000 LIMIT 10;
+--------------------------------+------+
| name                           | year |
+--------------------------------+------+
| #28                            | 2002 |
| $100,000 Pyramid, The          | 2001 |
| $300 y tickets                 | 2002 |
| $5.15/Hr.                      | 2004 |
| $windle                        | 2002 |
| '15'                           | 2002 |
| '60s Pop Rock Reunion          | 2004 |
| '88 Dodge Aries                | 2002 |
| 'Ang Galing galing mo, Babes'  | 2002 |
| 'As se hizo' - Torremolinos 73 | 2003 |
+--------------------------------+------+
10 rows in set (0.00 sec)

mysql> SELECT name, year FROM movies WHERE year>2000 LIMIT 10;
+--------------------------------+------+
| name                           | year |
+--------------------------------+------+
| #28                            | 2002 |
| $100,000 Pyramid, The          | 2001 |
| $300 y tickets                 | 2002 |
| $5.15/Hr.                      | 2004 |
| $windle                        | 2002 |
| '15'                           | 2002 |
| '60s Pop Rock Reunion          | 2004 |
| '88 Dodge Aries                | 2002 |
| 'Ang Galing galing mo, Babes'  | 2002 |
| 'As se hizo' - Torremolinos 73 | 2003 |
+--------------------------------+------+
10 rows in set (0.00 sec)

mysql> SELECT year, COUNT(year) year_count FROM movies WHERE rankscore>9 GROUP BY year HAVING year_count > 20;
+------+------------+
| year | year_count |
+------+------------+
| 1982 |         24 |
| 1996 |         34 |
| 1997 |         23 |
| 1998 |         21 |
| 1999 |         33 |
| 2000 |         45 |
| 2001 |         38 |
| 2002 |         77 |
| 2003 |         87 |
| 2004 |         48 |
+------+------------+
10 rows in set (0.09 sec)


The main diffrence between HAVING and WHERE are given below

WHERE is applied on individual elements, HAVING is applied on groups.
HAVING is applies after grouping, WHERE is applied before grouping.


