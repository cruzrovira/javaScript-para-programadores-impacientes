### 10 Comenzando con cuestionarios y ejercicios

#### 10.1 Cuestionarios

**Instalación:**

- Descarga y descomprime `impatient-js-quiz.zip`.

**Ejecución:**

- Abre `impatient-js-quiz/index.html` en un navegador web para ver el índice de todos los cuestionarios.

#### 10.2 Ejercicios

##### 10.2.1 Instalación de los ejercicios

Para instalar los ejercicios:

- Descarga y descomprime `impatient-js-code.zip`.
- Sigue las instrucciones en `README.txt`.

##### 10.2.2 Ejecución de los ejercicios

Los ejercicios se mencionan por ruta en este libro, por ejemplo: `exercises/quizzes-exercises/first_module_test.mjs`.
Dentro de cada archivo:

- La primera línea contiene el comando para ejecutar el ejercicio.
- Las siguientes líneas describen lo que tienes que hacer.

#### 10.3 Pruebas unitarias en JavaScript

Todos los ejercicios en este libro son pruebas que se ejecutan mediante el marco de pruebas Mocha. Esta sección ofrece una breve introducción.

##### 10.3.1 Una prueba típica

El código de prueba se divide en dos partes:

- Parte 1: el código a probar.
- Parte 2: las pruebas para el código.

Por ejemplo, los siguientes dos archivos:

- `id.mjs` (código a probar)
- `id_test.mjs` (pruebas)

##### 10.3.1.1 Parte 1: el código

El código en sí reside en `id.mjs`:

```javascript
export function id(x) {
  return x;
}
```

Lo clave aquí es: todo lo que queremos probar debe ser exportado. De lo contrario, el código de prueba no puede acceder a él.

##### 10.3.1.2 Parte 2: las pruebas

No te preocupes por los detalles exactos de las pruebas: siempre están implementadas para ti. Por lo tanto, solo necesitas leerlas, no escribirlas.

Las pruebas para el código residen en `id_test.mjs`:

```javascript
// npm t demos/quizzes-exercises/id_test.mjs
suite("id_test.mjs");

import * as assert from "assert/strict"; // (A)
import { id } from "./id.mjs"; // (B)

test("My test", () => {
  // (C)
  assert.equal(id("abc"), "abc"); // (D)
});
```

La esencia de este archivo de prueba es la línea D: una afirmación `assert.equal()` especifica que el resultado esperado de `id('abc')` es `'abc'`.

En cuanto a las otras líneas:

- El comentario al principio muestra el comando para ejecutar la prueba.
- Línea A: importamos la biblioteca de afirmaciones de Node.js (en modo estricto).
- Línea B: importamos la función a probar.
- Línea C: definimos una prueba llamando a la función `test()`:
  - Primer parámetro: el nombre de la prueba.
  - Segundo parámetro: el código de la prueba, proporcionado a través de una función flecha.

Para ejecutar la prueba, ejecutamos lo siguiente en una línea de comandos:

```bash
npm t demos/quizzes-exercises/id_test.mjs
```

La `t` es una abreviatura de `test`. Es decir, la versión larga de este comando es:

```bash
npm test demos/quizzes-exercises/id_test.mjs
```

**Ejercicio: Tu primer ejercicio**

El siguiente ejercicio te da una primera idea de cómo son los ejercicios:

`exercises/quizzes-exercises/first_module_test.mjs`

##### 10.3.2 Pruebas asincrónicas en Mocha

Escribir pruebas para código asincrónico requiere trabajo adicional: la prueba recibe sus resultados más tarde y debe señalar a Mocha que aún no ha terminado cuando retorna. Las siguientes subsecciones examinan tres formas de hacerlo.

##### 10.3.2.1 Asincronía mediante callbacks

Si el callback que pasamos a `test()` tiene un parámetro (por ejemplo, `done`), Mocha cambia a asincronía basada en callbacks. Cuando terminamos nuestro trabajo asincrónico, debemos llamar a `done`:

```javascript
test("divideCallback", (done) => {
  divideCallback(8, 4, (error, result) => {
    if (error) {
      done(error);
    } else {
      assert.strictEqual(result, 2);
      done();
    }
  });
});
```

Así es como se ve `divideCallback()`:

```javascript
function divideCallback(x, y, callback) {
  if (y === 0) {
    callback(new Error("Division by zero"));
  } else {
    callback(null, x / y);
  }
}
```

##### 10.3.2.2 Asincronía mediante Promesas

Si una prueba devuelve una Promesa, Mocha cambia a asincronía basada en Promesas. Una prueba se considera exitosa si la Promesa se cumple y fallida si la Promesa es rechazada o si una resolución toma más tiempo que un tiempo de espera.

```javascript
test("dividePromise 1", () => {
  return dividePromise(8, 4).then((result) => {
    assert.strictEqual(result, 2);
  });
});
```

`dividePromise()` se implementa de la siguiente manera:

```javascript
function dividePromise(x, y) {
  return new Promise((resolve, reject) => {
    if (y === 0) {
      reject(new Error("Division by zero"));
    } else {
      resolve(x / y);
    }
  });
}
```

##### 10.3.2.3 Funciones async como "cuerpos" de prueba

Las funciones async siempre devuelven Promesas. Por lo tanto, una función async es una forma conveniente de implementar una prueba asincrónica. El siguiente código es equivalente al ejemplo anterior.

```javascript
test("dividePromise 2", async () => {
  const result = await dividePromise(8, 4);
  assert.strictEqual(result, 2);
  // ¡No es necesario un return explícito!
});
```

No necesitamos devolver explícitamente nada: el undefined devuelto implícitamente se utiliza para cumplir con la Promesa devuelta por esta función async. Y si el código de prueba lanza una excepción, entonces la función async se encarga de rechazar la Promesa devuelta.
