### 13 Operadores

#### 13.1 Entender los operadores

##### 13.1.1 Los operadores convierten sus operandos a tipos apropiados

##### 13.1.2 La mayoría de los operadores solo funcionan con valores primitivos

#### 13.2 El operador más (+)

#### 13.3 Operadores de asignación

##### 13.3.1 El operador de asignación simple

##### 13.3.2 Operadores de asignación compuesta

#### 13.4 Igualdad: == vs. ===

##### 13.4.1 Igualdad suelta (== y !=)

##### 13.4.2 Igualdad estricta (=== y !==)

##### 13.4.3 Recomendación: usa siempre igualdad estricta

##### 13.4.4 Incluso más estricta que ===: Object.is()

#### 13.5 Operadores de orden

#### 13.6 Varios otros operadores

##### 13.6.1 Operador coma

##### 13.6.2 Operador void

### 13.1 Entender los operadores

Los operadores de JavaScript pueden parecer peculiares. Con las siguientes dos reglas, son más fáciles de entender:

1. Los operadores convierten sus operandos a tipos apropiados
2. La mayoría de los operadores solo funcionan con valores primitivos

#### 13.1.1 Los operadores convierten sus operandos a tipos apropiados

Si un operador recibe operandos que no tienen los tipos adecuados, rara vez arroja una excepción. En su lugar, convierte (automáticamente) los operandos para poder trabajar con ellos. Veamos dos ejemplos.

Primero, el operador de multiplicación solo puede trabajar con números. Por lo tanto, convierte las cadenas a números antes de calcular su resultado.

```javascript
> '7' * '3'
21
```

En segundo lugar, el operador de corchetes ([ ]) para acceder a las propiedades de un objeto solo puede manejar cadenas y símbolos. Todos los demás valores se convierten a cadena:

```javascript
const obj = {};
obj["true"] = 123;

// Convierte true a la cadena 'true'
assert.equal(obj[true], 123);
```

#### 13.1.2 La mayoría de los operadores solo funcionan con valores primitivos

Como se mencionó antes, la mayoría de los operadores solo funcionan con valores primitivos. Si un operando es un objeto, generalmente se convierte a un valor primitivo – por ejemplo:

```javascript
> [1,2,3] + [4,5,6]
'1,2,34,5,6'
```

¿Por qué? El operador más primero convierte sus operandos a valores primitivos:

```javascript
> String([1,2,3])
'1,2,3'
> String([4,5,6])
'4,5,6'
```

Luego, concatena las dos cadenas:

```javascript
> '1,2,3' + '4,5,6'
'1,2,34,5,6'
```

### 13.2 El operador más (+)

El operador más funciona de la siguiente manera en JavaScript:

Primero, convierte ambos operandos a valores primitivos. Luego cambia a uno de dos modos:

- Modo cadena: Si uno de los dos valores primitivos es una cadena, entonces convierte el otro a cadena, concatena ambas cadenas y devuelve el resultado.
- Modo número: De lo contrario, convierte ambos operandos a números, los suma y devuelve el resultado.

El modo cadena nos permite usar + para ensamblar cadenas:

```javascript
> 'There are ' + 3 + ' items'
'There are 3 items'
```

El modo número significa que si ninguno de los operandos es una cadena (o un objeto que se convierte en cadena), entonces todo se convierte a números:

```javascript
> 4 + true
5
```

Number(true) es 1.

### 13.3 Operadores de asignación

#### 13.3.1 El operador de asignación simple

El operador de asignación simple se utiliza para cambiar ubicaciones de almacenamiento:

```javascript
x = value; // asignar a una variable previamente declarada
obj.propKey = value; // asignar a una propiedad
arr[index] = value; // asignar a un elemento de un Array
```

Los inicializadores en las declaraciones de variables también pueden verse como una forma de asignación:

```javascript
const x = value;
let y = value;
```

#### 13.3.2 Operadores de asignación compuesta

JavaScript admite los siguientes operadores de asignación:

- Operadores de asignación aritmética: += -= \*= /= %= [ES1]
- += también se puede usar para concatenación de cadenas
- Introducido más tarde: \*\*= [ES2016]
- Operadores de asignación bit a bit: &= ^= |= [ES1]
- Operadores de asignación de desplazamiento bit a bit: <<= >>= >>>= [ES1]
- Operadores de asignación lógica: ||= &&= ??= [ES2021]

#### 13.3.2.1 Operadores de asignación lógica [ES2021]

Los operadores de asignación lógica funcionan de manera diferente a otros operadores de asignación compuesta:

| Operador de asignación | Equivalente a | Solo asigna si a es |
| ---------------------- | ------------- | ------------------- | --- | --- | ------- | ----- |
| a                      |               | = b                 | a   |     | (a = b) | Falso |
| a &&= b                | a && (a = b)  | Verdadero           |
| a ??= b                | a ?? (a = b)  | Nulo o indefinido   |

¿Por qué a ||= b es equivalente a la siguiente expresión?

```javascript
a || (a = b);
```

¿Por qué no a esta expresión?

```javascript
a = a || b;
```

La primera expresión tiene el beneficio de la evaluación de cortocircuito: La asignación solo se evalúa si a es falso. Por lo tanto, la asignación solo se realiza si es necesario. En contraste, la segunda expresión siempre realiza una asignación.

Para más información sobre ??=, consulta §14.4.5 "El operador de asignación de fusión nula (??=) [ES2021]".

#### 13.3.2.2 Los operadores de asignación compuesta restantes

Para operadores op distintos de || && ??, las siguientes dos formas de asignar son equivalentes:

```javascript
myvar op= value
myvar = myvar op value
```

Si, por ejemplo, op es +, entonces obtenemos el operador += que funciona de la siguiente manera:

```javascript
let str = "";
str += "<b>";
str += "Hello!";
str += "</b>";

assert.equal(str, "<b>Hello!</b>");
```

### 13.4 Igualdad: == vs. ===

JavaScript tiene dos tipos de operadores de igualdad: igualdad suelta (==) e igualdad estricta (===). La recomendación es usar siempre el último.

Otros nombres para == y ===

- == también se llama doble igual. Su nombre oficial en la especificación del lenguaje es comparación de igualdad abstracta.
- === también se llama triple igual.

#### 13.4.1 Igualdad suelta (== y !=)

La igualdad suelta es una de las peculiaridades de JavaScript. A menudo convierte operandos. Algunas de esas conversiones tienen sentido:

```javascript
> '123' == 123
true
> false == 0
true
```

Otras menos:

```javascript
> '' == 0
true
```

Los objetos se convierten a primitivos si (¡y solo si!) el otro operando es primitivo:

```javascript
> [1, 2, 3] == '1,2,3'
true
> ['1', '2', '3'] == '1,2,3'
true
```

Si ambos operandos son objetos, solo son iguales si son el mismo objeto:

```javascript
> [1, 2, 3] == ['1', '2', '3']
false
> [1, 2, 3] == [1, 2, 3]
false

> const arr = [1, 2, 3];
> arr == arr
true
```

Por último, == considera undefined y null como iguales:

```javascript
> undefined == null
true
```

#### 13.4.2 Igualdad estricta (=== y !==)

La igualdad estricta nunca convierte. Dos valores solo son iguales si tienen el mismo tipo. Volvamos a nuestra interacción anterior con el operador == y veamos qué hace el operador ===:

```javascript
> false === 0
false
> '123' === 123
false
```

Un objeto solo es igual a otro valor si ese valor es el mismo objeto:

```javascript
> [1, 2, 3] === '1,2,3'
false
> ['1', '2', '3'] === '1,2,3'
false

> [1, 2, 3] === ['1, 2, 3']
false
> [1, 2, 3] === [1, 2, 3]
false

> const arr = [1, 2, 3];
> arr === arr
true
```

El operador === no considera undefined y null como iguales:

```javascript
> undefined === null
false
```

### 13.4.3 Recomendación: usa siempre igualdad estricta

Recomiendo usar siempre ===. Hace que tu código sea más fácil de entender y te evita tener que pensar en las peculiaridades de ==.

Veamos dos casos de uso para == y lo que recomiendo hacer en su lugar.

#### 13.4.3.1 Caso de uso para ==: comparar con un número o una cadena

== te permite verificar si un valor x es un número o ese número como una cadena, con una sola comparación:

```javascript
if (x == 123) {
  // x es 123 o '123'
}
```

Prefiero cualquiera de las siguientes dos alternativas:

```javascript
if (x === 123 || x === '123') ···
if (Number(x) === 123) ···
```

También puedes convertir x a un número cuando lo encuentres por primera vez.

#### 13.4.3.2 Caso de uso para ==: comparar con undefined o null

Otro caso de uso para == es verificar si un valor x es undefined o null:

```javascript
if (x == null) {
  // x es null o undefined
}
```

El problema con este código es que no puedes estar seguro de si alguien quiso escribirlo de esa manera o si cometió un error tipográfico y quiso decir === null.

Prefiero cualquiera de las siguientes dos alternativas:

```javascript
if (x === undefined || x === null) ···
if (!x) ···
```

Una desventaja de la segunda alternativa es que acepta valores distintos a undefined y null, pero es un patrón bien establecido en JavaScript (explicado en detalle en §15.3 “Verificaciones de existencia basadas en la veracidad”).

Las siguientes tres condiciones también son aproximadamente equivalentes:

```javascript
if (x != null) ···
if (x !== undefined && x !== null) ···
if (x) ···
```

### 13.4.4 Incluso más estricta que ===: Object.is()

El método Object.is() compara dos valores:

```javascript
> Object.is(123, 123)
true
> Object.is(123, '123')
false
```

Es incluso más estricto que ===. Por ejemplo, considera que NaN, el valor de error para cálculos que involucran números, es igual a sí mismo:

```javascript
> Object.is(NaN, NaN)
true
> NaN === NaN
false
```

Eso es ocasionalmente útil. Por ejemplo, puedes usarlo para implementar una versión mejorada del método .indexOf() del Array:

```javascript
const myIndexOf = (arr, elem) => {
  return arr.findIndex((x) => Object.is(x, elem));
};
```

myIndexOf() encuentra NaN en un Array, mientras que .indexOf() no lo hace:

```javascript
> myIndexOf([0,NaN,2], NaN)
1
> [0,NaN,2].indexOf(NaN)
-1
```

El resultado -1 significa que .indexOf() no pudo encontrar su argumento en el Array.

### 13.5 Operadores de orden

Tabla 3: Operadores de orden de JavaScript.

| Operador | Nombre        |
| -------- | ------------- |
| <        | Menor que     |
| <=       | Menor o igual |
| >        | Mayor que     |
| >=       | Mayor o igual |

Los operadores de orden de JavaScript (tabla 3) funcionan tanto para números como para cadenas:

```javascript
> 5 >= 2
true
> 'bar' < 'foo'
true
```

<= y >= se basan en la igualdad estricta.

Los operadores de orden no funcionan bien para lenguajes humanos

Los operadores de orden no funcionan bien para comparar texto en un lenguaje humano, por ejemplo, cuando se trata de mayúsculas o acentos. Los detalles se explican en §20.6 “Comparación de cadenas”.

### 13.6 Varios otros operadores

Los siguientes operadores se cubren en otras partes de este libro:

- Operadores para booleanos, números, cadenas, objetos
- El operador de fusión nula (??) para valores predeterminados

Las dos subsecciones siguientes discuten dos operadores que rara vez se usan.

#### 13.6.1 Operador coma

El operador coma tiene dos operandos, evalúa ambos y devuelve el segundo:

```javascript
> 'a', 'b'
'b'
```

Para más información sobre este operador, consulta Speaking JavaScript.

#### 13.6.2 Operador void

El operador void evalúa su operando y devuelve undefined:

```javascript
> void (3 + 2)
undefined
```

Para más información sobre este operador, consulta Speaking JavaScript.

Quiz

Consulta la aplicación de quiz.
