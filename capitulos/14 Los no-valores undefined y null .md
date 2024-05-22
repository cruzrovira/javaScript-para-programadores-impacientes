### 14 Los no-valores undefined y null

#### 14.1 undefined vs. null

#### 14.2 Ocurrencias de undefined y null

##### 14.2.1 Ocurrencias de undefined

##### 14.2.2 Ocurrencias de null

#### 14.3 Comprobación de undefined o null

#### 14.4 El operador de coalescencia nula (??) para valores predeterminados [ES2020]

##### 14.4.1 Ejemplo: contando coincidencias

##### 14.4.2 Ejemplo: especificando un valor predeterminado para una propiedad

##### 14.4.3 Usando desestructuración para valores predeterminados

##### 14.4.4 Enfoque heredado: usando OR lógico (||) para valores predeterminados

##### 14.4.5 El operador de asignación de coalescencia nula (??=) [ES2021]

#### 14.5 undefined y null no tienen propiedades

#### 14.6 La historia de undefined y null

Muchos lenguajes de programación tienen un “no-valor” llamado null. Indica que una variable actualmente no apunta a un objeto, por ejemplo, cuando aún no se ha inicializado.

En contraste, JavaScript tiene dos de ellos: undefined y null.

#### 14.1 undefined vs. null

Ambos valores son muy similares y a menudo se utilizan indistintamente. Por lo tanto, su diferencia es sutil. El propio lenguaje hace la siguiente distinción:

- **undefined** significa “no inicializado” (por ejemplo, una variable) o “no existente” (por ejemplo, una propiedad de un objeto).
- **null** significa “la ausencia intencional de cualquier valor de objeto” (una cita de la especificación del lenguaje).

Los programadores pueden hacer la siguiente distinción:

- **undefined** es el no-valor utilizado por el lenguaje (cuando algo no está inicializado, etc.).
- **null** significa “desactivado explícitamente”. Es decir, ayuda a implementar un tipo que comprende tanto valores significativos como un meta-valor que representa “ningún valor significativo”. Tal tipo se llama tipo de opción o tipo maybe en la programación funcional.

#### 14.2 Ocurrencias de undefined y null

Las siguientes subsecciones describen dónde aparecen undefined y null en el lenguaje. Encontraremos varios mecanismos que se explican con más detalle más adelante en este libro.

##### 14.2.1 Ocurrencias de undefined

Variable no inicializada myVar:

```javascript
let myVar;
assert.equal(myVar, undefined);
```

Parámetro x no proporcionado:

```javascript
function func(x) {
  return x;
}
assert.equal(func(), undefined);
```

Falta la propiedad .unknownProp:

```javascript
const obj = {};
assert.equal(obj.unknownProp, undefined);
```

Si no especificamos explícitamente el resultado de una función mediante una declaración return, JavaScript devuelve undefined por nosotros:

```javascript
function func() {}
assert.equal(func(), undefined);
```

##### 14.2.2 Ocurrencias de null

El prototipo de un objeto es o bien un objeto o, al final de una cadena de prototipos, null. Object.prototype no tiene un prototipo:

```javascript
> Object.getPrototypeOf(Object.prototype)
null
```

Si coincidimos una expresión regular (como /a/) con una cadena (como 'x'), obtenemos un objeto con datos coincidentes (si la coincidencia fue exitosa) o null (si la coincidencia falló):

```javascript
> /a/.exec('x')
null
```

El formato de datos JSON no soporta undefined, solo null:

```javascript
> JSON.stringify({a: undefined, b: null})
'{"b":null}'
```

#### 14.3 Comprobación de undefined o null

Comprobación de cualquiera de los dos:

```javascript
if (x === null) ···
if (x === undefined) ···
```

¿Tiene x un valor?

```javascript
if (x !== undefined && x !== null) {
  // ···
}
if (x) {
  // ¿verdadero?
  // x no es: undefined, null, false, 0, NaN, ''
}
```

¿Es x undefined o null?

```javascript
if (x === undefined || x === null) {
  // ···
}
if (!x) {
  // ¿falso?
  // x es: undefined, null, false, 0, NaN, ''
}
```

Verdadero significa “es verdadero si se convierte a booleano”. Falso significa “es falso si se convierte a booleano”. Ambos conceptos se explican adecuadamente en §15.2 “Valores verdaderos y falsos”.

#### 14.4 El operador de coalescencia nula (??) para valores predeterminados [ES2020]

A veces recibimos un valor y solo queremos usarlo si no es null o undefined. De lo contrario, nos gustaría usar un valor predeterminado, como alternativa. Podemos hacerlo a través del operador de coalescencia nula (??):

```javascript
const valueToUse = receivedValue ?? defaultValue;
```

Las siguientes dos expresiones son equivalentes:

```javascript
a ?? b;
a !== undefined && a !== null ? a : b;
```

##### 14.4.1 Ejemplo: contando coincidencias

El siguiente código muestra un ejemplo del mundo real:

```javascript
function countMatches(regex, str) {
  const matchResult = str.match(regex); // null o Array
  return (matchResult ?? []).length;
}

assert.equal(countMatches(/a/g, "ababa"), 3);
assert.equal(countMatches(/b/g, "ababa"), 2);
assert.equal(countMatches(/x/g, "ababa"), 0);
```

Si hay una o más coincidencias para regex dentro de str, entonces .match() devuelve un Array. Si no hay coincidencias, desafortunadamente devuelve null (y no el Array vacío). Solucionamos eso con el operador ??.

También podríamos haber usado encadenamiento opcional:

```javascript
return matchResult?.length ?? 0;
```

##### 14.4.2 Ejemplo: especificando un valor predeterminado para una propiedad

```javascript
function getTitle(fileDesc) {
  return fileDesc.title ?? "(Untitled)";
}

const files = [{ path: "index.html", title: "Home" }, { path: "tmp.html" }];
assert.deepEqual(
  files.map((f) => getTitle(f)),
  ["Home", "(Untitled)"]
);
```

##### 14.4.3 Usando desestructuración para valores predeterminados

En algunos casos, la desestructuración también se puede usar para valores predeterminados, por ejemplo:

```javascript
function getTitle(fileDesc) {
  const { title = "(Untitled)" } = fileDesc;
  return title;
}
```

##### 14.4.4 Enfoque heredado: usando OR lógico (||) para valores predeterminados

Antes de ECMAScript 2020 y el operador de coalescencia nula, OR lógico se usaba para valores predeterminados. Eso tiene una desventaja.

|| funciona como se espera para undefined y null:

```javascript
> undefined || 'default'
'default'
> null || 'default'
'default'
```

Pero también devuelve el valor predeterminado para todos los demás valores falsos, por ejemplo:

```javascript
> false || 'default'
'default'
> 0 || 'default'
'default'
> 0n || 'default'
'default'
> '' || 'default'
'default'
```

Comparar eso con cómo funciona ??:

```javascript
> undefined ?? 'default'
'default'
> null ?? 'default'
'default'

> false ?? 'default'
false
> 0 ?? 'default'
0
> 0n ?? 'default'
0n
> '' ?? 'default'
''
```

##### 14.4.5 El operador de asignación de coalescencia nula (??=) [ES2021]

??= es un operador de asignación lógica. Las siguientes dos expresiones son aproximadamente equivalentes:

```javascript
a ??= b;
a ?? (a = b);
```

Eso significa que ??= es de cortocircuito: la asignación solo se realiza si a es undefined o null.

##### 14.4.5.1 Ejemplo: usando ??= para agregar propiedades faltantes

```javascript
const books = [
  {
    isbn: "123",
  },
  {
    title: "ECMAScript Language Specification",
    isbn: "456",
  },
];

// Agregar propiedad .title donde falte
for (const book of books) {
  book.title ??= "(Untitled)";
}

assert.deepEqual(books, [
  {
    isbn: "123",
    title: "(Untitled)",
  },
  {
    title: "ECMAScript Language Specification",
    isbn: "456",
  },
]);
```

### 14.5 undefined y null no tienen propiedades

undefined y null son los únicos dos valores de JavaScript donde obtenemos una excepción si intentamos leer una propiedad. Para explorar este fenómeno, usemos la siguiente función, que lee (obtiene) la propiedad .foo y devuelve el resultado.

```javascript
function getFoo(x) {
  return x.foo;
}
```

Si aplicamos getFoo() a varios valores, podemos ver que solo falla para undefined y null:

```javascript
> getFoo(undefined)
TypeError: Cannot read properties of undefined (reading 'foo')
> getFoo(null)
TypeError: Cannot read properties of null (reading 'foo')

> getFoo(true)
undefined
> getFoo({})
undefined
```

### 14.6 La historia de undefined y null

En Java (que inspiró muchos aspectos de JavaScript), los valores de inicialización dependen del tipo estático de una variable:

- Las variables con tipos de objetos se inicializan con null.
- Cada tipo primitivo tiene su propio valor de inicialización. Por ejemplo, las variables int se inicializan con 0.

En JavaScript, cada variable puede contener tanto valores de objeto como valores primitivos. Por lo tanto, si null significa “no un objeto”, JavaScript también necesita un valor de inicialización que signifique “ni un objeto ni un valor primitivo”. Ese valor de inicialización es undefined.

**Quiz**

Consulta la aplicación de cuestionarios.
