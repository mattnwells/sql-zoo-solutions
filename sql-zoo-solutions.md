# SQLZoo Exercises
Solutions to [SQLZOO Tutorals](http://sqlzoo.net/wiki/SQL_Tutorial).

Table to Contents:

* [SELECT basics](#SELECT-basics)
* [SELECT from world](#SELECT-from-world)
* [SELECT from nobel](#SELECT-from-nobel)
* [SELECT in SELECT](#SELECT-in-SELECT)
* [SUM and COUNT](#SUM-and-COUNT)
* [JOIN](#JOIN)
* [More JOIN](#More-JOIN)

## SELECT basics

1.
```sql
SELECT population 
FROM world
WHERE name = 'Germany';
```

2.
```sql
SELECT name, population 
FROM world
WHERE name IN ('Sweden', 'Norway', 'Denmark');
```

3.
```sql
SELECT name, area 
FROM world
WHERE area BETWEEN 200000 AND 250000;
```

## SELECT from world

1.
```sql
SELECT name, continent, population 
FROM world
```

2.
```sql
SELECT name 
FROM world
WHERE population >= 200000000;
```

3.
```sql
SELECT name, gdp/population
FROM world
WHERE population > 200000000;
```
4.
```sql
SELECT name, population/1000000
FROM world
WHERE continent = 'South America';
```

5.
```sql
SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy');
```

6.
```sql
SELECT name
FROM world
WHERE name LIKE '%United%';
```

7. 
```sql
SELECT name, population, area
FROM world
WHERE area > 3000000 OR population > 250000000;
```

8.
```sql
SELECT name, population, area 
FROM world
WHERE population > 250000000 XOR area > 3000000;
```

9.
```SQL
SELECT name, ROUND(population/1000000, 2),ROUND(gdp/1000000000, 2)
FROM world
WHERE continent = 'South America';
```

10.
```sql
SELECT name, ROUND(gdp/population, -3) AS 'per-capita gdp'
FROM world
WHERE gdp > 1000000000000;
```

11.
```sql
SELECT name, capital
FROM world
WHERE LEN(name) = LEN(capital);
```

12.
```sql
SELECT name, capital
FROM world
WHERE LEFT(name, 1) = LEFT(capital, 1) AND name <> capital;
```

13.
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

1.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1950;
```

2.
```sql
SELECT winner
FROM nobel
WHERE yr = 1962 AND subject = 'literature';
```

3.
```sql
SELECT yr, subject 
FROM nobel
WHERE winner = 'Albert Einstein';
```

4.
```sql
SELECT winner
FROM nobel
WHERE subject = 'peace' AND yr >= 2000;
```

5.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE yr >= 1980 AND yr <= 1989 AND subject = 'literature';
```

6.
```sql
SELECT * 
FROM nobel
WHERE winner IN ('Theodore Roosevelt', 'Woodrow Wilson', 'Jimmy Carter', 'Barack Obama');
```

7.
```sql
SELECT winner
FROM nobel
WHERE winner LIKE 'John %';
```

8.
```sql
SELECT *
FROM nobel
WHERE subject = 'physics' AND yr = 1980
   OR subject = 'chemistry' AND yr = 1984;
```

9.
```sql
SELECT *
FROM nobel
WHERE yr = 1980 AND NOT subject IN ('chemistry', 'medicine');
```

10.
```sql
SELECT *
FROM nobel
WHERE subject = 'medicine' AND yr < 1910 
   OR subject = 'literature' AND yr >= 2004;
```

11.
```sql
SELECT *
FROM nobel
WHERE winner = 'peter grünberg';
```

12.
```sql
SELECT * 
FROM nobel
WHERE winner = 'eugene o' + char(39) + 'neill';
```

13.
```sql
SELECT winner, yr, subject
FROM nobel
WHERE winner LIKE 'Sir %' ORDER BY yr DESC, winner;
```

14.
```sql
SELECT winner, subject
FROM nobel
WHERE yr = 1984
ORDER BY CASE WHEN subject IN ('physics', 'chemistry') THEN 1 ELSE 0 END, subject, winner;
```

## SELECT in SELECT

1.
```sql
SELECT name 
FROM world
WHERE population >
     (SELECT population FROM world
      WHERE name='Russia');
```

2.
```sql
SELECT name
FROM world
WHERE continent = 'Europe' AND gdp/population >
     (SELECT gdp/population FROM world 
      WHERE name = 'United Kingdom');
```

3.
```sql
SELECT name, continent
FROM world
WHERE continent IN
    (SELECT continent FROM world
     WHERE name IN ('Argentina', 'Australia'))
ORDER BY name;
```

4.
```sql
SELECT name, population
FROM world
WHERE population >
     (SELECT population FROM world
      WHERE name = 'United Kingdom') AND population <
     (SELECT population FROM world
      WHERE name = 'Germany');
```

5.
```sql
SELECT name, 
       CONCAT(ROUND(population/(SELECT population FROM world WHERE name = 'Germany')*100, 0), '%') AS 'percentage'
FROM world
WHERE continent = 'Europe';
```

6.
```sql
SELECT name
FROM world
WHERE gdp > ALL(SELECT gdp
                FROM world
                WHERE continent = 'Europe' AND gdp > 0);
```

7.
```sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL (SELECT area FROM world y
        WHERE y.continent = x.continent
          AND area > 0);
```

8.
```sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL (SELECT area FROM world y
        WHERE y.continent= x.continent
          AND area > 0);
```

9.
```sql
SELECT name, continent, population
FROM world x
WHERE 25000000 >= ALL(SELECT population
                      FROM world y
                      WHERE x.continent = y.continent AN   D y.population > 0);
```

10.
```sql
SELECT name, continent FROM world x
  WHERE population >= ALL(SELECT population*3
                         FROM world y
                         WHERE x.continent = y.continent
                         and y.name != x.name);
```

## SUM and COUNT

1.
```sql
SELECT SUM(population) AS 'world population'
FROM world;
```

2.
```sql
SELECT DISTINCT(continent)
FROM world;
```

3.
```sql
SELECT SUM(gdp)
FROM world
WHERE continent = 'Africa';
```

4.
```sql
SELECT COUNT(name)
FROM world
WHERE area >= 1000000;
```

5.
```sql
SELECT SUM(population)
FROM world
WHERE name IN ('Estonia', 'Latvia', 'Lithuania');
```

6.
```sql
SELECT continent, COUNT(name)
FROM world
GROUP BY continent;
```

7.
```sql
SELECT continent, COUNT(name)
FROM world
WHERE population >= 10000000
GROUP BY continent;
```

8.
```sql
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000;
```

##JOIN

1.
```sql
SELECT matchid, player 
FROM goal 
WHERE teamid = 'GER';
```

2.
```sql
SELECT id, stadium, team1, team2
FROM game
WHERE id = 1012;
```

3.
```sql
SELECT player, teamid, stadium, mdate
FROM game JOIN goal ON (game.id=goal.matchid)
WHERE teamid = 'GER';
```

4.
```sql
SELECT team1, team2, player
FROM game JOIN goal ON (game.id = goal.matchid)
WHERE player LIKE 'Mario%';
```

5.
```sql
SELECT player, teamid, coach, gtime
FROM goal JOIN eteam ON (goal.teamid = eteam.id)
WHERE gtime<=10;
```

6.
```sql
SELECT mdate, teamname
FROM game JOIN eteam ON (team1 = eteam.id)
WHERE coach LIKE 'Fernando Santos';
```

7.
```sql
SELECT player
FROM goal JOIN game ON (game.id = goal.matchid)
WHERE stadium = 'National Stadium, Warsaw';
```

8.
```sql
SELECT DISTINCT player
FROM game JOIN goal ON matchid = id 
WHERE (team1 = 'GER' OR team2 = 'GER') 
AND teamid != 'GER';
```

9.
```sql
SELECT teamname, COUNT(*)
FROM eteam JOIN goal ON id=teamid
GROUP BY teamname;
```

10.
```sql
SELECT stadium, COUNT(*)
FROM goal JOIN game ON goal.matchid = game.id
GROUP BY stadium;
```

11.
```sql
SELECT matchid, mdate, COUNT(*)
FROM game JOIN goal ON matchid = id
WHERE (team1 = 'POL' OR team2 = 'POL')
GROUP BY mdate, matchid;
```

12.
```sql
SELECT matchid, mdate, COUNT(*)
FROM game JOIN goal ON matchid = id
WHERE teamid = 'GER'
GROUP BY mdate, matchid;
```

13.
```sql
SELECT mdate, team1, SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END)score1, team2, SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END)score2
FROM game LEFT JOIN goal ON matchid = id
GROUP BY mdate, matchid, team1, team2;
```

##More JOIN


