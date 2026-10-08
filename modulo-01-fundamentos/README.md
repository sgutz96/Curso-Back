# Clase 1 --- Fundamentos de bases de datos

> **Propósito:** comprender por qué existen las bases de datos, cómo se
> organiza la información y dejar listo el entorno de trabajo para el
> resto del curso.

## 1. ¿Qué vamos a aprender?

Al terminar esta clase podrás:

-   Explicar qué es una base de datos y qué problema resuelve.
-   Diferenciar **dato**, **información** y **SGBD**.
-   Reconocer tabla, fila, columna, dominio y registro.
-   Diferenciar una base de datos relacional de una no relacional.
-   Identificar cuándo una hoja de cálculo deja de ser una buena
    solución.
-   Instalar y comprobar un motor de base de datos.

## 2. La idea principal

Una aplicación necesita recordar información: usuarios, productos,
pedidos, matrículas, reservas, pagos, etc.

Una base de datos es un sistema organizado para **guardar, consultar,
modificar y proteger** esa información.

### Ejemplo

Una universidad podría necesitar:

-   estudiantes;
-   programas académicos;
-   cursos;
-   profesores;
-   matrículas;
-   notas.

Guardar todo en archivos separados rápidamente produce duplicados,
errores y dificultad para relacionar información.

## 3. Dato, información y SGBD

-   **Dato:** valor individual, por ejemplo `18`.
-   **Información:** dato interpretado dentro de un contexto, por
    ejemplo: "Ana tiene 18 años".
-   **SGBD:** software que administra bases de datos. Ejemplos: MySQL,
    PostgreSQL, SQL Server, Oracle y SQLite.

Un SGBD permite almacenar, consultar, modificar, proteger y recuperar
información.

## 4. Modelo relacional

En este curso trabajaremos principalmente con el **modelo relacional**.

  Concepto   Significado
  ---------- ----------------------------------
  Tabla      Conjunto organizado de registros
  Fila       Un registro
  Columna    Un atributo
  Dominio    Valores válidos de una columna
  PK         Identificador único
  FK         Referencia a otra tabla

Ejemplo:

    id_estudiante nombre       correo
  --------------- ------------ --------------
                1 Ana Pérez    ana@uni.edu
                2 Luis Gómez   luis@uni.edu

La tabla tiene 2 filas y 3 columnas.

## 5. Relacional vs. no relacional

  Relacional             No relacional
  ---------------------- ---------------------------------------
  Tablas                 Documentos, clave-valor, grafos, etc.
  SQL                    Depende del motor
  Esquema estructurado   Mayor flexibilidad de estructura
  PK/FK y JOIN           Relaciones con estrategias diferentes
  MySQL, PostgreSQL      MongoDB, Redis, Neo4j

No significa que uno sea "mejor" que el otro. La elección depende del
problema.

## 6. Instalación

### Opción recomendada: MySQL + MySQL Workbench

Comprueba desde la terminal:

``` bash
mysql --version
```

Después entra al servidor:

``` bash
mysql -u root -p
```

Y ejecuta:

``` sql
SELECT VERSION();
SHOW DATABASES;
```

Si aparecen la versión y las bases disponibles, el entorno funciona.

### Alternativa

También puedes trabajar con PostgreSQL + pgAdmin o SQLite + DB Browser
for SQLite. Los conceptos fundamentales son los mismos, aunque algunas
instrucciones SQL cambian.

## 7. Primera exploración

Crea una base de práctica:

``` sql
CREATE DATABASE universidad;
USE universidad;
```

Después:

``` sql
SHOW TABLES;
```

Todavía no habrá tablas. Eso es correcto: en la siguiente clase
diseñaremos su estructura.

## 8. Actividad de clase

### Reto 1 --- Detectar el problema

Imagina una hoja de cálculo de una universidad con estas columnas:

`estudiante | programa | curso1 | curso2 | curso3 | profesor | nota`

Responde:

1.  ¿Qué datos están repetidos?
2.  ¿Qué sucede si un estudiante toma 20 cursos?
3.  ¿Qué pasa si cambia el nombre de un programa?
4.  ¿Cómo buscarías todos los estudiantes de un curso?
5.  ¿Qué problemas aparecen si dos personas editan el archivo al mismo
    tiempo?

### Reto 2 --- Identificar conceptos

En la tabla `estudiante`, identifica:

-   tabla;
-   filas;
-   columnas;
-   posible PK;
-   dominios de `nombre`, `correo` y `fecha_nacimiento`.

## 9. Errores comunes

**"Una base de datos es una tabla."**\
No exactamente. Una base de datos puede contener muchas tablas
relacionadas.

**"SQL es una base de datos."**\
SQL es un lenguaje. MySQL y PostgreSQL son SGBD que utilizan SQL.

**"Todas las bases de datos funcionan igual."**\
Comparten conceptos, pero cada motor tiene características y sintaxis
específicas.

## 10. Entregable

Entrega:

-   captura de la instalación funcionando;
-   resultado de `SELECT VERSION();`;
-   respuestas de los dos retos;
-   una explicación de 5 líneas sobre por qué una base de datos es
    preferible a un conjunto de archivos sueltos para un sistema real.

## 11. Criterios de logro

-   Comprende la función de un SGBD.
-   Identifica correctamente los componentes de una tabla.
-   Explica la diferencia entre dato e información.
-   Tiene el entorno preparado para la siguiente clase.
