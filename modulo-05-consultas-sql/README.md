# Clase 5 --- Consultas SQL: filtros, ordenamiento y agregaciones

> **Propósito:** aprender a hacer preguntas útiles a la base de datos en
> lugar de limitarse a mostrar todas las filas.

## 1. Objetivos

-   Seleccionar columnas específicas.
-   Filtrar con `WHERE`.
-   Combinar condiciones.
-   Buscar texto.
-   Ordenar resultados.
-   Limitar resultados.
-   Utilizar funciones de agregación.
-   Agrupar información con `GROUP BY` y `HAVING`.

## 2. SELECT

``` sql
SELECT nombre, correo
FROM estudiante;
```

Alias:

``` sql
SELECT nombre AS estudiante,
       correo AS email
FROM estudiante;
```

## 3. WHERE

``` sql
SELECT *
FROM estudiante
WHERE id_programa = 1;
```

Comparaciones:

``` sql
=   <>   >   <   >=   <=
```

## 4. AND, OR y NOT

``` sql
SELECT *
FROM estudiante
WHERE id_programa = 1
  AND activo = TRUE;
```

Con `OR`:

``` sql
SELECT *
FROM estudiante
WHERE id_programa = 1
   OR id_programa = 2;
```

Usa paréntesis cuando combines condiciones:

``` sql
WHERE activo = TRUE
  AND (id_programa = 1 OR id_programa = 2);
```

## 5. BETWEEN, IN, LIKE e IS NULL

### BETWEEN

``` sql
SELECT *
FROM curso
WHERE creditos BETWEEN 3 AND 5;
```

### IN

``` sql
SELECT *
FROM estudiante
WHERE id_programa IN (1, 2, 3);
```

### LIKE

``` sql
SELECT *
FROM estudiante
WHERE nombre LIKE '%ana%';
```

`%` representa cualquier cantidad de caracteres.

### IS NULL

Incorrecto:

``` sql
WHERE fecha_nacimiento = NULL
```

Correcto:

``` sql
WHERE fecha_nacimiento IS NULL
```

## 6. ORDER BY, LIMIT y DISTINCT

``` sql
SELECT nombre, correo
FROM estudiante
ORDER BY nombre ASC;
```

Últimos registros:

``` sql
SELECT *
FROM estudiante
ORDER BY id_estudiante DESC
LIMIT 5;
```

Valores únicos:

``` sql
SELECT DISTINCT id_programa
FROM estudiante;
```

## 7. Funciones de agregación

``` sql
COUNT()
SUM()
AVG()
MIN()
MAX()
```

Ejemplo:

``` sql
SELECT COUNT(*) AS total_estudiantes
FROM estudiante;
```

Promedio:

``` sql
SELECT AVG(nota) AS promedio
FROM inscripcion;
```

## 8. GROUP BY

¿Cuántos estudiantes hay por programa?

``` sql
SELECT id_programa,
       COUNT(*) AS total
FROM estudiante
GROUP BY id_programa;
```

## 9. HAVING

`WHERE` filtra filas antes de agrupar.\
`HAVING` filtra grupos después de agrupar.

``` sql
SELECT id_programa,
       COUNT(*) AS total
FROM estudiante
GROUP BY id_programa
HAVING COUNT(*) >= 5;
```

## 10. Actividad: preguntas de negocio

Resuelve con SQL:

1.  ¿Cuántos estudiantes hay?
2.  ¿Cuántos están activos?
3.  ¿Qué estudiantes pertenecen a un programa específico?
4.  ¿Qué cursos tienen entre 3 y 5 créditos?
5.  ¿Cuál es el promedio general de notas?
6.  ¿Cuál es la nota máxima?
7.  ¿Cuántos estudiantes hay por programa?
8.  ¿Qué programas tienen al menos 3 estudiantes?

## 11. Orden mental de una consulta

Una forma útil de pensar una consulta es:

``` text
FROM
↓
WHERE
↓
GROUP BY
↓
HAVING
↓
SELECT
↓
ORDER BY
↓
LIMIT
```

No es exactamente el orden en que escribimos el SQL, pero ayuda a
comprender cómo se construye.

## 12. Errores comunes

-   Usar `WHERE` para filtrar agregaciones.
-   Olvidar una columna en `GROUP BY`.
-   Usar `= NULL`.
-   Confundir `LIKE '%ana%'` con una coincidencia exacta.
-   Seleccionar demasiadas columnas cuando solo se necesitan dos.

## 13. Entregable

Entrega un archivo `consultas.sql` con al menos 12 consultas comentadas
y una breve explicación de qué pregunta responde cada una.
