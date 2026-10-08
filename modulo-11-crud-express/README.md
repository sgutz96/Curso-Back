# Clase 11 --- API REST: CRUD completo con Node.js y Express

> **Propósito:** convertir las operaciones de base de datos en una API
> HTTP que pueda ser consumida por una aplicación web, móvil u otro
> sistema.

## 1. Objetivos

-   Crear un servidor Express.
-   Organizar rutas y controladores.
-   Construir endpoints REST.
-   Conectar endpoints con MySQL.
-   Implementar CRUD.
-   Utilizar `req.params` y `req.query`.
-   Crear filtros y paginación.
-   Validar datos.
-   Probar la API con Postman o Thunder Client.

## 2. Arquitectura

Trabajaremos con una separación sencilla:

``` text
Cliente
  ↓ HTTP
Routes
  ↓
Controllers
  ↓
Database / SQL
  ↓
MySQL
```

Una estructura sugerida:

``` text
api-universidad/
├── src/
│   ├── db.js
│   ├── routes/
│   │   └── estudiantes.routes.js
│   ├── controllers/
│   │   └── estudiantes.controller.js
│   └── middlewares/
│       └── validar.js
├── .env
├── .env.example
├── .gitignore
├── index.js
└── package.json
```

## 3. Instalar Express

``` bash
npm install express
```

## 4. Crear servidor

`index.js`:

``` js
import express from 'express';
import estudiantesRoutes from './src/routes/estudiantes.routes.js';

const app = express();
const PORT = process.env.PORT || 3000;

app.use(express.json());

app.get('/', (req, res) => {
  res.json({ mensaje: 'API Universidad funcionando' });
});

app.use('/estudiantes', estudiantesRoutes);

app.listen(PORT, () => {
  console.log(`Servidor en http://localhost:${PORT}`);
});
```

## 5. REST y endpoints

  Operación    Método   Ruta                 Respuesta
  ------------ -------- -------------------- -----------
  Listar       GET      `/estudiantes`       200
  Obtener      GET      `/estudiantes/:id`   200
  Crear        POST     `/estudiantes`       201
  Actualizar   PUT      `/estudiantes/:id`   200
  Eliminar     DELETE   `/estudiantes/:id`   204

Códigos importantes:

-   `200` --- operación correcta;
-   `201` --- recurso creado;
-   `204` --- operación correcta sin contenido;
-   `400` --- datos inválidos;
-   `404` --- recurso inexistente;
-   `409` --- conflicto;
-   `500` --- error del servidor.

## 6. Rutas

`src/routes/estudiantes.routes.js`:

``` js
import { Router } from 'express';
import * as ctrl from '../controllers/estudiantes.controller.js';

const router = Router();

router.get('/', ctrl.listar);
router.get('/:id', ctrl.obtener);
router.post('/', ctrl.crear);
router.put('/:id', ctrl.actualizar);
router.delete('/:id', ctrl.eliminar);

export default router;
```

## 7. Controlador

``` js
import { pool } from '../db.js';

export async function obtener(req, res) {
  try {
    const [filas] = await pool.execute(
      'SELECT * FROM estudiante WHERE id_estudiante = ?',
      [req.params.id]
    );

    if (filas.length === 0) {
      return res.status(404).json({
        error: 'Estudiante no encontrado'
      });
    }

    res.json(filas[0]);
  } catch (error) {
    console.error(error);
    res.status(500).json({
      error: 'Error del servidor'
    });
  }
}
```

## 8. Crear

``` js
export async function crear(req, res) {
  const { nombre, correo, id_programa } = req.body;

  try {
    const [resultado] = await pool.execute(
      `INSERT INTO estudiante
       (nombre, correo, id_programa)
       VALUES (?, ?, ?)`,
      [nombre, correo, id_programa]
    );

    res.status(201).json({
      id_estudiante: resultado.insertId,
      nombre,
      correo,
      id_programa
    });
  } catch (error) {
    if (error.code === 'ER_DUP_ENTRY') {
      return res.status(409).json({
        error: 'El correo ya existe'
      });
    }

    console.error(error);
    res.status(500).json({
      error: 'Error del servidor'
    });
  }
}
```

## 9. Actualizar y eliminar

``` js
export async function actualizar(req, res) {
  const { nombre, correo, id_programa } = req.body;

  try {
    const [resultado] = await pool.execute(
      `UPDATE estudiante
       SET nombre = ?, correo = ?, id_programa = ?
       WHERE id_estudiante = ?`,
      [nombre, correo, id_programa, req.params.id]
    );

    if (resultado.affectedRows === 0) {
      return res.status(404).json({
        error: 'Estudiante no encontrado'
      });
    }

    res.json({
      id_estudiante: Number(req.params.id),
      nombre,
      correo,
      id_programa
    });
  } catch (error) {
    console.error(error);
    res.status(500).json({
      error: 'Error del servidor'
    });
  }
}
```

``` js
export async function eliminar(req, res) {
  try {
    const [resultado] = await pool.execute(
      'DELETE FROM estudiante WHERE id_estudiante = ?',
      [req.params.id]
    );

    if (resultado.affectedRows === 0) {
      return res.status(404).json({
        error: 'Estudiante no encontrado'
      });
    }

    res.status(204).send();
  } catch (error) {
    console.error(error);
    res.status(500).json({
      error: 'Error del servidor'
    });
  }
}
```

## 10. req.params vs req.query

### Parámetro de ruta

``` text
GET /estudiantes/5
```

JavaScript:

``` js
req.params.id
```

### Query string

``` text
GET /estudiantes?programa=1&buscar=ana
```

JavaScript:

``` js
req.query.programa
req.query.buscar
```

## 11. Filtros y paginación

``` js
export async function listar(req, res) {
  const {
    programa,
    buscar,
    pagina = 1,
    limite = 10
  } = req.query;

  const condiciones = [];
  const valores = [];

  if (programa) {
    condiciones.push('id_programa = ?');
    valores.push(programa);
  }

  if (buscar) {
    condiciones.push('nombre LIKE ?');
    valores.push(`%${buscar}%`);
  }

  const where = condiciones.length
    ? `WHERE ${condiciones.join(' AND ')}`
    : '';

  const limiteNum = Math.min(Number(limite) || 10, 100);
  const paginaNum = Math.max(Number(pagina) || 1, 1);
  const offset = (paginaNum - 1) * limiteNum;

  const [filas] = await pool.query(
    `SELECT *
     FROM estudiante
     ${where}
     ORDER BY nombre
     LIMIT ? OFFSET ?`,
    [...valores, limiteNum, offset]
  );

  res.json(filas);
}
```

## 12. JOIN desde la API

``` js
export async function inscripciones(req, res) {
  const [filas] = await pool.execute(
    `SELECT
       c.nombre AS curso,
       i.semestre,
       i.nota
     FROM inscripcion i
     JOIN curso c
       ON c.id_curso = i.id_curso
     WHERE i.id_estudiante = ?`,
    [req.params.id]
  );

  res.json(filas);
}
```

Ruta:

``` js
router.get('/:id/inscripciones', ctrl.inscripciones);
```

## 13. Validación

La API no debe confiar en los datos del cliente.

``` js
export function validarEstudiante(req, res, next) {
  const { nombre, correo } = req.body;
  const errores = [];

  if (typeof nombre !== 'string' || nombre.trim().length < 2) {
    errores.push('Nombre inválido');
  }

  if (
    typeof correo !== 'string' ||
    !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(correo)
  ) {
    errores.push('Correo inválido');
  }

  if (errores.length) {
    return res.status(400).json({ errores });
  }

  next();
}
```

En proyectos más grandes puedes utilizar `zod` o `joi`.

## 14. Pruebas

Con Postman o Thunder Client:

### GET

``` text
GET http://localhost:3000/estudiantes
```

### GET por ID

``` text
GET http://localhost:3000/estudiantes/1
```

### POST

``` json
{
  "nombre": "Sara León",
  "correo": "sara@uni.edu",
  "id_programa": 1
}
```

### PUT

``` json
{
  "nombre": "Sara León Gómez",
  "correo": "sara.gomez@uni.edu",
  "id_programa": 2
}
```

### DELETE

``` text
DELETE http://localhost:3000/estudiantes/1
```

## 15. Actividad integradora

Construye el CRUD de:

1.  `estudiante`;
2.  `curso`.

Después agrega:

-   filtros;
-   paginación;
-   un endpoint con JOIN;
-   validación;
-   colección de pruebas.

## 16. Errores comunes

-   No usar `express.json()`.
-   No parametrizar SQL.
-   Mezclar lógica de rutas y SQL sin estructura.
-   Devolver `200` para todo.
-   No controlar `404`.
-   Exponer mensajes internos de errores de base de datos al cliente.
-   No validar `req.body`.

## 17. Entregable

Entrega el proyecto completo, `.env.example` y una colección de
Postman/Thunder Client con pruebas exitosas y fallidas.
