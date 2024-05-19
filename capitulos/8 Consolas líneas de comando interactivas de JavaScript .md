### 8 Consolas: líneas de comando interactivas de JavaScript

#### 8.1 Probando código JavaScript

##### 8.1.1 Consolas del navegador

##### 8.1.2 El REPL de Node.js

##### 8.1.3 Otras opciones

#### 8.2 La API console.\*: imprimiendo datos y más

##### 8.2.1 Imprimiendo valores: console.log() (stdout)

##### 8.2.2 Imprimiendo información de error: console.error() (stderr)

##### 8.2.3 Imprimiendo objetos anidados con JSON.stringify()

### 8.1 Probando código JavaScript

Tienes muchas opciones para ejecutar rápidamente piezas de código JavaScript. Las siguientes subsecciones describen algunas de ellas.

#### 8.1.1 Consolas del navegador

Los navegadores web tienen consolas: líneas de comando interactivas donde puedes imprimir texto mediante console.log() y ejecutar piezas de código. La manera de abrir la consola varía según el navegador. La Figura 3 muestra la consola de Google Chrome.

Para saber cómo abrir la consola en tu navegador web, puedes buscar en la web "console «nombre-de-tu-navegador»". Estas son páginas para algunos navegadores comúnmente usados:

- Apple Safari
- Google Chrome
- Microsoft Edge
- Mozilla Firefox

_Figura 3: La consola del navegador web "Google Chrome" está abierta (en la mitad inferior de la ventana) mientras se visita una página web._

#### 8.1.2 El REPL de Node.js

REPL significa read-eval-print loop y básicamente es una línea de comando. Para usarlo, primero debes iniciar Node.js desde una línea de comando del sistema operativo, mediante el comando `node`. Luego, una interacción con él se ve como en la Figura 4: el texto después de `>` es la entrada del usuario; todo lo demás es salida de Node.js.

_Figura 4: Iniciando y usando el REPL de Node.js (línea de comando interactiva)._

Lectura: Interacciones REPL

Ocasionalmente demuestro JavaScript mediante interacciones REPL. Entonces, también uso símbolos de mayor que (`>`) para marcar la entrada, por ejemplo:

> 3 + 5
> 8

#### 8.1.3 Otras opciones

Otras opciones incluyen:

- Hay muchas aplicaciones web que te permiten experimentar con JavaScript en navegadores web, por ejemplo, el REPL de Babel.
- También hay aplicaciones nativas y complementos de IDE para ejecutar JavaScript.

_Consolas frecuentemente funcionan en modo no estricto_

En JavaScript moderno, la mayoría del código (por ejemplo, módulos) se ejecuta en modo estricto. Sin embargo, las consolas a menudo funcionan en modo no estricto. Por lo tanto, puedes obtener resultados ligeramente diferentes al usar una consola para ejecutar código de este libro.

### 8.2 La API console.\*: imprimiendo datos y más

En los navegadores, la consola es algo que puedes abrir y normalmente está oculta. Para Node.js, la consola es la terminal en la que Node.js se está ejecutando.

La API completa de console.\* está documentada en MDN web docs y en el sitio web de Node.js. No es parte del estándar del lenguaje JavaScript, pero mucha funcionalidad es compatible tanto con navegadores como con Node.js.

En este capítulo, solo veremos los siguientes dos métodos para imprimir datos ("imprimir" significa mostrar en la consola):

- `console.log()`
- `console.error()`

#### 8.2.1 Imprimiendo valores: console.log() (stdout)

Hay dos variantes de esta operación:

1. `console.log(...values: any[]): void`
2. `console.log(pattern: string, ...values: any[]): void`

##### 8.2.1.1 Imprimiendo múltiples valores

La primera variante imprime (representaciones de texto de) valores en la consola:

```javascript
console.log("abc", 123, true);
// Salida:
// abc 123 true
```

Al final, `console.log()` siempre imprime una nueva línea. Por lo tanto, si lo llamas sin argumentos, solo imprime una nueva línea.

##### 8.2.1.2 Imprimiendo una cadena con sustituciones

La segunda variante realiza una sustitución de cadena:

```javascript
console.log("Prueba: %s %j", 123, "abc");
// Salida:
// Prueba: 123 "abc"
```

Estos son algunos de los directivos que puedes usar para sustituciones:

- `%s` convierte el valor correspondiente en una cadena e inserta.

```javascript
console.log("%s %s", "abc", 123);
// Salida:
// abc 123
```

- `%o` inserta una representación de cadena de un objeto.

```javascript
console.log("%o", { foo: 123, bar: "abc" });
// Salida:
// { foo: 123, bar: 'abc' }
```

- `%j` convierte un valor en una cadena JSON e inserta.

```javascript
console.log("%j", { foo: 123, bar: "abc" });
// Salida:
// {"foo":123,"bar":"abc"}
```

- `%%` inserta un solo `%`.

```javascript
console.log("%s%%", 99);
// Salida:
// 99%
```

#### 8.2.2 Imprimiendo información de error: console.error() (stderr)

`console.error()` funciona igual que `console.log()`, pero lo que registra se considera información de error. Para Node.js, eso significa que la salida va a stderr en lugar de stdout en Unix.

#### 8.2.3 Imprimiendo objetos anidados con JSON.stringify()

`JSON.stringify()` es ocasionalmente útil para imprimir objetos anidados:

```javascript
console.log(JSON.stringify({first: 'Jane', last: 'Doe'}, null, 2));
// Salida:
{
  "first": "Jane",
  "last": "Doe"
}
```

---

Para más detalles, puedes leer la página completa [aquí](https://exploringjs.com/impatient-js/ch_console.html).
