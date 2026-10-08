# Clase 4 --- CRUD con SQL: INSERT, SELECT, UPDATE y DELETE

> **Propósito:** aprender a trabajar con registros reales y entender el
> ciclo de vida de la información.

## 1. Objetivos

Al terminar podrás:

-   Insertar registros.
-   Consultarlos.
-   Actualizarlos.
-   Eliminarlos.
-   Usar `WHERE` de forma segura.
-   Reconocer el riesgo de modificar datos sin filtros.
-   Utilizar transacciones básicas.

## 2. CRUD

CRUD representa las cuatro operaciones fundamentales:

  Operación   SQL
  ----------- --------
  Create      INSERT
  Read        SELECT
  Update      UPDATE
  Delete      DELETE

## 3. Preparar datos

Primero:

``` sql
USE universidad;
```

Insertamos programas:

``` sql
INSERT INTO programa (nombre)
VALUES
('Ingeniería Multimedia'),
('Diseño'),
('Ingeniería de Sistemas');
```

Consultar:

``` sql
SELECT * FROM programa;
```

## 4. CREATE --- INSERT

``` sql
INSERT INTO estudiante
(nombre, correo, fecha_nacimiento, id_programa)
VALUES
('Ana Pérez', 'ana@uni.edu', '2005-03-14', 1);
```

Puedes insertar varios registros:

``` sql
INSERT INTO estudiante (nombre, correo, id_programa)
VALUES
('Luis Gómez', 'luis@uni.edu', 1),
('Sara León', 'sara@uni.edu', 2);
```

## 5. READ --- SELECT

``` sql
SELECT * FROM estudiante;
```

Es mejor seleccionar únicamente lo que necesitas:

``` sql
SELECT id_estudiante, nombre, correo
FROM estudiante;
```

## 6. UPDATE

Nunca olvides el `WHERE`:

``` sql
UPDATE estudiante
SET correo = 'ana.perez@uni.edu'
WHERE id_estudiante = 1;
```

Comprueba:

``` sql
SELECT *
FROM estudiante
WHERE id_estudiante = 1;
```

## 7. DELETE

``` sql
DELETE FROM estudiante
WHERE id_estudiante = 3;
```

Antes de borrar, prueba primero:

``` sql
SELECT *
FROM estudiante
WHERE id_estudiante = 3;
```

## 8. El peligro del WHERE

Esto es extremadamente peligroso:

``` sql
UPDATE estudiante
SET activo = FALSE;
```

Actualiza todas las filas.

Y esto:

``` sql
DELETE FROM estudiante;
```

elimina todos los registros de la tabla.

Regla de clase:

> Antes de ejecutar `UPDATE` o `DELETE`, prueba el mismo filtro con
> `SELECT`.

## 9. Transacciones

Una transacción agrupa operaciones que deben tratarse como una unidad.

``` sql
START TRANSACTION;

UPDATE estudiante
SET id_programa = 2
WHERE id_estudiante = 1;

-- Si todo está bien:
COMMIT;
```

Si algo sale mal:

``` sql
ROLLBACK;
```

## 10. Taller integrador

Realiza:

1.  5 programas.
2.  10 estudiantes.
3.  5 cursos.
4.  15 inscripciones.
5.  Consulta todos los registros.
6.  Actualiza 3 estudiantes.
7.  Elimina un registro de prueba.
8.  Realiza una transacción con `COMMIT`.
9.  Realiza una transacción de prueba con `ROLLBACK`.

## 11. Errores comunes

-   Insertar una FK que no existe.
-   Usar un correo duplicado cuando está definido como `UNIQUE`.
-   Actualizar todas las filas por olvidar `WHERE`.
-   Borrar datos sin comprobar previamente el filtro.
-   Confundir `NULL` con una cadena vacía.

## 12. Entregable

Entrega `seed.sql` con datos de prueba y un archivo `crud.sql` que
contenga las operaciones realizadas, separadas por comentarios.

## 13. Criterios de logro

Debes demostrar que puedes crear, consultar, modificar y eliminar datos
sin romper la integridad del modelo.
