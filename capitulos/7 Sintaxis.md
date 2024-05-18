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

### 7.1.2 Módulos

Cada módulo es un solo archivo. Considera, por ejemplo, los siguientes dos archivos con módulos en ellos:

file-tools.mjs
main.mjs

El módulo en file-tools.mjs exporta su función isTextFile
