# 12 Valores

## 12.1 ¿Qué es un tipo?

## 12.2 La jerarquía de tipos de JavaScript

## 12.3 Los tipos de la especificación del lenguaje

## 12.4 Valores primitivos vs. objetos

### 12.4.1 Valores primitivos (en breve: primitivos)

### 12.4.2 Objetos

## 12.5 Los operadores typeof e instanceof: ¿cuál es el tipo de un valor?

### 12.5.1 typeof

### 12.5.2 instanceof

## 12.6 Clases y funciones constructoras

### 12.6.1 Funciones constructoras asociadas con tipos primitivos

## 12.7 Conversiones entre tipos

### 12.7.1 Conversión explícita entre tipos

### 12.7.2 Coerción (conversión automática entre tipos)

En este capítulo, examinaremos los tipos de valores que tiene JavaScript.

### Herramienta de apoyo: ===

En este capítulo, utilizaremos ocasionalmente el operador de igualdad estricta. `a === b` evalúa a `true` si `a` y `b` son iguales. Lo que eso significa exactamente se explica en §13.4.2 "Igualdad estricta (=== y !==)".

## 12.1 ¿Qué es un tipo?

Para este capítulo, considero que los tipos son conjuntos de valores; por ejemplo, el tipo booleano es el conjunto {false, true}.

## 12.2 La jerarquía de tipos de JavaScript

Figura 6: Una jerarquía parcial de los tipos de JavaScript. Faltan las clases para errores, las clases asociadas con tipos primitivos y más. El diagrama insinúa el hecho de que no todos los objetos son instancias de Object.

Figura 6: Una jerarquía parcial de los tipos de JavaScript. Faltan las clases para errores, las clases asociadas con tipos primitivos y más. El diagrama insinúa el hecho de que no todos los objetos son instancias de Object.

La figura 6 muestra la jerarquía de tipos de JavaScript. ¿Qué aprendemos de ese diagrama?

JavaScript distingue entre dos tipos de valores: valores primitivos y objetos. Pronto veremos cuál es la diferencia.
El diagrama diferencia entre objetos y las instancias de la clase Object. Cada instancia de Object es también un objeto, pero no viceversa. Sin embargo, prácticamente todos los objetos que encontrarás en la práctica son instancias de Object; por ejemplo, objetos creados mediante literales de objeto. Se explican más detalles sobre este tema en §29.7.3 "No todos los objetos son instancias de Object".

## 12.3 Los tipos de la especificación del lenguaje

La especificación de ECMAScript solo conoce un total de ocho tipos. Los nombres de esos tipos son (estoy usando los nombres de TypeScript, no los de la especificación):

- undefined con el único elemento undefined
- null con el único elemento null
- boolean con los elementos false y true
- number el tipo de todos los números (por ejemplo, -123, 3.141)
- bigint el tipo de todos los números grandes (por ejemplo, -123n)
- string el tipo de todas las cadenas de texto (por ejemplo, 'abc')
- symbol el tipo de todos los símbolos (por ejemplo, Symbol('Mi Símbolo'))
- object el tipo de todos los objetos (diferente de Object, el tipo de todas las instancias de la clase Object y sus subclases)

## 12.4 Valores primitivos vs. objetos

La especificación hace una distinción importante entre valores:

- Valores primitivos son los elementos de los tipos undefined, null, boolean, number, bigint, string, symbol.
- Todos los demás valores son objetos.

En contraste con Java (que inspiró a JavaScript aquí), los valores primitivos no son ciudadanos de segunda clase. La diferencia entre ellos y los objetos es más sutil. En resumen:

### Valores primitivos

- Son bloques atómicos de datos en JavaScript.
- Se pasan por valor: cuando los valores primitivos se asignan a variables o se pasan a funciones, su contenido se copia.
- Se comparan por valor: al comparar dos valores primitivos, se comparan sus contenidos.

### Objetos

- Son piezas compuestas de datos.
- Se pasan por identidad (término propio): cuando los objetos se asignan a variables o se pasan a funciones, sus identidades (piensa en punteros) se copian.
- Se comparan por identidad (término propio): al comparar dos objetos, se comparan sus identidades.

Aparte de eso, los valores primitivos y los objetos son bastante similares: ambos tienen propiedades (entradas clave-valor) y se pueden usar en las mismas ubicaciones.

A continuación, veremos en mayor profundidad los valores primitivos y los objetos.

### 12.4.1 Valores primitivos (en breve: primitivos)

#### 12.4.1.1 Los primitivos son inmutables

No puedes cambiar, agregar o eliminar propiedades de los primitivos:

```js
const str = "abc";
assert.equal(str.length, 3);
assert.throws(() => {
  str.length = 1;
}, /^TypeError: Cannot assign to read only property 'length'/);
```

#### 12.4.1.2 Los primitivos se pasan por valor

Los primitivos se pasan por valor: las variables (incluidos los parámetros) almacenan el contenido de los primitivos. Cuando se asigna un valor primitivo a una variable o se pasa como argumento a una función, su contenido se copia.

```js
const x = 123;
const y = x;
// `y` es lo mismo que cualquier otro número 123
assert.equal(y, 123);
```

Observar la diferencia entre pasar por valor y pasar por referencia:

Debido a que los valores primitivos son inmutables y se comparan por valor (ver la siguiente subsección), no hay forma de observar la diferencia entre pasar por valor y pasar por identidad (como se usa para los objetos en JavaScript).

#### 12.4.1.3 Los primitivos se comparan por valor

Los primitivos se comparan por valor: al comparar dos valores primitivos, comparamos sus contenidos.

```js
assert.equal(123 === 123, true);
assert.equal("abc" === "abc", true);
```

Para ver qué tiene de especial esta forma de comparar, continúa leyendo y descubre cómo se comparan los objetos.

### 12.4.2 Objetos

Los objetos se tratan en detalle en la sección §28 “Objetos” y en el capítulo siguiente. Aquí, nos enfocamos principalmente en cómo se diferencian de los valores primitivos.

Primero, exploremos dos formas comunes de crear objetos:

Literal de objeto:

```javascript
const obj = {
  first: "Jane",
  last: "Doe",
};
```

El literal de objeto comienza y termina con llaves {}. Crea un objeto con dos propiedades. La primera propiedad tiene la clave 'first' (una cadena) y el valor 'Jane'. La segunda propiedad tiene la clave 'last' y el valor 'Doe'. Para más información sobre los literales de objeto, consulta la sección §28.3.1 “Literals de objeto: propiedades”.

Literal de arreglo:

```javascript
const fruits = ["strawberry", "apple"];
```

El literal de arreglo comienza y termina con corchetes []. Crea un arreglo con dos elementos: 'strawberry' y 'apple'. Para más información sobre los literales de arreglo, consulta la sección §31.3.1 “Creando, leyendo, escribiendo arreglos”.

#### 12.4.2.1 Los objetos son mutables por defecto

Por defecto, puedes cambiar, añadir y eliminar libremente las propiedades de los objetos:

```javascript
const obj = {};

obj.count = 2; // añadir una propiedad
assert.equal(obj.count, 2);

obj.count = 3; // cambiar una propiedad
assert.equal(obj.count, 3);
```

#### 12.4.2.2 Los objetos se pasan por identidad

Los objetos se pasan por identidad (mi término): las variables (incluyendo los parámetros) almacenan las identidades de los objetos.

La identidad de un objeto es como un puntero (o una referencia transparente) a los datos reales del objeto en el montón (piensa en la memoria principal compartida de un motor de JavaScript).

Al asignar un objeto a una variable o pasarlo como argumento a una función, se copia su identidad. Cada literal de objeto crea un objeto nuevo en el montón y devuelve su identidad.

```javascript
const a = {}; // objeto vacío nuevo
// Pasar la identidad en `a` a `b`:
const b = a;

// Ahora `a` y `b` apuntan al mismo objeto
// (comparten ese objeto):
assert.equal(a === b, true);

// Cambiar `a` también cambia `b`:
a.name = "Tessa";
assert.equal(b.name, "Tessa");
```

JavaScript usa la recolección de basura para gestionar automáticamente la memoria:

```javascript
let obj = { prop: "value" };
obj = {};
```

Ahora, el valor antiguo `{ prop: 'value' }` de obj es basura (ya no se usa). JavaScript lo recolectará automáticamente (lo eliminará de la memoria) en algún momento (posiblemente nunca si hay suficiente memoria libre).

#### Detalles: pasar por identidad

“Pasar por identidad” significa que la identidad de un objeto (una referencia transparente) se pasa por valor. Este enfoque también se llama “pasar por compartición”.

#### 12.4.2.3 Los objetos se comparan por identidad

Los objetos se comparan por identidad (mi término): dos variables son iguales solo si contienen la misma identidad de objeto. No son iguales si se refieren a diferentes objetos con el mismo contenido.

```javascript
const obj = {}; // objeto vacío nuevo
assert.equal(obj === obj, true); // misma identidad
assert.equal({} === {}, false); // diferentes identidades, mismo contenido
```

### 12.5 Los operadores typeof e instanceof: ¿cuál es el tipo de un valor?

Los dos operadores typeof e instanceof te permiten determinar qué tipo tiene un valor dado x:

```javascript
if (typeof x === 'string') ···
if (x instanceof Array) ···
```

¿Cómo difieren?

- typeof distingue los 7 tipos de la especificación (menos una omisión, más una adición).
- instanceof prueba qué clase creó un valor dado.

#### Regla general: typeof es para valores primitivos; instanceof es para objetos

#### 12.5.1 typeof

Tabla 2: Los resultados del operador typeof.

| x                       | typeof x    |
| ----------------------- | ----------- |
| undefined               | 'undefined' |
| null                    | 'object'    |
| Boolean                 | 'boolean'   |
| Number                  | 'number'    |
| Bigint                  | 'bigint'    |
| String                  | 'string'    |
| Symbol                  | 'symbol'    |
| Function                | 'function'  |
| Todos los otros objetos | 'object'    |

La Tabla 2 enumera todos los resultados de typeof. Corresponden aproximadamente a los 7 tipos de la especificación del lenguaje. Sin embargo, hay dos diferencias, y son peculiaridades del lenguaje:

- typeof null devuelve 'object' y no 'null'. Eso es un error. Desafortunadamente, no se puede arreglar. TC39 intentó hacerlo, pero rompió demasiado código en la web.
- typeof de una función debería ser 'object' (las funciones son objetos). Introducir una categoría separada para funciones es confuso.

Estos son algunos ejemplos de uso de typeof:

```javascript
> typeof undefined
'undefined'
> typeof 123n
'bigint'
> typeof 'abc'
'string'
> typeof {}
'object'
```

#### Ejercicios: Dos ejercicios sobre typeof

exercises/values/typeof_exrc.mjs

#### Bonus: exercises/values/is_object_test.mjs

#### 12.5.2 instanceof

Este operador responde a la pregunta: ¿un valor x ha sido creado por una clase C?

```javascript
x instanceof C;
```

Por ejemplo:

```javascript
> (function() {}) instanceof Function
true
> ({}) instanceof Object
true
> [] instanceof Array
true
```

Los valores primitivos no son instancias de nada:

```javascript
> 123 instanceof Number
false
> '' instanceof String
false
> '' instanceof Object
false
```

#### Ejercicio: instanceof

exercises/values/instanceof_exrc.mjs

### 12.6 Clases y funciones constructoras

Las fábricas originales de objetos en JavaScript son funciones constructoras: funciones ordinarias que devuelven “instancias” de sí mismas si las invocas mediante el operador new.

ES6 introdujo las clases, que son principalmente una mejor sintaxis para las funciones constructoras.

En este libro, uso los términos función constructora y clase indistintamente.

Las clases pueden verse como particiones del único tipo object de la especificación en subtipos: nos dan más tipos que los limitados 7 de la especificación. Cada clase es el tipo de los objetos que fueron creados por ella.

#### 12.6.1 Funciones constructoras asociadas con tipos primitivos

Cada tipo primitivo (excepto los tipos internos de la especificación para undefined y null) tiene una función constructora asociada (piensa en clases):

- La función constructora Boolean está asociada con los booleanos.
- La función constructora Number está asociada con los números.
- La función constructora String está asociada con las cadenas.
- La función constructora Symbol está asociada con los símbolos.

Cada una de estas funciones desempeña varios roles. Por ejemplo, Number:

- Puedes usarla como función y convertir valores a números:

```javascript
assert.equal(Number("123"), 123);
```

- Number.prototype proporciona las propiedades para los números, por ejemplo, el método .toString():

```javascript
assert.equal((123).toString(), Number.prototype.toString);
```

- Number es un objeto contenedor/namespace para funciones de herramientas para números, por ejemplo:

```javascript
assert.equal(Number.isInteger(123), true);
```

- Por último, también puedes usar Number como una clase y crear objetos numéricos. Estos objetos son diferentes de los números reales y deben evitarse.

```javascript
assert.notEqual(new Number(123), 123);
assert.equal(new Number(123).valueOf(), 123);
```

#### 12.6.1.1 Envolviendo valores primitivos

Las funciones constructoras relacionadas con los tipos primitivos también se llaman tipos envolventes porque proporcionan la forma canónica de convertir valores primitivos en objetos. En el proceso, los valores primitivos se “envuelven” en objetos.

```javascript
const prim = true;
assert.equal(typeof prim, "boolean");
assert.equal(prim instanceof Boolean, false);

const wrapped = Object(prim);
assert.equal(typeof wrapped, "object");
assert.equal(wrapped instanceof Boolean, true);

assert.equal(wrapped.valueOf(), prim); // desenvuelve
```

Envolverse rara vez importa en la práctica, pero se usa internamente en la especificación del lenguaje para dar propiedades a los primitivos.

### 12.7 Convirtiendo entre tipos

Hay dos formas en que los valores se convierten a otros tipos en JavaScript:

- Conversión explícita: mediante funciones como String().
- Coerción (conversión automática): ocurre cuando una operación recibe operandos/parámetros con los que no puede trabajar.

### 12.7.1 Conversión explícita entre tipos

La función asociada con un tipo primitivo convierte explícitamente los valores a ese tipo:

```javascript
> Boolean(0)
false
> Number('123')
123
> String(123)
'123'
```

También puedes usar Object() para convertir valores en objetos:

```javascript
> typeof Object(123)
'object'
```

La siguiente tabla describe con más detalle cómo funciona esta conversión:

| x         | Object(x)                                      |
| --------- | ---------------------------------------------- |
| undefined | {}                                             |
| null      | {}                                             |
| boolean   | new Boolean(x)                                 |
| number    | new Number(x)                                  |
| bigint    | Una instancia de BigInt (new arroja TypeError) |
| string    | new String(x)                                  |
| symbol    | Una instancia de Symbol (new arroja TypeError) |
| object    | x                                              |

### 12.7.2 Coerción (conversión automática entre tipos)

Para muchas operaciones, JavaScript convierte automáticamente los operandos/parámetros si sus tipos no encajan. Este tipo de conversión automática se llama coerción.

Por ejemplo, el operador de multiplicación convierte sus operandos a números:

```javascript
> '7' * '3'
21
```

Muchas funciones integradas también realizan coerción. Por ejemplo, Number.parseInt() convierte su parámetro a una cadena antes de analizarlo. Esto explica el siguiente resultado:

```javascript
> Number.parseInt(123.45)
123
```

El número 123.45 se convierte en la cadena '123.45' antes de ser analizado. El análisis se detiene antes del primer carácter no numérico, por lo que el resultado es 123.

#### Ejercicio: Convirtiendo valores a primitivos

exercises/values/conversion_exrc.mjs

#### Quiz

Ver la aplicación de quiz.
