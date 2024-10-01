# Ejercicios SQL queries: 'Universidad'

1. **Retorna un listado con el primer apellido, segundo apellido y el nombre de todos los alumnos.**  
   - El listado deberá estar ordenado alfabéticamente de menor a mayor por el primer apellido, segundo apellido y nombre.
   ```sql
   SELECT p.apellido1, p.apellido2, p.nombre FROM persona p 
    WHERE p.tipo = 'alumno' 
    ORDER BY `p`.`apellido1` ASC, `p`.`apellido2` ASC, `p`.`nombre` asc
   ```
2. **Esbrina el nombre y los dos apellidos de los alumnos que no han dado de alta su número de teléfono en la base de datos.**
    ```sql
    SELECT p.nombre, p.apellido1, p.apellido2 FROM persona p WHERE p.tipo = 'alumno' AND (p.telefono IS NULL OR p.telefono = '');
    ```
3. **Retorna el listado de alumnos que nacieron en 1999.**
    ```sql
    SELECT p.nombre, p.apellido1, p.apellido2
    FROM persona p
    WHERE p.tipo = 'alumno' AND p.fecha_nacimiento LIKE "1999%"
    ```
4. **Retorna el listado de profesores que no han dado de alta su número de teléfono en la base de datos y además su NIF termina en K.**
    ```sql
    SELECT p.nombre, p.apellido1, p.apellido2 FROM persona p WHERE p.tipo = 'profesor' AND (p.telefono IS NULL OR p.telefono = '') AND p.nif LIKE "%K";
    ```   
5. **Retorna el listado de las asignaturas que se imparten en el primer cuatrimestre, en el tercer curso del grado que tiene el identificador 7.**
    ```sql
    SELECT nombre
    FROM asignatura
    WHERE cuatrimestre = 1
    AND curso = 3
    AND id_grado = 7;
    ```   
6. **Retorna un listado de los profesores junto con el nombre del departamento al que están vinculados.**  
   - El listado debe retornar cuatro columnas: primer apellido, segundo apellido, nombre y nombre del departamento.  
   - El resultado estará ordenado alfabéticamente de menor a mayor por los apellidos y el nombre.
    ```sql
    SELECT p.apellido1, p.apellido2, p.nombre, d.nombre AS nombre_departamento
    FROM profesor pr
    JOIN persona p ON pr.id_profesor = p.id
    JOIN departamento d ON pr.id_departamento = d.id
    ORDER BY p.apellido1, p.apellido2, p.nombre;
    ```   
7. **Retorna un listado con el nombre de las asignaturas, año de inicio y año de fin del curso escolar del alumno con NIF 26902806M.**
    ```sql
    SELECT a.nombre AS nombre_asignatura, ce.anyo_inicio, ce.anyo_fin
    FROM persona p
    JOIN alumno_se_matricula_asignatura am ON p.id = am.id_alumno
    JOIN asignatura a ON am.id_asignatura = a.id
    JOIN curso_escolar ce ON am.id_curso_escolar = ce.id
    WHERE p.nif = '26902806M';
    ```   
8. **Retorna un listado con el nombre de todos los departamentos que tienen profesores que imparten alguna asignatura en el Grado en Ingeniería Informática (Plan 2015).**
    ```sql
    SELECT de.nombre FROM departamento de
    JOIN profesor pr ON pr.id_departamento = de.id
    JOIN asignatura asi ON pr.id_profesor = asi.id_profesor
    JOIN grado gr ON asi.id_grado = gr.id
    WHERE gr.nombre = "Grado en Ingeniería Informática (Plan 2015)"
    GROUP BY gr.nombre;
    ```
9. **Retorna un listado con todos los alumnos que se han matriculado en alguna asignatura durante el curso escolar 2018/2019.**
    ```sql
    SELECT * FROM persona pe
    JOIN alumno_se_matricula_asignatura asma ON pe.id = asma.id_alumno
    JOIN curso_escolar ce ON ce.id = asma.id_curso_escolar
    WHERE ce.anyo_inicio = "2018" AND ce.anyo_fin = "2019"
    GROUP BY pe.id
    ```
## Consultas con LEFT JOIN y RIGHT JOIN
10. **Devuelve un listado con los nombres de todos los profesores/as y los departamentos que tienen vinculados. El listado también debe mostrar aquellos profesores/as que no tienen ningún departamento asociado.**
    - El listado debe devolver cuatro columnas, nombre del departamento, primer apellido, segundo apellido y nombre del profesor/a. 
    - El resultado estará ordenado alfabéticamente de menor a mayor por el nombre del departamento, apellidos y nombre.
    ```sql
    SELECT 
        d.nombre AS nombre_departamento, 
        p.apellido1 AS primer_apellido, 
        p.apellido2 AS segundo_apellido, 
        p.nombre AS nombre_profesor
    FROM profesor pr
    LEFT JOIN departamento d
        ON pr.id_departamento = d.id
    LEFT JOIN persona p
        ON pr.id_profesor = p.id
    WHERE p.tipo = 'profesor'
    ORDER BY d.nombre ASC, p.apellido1 ASC, p.apellido2 ASC, p.nombre ASC;
    ```
11. **Devuelve un listado con los profesores/as que no están asociados a un departamento.**
    ```sql
    SELECT 
        p.apellido1 AS primer_apellido, 
        p.apellido2 AS segundo_apellido, 
        p.nombre AS nombre_profesor
    FROM profesor pr
    LEFT JOIN departamento d
        ON pr.id_departamento = d.id
    LEFT JOIN persona p
        ON pr.id_profesor = p.id
    WHERE pr.id_departamento IS NULL
    AND p.tipo = 'profesor'
    ORDER BY p.apellido1 ASC, p.apellido2 ASC, p.nombre ASC;

    ```
12. **Retorna un listado con los departamentos que no tienen profesores asociados.**
    ```sql
    SELECT 
        d.nombre AS nombre_departamento
    FROM departamento d
    LEFT JOIN profesor pr
        ON d.id = pr.id_departamento
    WHERE pr.id_departamento IS NULL
    ORDER BY d.nombre ASC;
    ```
    ```sql
    SELECT 
        d.nombre AS nombre_departamento
    FROM profesor pr
    RIGHT JOIN departamento d
        ON pr.id_departamento = d.id
    WHERE pr.id_departamento IS NULL
    ORDER BY d.nombre ASC;
    
    ```
13. **Retorna un listado con los profesores que no imparten ninguna asignatura.**
    ```sql
    SELECT pe.nombre as nombre_profesor FROM persona pe
    INNER JOIN profesor pr ON pe.id = pr.id_profesor
    LEFT JOIN asignatura asi ON pr.id_profesor = asi.id_profesor
    WHERE asi.id_profesor IS NULL AND pe.tipo = "profesor";
    ```	
14. **Retorna un listado con las asignaturas que no tienen un profesor asignado.**
    ```sql
    SELECT asi.nombre as nombre_asignatura
        FROM asignatura asi
    LEFT JOIN profesor pr ON pr.id_profesor = asi.id_profesor
    WHERE pr.id_profesor IS null
    ```
15. **Retorna un listado con todos los departamentos que no han impartido asignaturas en ningún curso escolar.**
    ```sql
    SELECT d.id, d.nombre
    FROM departamento d
    LEFT JOIN profesor p ON d.id = p.id_departamento
    LEFT JOIN asignatura a ON p.id_profesor = a.id_profesor
    LEFT JOIN alumno_se_matricula_asignatura am ON a.id = am.id_asignatura
    LEFT JOIN curso_escolar ce ON am.id_curso_escolar = ce.id
    WHERE a.id IS NULL
    GROUP BY d.id;
    ```
## Consultas resumen

16. **Retorna el número total de alumnos que hay.**
    ```sql
    SELECT COUNT(DISTINCT p.id) AS num_alumnos
    FROM persona p
    JOIN alumno_se_matricula_asignatura asma ON asma.id_alumno = p.id
    WHERE p.tipo = 'alumno';
    ```
17. **Calcula cuántos alumnos nacieron en 1999.**
    _Cambiado a 1998 ya que de 1999 no hay_
    ```sql
    SELECT DISTINCT p.*
    FROM persona p
    JOIN alumno_se_matricula_asignatura asma ON asma.id_alumno = p.id
    WHERE p.tipo = 'alumno' AND p.fecha_nacimiento LIKE "1998-%";
    ```
18. **Calcula cuántos profesores hay en cada departamento.**  
    - El resultado solo debe mostrar dos columnas: una con el nombre del departamento y otra con el número de profesores que hay en este departamento.  
    - El resultado solo incluirá los departamentos que tienen profesores asociados y deberá estar ordenado de mayor a menor por el número de profesores.
    ```sql
    SELECT de.nombre AS nombre_departamento, COUNT(DISTINCT pr.id_profesor) AS numero_profesores
    FROM persona pe 
    INNER JOIN profesor pr ON pr.id_profesor = pe.id
    INNER JOIN departamento de ON de.id = pr.id_departamento
    GROUP BY de.nombre
    HAVING COUNT(DISTINCT pr.id_profesor) > 0
    ORDER BY numero_profesores DESC;
    ```
19. **Retorna un listado con todos los departamentos y el número de profesores que hay en cada uno de ellos.**  
    - Tenga en cuenta que pueden existir departamentos que no tienen profesores asociados.  
    - Estos departamentos también deben aparecer en el listado.
    ```sql
    SELECT de.nombre AS nombre_departamento, COUNT(DISTINCT pr.id_profesor) AS numero_profesores
    FROM departamento de
    LEFT JOIN profesor pr ON de.id = pr.id_departamento
    LEFT JOIN persona pe ON pr.id_profesor = pe.id
    GROUP BY de.nombre
    ORDER BY numero_profesores DESC;
    ```
20. **Retorna un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno.**  
    - Tenga en cuenta que pueden existir grados que no tienen asignaturas asociadas.  
    - Estos grados también deben aparecer en el listado.  
    - El resultado deberá estar ordenado de mayor a menor por el número de asignaturas.
    ```sql
    SELECT gr.nombre AS nombre_grado, COUNT(DISTINCT asi.id) AS numero_asignaturas
    FROM grado gr
    LEFT JOIN asignatura asi ON asi.id_grado = gr.id
    GROUP BY gr.nombre
    ORDER BY numero_asignaturas DESC;
    ```    
    - **Nombre de cada asignatura asociada a cada grado**
    ```sql
    SELECT gr.nombre AS nombre_grado, asi.nombre AS nombre_asignatura
    FROM grado gr
    LEFT JOIN asignatura asi ON asi.id_grado = gr.id
    ORDER BY gr.nombre, asi.nombre;
    ```
21. **Retorna un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno, de los grados que tengan más de 40 asignaturas asociadas.**
    ```sql
    SELECT gr.nombre AS nombre_grado, COUNT(DISTINCT asi.id) AS numero_asignaturas
    FROM grado gr
    LEFT JOIN asignatura asi ON asi.id_grado = gr.id
    GROUP BY gr.nombre
    HAVING numero_asignaturas > 40
    ORDER BY numero_asignaturas DESC;
    ```   
22. **Retorna un listado que muestre el nombre de los grados y la suma del número total de créditos que hay para cada tipo de asignatura.**  
    - El resultado debe tener tres columnas: nombre del grado, tipo de asignatura y la suma de los créditos de todas las asignaturas que hay de este tipo.
    ```sql
    SELECT gr.nombre AS nombre_grado, asi.tipo AS tipo_asignatura, SUM(asi.creditos) as suma_creditos 
    FROM grado gr 
    INNER JOIN asignatura asi ON asi.id_grado = gr.id 
    GROUP BY nombre_grado, tipo_asignatura;
    ```  
23. **Retorna un listado que muestre cuántos alumnos se han matriculado de alguna asignatura en cada uno de los cursos escolares.**  
    - El resultado deberá mostrar dos columnas: una columna con el año de inicio del curso escolar y otra con el número de alumnos matriculados.
    ```sql
    SELECT ce.anyo_inicio AS inicio_curso, COUNT(DISTINCT asma.id_alumno) as alumnos_matriculados
        FROM curso_escolar ce
    INNER JOIN alumno_se_matricula_asignatura asma ON asma.id_curso_escolar = ce.id
    GROUP BY inicio_curso
    ```
24. **Retorna un listado con el número de asignaturas que imparte cada profesor.**  
    - El listado debe tener en cuenta aquellos profesores que no imparten ninguna asignatura.  
    - El resultado mostrará cinco columnas: id, nombre, primer apellido, segundo apellido y número de asignaturas.  
    - El resultado estará ordenado de mayor a menor por el número de asignaturas.
    ```sql
    SELECT pe.id AS id_profesor, 
        pe.nombre AS nombre_profesor, 
        CONCAT(pe.apellido1, " ", pe.apellido2) AS apellidos_profesor, 
        COUNT(DISTINCT asi.id) AS numero_asignaturas
    FROM persona pe 
    LEFT JOIN profesor pr ON pr.id_profesor = pe.id
    LEFT JOIN asignatura asi ON asi.id_profesor = pr.id_profesor
    WHERE pe.tipo = "profesor"
    GROUP BY pe.id, pe.nombre, pe.apellido1, pe.apellido2
    ORDER BY numero_asignaturas DESC;
    ``` 
25. **Retorna todas las datos del alumno más joven.**
    ```sql
    SELECT p.*
    FROM persona p
    WHERE p.tipo = 'alumno'
    ORDER BY p.fecha_nacimiento DESC
    LIMIT 1;
    ``` 
26. **Retorna un listado con los profesores que tienen un departamento asociado y que no imparten ninguna asignatura.**
    ```sql
    SELECT pr.id_profesor AS id_profesor, 
        p.nombre AS nombre_profesor, 
        p.nif AS nif_profesor
    FROM profesor pr
    INNER JOIN departamento d ON d.id = pr.id_departamento
    INNER JOIN persona p ON p.id = pr.id_profesor
    LEFT JOIN asignatura a ON a.id_profesor = pr.id_profesor
    WHERE a.id IS NULL;
    ```