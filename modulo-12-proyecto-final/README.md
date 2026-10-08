# Clase 12 --- Proyecto final: de la idea a una API funcional

> **Propósito:** integrar todo el curso en un proyecto completo de bases
> de datos y backend.

## 1. Objetivo general

El estudiante deberá diseñar una solución de información, convertirla en
una base de datos relacional, poblarla con datos, consultar la
información y exponer operaciones mediante una API REST.

El proyecto representa el flujo completo:

``` text
Problema
   ↓
Modelo E-R
   ↓
Modelo relacional
   ↓
SQL / DDL
   ↓
Datos / CRUD
   ↓
Consultas
   ↓
Node.js
   ↓
Express API
   ↓
Pruebas
   ↓
Documentación
```

## 2. Dominios posibles

Puedes trabajar con:

-   biblioteca;
-   tienda en línea;
-   clínica veterinaria;
-   gimnasio;
-   hotel y reservas;
-   torneo deportivo;
-   alquiler de equipos;
-   sistema académico;
-   restaurante;
-   otro dominio aprobado por el docente.

La prioridad no es hacer una interfaz compleja: el foco está en
**modelar y gestionar correctamente los datos**.

## 3. Requisitos mínimos

El proyecto debe tener:

-   mínimo 4 tablas;
-   al menos una relación N:M;
-   una tabla intermedia;
-   modelo en 3FN;
-   PK y FK;
-   mínimo 3 tipos de restricciones;
-   datos suficientes para probar consultas;
-   CRUD completo sobre mínimo 2 recursos;
-   mínimo 1 JOIN;
-   filtros mediante `req.query`;
-   validación;
-   consultas parametrizadas;
-   `.env.example`;
-   documentación.

## 4. Etapa 1 --- Análisis del problema

Antes de programar, redacta:

### Problema

¿Qué situación estás resolviendo?

### Usuarios

¿Quién utilizará el sistema?

### Información

¿Qué datos necesita guardar?

### Operaciones

¿Qué necesita hacer el sistema?

Ejemplo:

``` text
Registrar estudiante
Consultar estudiante
Actualizar estudiante
Eliminar estudiante
Registrar curso
Matricular estudiante
Consultar notas
```

## 5. Etapa 2 --- Modelo E-R

Entrega:

-   entidades;
-   atributos;
-   PK;
-   relaciones;
-   cardinalidades;
-   participación cuando sea relevante.

Ejemplo:

``` text
PROGRAMA 1 ───── N ESTUDIANTE

ESTUDIANTE N ───── M CURSO
             ↓
        INSCRIPCION
```

## 6. Etapa 3 --- Modelo relacional y normalización

Transforma el diagrama en tablas.

Comprueba:

### 1FN

¿Hay listas o grupos repetidos?

### 2FN

¿Algún atributo depende solamente de una parte de una PK compuesta?

### 3FN

¿Hay atributos no clave que dependen de otros atributos no clave?

Entrega una breve justificación.

## 7. Etapa 4 --- `schema.sql`

El archivo debe poder ejecutarse sobre una base limpia.

Estructura sugerida:

``` text
sql/
├── schema.sql
└── seed.sql
```

`schema.sql` debe contener:

``` sql
CREATE DATABASE ...
CREATE TABLE ...
ALTER TABLE ...
```

Crea las tablas en un orden que respete las FK.

## 8. Etapa 5 --- Datos de prueba

`seed.sql` debe generar datos suficientes para probar:

-   coincidencias;
-   no coincidencias;
-   filtros;
-   agregaciones;
-   JOIN;
-   casos vacíos.

No diseñes todos los datos para que "todo funcione perfecto". Una buena
base de pruebas también contiene casos límite.

## 9. Etapa 6 --- Consultas

Entrega mínimo 8 consultas.

Deben incluir:

1.  selección;
2.  proyección;
3.  JOIN entre 3 tablas;
4.  agregación con `GROUP BY`;
5.  `HAVING`;
6.  subconsulta;
7.  operación de conjuntos;
8.  `LEFT JOIN` para detectar ausencia de relaciones;
9.  una vista.

Cada consulta debe responder una pregunta real.

Ejemplo:

> ¿Qué estudiantes tienen una nota superior al promedio?

``` sql
SELECT *
FROM inscripcion
WHERE nota > (
  SELECT AVG(nota)
  FROM inscripcion
);
```

## 10. Etapa 7 --- API

La API debe tener al menos dos recursos con CRUD.

Ejemplo:

``` text
GET    /estudiantes
GET    /estudiantes/:id
POST   /estudiantes
PUT    /estudiantes/:id
DELETE /estudiantes/:id

GET    /cursos
GET    /cursos/:id
POST   /cursos
PUT    /cursos/:id
DELETE /cursos/:id
```

Debe incluir:

-   Express;
-   conexión mediante pool;
-   consultas parametrizadas;
-   validación;
-   manejo de errores;
-   códigos HTTP apropiados;
-   `.env`.

## 11. Etapa 8 --- Pruebas

La colección debe demostrar:

### Casos exitosos

-   listar;
-   obtener;
-   crear;
-   actualizar;
-   eliminar.

### Casos de error

-   recurso inexistente;
-   datos inválidos;
-   registro duplicado;
-   parámetros incorrectos.

## 12. Estructura final

``` text
proyecto-final/
├── README.md
├── docs/
│   ├── modelo-er.png
│   └── consultas.md
├── sql/
│   ├── schema.sql
│   └── seed.sql
└── api/
    ├── package.json
    ├── package-lock.json
    ├── .env.example
    ├── .gitignore
    ├── index.js
    └── src/
        ├── db.js
        ├── routes/
        ├── controllers/
        └── middlewares/
```

## 13. README del proyecto

El README del proyecto debe explicar:

1.  qué problema resuelve;
2.  qué tecnologías utiliza;
3.  requisitos;
4.  cómo crear la base;
5.  cómo cargar datos;
6.  cómo configurar `.env`;
7.  cómo instalar dependencias;
8.  cómo iniciar la API;
9.  endpoints disponibles;
10. ejemplos de peticiones.

## 14. Presentación

Duración sugerida: 10 minutos.

### Estructura

**1 min --- Problema**\
¿Qué necesidad resuelve?

**2 min --- Modelo**\
Entidades, relaciones y normalización.

**2 min --- SQL**\
Mostrar esquema y consultas relevantes.

**3 min --- API**\
Demostrar CRUD y un JOIN.

**1 min --- Seguridad**\
Validación, parámetros y permisos.

**1 min --- Conclusiones**\
Qué aprendiste y qué mejorarías.

## 15. Rúbrica

  Criterio                        Peso
  ------------------------- ----------
  Análisis y modelo E-R            20%
  Normalización y SQL              20%
  Consultas                        20%
  API CRUD y arquitectura          25%
  Pruebas y documentación          15%
  **Total**                   **100%**

## 16. Cronograma sugerido

  Semana   Trabajo
  -------- ----------------------------------
  1        Problema + modelo E-R
  2        Modelo relacional + `schema.sql`
  3        `seed.sql` + consultas
  4        API Node.js
  5        CRUD + validación + pruebas
  6        Documentación + presentación

## 17. Checklist antes de entregar

-   [ ] El modelo E-R está completo.
-   [ ] Todas las PK están definidas.
-   [ ] Todas las relaciones tienen FK correctas.
-   [ ] El modelo está normalizado hasta 3FN.
-   [ ] `schema.sql` funciona desde una base limpia.
-   [ ] `seed.sql` funciona.
-   [ ] Hay suficientes datos para probar.
-   [ ] Existen mínimo dos CRUD completos.
-   [ ] Hay JOIN.
-   [ ] Hay filtros.
-   [ ] Hay validación.
-   [ ] Las consultas usan parámetros.
-   [ ] `.env` no está en el repositorio.
-   [ ] `.env.example` sí está.
-   [ ] La API fue probada.
-   [ ] El README explica cómo ejecutar el proyecto.
-   [ ] La presentación está preparada.

## 18. Criterio final

No se evalúa solamente que "el código funcione".

Un buen proyecto debe demostrar:

**buen análisis + buen modelo + integridad + consultas correctas + API
organizada + seguridad básica + documentación.**
