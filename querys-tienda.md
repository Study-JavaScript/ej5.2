# Exercisis SQL queries: 'Tienda'

1. **Llista el nom de tots els productes que hi ha en la taula `producto`.**
    ```sql
    SELECT nombre FROM producto;
    ```
2. **Llista els noms i els preus de tots els productes de la taula `producto`.**
    ```sql
    SELECT nombre, precio FROM producto;
    ```
3. **Llista totes les columnes de la taula `producto`.**
    ```sql
    SELECT * FROM producto;
    ```
4. **Llista el nom dels productes, el preu en euros i el preu en dòlars estatunidencs (USD).**
    ```sql
    SELECT nombre, precio as in_eur, precio * 1.1 AS in_usd FROM producto;
    ```
5. **Llista el nom dels productes, el preu en euros i el preu en dòlars estatunidencs (USD).**  
   Utilitza els següents àlies per a les columnes: `nom de producto`, `euros`, `dòlars`.
    ```sql
    SELECT nombre, precio as euros, precio * 1.1 AS dòlars FROM producto;
    ```
6. **Llista els noms i els preus de tots els productes de la taula `producto`, convertint els noms a majúscula.**
    ```sql
    SELECT UPPER(nombre) AS nombre, precio FROM producto;
    ```
7. **Llista els noms i els preus de tots els productes de la taula `producto`, convertint els noms a minúscula.**
    ```sql
    SELECT LOWER(nombre) AS nombre, precio FROM producto;
    ```
8. **Llista el nom de tots els fabricants en una columna, i en una altra columna obtingui en majúscules els dos primers caràcters del nom del fabricant.**
    ```sql
    SELECT nombre, UPPER(LEFT(nombre,2)) AS iniciales FROM fabricante
    ```
9. **Llista els noms i els preus de tots els productes de la taula `producto`, arrodonint el valor del preu.**
    ```sql
    SELECT nombre, ROUND(precio, 0) AS precio FROM producto;
    ```
10. **Llista els noms i els preus de tots els productes de la taula `producto`, truncant el valor del preu per a mostrar-lo sense cap xifra decimal.**
    ```sql
    SELECT nombre, TRUNCATE(precio, 0) AS precio FROM producto;
    ```
11. **Llista el codi dels fabricants que tenen productes en la taula `producto`.**
    ```sql
    SELECT codigo_fabricante FROM producto
    ```
12. **Llista el codi dels fabricants que tenen productes en la taula `producto`, eliminant els codis que apareixen repetits.**
    ```sql
    SELECT DISTINCT codigo_fabricante FROM producto
    ```
13. **Llista els noms dels fabricants ordenats de manera ascendent.**
    ```sql
    SELECT nombre FROM fabricante ORDER BY nombre asc;
    ```
14. **Llista els noms dels fabricants ordenats de manera descendent.**
    ```sql
    SELECT nombre FROM fabricante ORDER BY nombre desc;
    ```
15. **Llista els noms dels productes ordenats, en primer lloc, pel nom de manera ascendent i, en segon lloc, pel preu de manera descendent.**
    ```sql
    SELECT nombre, precio FROM producto ORDER BY nombre asc, precio desc;
    ```
16. **Retorna una llista amb les 5 primeres files de la taula `fabricante`.**
    ```sql
    SELECT * FROM fabricante LIMIT 5;
    ```
17. **Retorna una llista amb 2 files a partir de la quarta fila de la taula `fabricante`.**  
    La quarta fila també s'ha d'incloure en la resposta.
    ```sql
    SELECT * FROM fabricante LIMIT 3, 2;
    ```
18. **Llista el nom i el preu del producte més barat.**  
    (Utilitza solament les clàusules `ORDER BY` i `LIMIT`).  
    **NOTA:** Aquí no podria usar `MIN(preu)`, necessitaria `GROUP BY`.
    ```sql
    SELECT nombre, precio FROM producto ORDER BY precio asc LIMIT 1;
    ```
19. **Llista el nom i el preu del producte més car.**  
    (Utilitza solament les clàusules `ORDER BY` i `LIMIT`).  
    **NOTA:** Aquí no podria usar `MAX(preu)`, necessitaria `GROUP BY`.
    ```sql
    SELECT nombre, precio FROM producto ORDER BY precio desc LIMIT 1;
    ```
20. **Llista el nom de tots els productes del fabricant el codi de fabricant del qual és igual a 2.**
    ```sql
    SELECT nombre FROM producto WHERE codigo_fabricante = 2;
    ```
21. **Retorna una llista amb el nom del producte, preu i nom de fabricant de tots els productes de la base de dades.**
    ```sql
    SELECT p.nombre, p.precio, f.nombre as fabricante FROM producto p JOIN fabricante f ON f.codigo = p.codigo;
    ```
22. **Retorna una llista amb el nom del producte, preu i nom de fabricant de tots els productes de la base de dades.**  
    Ordena el resultat pel nom del fabricant, per ordre alfabètic.
    ```sql
    SELECT p.nombre, p.precio, f.nombre as fabricante FROM producto p JOIN fabricante f ON f.codigo = p.codigo ORDER BY f.nombre ASC;
    ```
23. **Retorna una llista amb el codi del producte, nom del producte, codi del fabricador i nom del fabricador, de tots els productes de la base de dades.**
    ```sql
    SELECT p.codigo as codigo_producto, p.nombre, f.codigo as codigo_fabricante, f.nombre as fabricante FROM producto p JOIN fabricante f ON f.codigo = p.codigo;
    ```
24. **Retorna el nom del producte, el seu preu i el nom del seu fabricant, del producte més barat.**
    ```sql
    SELECT  p.nombre,p.precio, f.nombre as fabricante 
    FROM producto p 
    JOIN fabricante f ON f.codigo = p.codigo
    ORDER BY p.precio ASC LIMIT 1;
    ```
25. **Retorna el nom del producte, el seu preu i el nom del seu fabricant, del producte més car.**
    ```sql
    SELECT  p.nombre,p.precio, f.nombre as fabricante 
    FROM producto p 
    JOIN fabricante f ON f.codigo = p.codigo
    ORDER BY p.precio DESC LIMIT 1;
    ```
26. **Retorna una llista de tots els productes del fabricant Lenovo.**
    ```sql
    SELECT  p.nombre,p.precio  
    FROM producto p 
    JOIN fabricante f ON f.codigo = p.codigo
    WHERE f.nombre = "Lenovo";
    ```
27. **Retorna una llista de tots els productes del fabricant Crucial que tinguin un preu major que 200 €.**
    ```sql
    SELECT  p.nombre,p.precio 
    FROM producto p 
    JOIN fabricante f ON f.codigo = p.codigo
    WHERE f.nombre = 'Crucial' AND p.precio > 200;
    ```
28. **Retorna un llistat amb tots els productes dels fabricants Asus, Hewlett-Packard i Seagate.**  
    Sense utilitzar l'operador `IN`.
    ```sql
    SELECT  p.nombre,p.precio
    FROM producto p 
    JOIN fabricante f ON f.codigo = p.codigo
    WHERE f.nombre = "Asus" OR f.nombre = "Hewlett-Packard" OR f.nombre = "Seagate";
    ```
29. **Retorna un llistat amb tots els productes dels fabricants Asus, Hewlett-Packard i Seagate.**  
    Fent servir l'operador `IN`.
    ```sql
    SELECT  p.nombre,p.precio
    FROM producto p 
    JOIN fabricante f ON f.codigo = p.codigo
    WHERE f.nombre IN ("Asus", "Hewlett-Packard" ,"Seagate");
    ```
30. **Retorna un llistat amb el nom i el preu de tots els productes dels fabricants el nom dels quals acabi per la vocal e.**
    ```sql
    SELECT  p.nombre,p.precio, f.nombre as fabricante 
    FROM producto p 
    JOIN fabricante f ON f.codigo = p.codigo
    WHERE f.nombre LIKE "%e"
    ```
31. **Retorna un llistat amb el nom i el preu de tots els productes el nom de fabricant dels quals contingui el caràcter w en el seu nom.**
    ```sql
    SELECT  p.nombre,p.precio, f.nombre as fabricante 
    FROM producto p 
    JOIN fabricante f ON f.codigo = p.codigo
    WHERE f.nombre LIKE "%w%"
    ```
32. **Retorna un llistat amb el nom de producte, preu i nom de fabricant, de tots els productes que tinguin un preu major o igual a 180 €.**  
    Ordena el resultat, en primer lloc, pel preu (en ordre descendent) i, en segon lloc, pel nom (en ordre ascendent).
    ```sql
    SELECT  p.nombre,p.precio, f.nombre as fabricante 
    FROM producto p 
    JOIN fabricante f ON f.codigo = p.codigo
    WHERE p.precio >= 180
    ORDER BY p.precio desc, p.nombre asc
    ```
33. **Retorna un llistat amb el codi i el nom de fabricant, solament d'aquells fabricants que tenen productes associats en la base de dades.**
    ```sql
    SELECT DISTINCT f.codigo, f.nombre 
    FROM fabricante f 
    JOIN producto p ON f.codigo = p.codigo_fabricante;
    ```
34. **Retorna un llistat de tots els fabricants que existeixen en la base de dades, juntament amb els productes que té cadascun d'ells.**  
    El llistat haurà de mostrar també aquells fabricants que no tenen productes associats.
    ```sql
    SELECT f.codigo, f.nombre AS fabricant, p.nombre AS producte
    FROM fabricante f
    LEFT JOIN producto p ON f.codigo = p.codigo_fabricante
    ```
35. **Retorna un llistat on només apareguin aquells fabricants que no tenen cap producte associat.**
    ```sql
    SELECT f.codigo, f.nombre AS fabricant, p.nombre AS producte
    FROM fabricante f
    LEFT JOIN producto p ON f.codigo = p.codigo_fabricante
    WHERE p.codigo_fabricante IS NULL;
    ```
36. **Retorna tots els productes del fabricador Lenovo.**  
    (Sense utilitzar `INNER JOIN`).
    ```sql
    SELECT *
    FROM producto 
    WHERE codigo_fabricant = (
        SELECT codigo
        FROM fabricante 
        WHERE nombre = 'Lenovo'
    );
    ```
37. **Retorna totes les dades dels productes que tenen el mateix preu que el producte més car del fabricant Lenovo.**  
    (Sense usar `INNER JOIN`).
    ```sql
    SELECT *
    FROM producto
    WHERE precio = (
        SELECT MAX(precio)
        FROM producto
        WHERE codigo_fabricante = (
            SELECT codigo
            FROM fabricante
            WHERE nombre="Lenovo"
        )
    )
    ```
38. **Llista el nom del producte més car del fabricant Lenovo.**
    ```sql
    SELECT p.nombre 
    FROM fabricante f 
    JOIN producto p ON f.codigo = p.codigo_fabricante;
    WHERE f.nombre = "Lenovo"
    ORDER BY p.precio desc LIMIT 1;
    ```
39. **Llista el nom del producte més barat del fabricant Hewlett-Packard.**
    ```sql
    SELECT p.nombre 
    FROM fabricante f 
    JOIN producto p ON f.codigo = p.codigo_fabricante
    WHERE f.nombre = "Hewlett-Packard"
    ORDER BY p.precio asc LIMIT 1;
    ```
40. **Retorna tots els productes de la base de dades que tenen un preu major o igual al producte més car del fabricant Lenovo.**
    ```sql
    SELECT *
    FROM producto
    WHERE precio >= (
        SELECT MAX(p.precio)
        FROM producto p
        JOIN fabricante f ON p.codigo_fabricante = f.codigo
        WHERE f.nombre = 'Lenovo'
    );
    ```
41. **Llesta tots els productes del fabricant Asus que tenen un preu superior al preu mitjà de tots els seus productes.**
    ```sql
    SELECT p.*
    FROM producto p
    JOIN fabricante f ON p.codigo_fabricante = f.codigo
    WHERE f.nombre = 'Asus'
    AND p.precio > (
        SELECT AVG(p1.precio)
        FROM producto p1
        JOIN fabricante f1 ON p1.codigo_fabricante = f1.codigo
        WHERE f1.nombre = 'Asus'
    );
    ```