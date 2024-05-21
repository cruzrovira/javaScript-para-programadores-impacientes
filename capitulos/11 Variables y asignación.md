# 11 Variables y asignación

## 11.1 let

## 11.2 const

### 11.2.1 const e inmutabilidad

### 11.2.2 const y bucles

## 11.3 Decidiendo entre const y let

## 11.4 El alcance de una variable

### 11.4.1 Variables sombría

## 11.5 (Avanzado)

## 11.6 Terminología: estático vs. dinámico

### 11.6.1 Fenómeno estático: alcances de variables

### 11.6.2 Fenómeno dinámico: llamadas a funciones

## 11.7 Variables globales y el objeto global

### 11.7.1 globalThis [ES2020]

## 11.8 Declaraciones: alcance y activación

### 11.8.1 const y let: zona muerta temporal

### 11.8.2 Declaraciones de funciones y activación temprana

### 11.8.3 Declaraciones de clases no se activan temprano

### 11.8.4 var: elevación (activación parcial temprana)

## 11.9 Cierres

### 11.9.1 Variables vinculadas vs. variables libres

### 11.9.2 ¿Qué es un cierre?

### 11.9.3 Ejemplo: una fábrica de incrementadores

### 11.9.4 Casos de uso de cierres

Estas son las principales formas de declarar variables en JavaScript:

- `let` declara variables mutables.
- `const` declara constantes (variables inmutables).

Antes de ES6, también existía `var`. Pero tiene varias peculiaridades, por lo que es mejor evitarlo en JavaScript moderno. Puedes leer más sobre esto en Speaking JavaScript.

## 11.1 let

Las variables declaradas mediante `let` son mutables:

```javascript
let i;
i = 0;
i = i + 1;
assert.equal(i, 1);
```

También puedes declarar y asignar al mismo tiempo:

```javascript
let i = 0;
```

## 11.2 const

Las variables declaradas mediante `const` son inmutables. Siempre debes inicializarlas inmediatamente:

```javascript
const i = 0; // debe inicializarse

assert.throws(
  () => {
    i = i + 1;
  },
  {
    name: "TypeError",
    message: "Assignment to constant variable.",
  }
);
```

### 11.2.1 const e inmutabilidad

En JavaScript, `const` solo significa que la vinculación (la asociación entre el nombre de la variable y el valor de la variable) es inmutable. El valor en sí mismo puede ser mutable, como `obj` en el siguiente ejemplo.

```javascript
const obj = { prop: 0 };

// Permitido: cambiar propiedades de `obj`
obj.prop = obj.prop + 1;
assert.equal(obj.prop, 1);

// No permitido: asignar a `obj`
assert.throws(
  () => {
    obj = {};
  },
  {
    name: "TypeError",
    message: "Assignment to constant variable.",
  }
);
```

### 11.2.2 const y bucles

Puedes usar `const` con bucles `for-of`, donde se crea una nueva vinculación para cada iteración:

```javascript
const arr = ["hello", "world"];
for (const elem of arr) {
  console.log(elem);
}
// Salida:
// 'hello'
// 'world'
```

En los bucles `for` normales, sin embargo, debes usar `let`:

```javascript
const arr = ["hello", "world"];
for (let i = 0; i < arr.length; i++) {
  const elem = arr[i];
  console.log(elem);
}
```

## 11.3 Decidiendo entre const y let

Recomiendo las siguientes reglas para decidir entre `const` y `let`:

- `const` indica una vinculación inmutable y que una variable nunca cambia su valor. Prefiérelo.
- `let` indica que el valor de una variable cambia. Úsalo solo cuando no puedas usar `const`.

  **Ejercicio: const**

  ejercicios/variables-asignación/const_exrc.mjs

## 11.4 El alcance de una variable

El alcance de una variable es la región de un programa donde se puede acceder a ella. Considera el siguiente código.

```javascript
{
  // Alcance A. Accesible: x
  const x = 0;
  assert.equal(x, 0);
  {
    // Alcance B. Accesible: x, y
    const y = 1;
    assert.equal(x, 0);
    assert.equal(y, 1);
    {
      // Alcance C. Accesible: x, y, z
      const z = 2;
      assert.equal(x, 0);
      assert.equal(y, 1);
      assert.equal(z, 2);
    }
  }
}
// Fuera. No accesible: x, y, z
assert.throws(() => console.log(x), {
  name: "ReferenceError",
  message: "x is not defined",
});
```

- El alcance A es el alcance (directo) de `x`.
- Los alcances B y C son alcances internos del alcance A.
- El alcance A es un alcance externo del alcance B y del alcance C.

Cada variable es accesible en su alcance directo y en todos los alcances anidados dentro de ese alcance.

Las variables declaradas mediante `const` y `let` se llaman de alcance de bloque porque sus alcances son siempre los bloques circundantes más internos.

### 11.4.1 Variables sombría

No puedes declarar la misma variable dos veces al mismo nivel:

```javascript
assert.throws(
  () => {
    eval("let x = 1; let x = 2;");
  },
  {
    name: "SyntaxError",
    message: "Identifier 'x' has already been declared",
  }
);
```

**¿Por qué eval()?**

`eval()` retrasa el análisis (y, por lo tanto, el `SyntaxError`) hasta que se ejecuta el callback de `assert.throws()`. Si no lo usáramos, ya obtendríamos un error cuando se analiza este código y `assert.throws()` ni siquiera se ejecutaría.

Sin embargo, puedes anidar un bloque y usar el mismo nombre de variable `x` que usaste fuera del bloque:

```javascript
const x = 1;
assert.equal(x, 1);
{
  const x = 2;
  assert.equal(x, 2);
}
assert.equal(x, 1);
```

Dentro del bloque, la variable interna `x` es la única accesible con ese nombre. Se dice que la `x` interna sombre la `x` externa. Una vez que sales del bloque, puedes acceder al valor antiguo nuevamente.

**Cuestionario: básico**

Ver aplicación de cuestionarios.

## 11.5 (Avanzado)

Todas las secciones restantes son avanzadas.

## 11.6 Terminología: estático vs. dinámico

Estos dos adjetivos describen fenómenos en los lenguajes de programación:

- Estático significa que algo está relacionado con el código fuente y se puede determinar sin ejecutar código.
- Dinámico significa en tiempo de ejecución.

Veamos ejemplos de estos dos términos.

### 11.6.1 Fenómeno estático: alcances de variables

Los alcances de variables son un fenómeno estático. Considera el siguiente código:

```javascript
function f() {
  const x = 3;
  // ···
}
```

`x` tiene un alcance estático (o léxico). Es decir, su alcance es fijo y no cambia en tiempo de ejecución.

Los alcances de variables forman un árbol estático (a través de anidamientos estáticos).

### 11.6.2 Fenómeno dinámico: llamadas a funciones

Las llamadas a funciones son un fenómeno dinámico. Considera el siguiente código:

```javascript
function g(x) {}
function h(y) {
  if (Math.random()) g(y); // (A)
}
```

Si la llamada a la función en la línea A sucede o no, solo se puede decidir en tiempo de ejecución.

Las llamadas a funciones forman un árbol dinámico (a través de llamadas dinámicas).

### 11.7 Variables globales y el objeto global

Los alcances de las variables en JavaScript están anidados, formando un árbol:

- El alcance más externo es la raíz del árbol.
- Los alcances directamente contenidos en ese alcance son los hijos de la raíz.
- Y así sucesivamente.

La raíz también se llama alcance global. En los navegadores web, la única ubicación donde uno está directamente en ese alcance es en el nivel superior de un script. Las variables del alcance global se llaman variables globales y son accesibles en todas partes. Hay dos tipos de variables globales:

1. **Variables declarativas globales**: Son variables normales.
   - Solo se pueden crear en el nivel superior de un script, mediante declaraciones const, let y class.
2. **Variables del objeto global**: Se almacenan en propiedades del llamado objeto global.
   - Se crean en el nivel superior de un script, mediante declaraciones var y function.

El objeto global se puede acceder mediante la variable global `globalThis`. Se puede usar para crear, leer y eliminar variables del objeto global. Aparte de eso, las variables del objeto global funcionan como variables normales.

El siguiente fragmento de HTML demuestra `globalThis` y los dos tipos de variables globales:

```html
<script>
  const declarativeVariable = "d";
  var objectVariable = "o";
</script>
<script>
  // Todos los scripts comparten el mismo alcance de nivel superior:
  console.log(declarativeVariable); // 'd'
  console.log(objectVariable); // 'o'

  // No todas las declaraciones crean propiedades del objeto global:
  console.log(globalThis.declarativeVariable); // undefined
  console.log(globalThis.objectVariable); // 'o'
</script>
```

Cada módulo ECMAScript tiene su propio alcance. Por lo tanto, las variables que existen en el nivel superior de un módulo no son globales. La Fig. 5 ilustra cómo se relacionan los diversos alcances.

Figura 5: El alcance global es el alcance más externo de JavaScript. Tiene dos tipos de variables: variables de objeto (gestionadas a través del objeto global) y variables declarativas normales. Cada módulo ECMAScript tiene su propio alcance que está contenido en el alcance global.

#### 11.7.1 `globalThis` [ES2020]

La variable global `globalThis` es la nueva forma estándar de acceder al objeto global. Recibió su nombre porque tiene el mismo valor que `this` en el alcance global.

`globalThis` no siempre apunta directamente al objeto global

Por ejemplo, en los navegadores, hay una indirecta. Esa indirecta normalmente no es notoria, pero está ahí y se puede observar.

#### 11.7.1.1 Alternativas a `globalThis`

Las formas más antiguas de acceder al objeto global dependen de la plataforma:

- Variable global `window`: es la forma clásica de referirse al objeto global. Pero no funciona en Node.js ni en Web Workers.
- Variable global `self`: está disponible en Web Workers y navegadores en general. Pero no es compatible con Node.js.
- Variable global `global`: solo está disponible en Node.js.

#### 11.7.1.2 Casos de uso para `globalThis`

El objeto global ahora se considera un error que JavaScript no puede eliminar, debido a la compatibilidad hacia atrás. Afecta negativamente al rendimiento y generalmente es confuso.

ECMAScript 6 introdujo varias características que facilitan evitar el objeto global, por ejemplo:

- Las declaraciones const, let y class no crean propiedades del objeto global cuando se usan en el alcance global.
- Cada módulo ECMAScript tiene su propio alcance local.

Usualmente es mejor acceder a las variables del objeto global a través de variables y no a través de propiedades de `globalThis`. Lo primero siempre ha funcionado igual en todas las plataformas de JavaScript.

Los tutoriales en la web ocasionalmente acceden a variables globales `globVar` a través de `window.globVar`. Pero el prefijo "window." no es necesario y recomiendo omitirlo:

```js
window.encodeURIComponent(str); // no
encodeURIComponent(str); // sí
```

Por lo tanto, hay relativamente pocos casos de uso para `globalThis`, por ejemplo:

- Polyfills que añaden nuevas características a motores de JavaScript antiguos.
- Detección de características, para averiguar qué características soporta un motor de JavaScript.

### 11.8 Declaraciones: alcance y activación

Estos son dos aspectos clave de las declaraciones:

- **Alcance**: ¿Dónde se puede ver una entidad declarada? Este es un rasgo estático.
- **Activación**: ¿Cuándo puedo acceder a una entidad? Este es un rasgo dinámico. Algunas entidades se pueden acceder tan pronto como entramos en sus alcances. Para otras, debemos esperar hasta que la ejecución alcance sus declaraciones.

La Tabla 1 resume cómo manejan estos aspectos las diversas declaraciones.

Tabla 1: Aspectos de las declaraciones. "Duplicados" describe si una declaración se puede usar dos veces con el mismo nombre (por alcance). "Prop. global" describe si una declaración añade una propiedad al objeto global, cuando se ejecuta en el alcance global de un script. TDZ significa zona muerta temporal (que se explica más adelante).

| Alcance  | Activación  | Duplicados           | Prop. global |
| -------- | ----------- | -------------------- | ------------ | --- |
| const    | Bloque      | decl. (TDZ)          | ✘            | ✘   |
| let      | Bloque      | decl. (TDZ)          | ✘            | ✘   |
| function | Bloque (\*) | inicio               | ✔            | ✔   |
| class    | Bloque      | decl. (TDZ)          | ✘            | ✘   |
| import   | Módulo      | igual que export     | ✘            | ✘   |
| var      | Función     | inicio, parcialmente | ✔            | ✔   |

`import` se describe en §27.5 "Módulos ECMAScript". Las siguientes secciones describen los otros constructos en más detalle.

#### 11.8.1 `const` y `let`: zona muerta temporal

Para JavaScript, TC39 necesitaba decidir qué pasa si accedes a una constante en su alcance directo, antes de su declaración:

```js
{
  console.log(x); // ¿Qué pasa aquí?
  const x;
}
```

Algunas posibles aproximaciones son:

- El nombre se resuelve en el alcance que rodea el alcance actual.
- Obtienes undefined.
- Hay un error.

La aproximación 1 fue rechazada porque no hay precedentes en el lenguaje para esta aproximación. Por lo tanto, no sería intuitivo para los programadores de JavaScript.

La aproximación 2 fue rechazada porque entonces `x` no sería una constante: tendría valores diferentes antes y después de su declaración.

`let` usa la misma aproximación 3 que `const`, para que ambos funcionen de manera similar y sea fácil alternar entre ellos.

El tiempo entre entrar en el alcance de una variable y ejecutar su declaración se llama la zona muerta temporal (TDZ) de esa variable:

- Durante este tiempo, la variable se considera no inicializada (como si fuera un valor especial que tiene).
- Si accedes a una variable no inicializada, obtienes un `ReferenceError`.
- Una vez que alcanzas una declaración de variable, la variable se establece en el valor del inicializador (especificado a través del símbolo de asignación) o en undefined, si no hay inicializador.

El siguiente código ilustra la zona muerta temporal:

```js
if (true) {
  // entrando en el alcance de `tmp`, comienza la TDZ
  // `tmp` no está inicializado:
  assert.throws(() => (tmp = "abc"), ReferenceError);
  assert.throws(() => console.log(tmp), ReferenceError);

  let tmp; // termina la TDZ
  assert.equal(tmp, undefined);
}
```

El siguiente ejemplo muestra que la zona muerta temporal es verdaderamente temporal (relacionada con el tiempo):

```js
if (true) {
  // entrando en el alcance de `myVar`, comienza la TDZ
  const func = () => {
    console.log(myVar); // ejecutado más tarde
  };

  // Estamos dentro de la TDZ:
  // Acceder a `myVar` causa `ReferenceError`

  let myVar = 3; // termina la TDZ
  func(); // OK, llamado fuera de la TDZ
}
```

Aunque `func()` está ubicado antes de la declaración de `myVar` y usa esa variable, podemos llamar a `func()`. Pero debemos esperar hasta que termine la zona muerta temporal de `myVar`.

### 11.8.2 Declaraciones de funciones y activación temprana

#### Más información sobre funciones

En esta sección, estamos utilizando funciones, antes de haber tenido la oportunidad de aprenderlas adecuadamente. Con suerte, todo tiene sentido. Siempre que no sea así, consulte §25 "Valores llamables".

Una declaración de función siempre se ejecuta al entrar en su ámbito, independientemente de dónde se encuentre dentro de ese ámbito. Eso permite llamar a una función `foo()` antes de que sea declarada:

```javascript
assert.equal(foo(), 123); // OK
function foo() {
  return 123;
}
```

La activación temprana de `foo()` significa que el código anterior es equivalente a:

```javascript
function foo() {
  return 123;
}
assert.equal(foo(), 123);
```

Si declaras una función mediante `const` o `let`, entonces no se activa temprano. En el siguiente ejemplo, solo puedes usar `bar()` después de su declaración.

```javascript
assert.throws(
  () => bar(), // antes de la declaración
  ReferenceError
);

const bar = () => {
  return 123;
};

assert.equal(bar(), 123); // después de la declaración
```

#### 11.8.2.1 Llamando por adelantado sin activación temprana

Incluso si una función `g()` no se activa temprano, puede ser llamada por una función precedente `f()` (en el mismo ámbito) si seguimos la siguiente regla: `f()` debe ser invocada después de la declaración de `g()`.

```javascript
const f = () => g();
const g = () => 123;

// Llamamos a `f()` después de que `g()` fue declarada:
assert.equal(f(), 123);
```

Las funciones de un módulo generalmente se invocan después de que se ejecuta todo su cuerpo. Por lo tanto, en los módulos, rara vez necesitas preocuparte por el orden de las funciones.

Finalmente, nota cómo la activación temprana mantiene automáticamente la regla mencionada anteriormente: al entrar en un ámbito, todas las declaraciones de funciones se ejecutan primero, antes de que se hagan llamadas.

#### 11.8.2.2 Una trampa de la activación temprana

Si dependes de la activación temprana para llamar a una función antes de su declaración, entonces debes tener cuidado de que no acceda a datos que no se activan temprano.

```javascript
funcDecl();

const MY_STR = "abc";
function funcDecl() {
  assert.throws(() => MY_STR, ReferenceError);
}
```

El problema desaparece si haces la llamada a `funcDecl()` después de la declaración de `MY_STR`.

#### 11.8.2.3 Los pros y contras de la activación temprana

Hemos visto que la activación temprana tiene una trampa y que puedes obtener la mayoría de sus beneficios sin usarla. Por lo tanto, es mejor evitar la activación temprana. Pero no tengo una opinión fuerte sobre esto y, como mencioné antes, a menudo uso declaraciones de funciones porque me gusta su sintaxis.

### 11.8.3 Las declaraciones de clases no se activan temprano

Aunque son similares a las declaraciones de funciones en algunos aspectos, las declaraciones de clases no se activan temprano:

```javascript
assert.throws(() => new MyClass(), ReferenceError);

class MyClass {}

assert.equal(new MyClass() instanceof MyClass, true);
```

¿Por qué es eso? Considera la siguiente declaración de clase:

```javascript
class MyClass extends Object {}
```

El operando de `extends` es una expresión. Por lo tanto, puedes hacer cosas como esta:

```javascript
const identity = (x) => x;
class MyClass extends identity(Object) {}
```

Evaluar tal expresión debe hacerse en la ubicación donde se menciona. Cualquier otra cosa sería confusa. Eso explica por qué las declaraciones de clases no se activan temprano.

### 11.8.4 var: elevación (activación temprana parcial)

`var` es una forma más antigua de declarar variables que precede a `const` y `let` (que ahora son preferidas). Considera la siguiente declaración `var`.

```javascript
var x = 123;
```

Esta declaración tiene dos partes:

- Declaración `var x`: El ámbito de una variable declarada con `var` es la función circundante más interna y no el bloque circundante más interno, como ocurre con la mayoría de las otras declaraciones. Tal variable ya está activa al comienzo de su ámbito e inicializada con `undefined`.
- Asignación `x = 123`: La asignación siempre se ejecuta en su lugar.

El siguiente código demuestra los efectos de `var`:

```javascript
function f() {
  // Activación temprana parcial:
  assert.equal(x, undefined);
  if (true) {
    var x = 123;
    // La asignación se ejecuta en su lugar:
    assert.equal(x, 123);
  }
  // El ámbito es la función, no el bloque:
  assert.equal(x, 123);
}
```

### 11.9 Closures

Antes de que podamos explorar las closures, necesitamos aprender sobre variables ligadas y variables libres.

#### 11.9.1 Variables ligadas vs. variables libres

Por ámbito, hay un conjunto de variables que se mencionan. Entre estas variables distinguimos:

- Variables ligadas: se declaran dentro del ámbito. Son parámetros y variables locales.
- Variables libres: se declaran externamente. También se llaman variables no locales.

Considera el siguiente código:

```javascript
function func(x) {
  const y = 123;
  console.log(z);
}
```

En el cuerpo de `func()`, `x` y `y` son variables ligadas. `z` es una variable libre.

#### 11.9.2 ¿Qué es una closure?

¿Qué es una closure entonces?

Una closure es una función más una conexión a las variables que existen en su "lugar de nacimiento".

¿Cuál es el punto de mantener esta conexión? Proporciona los valores para las variables libres de la función, por ejemplo:

```javascript
function funcFactory(value) {
  return () => {
    return value;
  };
}

const func = funcFactory("abc");
assert.equal(func(), "abc"); // (A)
```

`funcFactory` devuelve una closure que se asigna a `func`. Debido a que `func` tiene la conexión a las variables en su lugar de nacimiento, aún puede acceder a la variable libre `value` cuando se llama en la línea A (aunque "escapó" de su ámbito).

Todas las funciones en JavaScript son closures

El alcance estático es compatible mediante closures en JavaScript. Por lo tanto, cada función es una closure.

#### 11.9.3 Ejemplo: una fábrica de incrementadores

La siguiente función devuelve incrementadores (un nombre que acabo de inventar). Un incrementador es una función que internamente almacena un número. Cuando se llama, actualiza ese número sumándole el argumento y devuelve el nuevo valor.

```javascript
function createInc(startValue) {
  return (step) => {
    // (A)
    startValue += step;
    return startValue;
  };
}
const inc = createInc(5);
assert.equal(inc(2), 7);
```

Podemos ver que la función creada en la línea A mantiene su número interno en la variable libre `startValue`. Esta vez, no solo leemos del ámbito de nacimiento, lo usamos para almacenar datos que cambiamos y que persisten a través de las llamadas a funciones.

Podemos crear más espacios de almacenamiento en el ámbito de nacimiento, a través de variables locales:

```javascript
function createInc(startValue) {
  let index = -1;
  return (step) => {
    startValue += step;
    index++;
    return [index, startValue];
  };
}
const inc = createInc(5);
assert.deepEqual(inc(2), [0, 7]);
assert.deepEqual(inc(2), [1, 9]);
assert.deepEqual(inc(2), [2, 11]);
```

#### 11.9.4 Casos de uso de las closures

¿Para qué sirven las closures?

Para empezar, son simplemente una implementación del alcance estático. Como tal, proporcionan datos de contexto para callbacks.

También pueden ser utilizadas por funciones para almacenar estado que persiste a través de las llamadas a funciones. `createInc()` es un ejemplo de eso.

Y pueden proporcionar datos privados para objetos (producidos mediante literales o clases). Los detalles de cómo funciona eso se explican en Exploring ES6.

#### Quiz: avanzado

Consulta la aplicación de cuestionarios.
