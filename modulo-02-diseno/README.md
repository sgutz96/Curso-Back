# Clase 2 --- Diseño de bases de datos: modelo E-R y normalización

> **Propósito:** pasar de un problema del mundo real a un modelo de
> datos claro, consistente y preparado para convertirse en SQL.

## 1. Objetivos

Al terminar podrás:

-   Identificar entidades, atributos y relaciones.
-   Definir cardinalidades 1:1, 1:N y N:M.
-   Elegir claves primarias y foráneas.
-   Transformar un modelo E-R en tablas.
-   Comprender 1FN, 2FN y 3FN.
-   Reconocer anomalías de inserción, actualización y eliminación.

## 2. Antes de escribir SQL

El error más frecuente al comenzar bases de datos es abrir el editor y
empezar a crear tablas sin haber pensado el problema.

El orden recomendado es:

**Problema → entidades → atributos → relaciones → cardinalidades →
modelo relacional → SQL**

## 3. Entidades y atributos

Una **entidad** representa algo sobre lo que necesitamos guardar
información.

Ejemplos:

-   `Estudiante`
-   `Programa`
-   `Curso`
-   `Profesor`

Un **atributo** describe una entidad:

`Estudiante(id_estudiante, nombre, correo, fecha_nacimiento)`

El identificador debe distinguir cada registro.

## 4. Relaciones y cardinalidad

La relación explica cómo se conectan las entidades.

### 1:1

Una persona tiene un único pasaporte y un pasaporte pertenece a una
persona.

### 1:N

Un programa tiene muchos estudiantes, pero cada estudiante pertenece a
un programa.

``` text
PROGRAMA 1 ───────── N ESTUDIANTE
```

### N:M

Un estudiante puede tomar muchos cursos y un curso puede tener muchos
estudiantes.

``` text
ESTUDIANTE N ─────── M CURSO
```

Esta relación necesita una tabla intermedia.

## 5. Claves

### Primary Key --- PK

Identifica de manera única una fila.

``` text
id_estudiante
```

### Foreign Key --- FK

Conecta una tabla con otra.

``` text
estudiante.id_programa
        ↓
programa.id_programa
```

### Clave compuesta

Una tabla intermedia puede utilizar varias columnas como identificador:

``` text
PK(id_estudiante, id_curso, semestre)
```

## 6. Pasar E-R a modelo relacional

Caso de estudio:

``` text
programa
  └──< estudiante

estudiante >──< curso
        mediante inscripcion
```

Se convierte en:

``` text
programa(
  id_programa PK,
  nombre
)

estudiante(
  id_estudiante PK,
  nombre,
  correo,
  fecha_nacimiento,
  id_programa FK
)

curso(
  id_curso PK,
  nombre,
  creditos
)

inscripcion(
  id_estudiante FK,
  id_curso FK,
  semestre,
  nota,
  PK(id_estudiante, id_curso, semestre)
)
```

## 7. Normalización

La normalización reduce redundancia y evita anomalías.

### 1FN --- valores atómicos

No guardes:

``` text
cursos = "Bases de datos, Redes, UX"
```

Cada celda debe representar un valor indivisible para el modelo.

### 2FN --- dependencia completa

Si la PK es compuesta `(id_estudiante, id_curso)` y `nombre_curso`
depende únicamente de `id_curso`, el dato no pertenece a la tabla de
inscripción.

Se mueve a `curso`.

### 3FN --- sin dependencia transitiva

Si:

``` text
estudiante → id_programa → nombre_programa
```

entonces `nombre_programa` no debería repetirse en `estudiante`. Debe
vivir en `programa`.

## 8. Integridad

Una base de datos bien diseñada protege tres cosas:

-   **Integridad de entidad:** PK única y no nula.
-   **Integridad referencial:** una FK no debe apuntar a un registro
    inexistente.
-   **Integridad de dominio:** los valores deben respetar tipo y reglas.

## 9. Actividad guiada --- Biblioteca

Diseña una base para:

-   libros;
-   autores;
-   socios;
-   préstamos.

Preguntas:

1.  ¿Cuáles son las entidades?
2.  ¿Qué atributos tiene cada una?
3.  ¿Qué relación existe entre libro y autor?
4.  ¿Puede un socio tener varios préstamos?
5.  ¿Qué PK tendría cada tabla?
6.  ¿Dónde irían las FK?

## 10. Herramientas recomendadas

Puedes dibujar el modelo con:

-   draw.io;
-   dbdiagram.io;
-   MySQL Workbench;
-   Figma.

Lo importante no es la herramienta sino que el modelo sea entendible.

## 11. Errores comunes

-   Crear una tabla para cada frase del enunciado sin analizar
    relaciones.
-   Poner una FK en el lado equivocado de una relación 1:N.
-   Guardar listas dentro de una columna.
-   Repetir información que ya existe en otra tabla.
-   Confundir un atributo con una entidad.

## 12. Entregable

Entrega un diagrama E-R de una biblioteca, el modelo relacional
correspondiente y una breve explicación de cómo aplicaste 1FN, 2FN y
3FN.

## 13. Criterios de logro

El diseño debe permitir agregar información sin duplicarla
innecesariamente y debe representar correctamente las relaciones del
problema.
