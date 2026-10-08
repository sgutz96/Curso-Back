# Clase 7 --- Consultas con varias tablas

> **Propósito:** aprender a obtener información real combinando tablas
> relacionadas.

## 1. Objetivos

-   Utilizar `INNER JOIN`.
-   Diferenciar `LEFT`, `RIGHT` y `FULL JOIN`.
-   Construir subconsultas.
-   Utilizar operaciones de conjuntos.
-   Elegir correctamente la estrategia de consulta.

## 2. ¿Por qué necesitamos JOIN?

La normalización evita guardar toda la información en una sola tabla.

Por eso una pregunta real suele necesitar varias tablas:

> "¿Qué estudiantes pertenecen a Ingeniería Multimedia?"

La información está repartida:

``` text
estudiante → programa
```

## 3. INNER JOIN

Devuelve coincidencias entre ambas tablas.

``` sql
SELECT
  e.nombre AS estudiante,
  p.nombre AS programa
FROM estudiante e
INNER JOIN programa p
  ON e.id_programa = p.id_programa;
```

## 4. LEFT JOIN

Conserva todas las filas de la tabla izquierda aunque no exista
coincidencia.

``` sql
SELECT
  p.nombre AS programa,
  e.nombre AS estudiante
FROM programa p
LEFT JOIN estudiante e
  ON e.id_programa = p.id_programa;
```

Esto permite encontrar programas sin estudiantes:

``` sql
SELECT p.nombre
FROM programa p
LEFT JOIN estudiante e
  ON e.id_programa = p.id_programa
WHERE e.id_estudiante IS NULL;
```

## 5. RIGHT JOIN

Es el equivalente conceptual de `LEFT JOIN` invirtiendo las tablas. En
la práctica, muchos equipos prefieren reorganizar el orden de las tablas
y utilizar `LEFT JOIN` para mantener consultas más fáciles de leer.

## 6. FULL JOIN

Devuelve coincidencias y no coincidencias de ambos lados. Su
disponibilidad depende del motor.

Cuando el motor no lo soporta directamente, puede simularse con
combinaciones de `LEFT JOIN`, `UNION` y filtros.

## 7. JOIN entre tres tablas

``` sql
SELECT
  e.nombre AS estudiante,
  c.nombre AS curso,
  i.nota
FROM inscripcion i
JOIN estudiante e
  ON e.id_estudiante = i.id_estudiante
JOIN curso c
  ON c.id_curso = i.id_curso;
```

## 8. Subconsultas

Una consulta dentro de otra.

Ejemplo:

``` sql
SELECT nombre
FROM estudiante
WHERE id_programa = (
  SELECT id_programa
  FROM programa
  WHERE nombre = 'Ingeniería Multimedia'
);
```

Para saber quién tiene una nota superior al promedio:

``` sql
SELECT id_estudiante, id_curso, nota
FROM inscripcion
WHERE nota > (
  SELECT AVG(nota)
  FROM inscripcion
);
```

## 9. UNION

Combina resultados compatibles:

``` sql
SELECT correo FROM estudiante_programa_a
UNION
SELECT correo FROM estudiante_programa_b;
```

`UNION` elimina duplicados. `UNION ALL` conserva duplicados.

## 10. Actividad práctica

Construye consultas para responder:

1.  estudiante + programa;
2.  estudiante + curso + nota;
3.  programas sin estudiantes;
4.  estudiantes cuya nota supera el promedio;
5.  cursos con estudiantes inscritos;
6.  estudiantes que no tienen inscripciones.

## 11. Errores comunes

-   Hacer JOIN con una columna incorrecta.
-   Olvidar la condición `ON`.
-   Crear duplicados inesperados por una relación N:M.
-   Confundir `WHERE` con una condición del `ON` en un `LEFT JOIN`.
-   Usar subconsultas cuando un JOIN sería más claro.

## 12. Entregable

Entrega `joins.sql` con 10 consultas y un comentario encima de cada una
explicando la pregunta de negocio que resuelve.
