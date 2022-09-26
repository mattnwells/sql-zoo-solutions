# SQLZoo Exercises
Solutions to [SQLZOO Tutoral](http://sqlzoo.net/wiki/SQL_Tutorial).

Table to Contents:

[SELECT basics](#SELECT-basics) <br>
[SELECT from world](#SELECT-from-world) <br>
[SELECT from nobel](#SELECT-from-nobel) <br>
[SELECT in SELECT](#SELECT-in-SELECT) <br>
[SUM and COUNT](#SUM-and-COUNT) <br>
[JOIN](#JOIN) <br>
[More JOIN](#More-JOIN) <br>
[Using NULL](#Using-NULL) <br>
[Self JOIN](#Self-JOIN)

## SELECT basics

1. Modify it *(the query)* to show the population of Germany.
```sql
SELECT population 
FROM world
WHERE name = 'Germany';
```

2. Show the name and the population for 'Sweden', 'Norway' and 'Denmark'.
```sql
SELECT name, population 
FROM world
WHERE name IN ('Sweden', 'Norway', 'Denmark');
```

3. Modify it *(the query)* to show the country and the area for countries with an area between 200,000 and 250,000.
```sql
SELECT name, area 
FROM world
WHERE area BETWEEN 200000 AND 250000;
```

## SELECT from world

1. Observe the result of running this SQL command to show the name, continent and population of all countries.
```sql
SELECT name, continent, population 
FROM world
```

2. Show the name for the countries that have a population of at least 200 million.
```sql
SELECT name 
FROM world
WHERE population >= 200000000;
```

3.Give the name and the per capita GDP for those countries with a population of at least 200 million.
```sql
SELECT name, gdp/population
FROM world
WHERE population > 200000000;
```

4. Show the name and population in millions for the countries of the continent 'South America'.
```sql
SELECT name, population/1000000
FROM world
WHERE continent = 'South America';
```

5. Show the name and population for France, Germany, Italy.
```sql
SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy');
```

6. Show the countries which have a name that includes the word 'United
```sql
SELECT name
FROM world
WHERE name LIKE '%United%';
```

7. Two ways to be big: A country is big if it has an area of more than 3 million sq km or it has a population of more than 250 million. Show the countries that are big by area or big by population. Show name, population and area.
```sql
SELECT name, population, area
FROM world
WHERE area > 3000000 OR population > 250000000;
```

8. Exclusive OR (XOR). Show the countries that are big by area (more than 3 million) or big by population (more than 250 million) but not both. Show name, population and area.
```sql
SELECT name, population, area 
FROM world
WHERE population > 250000000 XOR area > 3000000;
```

9. Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'. Use the ROUND function to show the values to two decimal places. For South America show population in millions and GDP in billions both to 2 decimal places.
```SQL
SELECT name, ROUND(population/1000000, 2) AS population, ROUND(gdp/1000000000, 2) AS GDP
FROM world
WHERE continent = 'South America';
```

10. Show the name and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). Round this value to the nearest 1000. Show per-capita GDP for the trillion dollar countries to the nearest $1000.
```sql
SELECT name, ROUND(gdp/population, -3) AS 'per-capita gdp'
FROM world
WHERE gdp > 1000000000000;
```

11. Show the name and capital where the name and the capital have the same number of characters.
```sql
SELECT name, capital
FROM world
WHERE LEN(name) = LEN(capital);
```

12. The capital of Sweden is Stockholm. Both words start with the letter 'S'. Show the name and the capital where the first letters of each match. Don't include countries where the name and the capital are the same word.
```sql
SELECT name, capital
FROM world
WHERE LEFT(name, 1) = LEFT(capital, 1) AND name <> capital;
```

13. Equatorial Guinea and Dominican Republic have all of the vowels (a e i o u) in the name. They don't count because they have more than one word in the name. Find the country that has all the vowels and no spaces in its name.
```sql
SELECT name
FROM world
WHERE name LIKE '%a%' AND 
      name LIKE '%e%' AND 
      name LIKE '%i%' AND 
      name LIKE '%o%' AND 
      name LIKE '%u%' AND 
      name NOT LIKE '% %';
```
  
## SELECT from nobel

1. Change the query shown so that it displays Nobel prizes for 1950.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1950;
```

2. Show who won the 1962 prize for literature.
```sql
SELECT winner
FROM nobel
WHERE yr = 1962 AND subject = 'literature';
```

3. Show the year and subject that won 'Albert Einstein' his prize.
```sql
SELECT yr, subject 
FROM nobel
WHERE winner = 'Albert Einstein';
```

4. Give the name of the 'peace' winners since the year 2000, including 2000.
```sql
SELECT winner
FROM nobel
WHERE subject = 'peace' AND yr >= 2000;
```

5. Show all details (yr, subject, winner) of the literature prize winners for 1980 to 1989 inclusive.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE yr >= 1980 AND yr <= 1989 AND subject = 'literature';
```

6. Show all details of the presidential winners:

Theodore Roosevelt
Thomas Woodrow Wilson
Jimmy Carter
Barack Obama
```sql
SELECT * 
FROM nobel
WHERE winner IN ('Theodore Roosevelt', 'Woodrow Wilson', 'Jimmy Carter', 'Barack Obama');
```

7. Show the winners with first name John.
```sql
SELECT winner
FROM nobel
WHERE winner LIKE 'John %';
```

8. Show the year, subject, and name of physics winners for 1980 together with the chemistry winners for 1984.
```sql
SELECT *
FROM nobel
WHERE subject = 'physics' AND yr = 1980 OR subject = 'chemistry' AND yr = 1984;
```

9. Show the year, subject, and name of winners for 1980 excluding chemistry and medicine.
```sql
SELECT *
FROM nobel
WHERE yr = 1980 AND NOT subject IN ('chemistry', 'medicine');
```

10. Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004).
```sql
SELECT *
FROM nobel
WHERE subject = 'medicine' AND yr < 1910 OR subject = 'literature' AND yr >= 2004;
```

11. Find all details of the prize won by PETER GRÜNBERG.
```sql
SELECT *
FROM nobel
WHERE winner = 'peter grünberg';
```

12. Find all details of the prize won by EUGENE O'NEILL.
```sql
SELECT * 
FROM nobel
WHERE winner = 'eugene o' + char(39) + 'neill';
```

13. List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
```sql
SELECT winner, yr, subject
FROM nobel
WHERE winner LIKE 'Sir %' ORDER BY yr DESC, winner;
```

14. The expression subject IN ('chemistry','physics') can be used as a value - it will be 0 or 1. Show the 1984 winners and subject ordered by subject and winner name; but list chemistry and physics last.
```sql
SELECT winner, subject
FROM nobel
WHERE yr = 1984
ORDER BY CASE WHEN subject IN ('physics', 'chemistry') THEN 1 ELSE 0 END, subject, winner;
```

## SELECT in SELECT

1. List each country name where the population is larger than that of 'Russia'.
```sql
SELECT name 
FROM world
WHERE population > (SELECT population FROM world WHERE name='Russia');
```

2. Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
```sql
SELECT name
FROM world
WHERE continent = 'Europe' AND gdp/population > (SELECT gdp/population FROM world WHERE name = 'United Kingdom');
```

3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```sql
SELECT name, continent
FROM world
WHERE continent IN (SELECT continent FROM world WHERE name IN ('Argentina', 'Australia'))
ORDER BY name;
```

4. Which country has a population that is more than United Kingom but less than Germany? Show the name and the population.
```sql
SELECT name, population
FROM world
WHERE population > (SELECT population FROM world WHERE name = 'United Kingdom') AND 
      population < (SELECT population FROM world WHERE name = 'Germany');
```

5. Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
```sql
SELECT name, CONCAT(ROUND(population/(SELECT population FROM world WHERE name = 'Germany')*100, 0), '%') AS 'percentage'
FROM world
WHERE continent = 'Europe';
```

6. Which countries have a GDP greater than every country in Europe? 
```sql
SELECT name
FROM world
WHERE gdp > ALL(SELECT gdp FROM world WHERE continent = 'Europe' AND gdp > 0);
```

7. Find the largest country (by area) in each continent, show the continent, the name and the area.
```sql
SELECT continent, name, area 
FROM world x
WHERE area >= ALL (SELECT area FROM world y WHERE y.continent = x.continent AND area > 0);
```

8. List each continent and the name of the country that comes first alphabetically.
```sql
SELECT continent, name, area 
FROM world x
WHERE area >= ALL (SELECT area FROM world y WHERE y.continent= x.continent AND area > 0);
```

9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
```sql
SELECT name, continent, population
FROM world x
WHERE 25000000 >= ALL(SELECT population
                      FROM world y
                      WHERE x.continent = y.continent AND y.population > 0);
```

10. Some countries have populations more than three times that of all of their neighbours (in the same continent). Give the countries and continents.
```sql
SELECT name, continent 
FROM world x
WHERE population >= ALL(SELECT population*3
                        FROM world y
                        WHERE x.continent = y.continent AND y.name != x.name);
```

## SUM and COUNT

1. Show the total population of the world.
```sql
SELECT SUM(population) AS 'world population'
FROM world;
```

2. List all the continents - just once each.
```sql
SELECT DISTINCT(continent)
FROM world;
```

3. Give the total GDP of Africa.
```sql
SELECT SUM(gdp)
FROM world
WHERE continent = 'Africa';
```

4. How many countries have an area of at least 1000000.
```sql
SELECT COUNT(name)
FROM world
WHERE area >= 1000000;
```

5. What is the total population of ('Estonia', 'Latvia', 'Lithuania').
```sql
SELECT SUM(population)
FROM world
WHERE name IN ('Estonia', 'Latvia', 'Lithuania');
```

6. For each continent show the continent and number of countries.
```sql
SELECT continent, COUNT(name)
FROM world
GROUP BY continent;
```

7. For each continent show the continent and number of countries with populations of at least 10 million.
```sql
SELECT continent, COUNT(name) AS population
FROM world
WHERE population >= 10000000
GROUP BY continent;
```

8. List the continents that have a total population of at least 100 million.
```sql
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000;
```

##JOIN

1. Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'.
```sql
SELECT matchid, player 
FROM goal 
WHERE teamid = 'GER';
```

2. Show id, stadium, team1, team2 for just game 1012.
```sql
SELECT id, stadium, team1, team2
FROM game
WHERE id = 1012;
```

3. Modify it to show the player, teamid, stadium and mdate for every German goal.
```sql
SELECT player, teamid, stadium, mdate
FROM game JOIN goal ON (game.id=goal.matchid)
WHERE teamid = 'GER';
```

4. Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'.
```sql
SELECT team1, team2, player
FROM game JOIN goal ON (game.id = goal.matchid)
WHERE player LIKE 'Mario%';
```

5. Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10.
```sql
SELECT player, teamid, coach, gtime
FROM goal JOIN eteam ON (goal.teamid = eteam.id)
WHERE gtime<=10;
```

6. List the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.
```sql
SELECT mdate, teamname
FROM game JOIN eteam ON (team1 = eteam.id)
WHERE coach LIKE 'Fernando Santos';
```

7. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'
```sql
SELECT player
FROM goal JOIN game ON (game.id = goal.matchid)
WHERE stadium = 'National Stadium, Warsaw';
```

8. The example query shows all goals scored in the Germany-Greece quarterfinal. Instead show the name of all players who scored a goal against Germany.
```sql
SELECT DISTINCT player
FROM game JOIN goal ON matchid = id 
WHERE (team1 = 'GER' OR team2 = 'GER') 
AND teamid != 'GER';
```

9. Show teamname and the total number of goals scored.
```sql
SELECT teamname, COUNT(*)
FROM eteam JOIN goal ON id=teamid
GROUP BY teamname;
```

10. Show the stadium and the number of goals scored in each stadium.
```sql
SELECT stadium, COUNT(*)
FROM goal JOIN game ON goal.matchid = game.id
GROUP BY stadium;
```

11. For every match involving 'POL', show the matchid, date and the number of goals scored.
```sql
SELECT matchid, mdate, COUNT(*)
FROM game JOIN goal ON matchid = id
WHERE (team1 = 'POL' OR team2 = 'POL')
GROUP BY mdate, matchid;
```

12. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'.
```sql
SELECT matchid, mdate, COUNT(*)
FROM game JOIN goal ON matchid = id
WHERE teamid = 'GER'
GROUP BY mdate, matchid;
```

13. List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises. Sort your result by mdate, matchid, team1 and team2.
```sql
SELECT mdate, team1, SUM(CASE WHEN teamid = team1 THEN 1 ELSE 0 END) AS score1, team2, SUM(CASE WHEN teamid = team2 THEN 1 ELSE 0 END) AS score2
FROM game LEFT JOIN goal ON matchid = id
GROUP BY mdate, matchid, team1, team2;
```

##More JOIN

1. List the films where the yr is 1962 (show id, title).
```sql
SELECT id, title
FROM movie
WHERE yr=1962;
```

2. Give year of 'Citizen Kane'.
```sql
SELECT yr
FROM movie
WHERE title = 'Citizen Kane';
```

3. List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.
```sql
SELECT id, title, yr
FROM movie
WHERE title LIKE '%Star trek%'
ORDER BY yr;
```

4. What id number does the actor 'Glenn Close' have?
```sql
SELECT id
FROM actor
WHERE name = 'Glenn Close';
```

5. What is the id of the film 'Casablanca'.
```sql
SELECT id
FROM movie
WHERE title = 'Casablanca';
```

6. Obtain the cast list for 'Casablanca'. Use movieid=11768, (or whatever value you got from the previous question).
```sql
SELECT actor.name
FROM actor JOIN casting ON casting.actorid = actor.id
WHERE casting.movieid = 27;
```

7. Obtain the cast list for the film 'Alien'.
```sql
SELECT name
FROM actor JOIN casting ON casting.actorid = actor.id
JOIN movie ON actor.id = casting.movieid
WHERE title = 'Alien';
```

8. List the films in which 'Harrison Ford' has appeared
```sql
SELECT movie.title
FROM movie JOIN casting ON casting.movieid = movie.id
JOIN actor ON actor.id = casting.actorid
WHERE actor.name = 'Harrison Ford';
```

9. List the films where 'Harrison Ford' has appeared - but not in the starring role.
```sql
SELECT title
FROM movie JOIN casting ON movie.id = casting.movieid
JOIN actor ON casting.actorid = actor.id
WHERE name = 'Harrison Ford' AND ord != 1;
```

10. List the films together with the leading star for all 1962 films.
```sql
SELECT title, name
FROM movie JOIN casting ON casting.movieid = movie.id
JOIN actor ON actor.id = casting.actorid
WHERE movie.yr = 1962 AND casting.ord = 1;
```

11. Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any year in which he made more than 2 movies.
```sql
SELECT yr, COUNT(title)
FROM movie JOIN casting ON movie.id = movieid
JOIN actor ON actorid=actor.id
WHERE name = 'Rock Hudson'
GROUP BY yr
HAVING COUNT(title) >= 2;
```

12. List the film title and the leading actor for all of the films 'Julie Andrews' played in *(Difficult!)*.
```sql
SELECT DISTINCT m.title, a.name
FROM (SELECT movie.*
      FROM movie
      JOIN casting
      ON casting.movieid = movie.id
      JOIN actor
      ON actor.id = casting.actorid
      WHERE actor.name = 'Julie Andrews') AS m
JOIN (SELECT actor.*, casting.movieid AS movieid
      FROM actor
      JOIN casting
      ON casting.actorid = actor.id
      WHERE casting.ord = 1) as a
ON m.id = a.movieid
ORDER BY m.title;
```

13. Obtain a list, in alphabetical order, of actors who've had at least 15 starring roles.
```sql
SELECT name
FROM actor JOIN casting ON actor.id = casting.actorid
WHERE ord = 1
GROUP BY name
HAVING COUNT(*) >= 15;
```

14. List the films released in the year 1978 ordered by the number of actors in the cast, then by title.
```sql
SELECT title, COUNT(actorid) AS actors 
FROM movie
JOIN casting ON id = movieid
WHERE yr = 1978
GROUP BY title
ORDER BY actors DESC, title;
```

15. List all the people who have worked with 'Art Garfunkel'.
```sql
SELECT DISTINCT name
FROM movie
JOIN casting
ON casting.movieid = movie.id
JOIN actor
ON actor.id = casting.actorid
WHERE movie.id in (SELECT movieid FROM casting JOIN actor ON id =actorid WHERE 
actor.name = 'Art Garfunkel') AND actor.name != 'Art Garfunkel';
```

## Using NULL

1.
```sql

```

2.
```sql

```

3.
```sql

```

4.
```sql

```

5.
```sql

```

6.
```sql

```

7.
```sql

```

8.
```sql

```

10.
```sql

```

## Self JOIN

1.
```sql

```

2.
```sql

```

3.
```sql

```

4.
```sql

```

5.
```sql

```

6.
```sql

```

7.
```sql

```

8.
```sql

```

10.
```sql

```



