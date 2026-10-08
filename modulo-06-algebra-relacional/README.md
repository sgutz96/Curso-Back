# Clase 6 --- Álgebra relacional

> **Propósito:** comprender la lógica matemática que fundamenta las
> consultas relacionales y conectar esa lógica con SQL.

## 1. Objetivos

Al terminar podrás:

-   Explicar qué es una operación del álgebra relacional.
-   Utilizar selección y proyección.
-   Combinar relaciones.
-   Comprender joins y producto cartesiano.
-   Reconocer operaciones de conjuntos.
-   Traducir expresiones básicas a SQL.

## 2. ¿Por qué estudiar álgebra relacional?

SQL es el lenguaje práctico que utilizaremos, pero detrás de una
consulta existe una idea fundamental:

> tomar relaciones y transformarlas mediante operaciones para obtener la
> información que necesitamos.

Esto ayuda a construir consultas más claras y a entender qué está
haciendo el motor.

## 3. Selección --- σ

La selección filtra **filas**.

``` text
σ id_programa = 1 (estudiante)
```

Equivalente en SQL:

``` sql
SELECT *
FROM estudiante
WHERE id_programa = 1;
```

## 4. Proyección --- π

La proyección selecciona **columnas**.

``` text
π nombre, correo (estudiante)
```

SQL:

``` sql
SELECT nombre, correo
FROM estudiante;
```

Regla para memorizar:

**σ = filas**\
**π = columnas**

## 5. Unión, intersección y diferencia

Las operaciones de conjuntos trabajan con relaciones compatibles.

### Unión

``` text
A ∪ B
```

En SQL:

``` sql
SELECT correo FROM grupo_a
UNION
SELECT correo FROM grupo_b;
```

### Intersección

Conceptualmente:

``` text
A ∩ B
```

En motores que lo soportan:

``` sql
SELECT correo FROM grupo_a
INTERSECT
SELECT correo FROM grupo_b;
```

### Diferencia

``` text
A − B
```

En SQL moderno:

``` sql
SELECT correo FROM grupo_a
EXCEPT
SELECT correo FROM grupo_b;
```

## 6. Producto cartesiano --- ×

Combina cada fila de una relación con cada fila de otra.

``` text
estudiante × curso
```

SQL:

``` sql
SELECT *
FROM estudiante
CROSS JOIN curso;
```

Si hay 10 estudiantes y 5 cursos, el resultado potencial es de 50
combinaciones.

Por eso no debe utilizarse como sustituto accidental de un JOIN.

## 7. Renombramiento --- ρ

Permite trabajar con un nombre alternativo para una relación o atributo.

En SQL utilizamos alias:

``` sql
SELECT e.nombre
FROM estudiante AS e;
```

## 8. Join --- ⋈

Relaciona filas de diferentes tablas mediante una condición.

``` text
estudiante ⋈ estudiante.id_programa = programa.id_programa programa
```

SQL:

``` sql
SELECT e.nombre, p.nombre AS programa
FROM estudiante e
JOIN programa p
  ON e.id_programa = p.id_programa;
```

## 9. División --- ÷

La división representa preguntas del tipo:

> ¿Qué entidades están relacionadas con **todos** los elementos de otro
> conjunto?

Es más avanzada que las operaciones anteriores y suele resolverse en SQL
mediante combinaciones de `NOT EXISTS`, `GROUP BY` o subconsultas.

Ejemplo conceptual:

> estudiantes que han cursado todos los cursos obligatorios.

## 10. Expresiones combinadas

Las operaciones pueden encadenarse.

Conceptualmente:

``` text
π nombre (
  σ activo = TRUE (
    estudiante
  )
)
```

SQL:

``` sql
SELECT nombre
FROM estudiante
WHERE activo = TRUE;
```

## 11. Actividad

Para cada pregunta escribe:

1.  expresión de álgebra relacional;
2.  SQL equivalente;
3.  explicación de la operación principal.

Preguntas:

-   estudiantes activos;
-   nombres y correos de estudiantes;
-   estudiantes con su programa;
-   cursos con más de 3 créditos;
-   estudiantes matriculados en un curso específico.

## 12. Error conceptual frecuente

No memorices símbolos sin comprender qué transforman.

-   **Selección:** reduce filas.
-   **Proyección:** reduce columnas.
-   **Join:** relaciona información.
-   **Producto cartesiano:** combina todas las posibilidades.

## 13. Entregable

Entrega `algebra-relacional.md` o `algebra-relacional.pdf` con al menos
8 ejercicios resueltos y su equivalente en SQL.
