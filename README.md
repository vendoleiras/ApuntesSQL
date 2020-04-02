# Apuntes SQL

## Introducción
El lenguaje SQL es un lenguaje de dominio específico utilizado en programación, diseñado para administrar y recuperar información de sistemas de gestión de bases de datos relacionales y pudiéndose integrar a lenguajes de programación como PHP o ASP.
Sus siglas (SQL) significan _**Structured Query Language**_.
Se suele describir como un *lenguaje declarativo* ya que a la hora de programar declaras *que* quieres hacer y no *como* lo quieres hacer.
***
### SUBLENGUAJES SQL

 - **DDL Data Definition Language.**
 
 - **TCL Transaction Control Language.**

  

 - **DCL Data Control Language.**

  

 - **DML Data Manipulation Language.**

  

 - **DRL/DQL Data Retrieval Language/Data Query Language.**

   

 - **SCL Session Control Language**

## Conceptos básicos

 - **FROM**: Lo utilizamos para indicar la tabla de la cuál queremos sacar la información.
 - **WHERE**: Lo usamos si solo queremos recuperar la información que cumpla una determinada condición, es decir filtramos los resultados obtenidos con el `SELECT`.
 - **SELECT**: Con `SELECT` indicamos los datos que queremos mostrar de la tabla que previamente hemos seleccionado.
 El resultado se almacena en una tabla de resultados que también recibe el nombre de *conjunto resultante*. 
             
> TODAS las consultas de SQL se terminan con un punto y coma **;**
> Los Strings van entre comillas simples ***'hola'***
---
### SELECT 
>**EJEMPLOS:**

Nos mostrará la  `population`  de la tabla `world`.
```SQL
SELECT world.population
FROM world;
```
Nos mostrará el `continent` y la `area` de la tabla `world`.
```SQL
SELECT world.continent, world.area
FROM world;
```

> Con  **_*_** seleccionaremos todos los datos de la tabla
> `SELECT *`

Además en el `SELECT` se pueden realizar operaciones.
Esto nos mostrará el *GDP per capita* que es el resultado de dividir el `gdp` entre la `population`.
```SQL
SELECT name, gdp/population AS 'GDP per capita'
FROM world
WHERE population > 200000000;
```

> **`AS`** nos sirve para ponerle alias a los campos y así hacer la tarea de identificarlos más sencilla.
---
#### SELECT DISTINCT
Nos muestra los resultados sin repetidos del campo que le especifiquemos, es decir que los datos se muestren *una única vez* en el conjunto resultante.
>**EJEMPLO:**

Nos mostrará los distintos `continent` que hay en la tabla `world`.
```SQL
SELECT DISTINCT world.continent
FROM world;
```
---
#### SELECT REPLACE
Nos sirve para **reemplazar** caracteres dentro de un String.
La sintaxis es `REPLACE (1, '2', '3')`.

 **1.** Cadena en la que se va a reemplazar.
 **2.** *(Entre comillas)* Los caracteres que se reemplazarán.
 **3.** *(Entre comillas)* Los que ocupan el lugar de los reemplazados.
>**EJEMPLO:**

Nos mostrará los `name` y los `capital`  con los caracteres `'Be'` en vez de `'a'`, por ejemplo **anin** en vez de **Benin**.
```SQL
SELECT world.name, REPLACE(world.capital,'Be','a')
FROM world;
```
---
#### SELECT ROUND
Nos sirve para **redondear** un número a una cifra de decimales que debemos especificar.
La sintaxis es `ROUND (1, 2)`.

 **1.** Datos que se van a redondear.
 **2.** El número de decimales que se mostrará. Si se ponen números negativos se truncará.
 >**EJEMPLO:**

Nos mostrará el `name` y la `population` **en millones** cuando `continent` sea igual a `'South America'`.
```SQL
SELECT world.name, ROUND(world.population/1000000,2)
FROM world
WHERE world.continent= 'South America';
```
---
#### SELECT LENGTH
Nos sirve para obtener la longitud de la cadena que le pasamos como parámetro.
>**EJEMPLO:**
>
Nos mostrará los `name` y el **numero de caracteres** de los `capital`.
```SQL
SELECT world.name, LENGTH (world.capital)
FROM world;
```
---
#### SELECT LEFT/RIGHT
Nos sirve para mostrar los primeros *'n'* caracteres de una cadena.
Si usamos `LEFT` empezará por la izquierda y si usamos `RIGHT` empezará por la derecha.
>**EJEMPLO:**

Nos mostrará los `winner` y los **primeros tres caracteres** empezando por la *izquierda* de `subject`.
```SQL
SELECT nobel.winner, LEFT(nobel.subject,3)
FROM nobel;
```
---
#### SELECT SUM
Nos sirve para calular la suma total de los valores de una columna
>**EJEMPLO:**

Nos mostrará la **suma total** de `population`.
```SQL
SELECT SUM(world.population)
FROM world;
```
---
#### SELECT COUNT
Nos sirve para contar el número de tuplas.
>**EJEMPLO:**

Nos mostrará **el número de** `name` que tengan el `area` mayor o igual que `1000000`.
```SQL
SELECT COUNT(world.name)
FROM world
WHERE world.area >= 1000000;
```
---
#### SELECT AVG
Nos sirve para obtener el promedio de una columna numérica.
>**EJEMPLO:**

Nos mostrará el **promedio** de todos los `yr`.
```SQL
SELECT AVG(nobel.yr)
FROM nobel;
```
---
#### SELECT MAX/MIN
Nos sirve para obtener el valor máximo o mínimo del atributo que le especifiquemos.
>**EJEMPLO:**

Nos mostrará el `name` y la `population` **más alta**.
```SQL
SELECT world.name, MAX(world.population)
FROM world;
```
---
#### SELECT CONCAT
Nos sirve para unir dos o más valores de un campo.
>**EJEMPLO:**

Nos mostrará los `winner` con la palabra `'yepa'` al final.
```SQL
SELECT CONCAT(nobel.winner, 'yepa')
FROM nobel;
```
---
> Estas funciones se pueden usar tanto en el **`SELECT`** como en el **`HAVING`** del **`GROUP BY`**.
---
### WHERE
>**EJEMPLOS:**

Nos mostrará la `population` de los `name` que sean igual a `'Germany'`
```SQL
SELECT world.population
FROM world;
WHERE world.name = 'Germany';
```
---
#### Operadores aritméticos

 - Operador igual **=**
 - Operador distinto **<>**
 - Operador mayor **>**
 - Operador menor **<**
 - Operador mayor o igual **>=**
 - Operador menor o igual **<=**
 >**EJEMPLOS:**
 
 Nos mostrará los `name` en los que `population` es mayor que `200000000`.
 ```SQL
SELECT world.name
FROM world;
WHERE world.population > 200000000;
```
Nos mostrará los `name` en los que `continent` sea distinto de `Africa`.
```SQL
SELECT world.name
FROM world;
WHERE world.continent <> 'Africa';
```
---
#### Operador AND
Sirve para agrupar condiciones.
>**EJEMPLO:**

Nos mostrará los `winner` en los que `yr` es `1960` y `subject` es `Physics`, es decir se tienen que cumplir ambas condiciones.
```SQL
SELECT nobel.winner
FROM nobel;
WHERE nobel.yr = 1960 AND nobel.subject = 'Physics';
```
---
#### Operador OR
Muestra los datos que cumplan cualquiera de las condiciones separadas por el **`OR`**.
>**EJEMPLO:**

Nos mostrará los `name`, `population` y `area` en los que **o** `area` sea mayor que `3000000` **o** la `population` sea mayor que `250000000`
```SQL
SELECT world.name, world.population, world.area
FROM world;
WHERE world.area > 3000000 OR world.population > 250000000;
```
---
#### Operador NOT
Muestra los datos que NO cumplan las condiciones que se sitúen después del `NOT`

> Podemos usar también **<>**

>**EJEMPLO:**

Nos mostrará los `winner` y los `yr` en los que `subject` no sea igual a `'Physics'`.
```SQL
SELECT nobel.winner, nobel.yr
FROM nobel;
WHERE NOT nobel.subject = 'Physics';
```
Sería lo mismo que poner lo siguiente
```SQL
SELECT nobel.winner, nobel.yr
FROM nobel;
WHERE nobel.subject <> 'Physics';
```
---
#### Operador IN
La notación IN nos sirve para especificar un conjunto de valores que queremos que aparezcan en el resultado.
>**EJEMPLO:**

Nos mostrará los `name` y `population` en los que `name` sea igual a `'Sweden'`, `'Norway'` y `'Denmark'`.
```SQL
SELECT world.name, world.population 
FROM world
WHERE world.name IN ('Sweden','Norway', 'Denmark');
  ```
  ---
  #### Operador NOT IN
  Igual que el operador `NOT` pero con un conjunto de valores.
  >**EJEMPLO:**
  
  Nos mostrará todos los datos de la tabla `*` en los que `subject` no sea igual a `'Chemistry'` y `'Medicine'`, es decir estos valores no saldrán en la tabla resultante.                  
 ```SQL
SELECT *
FROM nobel
WHERE nobel.subject NOT IN ('Chemistry','Medicine');
```
---
#### Operador BETWEEN
Muestra los datos que se sitúen dentro de un rango de valores, siendo la sintaxis `BETWEEN valor AND valor`.
  >**EJEMPLO:**

Nos mostrará los `name` y `area` en los que `area` se encuentre entre `250000` y `300000`.
```SQL
SELECT world.name, world.area 
FROM world
WHERE world.area BETWEEN 250000 AND 300000;
```
---
#### Operador XOR
Nos sirve para mostrar los datos que cumplan una condición, la otra pero ambas no.
  >**EJEMPLO:**

Nos mostrará los `name`, `population` y `area` en los que se cumpla  que `area` sea mayor que `3000000` o que `population` sea mayor que `250000000`, siempre y cuando no se cumplan ambas condiciones.
```SQL
SELECT world.name, world.population, world.area
FROM world
WHERE world.population>250000000 XOR world.area>3000000;
```

#### Operador LIKE
Muestra los datos que respeten un patrón predefinido.

> **%** - representa uno, muchos o ningún carácter.
> **_** - representa un carácter


> Para expresiones regulares **debemos** usar `LIKE` en vez de `=`

  >**EJEMPLOS:**

Nos mostrará los `name` en los que `name` **incluya** la palabra `'United'`
```SQL
SELECT world.name 
FROM world
WHERE world.name LIKE '%United%';
```
Nos mostrará los `winner` en los que `name` **empiece** por `John_ `.
```SQL
SELECT nobel.winner 
FROM nobel
WHERE nobel.winner LIKE 'John %';
```
Nos mostrará los `name` en los que `name` **tenga** 5 caracteres.
```SQL
SELECT world.name 
FROM world
WHERE world.name LIKE '______';
```
Nos mostrará los `winner` en los que `winner` tenga `o` como segundo carácter.
```SQL
SELECT nobel.winner 
FROM nobel
WHERE nobel.winner LIKE '_o'
```
---
#### Operador ALL
Nos sirve para comprobar si una función se cumple en todas las filas.

> Deberemos usar **`IS NOT NULL`** para evitar errores

>**EJEMPLO:**

Nos mostrará el `name` en el que `gdp` es **mayor a todos** los de `'Africa'`
```SQL
SELECT world.name 
FROM world
WHERE world.gdp > ALL (
                       SELECT world.gdp
                       FROM world
                       WHERE world.continent = 'Africa'
                       AND world.gdp IS NOT NULL);
```
---

### GROUP BY
Nos sirve para agrupar en subtablas los valores que se encuentren repetidos, es decir nos permite agrupar los resultados por un campo concreto.
Una vez que estos estén agrupados se pueden usar funciones como **`COUNT, MAX, MIN...`**
>**EJEMPLOS:**

Nos mostrará `continent` y el número de valores `name` **para cada** `continent`.
```SQL
SELECT world.continent, COUNT(world.name)
FROM world
GROUP BY world.continent;
```
Nos mostrará `continent` y el número de valores `name` **para cada** `continent` donde `population` sea mayor o igual que `10000000`
```SQL
SELECT world.continent, COUNT(world.name)
FROM world
WHERE world.population >= 10000000
GROUP BY world.continent;
```
---
### HAVING
Con el `HAVING` podemos usar las mismas funciones de agregado que usamos en el `SELECT`, es decir con `HAVING` podemos filtrar los resultados obtenidos mediante el `GROUP BY`.

> Es necesario que anteriormente hubiésemos usado un **`GROUP BY`**.

>**EJEMPLO:**

Nos mostrará los `continent` en los que la suma de `population` sea mayor o igual a `100000000`.
```SQL
SELECT world.continent
FROM world
GROUP BY world.continent
HAVING SUM(world.population)>= 100000000;
```
---
### ORDER BY
Nos sirve para ordenar una consulta según unos datos de manera *ascendiente* **`ASC`** o *descendiente* **`DESC`**.
>**EJEMPLOS:**

Nos mostrará los `name` donde `continent` sea igual a `'Europe'` ordenados ascendentemente.
```SQL
SELECT world.name 
FROM world
WHERE world.continent = 'Europe'
ORDER BY world.name;
```
> Por defecto si no ponemos nada es como si estuviera el valor **`ASC`**, es decir, estarían ordenados ascendentemente.
>
Nos mostrará los `winner`, `yr`, `subject` en los que `winner` empiece por `Sir`. Esto estará ordenado *descendentemente* por `yr` y *ascendentemente* por `winner`.
```SQL
SELECT nobel.winner, nobel.yr, nobel.subject
FROM nobel 
WHERE nobel.winner LIKE 'Sir%'
ORDER BY nobel.yr DESC, nobel.winner;
```
---
### SELECTS ANIDADOS
Nos sirven para realizar subconsultas dentro de una consulta, o lo que es lo mismo, realizar `SELECT` dentro de `WHERE`.
>**EJEMPLOS:**

Nos mostrará los `name` en los que `population` sea mayor que la `population` de `'Russia'`.
```SQL
SELECT world.name 
FROM world
WHERE world.population >
     (SELECT world.population FROM world
      WHERE world.name='Russia');
```
Nos mostrará el `continent`, `name` y `area` con mayor `area` de cada `continent`.
```SQL       
SELECT x.continent, x.name, x.area 
FROM world x
WHERE x.area >= ALL
    (SELECT y.area FROM world y
                 WHERE y.continent=x.continent
                 AND y.area IS NOT NULL );
```

> Es necesario **usar alias** en `world` porque si no lo compararíamos con el de la misma consulta y nos daría error

### JOIN
Nos sirve para poder trabajar con más tablas que unimos con `JOIN` dándonos una tabla como resultado.
Hay tres tipos de JOIN:

 - **`INNER JOIN`** es lo mismo que `JOIN`, devuelve  **todas las filas** cuando hay al menos **una coincidencia** en **ambas** tablas.
 ![enter image description here](http://www.w3resource.com/sql/joins/joins-output/sql-inner-jon.gif)
 - **`LEFT JOIN`** Devuelve todas las filas de la tabla de la **izquierda**, y las filas coincidentes de la tabla de la **derecha**.
Esto significará que si no hay coincidencias para la cláusula **ON** en la tabla de la derecha, aún así la combinación retornará las filas de la primera tabla en el resultado.
 ![enter image description here](http://www.w3resource.com/sql/joins/joins-output/sql-left-jon.png)
 - **`RIGHT JOIN`**: Devuelve todas las filas de la tabla de la **derecha**, y las filas coincidentes de la tabla de la **izquierda**.
Esto significará que si no hay coincidencias para la cláusula **ON** en la tabla de la izquierda, aún así la combinación retornará las filas de la primera tabla en el resultado.
 ![enter image description here](http://www.w3resource.com/sql/joins/joins-output/sql-right-jon.gif)
 >**EJEMPLOS:**

Nos mostrará el `player`, `teamid` de `goal` y el `stadium` y `mdate` de `game` en el que `goal.teamid` sea igual a `'GER'`, es decir, goles que marcó Alemania.
```SQL 
SELECT goal.player, goal.teamid, game.stadium,  game.mdate
FROM game JOIN goal ON (id=matchid)
WHERE goal.teamid='GER';
```
Nos mostrará el `yr` y el número total de `movie` que ha hecho cada año en el cuál haya hecho más de dos `movie`.
```SQL 
SELECT movie.yr,COUNT(movie.title) 
FROM movie INNER JOIN casting ON movie.id=movieid
           INNER JOIN actor ON actorid=actor.id
WHERE actor.name='Rock Hudson'
GROUP BY movie.yr
HAVING COUNT(movie.title) > 2;
```
Nos mostrará agrupados por `matchid` y `mdate` los goles donde `'POL'` jugó, es decir, que o era `team1` o era `team2`.
```SQL 
SELECT goal.matchid, game.mdate, COUNT(*) AS 'Goals'  
FROM game JOIN goal ON matchid = id  
WHERE (game.team1 = 'POL' OR game.team2 = 'POL')  
GROUP BY goal.matchid, game.mdate;
```

### NULL
Nos sirve para poder filtrar valores nulos.
 >**EJEMPLOS:**

Nos mostrará los `name` en los que `dept` sea `NULL`.
```SQL
SELECT teacher.name
FROM teacher
WHERE teacher.dept IS NULL;
```
Nos mostrará los `name` en los que `dept` no sea NULL.
```SQL
SELECT teacher.name
FROM teacher
WHERE teacher.dept IS NOT NULL;
```
Nos mostrará el `name` y el `dept.name`, en caso de que este sea nulo se mostrará el valor `'None'`
```SQL
SELECT teacher.name, COALESCE(dept.name,'None')
FROM teacher LEFT JOIN dept ON teacher.dept=dept.id
```
![Texto Alternativo](C:\Users\iagog\Pictures\mysql\mysql1.png)
