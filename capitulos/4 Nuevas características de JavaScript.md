# 4 Nuevas características de JavaScript

## 4.1 Nuevo en ECMAScript 2022

ES2022 probablemente se convertirá en un estándar en junio de 2022. Las siguientes propuestas han alcanzado la etapa 4 y están programadas para ser parte de ese estándar:

### Nuevos miembros de las clases

- **Propiedades (slots públicos)**: Ahora se pueden crear mediante:
  - Campos públicos de instancia
  - Campos públicos estáticos
- **Slots privados**: Son nuevos y se pueden crear mediante:
  - Campos privados (campos privados de instancia y campos privados estáticos)
  - Métodos privados y accesores (no estáticos y estáticos)
- **Bloques de inicialización estática**
- **Verificaciones de slots privados ("verificaciones ergonómicas de marca para campos privados")**: La siguiente expresión verifica si `obj` tiene un slot privado `#privateSlot`:
  ```javascript
  #privateSlot in obj;
  ```

### Await de nivel superior en módulos

- Ahora se puede usar `await` en los niveles superiores de los módulos sin necesidad de entrar en funciones o métodos asincrónicos.

### error.cause

- `Error` y sus subclases ahora permiten especificar qué error causó el actual:
  ```javascript
  new Error("Algo salió mal", { cause: otherError });
  ```

### Método .at() de valores indexables

- Permite leer un elemento en un índice dado (soporta índices negativos).
  ```javascript
  ["a", "b", "c"]
    .at(0) // 'a'
    [("a", "b", "c")].at(-1); // 'c'
  ```

### Índices de coincidencia de RegExp

- Si se añade una bandera a una expresión regular, usarla produce objetos de coincidencia que registran el inicio y el final de cada captura de grupo.

### Object.hasOwn()

- Proporciona una manera segura de verificar si un objeto `obj` tiene una propiedad propia con la clave `propKey`. A diferencia de `Object.prototype.hasOwnProperty`, funciona con todos los objetos.
  ```javascript
  Object.hasOwn(obj, propKey);
  ```

Pueden añadirse más características a ES2022. Si eso sucede, este libro se actualizará en consecuencia.

## 4.2 Nuevo en ECMAScript 2021

Las siguientes características se añadieron en ECMAScript 2021:

### String.prototype.replaceAll()

- Permite reemplazar todas las coincidencias de una expresión regular o una cadena:
  ```javascript
  "abbbaab".replaceAll("b", "x"); // 'axxxaax'
  ```

### Promise.any() y AggregateError

- `Promise.any()` devuelve una promesa que se cumple tan pronto como la primera promesa en un iterable de promesas se cumpla. Si solo hay rechazos, se ponen en un `AggregateError` que se convierte en el valor de rechazo.

### Operadores de asignación lógica

- `a ||= b`
- `a &&= b`
- `a ??= b`

### Guiones bajos (\_) como separadores

- Literales numéricos: `123_456.789_012`
- Literales bigint: `6_000_000_000_000_000_000_000_000n`

### WeakRefs

- Esta característica está fuera del alcance de este libro. Para más información, consulta su propuesta.

## 4.3 Nuevo en ECMAScript 2020

Las siguientes características se añadieron en ECMAScript 2020:

### Nuevas características de los módulos

- **Importaciones dinámicas vía `import()`**: La declaración `import` normal es estática: solo se puede usar en los niveles superiores de los módulos y su especificador de módulo es una cadena fija. `import()` cambia eso. Se puede usar en cualquier lugar (incluyendo declaraciones condicionales) y se puede calcular su argumento.

- `import.meta` contiene metadatos para el módulo actual. Su primera propiedad ampliamente soportada es `import.meta.url`, que contiene una cadena con la URL del archivo del módulo actual.

### Reexportación de espacios de nombres

- La siguiente expresión importa todas las exportaciones del módulo 'mod' en un objeto espacio de nombres `ns` y exporta ese objeto.
  ```javascript
  export * as ns from "mod";
  ```

### Encadenamiento opcional para accesos a propiedades y llamadas a métodos

- Un ejemplo de encadenamiento opcional es:
  ```javascript
  value.?prop
  ```
  Esta expresión evalúa a `undefined` si `value` es `undefined` o `null`. De lo contrario, se evalúa a `value.prop`. Esta característica es especialmente útil en cadenas de lecturas de propiedades cuando algunas de las propiedades pueden faltar.

### Operador de fusión de nulos (??)

- ```javascript
  value ?? defaultValue;
  ```
  Esta expresión es `defaultValue` si `value` es `undefined` o `null` y `value` de lo contrario. Este operador permite usar un valor predeterminado cuando algo falta.

### Bigints – enteros de precisión arbitraria

- Los Bigints son un nuevo tipo primitivo. Soporta números enteros que pueden ser arbitrariamente grandes (el almacenamiento para ellos crece según sea necesario).

### String.prototype.matchAll()

- Este método lanza un error si no se establece la bandera /g y devuelve un iterable con todos los objetos de coincidencia para una cadena dada.

### Promise.allSettled()

- Recibe un iterable de promesas. Devuelve una promesa que se cumple una vez que todas las promesas de entrada se hayan resuelto. El valor de cumplimiento es un array con un objeto por cada promesa de entrada, ya sea:
  ```javascript
  { status: 'fulfilled', value: «valor de cumplimiento» }
  { status: 'rejected', reason: «valor de rechazo» }
  ```

### globalThis

- Proporciona una manera de acceder al objeto global que funciona tanto en navegadores como en plataformas del lado del servidor como Node.js y Deno.

### Mecánica del for-in

- Esta característica está fuera del alcance de este libro. Para más información, consulta su propuesta.

## 4.4 Nuevo en ECMAScript 2019

Las siguientes características se añadieron en ECMAScript 2019:

### Método Array.prototype.flatMap()

- Funciona como `map()`, pero permite que la función de devolución devuelva arrays de cero o más valores en lugar de valores individuales. Los arrays devueltos se concatenan y se convierten en el resultado de `flatMap()`. Los casos de uso incluyen:
  - Filtrado y mapeo al mismo tiempo
  - Mapeo de valores de entrada únicos a múltiples valores de salida

### Método Array.prototype.flat()

- Convierte arrays anidados en arrays planos. Opcionalmente, se puede indicar a qué profundidad de anidamiento debe dejar de aplanar.

### Object.fromEntries()

- Crea un objeto a partir de un iterable sobre entradas. Cada entrada es un array de dos elementos con una clave de propiedad y un valor de propiedad.

### Métodos de cadena: String.prototype.trimStart() y String.prototype.trimEnd()

- Funcionan como `trim()`, pero eliminan espacios en blanco solo al inicio o solo al final de una cadena.

### Parámetro opcional en el catch

- Ahora se puede omitir el parámetro de una cláusula catch si no se utiliza.

### Symbol.prototype.description

- Es un accesor para leer la descripción de un símbolo. Anteriormente, la descripción se incluía en el resultado de `.toString()` pero no se podía acceder individualmente.

Estas nuevas características de ES2019 están fuera del alcance de este libro:

- Superconjunto de JSON: Ver publicación del blog 2ality.
- JSON.stringify() bien formado: Ver publicación del blog 2ality.
- Revisión de Function.prototype.toString(): Ver publicación del blog 2ality.

## 4.5 Nuevo en ECMAScript 2018

Las siguientes características se añadieron en ECMAScript 2018:

### Iteración asincrónica

- Es la versión asincrónica de la iteración síncrona. Está basada en promesas:
  - Con iterables síncronos, se puede acceder inmediatamente a cada elemento. Con iterables asincrónicos, se debe esperar antes de poder acceder a un elemento.
  - Con iterables síncronos, se usan bucles for-of. Con iterables asincrónicos, se usan bucles for-await-of.

### Expansión en literales de objeto

- Usando la expansión (...) dentro de un literal de objeto, se pueden copiar las propiedades de otro objeto en el actual. Un caso de uso es crear una copia superficial de un objeto `obj`:
  ```javascript
  const shallowCopy = { ...obj };
  ```

### Propiedades de resto (desestructuración)

- Al desestructurar un valor de objeto, ahora se puede usar la sintaxis de resto (...) para obtener todas las propiedades no mencionadas previamente en un objeto.
  ```javascript
  const { a, ...remaining } = { a: 1, b: 2, c: 3 };
  assert.deepEqual(remaining, { b: 2, c: 3 });
  ```

### Promise.prototype.finally()

- Está relacionado con la cláusula finally de una declaración try-catch-finally, de manera similar a cómo el método Promise `.then()` está relacionado con la cláusula try y `.catch()` está relacionado con la cláusula catch.

- En otras palabras: La devolución de llamada de `.finally()` se ejecuta independientemente de si una promesa
