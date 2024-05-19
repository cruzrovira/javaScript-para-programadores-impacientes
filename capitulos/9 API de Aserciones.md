### 9 API de Aserciones

#### 9.1 Aserciones en el desarrollo de software

En el desarrollo de software, las aserciones declaran hechos sobre valores o partes del código que deben ser verdaderos. Si no lo son, se lanza una excepción. Node.js admite aserciones a través de su módulo incorporado `assert`. Por ejemplo:

```javascript
import * as assert from "assert/strict";
assert.equal(3 + 5, 8);
```

Esta aserción establece que el resultado esperado de 3 más 5 es 8. La declaración `import` utiliza la versión recomendada `strict` de `assert`.

#### 9.2 Cómo se usan las aserciones en este libro

En este libro, las aserciones se usan de dos maneras: para documentar resultados en ejemplos de código y para implementar ejercicios guiados por pruebas.

##### 9.2.1 Documentar resultados en ejemplos de código mediante aserciones

En los ejemplos de código, las aserciones expresan resultados esperados. Tomemos como ejemplo la siguiente función:

```javascript
function id(x) {
  return x;
}
```

`id()` devuelve su parámetro. Podemos mostrarlo en acción mediante una aserción:

```javascript
assert.equal(id("abc"), "abc");
```

En los ejemplos, normalmente omito la declaración para importar `assert`.

La motivación detrás del uso de aserciones es:

- Puedes especificar con precisión lo que se espera.
- Los ejemplos de código se pueden probar automáticamente, lo que garantiza que realmente funcionen.

##### 9.2.2 Implementación de ejercicios guiados por pruebas mediante aserciones

Los ejercicios de este libro se basan en pruebas, a través del marco de pruebas Mocha. Las comprobaciones dentro de las pruebas se realizan mediante métodos de `assert`.

El siguiente es un ejemplo de una prueba de este tipo:

```javascript
// Para el ejercicio, debes implementar la función hello().
// La prueba verifica si lo has hecho correctamente.
test("Primer ejercicio", () => {
  assert.equal(hello("world"), "Hello world!");
  assert.equal(hello("Jane"), "Hello Jane!");
  assert.equal(hello("John"), "Hello John!");
  assert.equal(hello(""), "Hello !");
});
```

Para obtener más información, consulta §10 "Comenzando con cuestionarios y ejercicios".

#### 9.3 Comparación normal vs. comparación profunda

La función `equal()` usa `===` para comparar valores. Por lo tanto, un objeto solo es igual a sí mismo, incluso si otro objeto tiene el mismo contenido (porque `===` no compara los contenidos de los objetos, solo sus identidades):

```javascript
assert.notEqual({ foo: 1 }, { foo: 1 });
```

`deepEqual()` es una mejor opción para comparar objetos:

```javascript
assert.deepEqual({ foo: 1 }, { foo: 1 });
```

Este método también funciona para arrays:

```javascript
assert.notEqual(["a", "b", "c"], ["a", "b", "c"]);
assert.deepEqual(["a", "b", "c"], ["a", "b", "c"]);
```

#### 9.4 Referencia rápida: módulo `assert`

Para la documentación completa, consulta los documentos de Node.js.

##### 9.4.1 Igualdad normal

```javascript
function equal(actual: any, expected: any, message?: string): void
```

`actual === expected` debe ser verdadero. Si no, se lanza un `AssertionError`.

```javascript
assert.equal(3 + 3, 6);
```

```javascript
function notEqual(actual: any, expected: any, message?: string): void
```

`actual !== expected` debe ser verdadero. Si no, se lanza un `AssertionError`.

```javascript
assert.notEqual(3 + 3, 22);
```

El último parámetro opcional `message` se puede usar para explicar lo que se afirma. Si la aserción falla, el mensaje se usa para configurar el `AssertionError` que se lanza.

```javascript
let e;
try {
  const x = 3;
  assert.equal(x, 8, "x debe ser igual a 8");
} catch (err) {
  assert.equal(
    String(err),
    "AssertionError [ERR_ASSERTION]: x debe ser igual a 8"
  );
}
```

##### 9.4.2 Igualdad profunda

```javascript
function deepEqual(actual: any, expected: any, message?: string): void
```

`actual` debe ser profundamente igual a `expected`. Si no, se lanza un `AssertionError`.

```javascript
assert.deepEqual([1, 2, 3], [1, 2, 3]);
assert.deepEqual([], []);
// Para .equal(), un objeto solo es igual a sí mismo:
assert.notEqual([], []);
```

```javascript
function notDeepEqual(actual: any, expected: any, message?: string): void
```

`actual` no debe ser profundamente igual a `expected`. Si lo es, se lanza un `AssertionError`.

```javascript
assert.notDeepEqual([1, 2, 3], [1, 2]);
```

##### 9.4.3 Esperando excepciones

Si esperas recibir una excepción, necesitas `throws()`: Esta función llama a su primer parámetro, el bloque de función, y solo tiene éxito si lanza una excepción. Los parámetros adicionales se pueden usar para especificar cómo debe ser esa excepción.

```javascript
function throws(block: Function, message?: string): void
```

```javascript
assert.throws(() => {
  null.prop;
});
```

```javascript
function throws(block: Function, error: Function, message?: string): void
```

```javascript
assert.throws(() => {
  null.prop;
}, TypeError);
```

```javascript
function throws(block: Function, error: RegExp, message?: string): void
```

```javascript
assert.throws(() => {
  null.prop;
}, /^TypeError: Cannot read properties of null \(reading 'prop'\)$/);
```

```javascript
function throws(block: Function, error: Object, message?: string): void
```

```javascript
assert.throws(
  () => {
    null.prop;
  },
  {
    name: "TypeError",
    message: "Cannot read properties of null (reading 'prop')",
  }
);
```

##### 9.4.4 Otra función útil

```javascript
function fail(message: string | Error): never
```

Siempre lanza un `AssertionError` cuando se llama. Eso es ocasionalmente útil para pruebas unitarias.

```javascript
try {
  functionThatShouldThrow();
  assert.fail();
} catch (_) {
  // Éxito
}
```

Quiz
Consulta la aplicación de cuestionarios.
