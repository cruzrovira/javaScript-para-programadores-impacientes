# 7 Sintaxis

## 7.1 Una visión general de la sintaxis de JavaScript

### 7.1.1 Constructos básicos

### 7.1.2 Módulos

### 7.1.3 Clases

### 7.1.4 Manejo de excepciones

### 7.1.5 Nombres de variables y propiedades legales

### 7.1.6 Estilos de mayúsculas y minúsculas

### 7.1.7 Capitalización de nombres

### 7.1.8 Más convenciones de nombres

### 7.1.9 ¿Dónde poner los puntos y comas?

## 7.2 (Avanzado)

## 7.3 Identificadores

### 7.3.1 Identificadores válidos (nombres de variables, etc.)

### 7.3.2 Palabras reservadas

## 7.4 Declaración vs. expresión

### 7.4.1 Declaraciones

### 7.4.2 Expresiones

### 7.4.3 ¿Qué está permitido dónde?

## 7.5 Sintaxis ambigua

### 7.5.1 Misma sintaxis: declaración de función y expresión de función

### 7.5.2 Misma sintaxis: literal de objeto y bloque

### 7.5.3 Desambiguación

## 7.6 Puntos y comas

### 7.6.1 Regla general para los puntos y comas

### 7.6.2 Puntos y comas: declaraciones de control

## 7.7 Inserción automática de puntos y comas (ASI)

### 7.7.1 ASI activada inesperadamente

### 7.7.2 ASI inesperadamente no activada

## 7.8 Puntos y comas: mejores prácticas

## 7.9 Modo estricto vs. modo flexible

### 7.9.1 Activación del modo estricto

### 7.9.2 Mejoras en el modo estricto

### 7.1 Una visión general de la sintaxis de JavaScript

Esta es una primera mirada a la sintaxis de JavaScript. No te preocupes si algunas cosas no tienen sentido aún. Todo se explicará con más detalle más adelante en este libro.

Este resumen tampoco es exhaustivo. Se centra en lo esencial.

#### 7.1.1 Constructos básicos

##### 7.1.1.1 Comentarios

```javascript
// Comentario de una sola línea

/*
Comentario con
múltiples líneas
*/
```

##### 7.1.1.2 Valores primitivos (atómicos)

Booleans:

```javascript
true;
false;
```

Números:

```javascript
1.141 - 123;
```

El tipo de número básico se usa tanto para números de punto flotante (dobles) como para enteros.

Bigints:

```javascript
17n - 49n;
```

El tipo de número básico solo puede representar correctamente enteros dentro de un rango de 53 bits más signo. Los bigints pueden crecer arbitrariamente en tamaño.

Cadenas:

```javascript
"abc";
"abc"`Cadena con valores interpolados: ${256} y ${true}`;
```

JavaScript no tiene un tipo extra para caracteres. Usa cadenas para representarlos.

##### 7.1.1.3 Aserciones

Una aserción describe cómo se espera que sea el resultado de un cálculo y lanza una excepción si esas expectativas no son correctas. Por ejemplo, la siguiente aserción indica que el resultado del cálculo 7 más 1 debe ser 8:

```javascript
assert.equal(7 + 1, 8);
```

assert.equal() es una llamada de método (el objeto es assert, el método es .equal()) con dos argumentos: el resultado real y el resultado esperado. Es parte de una API de aserciones de Node.js que se explica más adelante en este libro.

También existe assert.deepEqual() que compara objetos en profundidad.

##### 7.1.1.4 Registro en la consola

Registro en la consola de un navegador o Node.js:

```javascript
// Imprimir un valor en la salida estándar (otra llamada de método)
console.log("¡Hola!");

// Imprimir información de error en la salida de errores
console.error("¡Algo salió mal!");
```

##### 7.1.1.5 Operadores

```javascript
// Operadores para booleans
assert.equal(true && false, false); // Y
assert.equal(true || false, true); // O

// Operadores para números
assert.equal(3 + 4, 7);
assert.equal(5 - 1, 4);
assert.equal(3 * 4, 12);
assert.equal(10 / 4, 2.5);

// Operadores para bigints
assert.equal(3n + 4n, 7n);
assert.equal(5n - 1n, 4n);
assert.equal(3n * 4n, 12n);
assert.equal(10n / 4n, 2n);

// Operadores para cadenas
assert.equal("a" + "b", "ab");
assert.equal("Veo " + 3 + " monos", "Veo 3 monos");

// Operadores de comparación
assert.equal(3 < 4, true);
assert.equal(3 <= 4, true);
assert.equal("abc" === "abc", true);
assert.equal("abc" !== "def", true);
```

JavaScript también tiene un operador de comparación ==. Recomiendo evitarlo – por qué se explica en §13.4.3 "Recomendación: usar siempre igualdad estricta".

##### 7.1.1.6 Declaración de variables

const crea enlaces de variables inmutables: cada variable debe ser inicializada inmediatamente y no podemos asignar un valor diferente más tarde. Sin embargo, el valor en sí puede ser mutable y podemos cambiar su contenido. En otras palabras: const no hace que los valores sean inmutables.

```javascript
// Declarando e inicializando x (enlace inmutable):
const x = 8;

// Causaría un TypeError:
// x = 9;
```

let crea enlaces de variables mutables:

```javascript
// Declarando y (enlace mutable):
let y;

// Podemos asignar un valor diferente a y:
y = 3 * 5;

// Declarando e inicializando z:
let z = 3 * 5;
```

##### 7.1.1.7 Declaraciones de funciones ordinarias

```javascript
// add1() tiene los parámetros a y b
function add1(a, b) {
  return a + b;
}
// Llamando a la función add1()
assert.equal(add1(5, 2), 7);
```

##### 7.1.1.8 Expresiones de funciones flecha

Las expresiones de funciones flecha se usan especialmente como argumentos de llamadas de funciones y métodos:

```javascript
const add2 = (a, b) => {
  return a + b;
};
// Llamando a la función add2()
assert.equal(add2(5, 2), 7);

// Equivalente a add2:
const add3 = (a, b) => a + b;
```

El código anterior contiene las siguientes dos funciones flecha (los términos expresión y declaración se explican más adelante en este capítulo):

```javascript
// Una función flecha cuyo cuerpo es un bloque de código
(a, b) => {
  return a + b;
};

// Una función flecha cuyo cuerpo es una expresión
(a, b) => a + b;
```

##### 7.1.1.9 Objetos simples

```javascript
// Creando un objeto simple mediante un literal de objeto
const obj = {
  first: "Jane", // propiedad
  last: "Doe", // propiedad
  getFullName() {
    // propiedad (método)
    return this.first + " " + this.last;
  },
};

// Obteniendo un valor de propiedad
assert.equal(obj.first, "Jane");
// Estableciendo un valor de propiedad
obj.first = "Janey";

// Llamando al método
assert.equal(obj.getFullName(), "Janey Doe");
```

##### 7.1.1.10 Arrays

```javascript
// Creando un Array mediante un literal de Array
const arr = ["a", "b", "c"];
assert.equal(arr.length, 3);

// Obteniendo un elemento del Array
assert.equal(arr[1], "b");
// Estableciendo un elemento del Array
arr[1] = "β";

// Añadiendo un elemento a un Array:
arr.push("d");

assert.deepEqual(arr, ["a", "β", "c", "d"]);
```

##### 7.1.1.11 Declaraciones de flujo de control

Declaración condicional:

```javascript
if (x < 0) {
  x = -x;
}
```

Bucle for-of:

```javascript
const arr = ["a", "b"];
for (const element of arr) {
  console.log(element);
}
// Salida:
// 'a'
// 'b'
```

### 7.1.2 Módulos

Cada módulo es un solo archivo. Consideremos, por ejemplo, los siguientes dos archivos con módulos en ellos:

- `file-tools.mjs`
- `main.mjs`

El módulo en `file-tools.mjs` exporta su función `isTextFilePath()`:

```javascript
export function isTextFilePath(filePath) {
  return filePath.endsWith(".txt");
}
```

El módulo en `main.mjs` importa todo el módulo `path` y la función `isTextFilePath()`:

```javascript
// Importar todo el módulo como objeto de espacio de nombres `path`
import * as path from "path";
// Importar una sola exportación del módulo `file-tools.mjs`
import { isTextFilePath } from "./file-tools.mjs";
```

### 7.1.3 Clases

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  describe() {
    return `Person named ${this.name}`;
  }
  static logNames(persons) {
    for (const person of persons) {
      console.log(person.name);
    }
  }
}

class Employee extends Person {
  constructor(name, title) {
    super(name);
    this.title = title;
  }
  describe() {
    return super.describe() + ` (${this.title})`;
  }
}

const jane = new Employee("Jane", "CTO");
assert.equal(jane.describe(), "Person named Jane (CTO)");
```

### 7.1.4 Manejo de excepciones

```javascript
function throwsException() {
  throw new Error("Problem!");
}

function catchesException() {
  try {
    throwsException();
  } catch (err) {
    assert.ok(err instanceof Error);
    assert.equal(err.message, "Problem!");
  }
}
```

Nota:

- `try-finally` y `try-catch-finally` también son compatibles.
- Podemos lanzar cualquier valor, pero características como trazas de pila solo son compatibles con `Error` y sus subclases.

### 7.1.5 Nombres legales de variables y propiedades

La categoría gramatical de los nombres de variables y propiedades se llama identificador.

A los identificadores se les permite tener los siguientes caracteres:

- Letras Unicode: A–Z, a–z (etc.)
- $, \_
- Dígitos Unicode: 0–9 (etc.)

Los nombres de variables no pueden empezar con un dígito.

Algunas palabras tienen un significado especial en JavaScript y se llaman palabras reservadas. Ejemplos incluyen: `if`, `true`, `const`.

Las palabras reservadas no pueden ser usadas como nombres de variables:

```javascript
const if = 123; // SyntaxError: Unexpected token if
```

Pero se permiten como nombres de propiedades:

```javascript
const obj = { if: 123 };
console.log(obj.if); // 123
```

### 7.1.6 Estilos de mayúsculas y minúsculas

Estilos comunes de mayúsculas y minúsculas para concatenar palabras son:

- Camel case: `threeConcatenatedWords`
- Underscore case (también llamado snake case): `three_concatenated_words`
- Dash case (también llamado kebab case): `three-concatenated-words`

### 7.1.7 Capitalización de nombres

En general, JavaScript usa camel case, excepto para constantes.

- Minúsculas:

  - Funciones, variables: `myFunction`
  - Métodos: `obj.myMethod`

- CSS:

  - Entidad CSS: `special-class`
  - Variable JavaScript correspondiente: `specialClass`

- Mayúsculas:

  - Clases: `MyClass`
  - Constantes: `MY_CONSTANT`

Las constantes también se escriben a menudo en camel case: `myConstant`

### 7.1.8 Más convenciones de nombres

Las siguientes convenciones de nombres son populares en JavaScript:

- Si el nombre de un parámetro empieza con un guion bajo (o es un guion bajo) significa que este parámetro no se usa – por ejemplo:

```javascript
arr.map((_x, i) => i);
```

- Si el nombre de una propiedad de un objeto empieza con un guion bajo, entonces esa propiedad se considera privada:

```javascript
class ValueWrapper {
  constructor(value) {
    this._value = value;
  }
}
```

### 7.1.9 ¿Dónde poner puntos y comas?

Al final de una declaración:

```javascript
const x = 123;
func();
```

Pero no si esa declaración termina con una llave:

````

```javascript
if (true) {
  console.log('hi')
}
````

En general, poner puntos y comas al final de las declaraciones es una buena práctica, incluso si JavaScript no siempre los requiere.

### 7.2 (Avanzado)

En esta sección se tratarán temas avanzados de la sintaxis de JavaScript. Siéntase libre de saltar esta sección y volver más tarde si es necesario.

### 7.3 Identificadores

### 7.3.1 Identificadores válidos (nombres de variables, etc.)

Un identificador es un nombre utilizado para identificar una variable, función, clase, módulo, etc. Los identificadores en JavaScript deben seguir estas reglas:

1. Pueden contener letras, dígitos, guiones bajos y signos de dólar.
2. No pueden comenzar con un dígito.
3. Son sensibles a mayúsculas y minúsculas (`myVar` y `myvar` son diferentes).
4. No pueden ser palabras reservadas.

Ejemplos de identificadores válidos:

```javascript
let myVar;
let _myVar;
let $myVar;
let myVar1;
```

Ejemplos de identificadores inválidos:

```javascript
let 1myVar; // No puede comenzar con un dígito
let my-var; // Guion no permitido
let let;    // 'let' es una palabra reservada
```

### 7.3.2 Palabras reservadas

Las palabras reservadas en JavaScript no pueden ser usadas como identificadores. Aquí hay una lista de algunas de las palabras reservadas más comunes:

```text
break, case, catch, class, const, continue, debugger, default, delete, do, else, export, extends, false, finally, for, function, if, import, in, instanceof, new, null, return, super, switch, this, throw, true, try, typeof, var, void, while, with, yield
```

### 7.4 Declaración vs. expresión

### 7.4.1 Declaraciones

Una declaración es una instrucción que realiza una acción, como declarar una variable o una función.

```javascript
let x = 5; // Declaración de variable
function myFunction() {} // Declaración de función
class MyClass {} // Declaración de clase
```

### 7.4.2 Expresiones

Una expresión es un fragmento de código que produce un valor.

```javascript
x + y; // Expresión aritmética
myFunction(); // Expresión de llamada a función
```

### 7.4.3 ¿Qué está permitido dónde?

Las declaraciones pueden aparecer en cualquier lugar donde se permitan instrucciones, como dentro de bloques `{}`. Las expresiones, sin embargo, no pueden estar solas; deben ser parte de una declaración.

### 7.5 Sintaxis ambigua

### 7.5.1 Misma sintaxis: declaración de función y expresión de función

Las declaraciones de funciones y las expresiones de funciones pueden parecer similares, pero tienen diferencias clave en el ámbito y el tiempo de evaluación.

```javascript
// Declaración de función
function foo() {}

// Expresión de función
const bar = function () {};
```

### 7.5.2 Misma sintaxis: literal de objeto y bloque

Un literal de objeto y un bloque de código pueden parecer iguales, pero se interpretan de manera diferente según el contexto.

```javascript
// Bloque de código
{
  console.log("This is a block");
}

// Literal de objeto
const obj = {
  name: "Alice",
};
```

### 7.5.3 Desambiguación

Para desambiguar entre un literal de objeto y un bloque, use paréntesis alrededor del literal de objeto.

```javascript
const obj = { name: "Alice" }; // Literal de objeto
```

### 7.6 Puntos y comas

### 7.6.1 Regla general para puntos y comas

Es una buena práctica usar puntos y comas al final de cada declaración.

### 7.6.2 Puntos y comas: declaraciones de control

Los puntos y comas no se usan después de las llaves que cierran bloques de control como `if`, `for`, `while`, `function`, etc.

```javascript
if (true) {
  console.log("hi");
} // No se necesita punto y coma aquí
```

### 7.7 Inserción automática de puntos y comas (ASI)

JavaScript intenta corregir automáticamente las declaraciones que carecen de puntos y comas insertándolos donde cree que deberían estar.

### 7.7.1 ASI activado inesperadamente

A veces, la ASI puede causar errores inesperados.

```javascript
return; // ASI inserta un punto y coma aquí
{
  key: "value";
}
```

El código anterior devuelve `undefined` en lugar del objeto esperado debido a la inserción automática de puntos y comas.

### 7.7.2 ASI inesperadamente no activado

En algunos casos, ASI no se activa cuando se espera, causando errores de sintaxis.

```javascript
let x = (y[z] = [1]); // SyntaxError
```

### 7.8 Puntos y comas: mejores prácticas

Para evitar problemas con ASI, siempre use puntos y comas al final de las declaraciones.

### 7.9 Modo estricto vs. modo relajado

El modo estricto es una versión más segura de JavaScript que impone restricciones adicionales.

### 7.9.1 Activando el modo estricto

Para activar el modo estricto, agregue `"use strict";` al comienzo de un archivo o función.

```javascript
"use strict";
function myFunction() {
  // Código en modo estricto
}
```

### 7.9.2 Mejoras en el modo estricto

Veamos tres cosas que el modo estricto hace mejor que el modo laxo. Solo en esta sección, todos los fragmentos de código se ejecutan en modo laxo.

### 7.9.2.1 Trampa del modo laxo: cambiar una variable no declarada crea una variable global

En modo no estricto, cambiar una variable no declarada crea una variable global.

```javascript
function sloppyFunc() {
  undeclaredVar1 = 123;
}
sloppyFunc();
// Variable global `undeclaredVar1` creada:
assert.equal(undeclaredVar1, 123);
```

El modo estricto lo hace mejor y lanza un ReferenceError. Eso facilita la detección de errores tipográficos.

```javascript
function strictFunc() {
  "use strict";
  undeclaredVar2 = 123;
}
assert.throws(() => strictFunc(), {
  name: "ReferenceError",
  message: "undeclaredVar2 is not defined",
});
```

El `assert.throws()` establece que su primer argumento, una función, lanza un ReferenceError cuando se llama.

### 7.9.2.2 Las declaraciones de funciones están limitadas al bloque en el modo estricto, limitadas a la función en el modo laxo

En modo estricto, una variable creada mediante una declaración de función solo existe dentro del bloque más cercano que la contiene:

```javascript
function strictFunc() {
  "use strict";
  {
    function foo() {
      return 123;
    }
  }
  return foo(); // ReferenceError
}
assert.throws(() => strictFunc(), {
  name: "ReferenceError",
  message: "foo is not defined",
});
```

En modo laxo, las declaraciones de funciones están limitadas a la función:

```javascript
function sloppyFunc() {
  {
    function foo() {
      return 123;
    }
  }
  return foo(); // funciona
}
assert.equal(sloppyFunc(), 123);
```

### 7.9.2.3 El modo laxo no lanza excepciones al cambiar datos inmutables

En modo estricto, se obtiene una excepción si se intenta cambiar datos inmutables:

```javascript
function strictFunc() {
  "use strict";
  true.prop = 1; // TypeError
}
assert.throws(() => strictFunc(), {
  name: "TypeError",
  message: "Cannot create property 'prop' on boolean 'true'",
});
```

En modo laxo, la asignación falla silenciosamente:

```javascript
function sloppyFunc() {
  true.prop = 1; // falla silenciosamente
  return true.prop;
}
assert.equal(sloppyFunc(), undefined);
```

### Lectura adicional: modo laxo

Para obtener más información sobre cómo el modo laxo difiere del modo estricto, consulta MDN.

### Quiz: avanzado

Consulta la aplicación de cuestionarios.
