# Clase 8 --- Vistas, índices y seguridad

> **Propósito:** aprender técnicas que hacen una base de datos más
> reutilizable, rápida y segura.

## 1. Objetivos

-   Crear y utilizar vistas.
-   Comprender qué es un índice.
-   Saber cuándo un índice puede ayudar o perjudicar.
-   Crear usuarios y permisos.
-   Reconocer y prevenir la inyección SQL.

## 2. Vistas --- VIEW

Una vista es una consulta guardada que puede consultarse como si fuera
una tabla.

``` sql
CREATE VIEW vista_estudiantes AS
SELECT
  e.id_estudiante,
  e.nombre AS estudiante,
  p.nombre AS programa
FROM estudiante e
JOIN programa p
  ON p.id_programa = e.id_programa;
```

Después:

``` sql
SELECT *
FROM vista_estudiantes;
```

### ¿Para qué sirven?

-   simplificar consultas complejas;
-   ocultar columnas que no deberían exponerse;
-   reutilizar una consulta;
-   crear una capa de presentación de datos.

Una vista normal no significa necesariamente que los datos estén
duplicados: depende del motor y del tipo de vista.

## 3. Índices

Un índice ayuda al motor a encontrar datos con mayor eficiencia.

Ejemplo:

``` sql
CREATE INDEX idx_estudiante_nombre
ON estudiante(nombre);
```

Un índice puede ser útil para columnas que se consultan frecuentemente
mediante:

``` sql
WHERE
JOIN
ORDER BY
```

Pero no debes indexar todo. Los índices también ocupan espacio y hacen
más costosas algunas operaciones de `INSERT`, `UPDATE` y `DELETE`.

## 4. Ver índices

En MySQL:

``` sql
SHOW INDEX FROM estudiante;
```

## 5. Usuarios y permisos

Una aplicación no debería conectarse siempre como `root`.

Ejemplo conceptual en MySQL:

``` sql
CREATE USER 'app_user'@'localhost'
IDENTIFIED BY 'cambia_esta_clave';

GRANT SELECT, INSERT, UPDATE, DELETE
ON universidad.*
TO 'app_user'@'localhost';
```

Principio:

> entregar solamente los permisos que realmente necesita cada usuario.

## 6. Inyección SQL

Una aplicación vulnerable podría construir:

``` js
const sql = `SELECT * FROM estudiante WHERE correo = '${correo}'`;
```

El problema es que el usuario controla parte del SQL.

La solución es utilizar consultas parametrizadas:

``` js
const [rows] = await pool.execute(
  'SELECT * FROM estudiante WHERE correo = ?',
  [correo]
);
```

El valor viaja separado de la instrucción SQL.

## 7. Seguridad por capas

La seguridad no depende de una sola técnica:

``` text
API
 ↓
validación
 ↓
consultas parametrizadas
 ↓
usuario de BD con permisos limitados
 ↓
restricciones de la BD
```

## 8. Actividad

1.  Crear una vista de estudiantes y programas.
2.  Crear un índice para una búsqueda frecuente.
3.  Consultar el índice.
4.  Crear un usuario de aplicación con permisos limitados.
5.  Explicar con un ejemplo qué es una inyección SQL.
6.  Reescribir una consulta vulnerable usando parámetros.

## 9. Errores comunes

-   Crear índices para todas las columnas.
-   Usar `root` desde una aplicación.
-   Pensar que validar el frontend protege la base de datos.
-   Construir SQL concatenando valores del usuario.

## 10. Entregable

Entrega `seguridad.sql` y un documento breve explicando qué protegiste,
por qué creaste el índice y qué permisos necesita la aplicación.
