# SQLZoo Exercises
My solutions to SQLZoo quesitons.

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
```sql

```
```sql

```
```sql

```
```sql

```
