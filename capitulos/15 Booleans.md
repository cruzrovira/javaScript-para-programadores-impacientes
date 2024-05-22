### 15 Booleans

#### 15.1 Conversión a booleano

#### 15.2 Valores falsy y truthy

##### 15.2.1 Comprobación de valores truthy o falsy

#### 15.3 Comprobaciones de existencia basadas en truthiness

##### 15.3.1 Problema: las comprobaciones de existencia basadas en truthiness son imprecisas

##### 15.3.2 Caso de uso: ¿se proporcionó un parámetro?

##### 15.3.3 Caso de uso: ¿existe una propiedad?

#### 15.4 Operador condicional (? :)

#### 15.5 Operadores lógicos binarios: And (x && y), Or (x || y)

##### 15.5.1 Preservación de valor

##### 15.5.2 Evaluación corta (short-circuiting)

##### 15.5.3 And lógico (x && y)

##### 15.5.4 Or lógico (||)

#### 15.6 Negación lógica (!)

El tipo primitivo boolean comprende dos valores: false y true:

```javascript
> typeof false
'boolean'
> typeof true
'boolean'
```

### 15.1 Conversión a booleano

#### El significado de “convertir a [tipo]”

“Convertir a [tipo]” es una abreviatura de “Convertir valores arbitrarios a valores del tipo [tipo]”.

Estas son tres formas en que puedes convertir un valor arbitrario x a un booleano:

- **Boolean(x)**  
  Más descriptivo; recomendado.
- **x ? true : false**  
  Utiliza el operador condicional (explicado más adelante en este capítulo).
- **!!x**  
  Utiliza el operador de negación lógica (!). Este operador convierte su operando a booleano. Se aplica una segunda vez para obtener un resultado no negado.

Tabla 4 describe cómo varios valores se convierten a booleanos.

**Tabla 4: Conversión de valores a booleanos.**
| x | Boolean(x) |
|--------------|------------|
| undefined | false |
| null | false |
| boolean | x (sin cambio) |
| number | 0 → false, NaN → false, Otros números → true |
| bigint | 0n → false, Otros números → true |
| string | '' → false, Otras cadenas → true |
| symbol | true |
| object | Siempre true |

### 15.2 Valores falsy y truthy

Al comprobar la condición de una instrucción if, un bucle while o un bucle do-while, JavaScript funciona de manera diferente a lo que podrías esperar. Por ejemplo, considera la siguiente condición:

```javascript
if (value) {
}
```

En muchos lenguajes de programación, esta condición es equivalente a:

```javascript
if (value === true) {
}
```

Sin embargo, en JavaScript, es equivalente a:

```javascript
if (Boolean(value) === true) {
}
```

Es decir, JavaScript verifica si value es verdadero cuando se convierte a booleano. Este tipo de comprobación es tan común que se introdujeron los siguientes nombres:

- Un valor se llama **truthy** si es verdadero cuando se convierte a booleano.
- Un valor se llama **falsy** si es falso cuando se convierte a booleano.

Cada valor es truthy o falsy. Consultando la tabla 4, podemos hacer una lista exhaustiva de valores falsy:

- undefined
- null
- Boolean: false
- Números: 0, NaN
- Bigint: 0n
- String: ''

Todos los demás valores (incluidos todos los objetos) son truthy:

```javascript
> Boolean('abc')
true
> Boolean([])
true
> Boolean({})
true
```

#### 15.2.1 Comprobación de valores truthy o falsy

```javascript
if (x) {
  // x es truthy
}

if (!x) {
  // x es falsy
}

if (x) {
  // x es truthy
} else {
  // x es falsy
}

const result = x ? "truthy" : "falsy";
```

El operador condicional que se usa en la última línea se explica más adelante en este capítulo.

**Ejercicio: Truthiness**

exercises/booleans/truthiness_exrc.mjs

### 15.3 Comprobaciones de existencia basadas en truthiness

En JavaScript, si lees algo que no existe (por ejemplo, un parámetro faltante o una propiedad faltante), usualmente obtienes undefined como resultado. En estos casos, una comprobación de existencia equivale a comparar un valor con undefined. Por ejemplo, el siguiente código verifica si el objeto obj tiene la propiedad .prop:

```javascript
if (obj.prop !== undefined) {
  // obj tiene la propiedad .prop
}
```

Debido a que undefined es falsy, podemos acortar esta verificación a:

```javascript
if (obj.prop) {
  // obj tiene la propiedad .prop
}
```

#### 15.3.1 Problema: las comprobaciones de existencia basadas en truthiness son imprecisas

Las comprobaciones de existencia basadas en truthiness tienen un problema: no son muy precisas. Considera el ejemplo anterior:

```javascript
if (obj.prop) {
  // obj tiene la propiedad .prop
}
```

El cuerpo de la instrucción if se omite si:

- obj.prop falta (en cuyo caso, JavaScript devuelve undefined).

Sin embargo, también se omite si:

- obj.prop es undefined.
- obj.prop es cualquier otro valor falsy (null, 0, '', etc.).

En la práctica, esto rara vez causa problemas, pero debes estar consciente de este problema.

#### 15.3.2 Caso de uso: ¿se proporcionó un parámetro?

Una comprobación de truthiness se usa a menudo para determinar si el llamador de una función proporcionó un parámetro:

```javascript
function func(x) {
  if (!x) {
    throw new Error("Missing parameter x");
  }
  // ···
}
```

Por un lado, este patrón es establecido y corto. Lanza correctamente errores para undefined y null.

Por otro lado, tiene el problema mencionado anteriormente: el código también lanza errores para todos los demás valores falsy.

Una alternativa es verificar si es undefined:

```javascript
if (x === undefined) {
  throw new Error("Missing parameter x");
}
```

#### 15.3.3 Caso de uso: ¿existe una propiedad?

Las comprobaciones de truthiness también se usan a menudo para determinar si una propiedad existe:

```javascript
function readFile(fileDesc) {
  if (!fileDesc.path) {
    throw new Error("Missing property: .path");
  }
  // ···
}
readFile({ path: "foo.txt" }); // sin error
```

Este patrón también está establecido y tiene la advertencia habitual: no solo lanza errores si falta la propiedad, sino también si existe y tiene cualquiera de los valores falsy.

Si realmente quieres comprobar si la propiedad existe, tienes que usar el operador in:

```javascript
if (!("path" in fileDesc)) {
  throw new Error("Missing property: .path");
}
```

### 15.4 Operador condicional (? :)

El operador condicional es la versión en expresión de la instrucción if. Su sintaxis es:

```javascript
«condition» ? «thenExpression» : «elseExpression»
```

Se evalúa de la siguiente manera:

- Si condition es truthy, evalúa y devuelve thenExpression.
- De lo contrario, evalúa y devuelve elseExpression.

El operador condicional también se llama operador ternario porque tiene tres operandos.

Ejemplos:

```javascript
> true ? 'yes' : 'no'
'yes'
> false ? 'yes' : 'no'
'no'
> '' ? 'yes' : 'no'
'no'
```

El siguiente código demuestra que, independientemente de la rama “then” u “else” elegida mediante la condición, solo se evalúa esa rama. La otra rama no se evalúa.

```javascript
const x = true ? console.log("then") : console.log("else");

// Salida:
// 'then'
```

### 15.5 Operadores lógicos binarios: And (x && y), Or (x || y)

Los operadores lógicos binarios && y || preservan el valor y tienen evaluación corta (short-circuiting).

#### 15.5.1 Preservación de valor

La preservación de valor significa que los operandos se interpretan como booleanos, pero se devuelven sin cambios:

```javascript
> 12 || 'hello'
12
> 0 || 'hello'
'hello'
```

#### 15.5.2 Evaluación corta (short-circuiting)

La evaluación corta significa que si el primer operando ya determina el resultado, entonces el segundo operando no se evalúa. El único otro operador que retrasa la evaluación de sus operandos es el operador condicional. Usualmente, todos los operandos se evalúan antes de realizar una operación.

Por ejemplo, And lógico (&&) no evalúa su segundo operando si el primero es falsy:

```javascript
const x = false && console.log("hello");
// Sin salida
```

Si el primer operando es truthy, se ejecuta console.log():

```javascript
const x = true && console.log("hello");

// Salida:
// 'hello'
```

### 15.5.3 And lógico (x && y)

La expresión a && b (“a And b”) se evalúa de la siguiente manera:

1. Evalúa a.
2. ¿El resultado es falsy? Devuélvelo.
3. De lo contrario, evalúa b y devuelve el resultado.

En otras palabras, las siguientes dos expresiones son aproximadamente equivalentes:

```javascript
a && b;
!a ? a : b;
```

Ejemplos:

```javascript
> false && true
false
> false && 'abc'
false

> true && false
false
> true && 'abc'
'abc'

> '' && 'abc'
''
```

### 15.5.4 Or lógico (||)

La expresión a || b (“a Or b”) se evalúa de la siguiente manera:

1. Evalúa a.
2. ¿El resultado es truthy? Devuélvelo.
3. De lo contrario, evalúa b y devuelve el resultado.

En otras palabras, las siguientes dos expresiones son aproximadamente equivalentes:

```javascript
a || b;
a ? a : b;
```

Ejemplos:

```javascript
> true || false
true
> true || 'abc'
true

> false || true
true
> false || 'abc'
'abc'

> 'abc' || 'def'
'abc'
```

#### 15.5.4.1 Caso de uso legado para Or lógico (||): proporcionar valores predeterminados

ECMAScript 2020 introdujo el operador de coalescencia nula (??) para valores predeterminados. Antes de eso, se usaba el operador Or lógico para este propósito:

```javascript
const valueToUse = receivedValue || defaultValue;
```

Consulta §14.4 “El operador de coalescencia nula (??) para valores predeterminados [ES2020]” para obtener más información sobre ?? y las desventajas de || en este caso.

**Ejercicio legado: Valores predeterminados mediante el operador Or (||)**

exercises/booleans/default_via_or_exrc.mjs

### 15.6 Negación lógica (!)

La expresión !x (“Not x”) se evalúa de la siguiente manera:

1. Evalúa x.
2. ¿Es truthy? Devuelve false.
3. De lo contrario, devuelve true.

Ejemplos:

```javascript
> !false
true
> !true
false

> !0
true
> !123
false

> !''
true
> !'abc'
false
```

**Quiz**

Consulta la aplicación de cuestionarios.
