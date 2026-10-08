# Clase 3 --- DDL: crear la estructura de la base de datos

> **Propósito:** convertir el modelo diseñado en la clase anterior en
> una estructura real utilizando SQL.

## 1. Objetivos

-   Crear bases de datos.
-   Crear tablas.
-   Elegir tipos de datos.
-   Definir PK y FK.
-   Aplicar restricciones.
-   Modificar y eliminar estructuras.
-   Comprender el orden correcto para crear tablas relacionadas.

## 2. ¿Qué es DDL?

**DDL --- Data Definition Language** define la estructura.

Comandos principales:

``` sql
CREATE
ALTER
DROP
```

No estamos trabajando todavía con los registros; estamos construyendo el
"esqueleto" de la base de datos.

## 3. Crear la base

En MySQL:

``` sql
CREATE DATABASE universidad
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;

USE universidad;
```

Comprueba:

``` sql
SHOW DATABASES;
SELECT DATABASE();
```

## 4. Tipos de datos

  Tipo           Uso
  -------------- ---------------------------
  INT            Identificadores y enteros
  VARCHAR(n)     Texto corto/medio
  TEXT           Texto largo
  DATE           Fecha
  DATETIME       Fecha y hora
  DECIMAL(p,s)   Valores numéricos exactos
  BOOLEAN        Verdadero/falso

Para dinero o valores donde importa la precisión, prefiere `DECIMAL`
frente a `FLOAT`.

## 5. Crear tablas

Comenzamos por las tablas que no dependen de otras:

``` sql
CREATE TABLE programa (
  id_programa INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL UNIQUE
);
```

Después creamos las tablas que contienen FK:

``` sql
CREATE TABLE estudiante (
  id_estudiante INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  correo VARCHAR(120) NOT NULL UNIQUE,
  fecha_nacimiento DATE,
  activo BOOLEAN DEFAULT TRUE,
  id_programa INT,
  FOREIGN KEY (id_programa)
    REFERENCES programa(id_programa)
);
```

## 6. Restricciones

  Restricción   Función
  ------------- ---------------------
  PRIMARY KEY   Identifica la fila
  FOREIGN KEY   Relaciona tablas
  NOT NULL      Campo obligatorio
  UNIQUE        Evita duplicados
  DEFAULT       Valor automático
  CHECK         Regla de validación

Ejemplo:

``` sql
creditos INT NOT NULL CHECK (creditos BETWEEN 1 AND 10)
```

## 7. Caso de estudio completo

``` sql
CREATE TABLE curso (
  id_curso INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  creditos INT NOT NULL CHECK (creditos BETWEEN 1 AND 10)
);

CREATE TABLE inscripcion (
  id_estudiante INT,
  id_curso INT,
  semestre VARCHAR(20) NOT NULL,
  nota DECIMAL(3,1),
  PRIMARY KEY (id_estudiante, id_curso, semestre),
  FOREIGN KEY (id_estudiante)
    REFERENCES estudiante(id_estudiante),
  FOREIGN KEY (id_curso)
    REFERENCES curso(id_curso)
);
```

## 8. ¿Por qué importa el orden?

No puedes crear una FK que referencia una tabla que todavía no existe.

Orden recomendado:

``` text
programa
   ↓
estudiante

curso
   ↓
inscripcion
```

## 9. ALTER TABLE

Agregar una columna:

``` sql
ALTER TABLE estudiante
ADD telefono VARCHAR(20);
```

Agregar una restricción:

``` sql
ALTER TABLE estudiante
ADD CONSTRAINT uq_estudiante_telefono UNIQUE (telefono);
```

## 10. DROP TABLE

``` sql
DROP TABLE inscripcion;
```

**Cuidado:** `DROP` elimina la estructura y sus datos.

Antes de ejecutar una operación destructiva verifica la tabla y el
entorno.

## 11. Comprobaciones

``` sql
SHOW TABLES;
DESCRIBE estudiante;
SHOW CREATE TABLE estudiante;
```

## 12. Actividad

Construye la base `universidad` completa:

1.  Crear la base.
2.  Crear `programa`.
3.  Crear `estudiante`.
4.  Crear `curso`.
5.  Crear `inscripcion`.
6.  Comprobar PK y FK.
7.  Agregar una columna adicional con `ALTER TABLE`.

## 13. Errores comunes

**Error de FK:** normalmente indica que la tabla referenciada no existe,
el tipo de las columnas no coincide o la clave no está correctamente
definida.

**Error de nombre:** SQL puede ser sensible a ciertos detalles según
sistema operativo y configuración.

**Crear todo en una sola sentencia gigante:** para aprender, es mejor
ejecutar por etapas y comprobar cada resultado.

## 14. Entregable

Entrega `schema.sql` con todo el DDL del proyecto de clase y una captura
del esquema funcionando.

## 15. Criterios de logro

La base debe poder crearse desde cero ejecutando el script en orden y
sin errores.
