# Clase 10 --- Conectar Node.js con MySQL

> **Propósito:** crear una conexión real entre el backend y la base de
> datos y ejecutar operaciones SQL desde JavaScript.

## 1. Objetivos

-   Instalar el driver `mysql2`.
-   Configurar credenciales con `.env`.
-   Crear un pool.
-   Ejecutar SELECT, INSERT, UPDATE y DELETE.
-   Utilizar consultas parametrizadas.
-   Manejar errores.
-   Comprender transacciones desde Node.js.

## 2. Instalar mysql2

Dentro del proyecto:

``` bash
npm install mysql2 dotenv
```

## 3. Configuración

`.env`:

``` env
DB_HOST=localhost
DB_PORT=3306
DB_USER=app_user
DB_PASSWORD=tu_clave
DB_NAME=universidad
```

## 4. Crear la conexión

`src/db.js`:

``` js
import mysql from 'mysql2/promise';
import 'dotenv/config';

export const pool = mysql.createPool({
  host: process.env.DB_HOST,
  port: Number(process.env.DB_PORT || 3306),
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME,
  waitForConnections: true,
  connectionLimit: 10
});
```

## 5. ¿Por qué un pool?

Abrir y cerrar una conexión por cada consulta puede ser costoso.

Un pool mantiene un conjunto de conexiones reutilizables:

``` text
Aplicación
    ↓
   Pool
 ┌──┼──┐
 ↓  ↓  ↓
BD BD BD
```

## 6. Primera consulta

`index.js`:

``` js
import { pool } from './src/db.js';

async function main() {
  try {
    const [rows] = await pool.query(
      'SELECT VERSION() AS version'
    );

    console.log(rows);
  } catch (error) {
    console.error('Error de conexión:', error.message);
  } finally {
    await pool.end();
  }
}

main();
```

## 7. SELECT

``` js
const [rows] = await pool.execute(
  'SELECT id_estudiante, nombre, correo FROM estudiante'
);

console.table(rows);
```

## 8. INSERT parametrizado

``` js
const [result] = await pool.execute(
  `INSERT INTO estudiante
   (nombre, correo, id_programa)
   VALUES (?, ?, ?)`,
  ['Laura Díaz', 'laura@uni.edu', 1]
);

console.log(result.insertId);
```

Los `?` separan la instrucción de los valores.

## 9. UPDATE

``` js
await pool.execute(
  `UPDATE estudiante
   SET nombre = ?
   WHERE id_estudiante = ?`,
  ['Laura Díaz Gómez', 1]
);
```

## 10. DELETE

``` js
await pool.execute(
  'DELETE FROM estudiante WHERE id_estudiante = ?',
  [1]
);
```

## 11. Manejo de errores

Nunca ocultes el error durante el desarrollo:

``` js
try {
  // operación
} catch (error) {
  console.error(error);
}
```

En una API, posteriormente transformaremos estos errores en respuestas
HTTP apropiadas.

## 12. Transacciones

Ejemplo conceptual:

``` js
const connection = await pool.getConnection();

try {
  await connection.beginTransaction();

  await connection.execute(
    'UPDATE estudiante SET id_programa = ? WHERE id_estudiante = ?',
    [2, 1]
  );

  await connection.execute(
    'INSERT INTO inscripcion (id_estudiante, id_curso, semestre, nota) VALUES (?, ?, ?, ?)',
    [1, 2, '2026-2', 4.5]
  );

  await connection.commit();
} catch (error) {
  await connection.rollback();
  throw error;
} finally {
  connection.release();
}
```

La transacción permite que ambas operaciones se confirmen juntas o se
reviertan.

## 13. Actividad

Crea funciones:

``` text
listarEstudiantes()
crearEstudiante()
actualizarEstudiante()
eliminarEstudiante()
```

Todas deben utilizar el pool y consultas parametrizadas.

## 14. Errores comunes

-   Credenciales incorrectas.
-   MySQL apagado.
-   Base de datos inexistente.
-   No instalar `mysql2`.
-   No utilizar parámetros.
-   Cerrar el pool demasiado pronto.
-   No liberar conexiones obtenidas manualmente.

## 15. Entregable

Entrega el proyecto con conexión funcional y un script que demuestre
SELECT, INSERT, UPDATE y DELETE.
