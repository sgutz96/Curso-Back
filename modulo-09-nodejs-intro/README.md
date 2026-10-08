# Clase 9 --- Introducción a Node.js para trabajar con bases de datos

> **Propósito:** preparar el entorno JavaScript del lado del servidor
> que utilizaremos para conectar la base de datos con una API.

## 1. Objetivos

-   Comprender qué es Node.js.
-   Instalar Node.js y npm.
-   Crear un proyecto.
-   Instalar dependencias.
-   Comprender módulos.
-   Entender asincronía.
-   Proteger configuración mediante variables de entorno.

## 2. ¿Qué es Node.js?

Node.js permite ejecutar JavaScript fuera del navegador.

En este curso lo utilizaremos para construir el backend:

``` text
Cliente
   ↓
API Node.js / Express
   ↓
Base de datos MySQL
```

## 3. Comprobar instalación

``` bash
node --version
npm --version
```

Si ambos comandos muestran una versión, el entorno está listo.

## 4. Crear el proyecto

``` bash
mkdir api-universidad
cd api-universidad
npm init -y
```

Esto crea `package.json`.

## 5. Instalar dependencias

Para las siguientes clases:

``` bash
npm install mysql2 dotenv
```

Más adelante:

``` bash
npm install express
```

## 6. Módulos ES

En `package.json` puedes utilizar:

``` json
{
  "type": "module"
}
```

Así puedes escribir:

``` js
import mysql from 'mysql2/promise';
```

en lugar de la sintaxis CommonJS.

## 7. Primer programa

Crea `index.js`:

``` js
console.log('Backend funcionando');
```

Ejecuta:

``` bash
node index.js
```

## 8. Asincronía

Las operaciones de base de datos tardan un tiempo. Node.js utiliza un
modelo orientado a operaciones asíncronas.

Ejemplo:

``` js
async function cargarDatos() {
  const resultado = await obtenerDatos();
  console.log(resultado);
}
```

`async/await` permite escribir código asíncrono de forma legible.

## 9. Variables de entorno

Nunca es buena práctica escribir credenciales directamente en el código.

Instala:

``` bash
npm install dotenv
```

Archivo `.env`:

``` env
DB_HOST=localhost
DB_PORT=3306
DB_USER=app_user
DB_PASSWORD=tu_clave
DB_NAME=universidad
```

En JavaScript:

``` js
import 'dotenv/config';

console.log(process.env.DB_HOST);
```

Agrega `.env` a `.gitignore`:

``` gitignore
node_modules/
.env
```

## 10. Actividad

Crea un proyecto llamado `api-universidad` que:

1.  tenga `package.json`;
2.  use módulos ES;
3.  tenga `dotenv`;
4.  tenga un `.env`;
5.  tenga `.gitignore`;
6.  imprima un mensaje al ejecutar `node index.js`.

## 11. Errores comunes

-   Ejecutar `npm` fuera de la carpeta del proyecto.
-   Confundir `node` con `npm`.
-   Subir `.env` a GitHub.
-   Olvidar instalar una dependencia.
-   Mezclar `require()` e `import` sin entender la configuración del
    proyecto.

## 12. Entregable

Entrega el proyecto sin `node_modules`, incluyendo `package.json`,
`package-lock.json`, `.gitignore`, `.env.example` e `index.js`.

`.env.example`:

``` env
DB_HOST=localhost
DB_PORT=3306
DB_USER=
DB_PASSWORD=
DB_NAME=universidad
```
