### 16 Números

16.1 Los números se utilizan tanto para números de punto flotante como para enteros

16.2 Literales numéricos
16.2.1 Literales enteros
16.2.2 Literales de punto flotante
16.2.3 Trampa sintáctica: propiedades de los literales enteros
16.2.4 Guiones bajos (\_) como separadores en literales numéricos [ES2021]

16.3 Operadores aritméticos
16.3.1 Operadores aritméticos binarios
16.3.2 Más (+) y negación (-) unarios
16.3.3 Incremento (++) y decremento (--)

16.4 Convertir a número

16.5 Valores de error
16.5.1 Valor de error: NaN
16.5.2 Valor de error: Infinity

16.6 La precisión de los números: cuidado con las fracciones decimales

16.7 (Avanzado)

16.8 Antecedentes: precisión de punto flotante
16.8.1 Una representación simplificada de los números de punto flotante

16.9 Números enteros en JavaScript
16.9.1 Convertir a entero
16.9.2 Rangos de números enteros en JavaScript
16.9.3 Enteros seguros

16.10 Operadores a nivel de bits
16.10.1 Internamente, los operadores a nivel de bits trabajan con enteros de 32 bits
16.10.2 Not a nivel de bits
16.10.3 Operadores binarios a nivel de bits
16.10.4 Operadores de desplazamiento a nivel de bits
16.10.5 b32(): mostrar enteros sin signo de 32 bits en notación binaria

16.11 Referencia rápida: números
16.11.1 Funciones globales para números
16.11.2 Propiedades estáticas de Number
16.11.3 Métodos estáticos de Number
16.11.4 Métodos de Number.prototype
16.11.5 Fuentes

JavaScript tiene dos tipos de valores numéricos:

Los números son números de punto flotante de 64 bits y también se utilizan para enteros más pequeños (dentro de un rango de más/menos 53 bits).
Los bigints representan enteros con una precisión arbitraria.
Este capítulo cubre los números. Los bigints se tratan más adelante en este libro.

### 16.1 Los números se utilizan tanto para números de punto flotante como para enteros

El tipo número se utiliza tanto para enteros como para números de punto flotante en JavaScript:

```javascript
98;
123.45;
```

Sin embargo, todos los números son dobles, números de punto flotante de 64 bits implementados de acuerdo con el Estándar IEEE para Aritmética de Punto Flotante (IEEE 754).

Los números enteros son simplemente números de punto flotante sin una fracción decimal:

```javascript
> 98 === 98.0
true
```

Nota que, en el fondo, la mayoría de los motores de JavaScript a menudo pueden usar enteros reales, con todos los beneficios asociados de rendimiento y tamaño de almacenamiento.

### 16.2 Literales numéricos

Examinemos los literales de números.

#### 16.2.1 Literales enteros

Varios literales enteros nos permiten expresar enteros con varias bases:

```javascript
// Binario (base 2)
assert.equal(0b11, 3); // ES6

// Octal (base 8)
assert.equal(0o10, 8); // ES6

// Decimal (base 10)
assert.equal(35, 35);

// Hexadecimal (base 16)
assert.equal(0xe7, 231);
```

#### 16.2.2 Literales de punto flotante

Los números de punto flotante solo se pueden expresar en base 10.

Fracciones:

```javascript
> 35.0
35
```

Exponente: eN significa ×10^N

```javascript
> 3e2
300
> 3e-2
0.03
> 0.3e2
30
```

#### 16.2.3 Trampa sintáctica: propiedades de los literales enteros

Acceder a una propiedad de un literal entero conlleva una trampa: si el literal entero es seguido inmediatamente por un punto, entonces ese punto se interpreta como un punto decimal:

```javascript
7.toString(); // error de sintaxis
```

Hay cuatro formas de evitar esta trampa:

```javascript
(7.0).toString()(7).toString();
(7).toString();
(7).toString(); // espacio antes del punto
```

#### 16.2.4 Guiones bajos (\_) como separadores en literales numéricos [ES2021]

Agrupar dígitos para hacer números largos más legibles tiene una larga tradición. Por ejemplo:

En 1825, Londres tenía 1,335,000 habitantes.
La distancia entre la Tierra y el Sol es de 149,600,000 km.
Desde ES2021, podemos usar guiones bajos como separadores en literales numéricos:

```javascript
const habitantesDeLondres = 1_335_000;
const distanciaTierraSolEnKm = 149_600_000;
```

Con otras bases, el agrupamiento también es importante:

```javascript
const permisoDeSistemaDeArchivos = 0b111_111_000;
const bytes = 0b1111_10101011_11110000_00001101;
const palabras = 0xfab_f00d;
```

También podemos usar el separador en fracciones y exponentes:

```javascript
const masaDelElectronEnKg = 9.109_383_56e-31;
const billonEnEscalaCorta = 1e1_2;
```

#### 16.2.4.1 ¿Dónde podemos poner separadores?

Las ubicaciones de los separadores están restringidas de dos maneras:

Solo podemos poner guiones bajos entre dos dígitos. Por lo tanto, todos los siguientes literales numéricos son ilegales:

```javascript
3_.141
3._141

1_e12
1e_12

_1464301  // ¡nombre de variable válido!
1464301_

0_b111111000
0b_111111000
```

No podemos usar más de un guion bajo en fila:

```javascript
123__456 // dos guiones bajos – no permitido
```

La motivación detrás de estas restricciones es mantener el análisis simple y evitar casos límite extraños.

#### 16.2.4.2 Analizando números con separadores

Las siguientes funciones para analizar números no admiten separadores:

Number()
Number.parseInt()
Number.parseFloat()
Por ejemplo:

```javascript
> Number('123_456')
NaN
> Number.parseInt('123_456')
123
```

La razón es que los separadores numéricos son para el código. Otros tipos de entrada deben procesarse de manera diferente.

### 16.3 Operadores aritméticos

#### 16.3.1 Operadores aritméticos binarios

La tabla 5 enumera los operadores aritméticos binarios de JavaScript.

| Operador | Nombre         | Ejemplo     |
| -------- | -------------- | ----------- | ----------------- |
| n + m    | Suma           | ES1         | 3 + 4 → 7         |
| n - m    | Resta          | ES1         | 9 - 1 → 8         |
| n \* m   | Multiplicación | ES1         | 3 \* 2.25 → 6.75  |
| n / m    | División       | ES1         | 5.625 / 5 → 1.125 |
| n % m    | Resto          | ES1         | 8 % 5 → 3         |
|          |                | -8 % 5 → -3 |
| n \*\* m | Exponenciación | ES2016      | 4 \*\* 2 → 16     |

##### 16.3.1.1 % es un operador de resto

% es un operador de resto, no un operador de módulo. Su resultado tiene el signo del primer operando:

```javascript
> 5 % 3
2
> -5 % 3
-2
```

Para más información sobre la diferencia entre resto y módulo, consulte la publicación del blog "Resto vs. operador de módulo (con código JavaScript)" en 2ality.

#### 16.3.2 Más (+) y negación (-) unarios

La tabla 6 resume los dos operadores más unario (+) y negación (-).

| Operador | Nombre          | Ejemplo |
| -------- | --------------- | ------- | ---------- |
| +n       | Más unario      | ES1     | +(-7) → -7 |
| -n       | Negación unaria | ES1     | -(-7) → 7  |

Ambos operadores convierten sus operandos a números:

```javascript
> +'5'
5
> +'-12'
-12
> -'9'
-9
```

Por lo tanto, el más unario nos permite convertir valores arbitrarios a números.

### 16.3.3 Incrementar (++) y decrementar (--)

El operador de incremento ++ existe en una versión prefija y una versión sufija. En ambas versiones, agrega uno a su operando de manera destructiva. Por lo tanto, su operando debe ser una ubicación de almacenamiento que pueda ser cambiada.

El operador de decremento -- funciona de la misma manera, pero resta uno a su operando. Los siguientes dos ejemplos explican la diferencia entre la versión prefija y la sufija.

La Tabla 7 resume los operadores de incremento y decremento.

| Tabla 7: Operadores de incremento y decremento |
| ---------------------------------------------- | ---------- | ------------------------------ |
| Operador                                       | Nombre     | Ejemplo                        |
| v++                                            | Incremento | ES1 let v=0; [v++, v] → [0, 1] |
| ++v                                            | Incremento | ES1 let v=0; [++v, v] → [1, 1] |
| v--                                            | Decremento | ES1 let v=1; [v--, v] → [1, 0] |
| --v                                            | Decremento | ES1 let v=1; [--v, v] → [0, 0] |

A continuación, veremos ejemplos de estos operadores en uso.

Los operadores prefijos ++ y prefijos -- cambian sus operandos y luego los devuelven.

```javascript
let foo = 3;
assert.equal(++foo, 4);
assert.equal(foo, 4);

let bar = 3;
assert.equal(--bar, 2);
assert.equal(bar, 2);
```

Los operadores sufijos ++ y sufijos -- devuelven sus operandos y luego los cambian.

```javascript
let foo = 3;
assert.equal(foo++, 3);
assert.equal(foo, 4);

let bar = 3;
assert.equal(bar--, 3);
assert.equal(bar, 2);
```

### 16.3.3.1 Operandos: no solo variables

También podemos aplicar estos operadores a valores de propiedades:

```javascript
const obj = { a: 1 };
++obj.a;
assert.equal(obj.a, 2);
```

Y a elementos de Array:

```javascript
const arr = [4];
arr[0]++;
assert.deepEqual(arr, [5]);
```

#### Ejercicio: Operadores de números

exercises/numbers-math/is_odd_test.mjs

### 16.4 Convertir a número

Estas son tres formas de convertir valores a números:

- `Number(value)`
- `+value`
- `parseFloat(value)` (evitar; diferente a las otras dos)

Recomendación: usar el descriptivo `Number()`. La Tabla 8 resume cómo funciona.

| Tabla 8: Convertir valores a números |
| ------------------------------------ | ------------------------------------------------------------- |
| x                                    | Number(x)                                                     |
| undefined                            | NaN                                                           |
| null                                 | 0                                                             |
| boolean                              | false → 0, true → 1                                           |
| number                               | x (sin cambio)                                                |
| bigint                               | -1n → -1, 1n → 1, etc.                                        |
| string                               | '' → 0                                                        |
| Otro                                 | número parseado, ignorando espacios en blanco al inicio/final |
| symbol                               | Lanza TypeError                                               |
| objeto                               | Configurable (por ejemplo, a través de `.valueOf()`)          |

Ejemplos:

```javascript
assert.equal(Number(123.45), 123.45);

assert.equal(Number(""), 0);
assert.equal(Number("\n 123.45 \t"), 123.45);
assert.equal(Number("xyz"), NaN);

assert.equal(Number(-123n), -123);
```

Cómo los objetos se convierten en números puede configurarse – por ejemplo, sobrescribiendo `.valueOf()`:

```javascript
> Number({ valueOf() { return 123 } })
123
```

#### Ejercicio: Convertir a número

exercises/numbers-math/parse_number_test.mjs

### 16.5 Valores de error

Dos valores numéricos se devuelven cuando ocurren errores:

- NaN
- Infinity

### 16.5.1 Valor de error: NaN

NaN es una abreviatura de “not a number” (no un número). Irónicamente, JavaScript lo considera un número:

```javascript
> typeof NaN
'number'
```

¿Cuándo se devuelve NaN?

NaN se devuelve si no se puede parsear un número:

```javascript
> Number('$$$')
NaN
> Number(undefined)
NaN
```

NaN se devuelve si no se puede realizar una operación:

```javascript
> Math.log(-1)
NaN
> Math.sqrt(-1)
NaN
```

NaN se devuelve si un operando o argumento es NaN (para propagar errores):

```javascript
> NaN - 3
NaN
> 7 ** NaN
NaN
```

### 16.5.1.1 Comprobación de NaN

NaN es el único valor en JavaScript que no es estrictamente igual a sí mismo:

```javascript
const n = NaN;
assert.equal(n === n, false);
```

Estas son varias maneras de comprobar si un valor x es NaN:

```javascript
const x = NaN;

assert.equal(Number.isNaN(x), true); // preferido
assert.equal(Object.is(x, NaN), true);
assert.equal(x !== x, true);
```

En la última línea, utilizamos la peculiaridad de la comparación para detectar NaN.

### 16.5.1.2 Encontrar NaN en Arrays

Algunos métodos de Array no pueden encontrar NaN:

```javascript
> [NaN].indexOf(NaN)
-1
```

Otros sí pueden:

```javascript
> [NaN].includes(NaN)
true
> [NaN].findIndex(x => Number.isNaN(x))
0
> [NaN].find(x => Number.isNaN(x))
NaN
```

Desafortunadamente, no hay una regla simple. Debemos comprobar cómo maneja cada método NaN.

### 16.5.2 Valor de error: Infinity

¿Cuándo se devuelve el valor de error Infinity?

Infinity se devuelve si un número es demasiado grande:

```javascript
> Math.pow(2, 1023)
8.98846567431158e+307
> Math.pow(2, 1024)
Infinity
```

Infinity se devuelve si hay una división por cero:

```javascript
> 5 / 0
Infinity
> -5 / 0
-Infinity
```

### 16.5.2.1 Infinity como valor predeterminado

Infinity es mayor que todos los demás números (excepto NaN), lo que lo convierte en un buen valor predeterminado:

```javascript
function findMinimum(numbers) {
  let min = Infinity;
  for (const n of numbers) {
    if (n < min) min = n;
  }
  return min;
}

assert.equal(findMinimum([5, -1, 2]), -1);
assert.equal(findMinimum([]), Infinity);
```

### 16.5.2.2 Comprobación de Infinity

Estas son dos formas comunes de comprobar si un valor x es Infinity:

```javascript
const x = Infinity;

assert.equal(x === Infinity, true);
assert.equal(Number.isFinite(x), false);
```

#### Ejercicio: Comparar números

exercises/numbers-math/find_max_test.mjs

### 16.6 La precisión de los números: cuidado con las fracciones decimales

Internamente, los números de punto flotante en JavaScript se representan con base 2 (según el estándar IEEE 754). Eso significa que las fracciones decimales (base 10) no siempre pueden representarse con precisión:

```javascript
> 0.1 + 0.2
0.30000000000000004
> 1.3 * 3
3.9000000000000004
> 1.4 * 100000000000000
139999999999999.98
```

Por lo tanto, debemos tener en cuenta los errores de redondeo al realizar aritmética en JavaScript.

Continúe leyendo para una explicación de este fenómeno.

#### Quiz: básico

Vea la aplicación de quiz.

### 16.7 (Avanzado)

Todas las secciones restantes de este capítulo son avanzadas.

### 16.8 Antecedentes: precisión de punto flotante

En JavaScript, los cálculos con números no siempre producen resultados correctos. Por ejemplo:

> 0.1 + 0.2
> 0.30000000000000004

Para entender por qué, necesitamos explorar cómo JavaScript representa internamente los números de punto flotante. Utiliza tres enteros para hacerlo, que ocupan un total de 64 bits de almacenamiento (doble precisión):

| Componente | Tamaño  | Rango de enteros |
| ---------- | ------- | ---------------- |
| Signo      | 1 bit   | [0, 1]           |
| Fracción   | 52 bits | [0, 2^52−1]      |
| Exponente  | 11 bits | [−1023, 1024]    |

El número de punto flotante representado por estos enteros se calcula de la siguiente manera:

(–1)^sign × 0b1.fraction × 2^exponent

Esta representación no puede codificar un cero porque su segundo componente (involucrando la fracción) siempre tiene un 1 líder. Por lo tanto, un cero se codifica mediante el exponente especial −1023 y una fracción de 0.

### 16.8.1 Una representación simplificada de los números de punto flotante

Para facilitar las discusiones posteriores, simplificamos la representación anterior:

- En lugar de la base 2 (binario), usamos la base 10 (decimal) porque es la que la mayoría de las personas están más familiarizadas.
- La fracción es un número natural que se interpreta como una fracción (dígitos después de un punto). Cambiamos a una mantisa, un entero que se interpreta por sí mismo. Como consecuencia, el exponente se usa de manera diferente, pero su función fundamental no cambia.
- Como la mantisa es un entero (con su propio signo), ya no necesitamos un signo separado.

La nueva representación funciona así:

mantisa × 10^exponente

Probemos esta representación para algunos números de punto flotante.

Para el entero −123, necesitamos principalmente la mantisa:

> -123 \* (10 \*\* 0)
> -123

Para el número 1.5, imaginamos que hay un punto después de la mantisa. Usamos un exponente negativo para mover ese punto un dígito a la izquierda:

> 15 \* (10 \*\* -1)
> 1.5

Para el número 0.25, movemos el punto dos dígitos a la izquierda:

> 25 \* (10 \*\* -2)
> 0.25

Las representaciones con exponentes negativos también pueden escribirse como fracciones con exponentes positivos en los denominadores:

> 15 _ (10 ** -1) === 15 / (10 ** 1)
> true
> 25 _ (10 ** -2) === 25 / (10 ** 2)
> true

Estas fracciones ayudan a entender por qué hay números que nuestra codificación no puede representar:

1/10 puede ser representado. Ya tiene el formato requerido: una potencia de 10 en el denominador.

1/2 puede ser representado como 5/10. Convertimos el 2 en el denominador en una potencia de 10 multiplicando el numerador y el denominador por 5.

1/4 puede ser representado como 25/100. Convertimos el 4 en el denominador en una potencia de 10 multiplicando el numerador y el denominador por 25.

1/3 no puede ser representado. No hay manera de convertir el denominador en una potencia de 10. (Los factores primos de 10 son 2 y 5. Por lo tanto, cualquier denominador que solo tenga estos factores primos puede convertirse en una potencia de 10, multiplicando tanto el numerador como el denominador por suficientes doses y cincos. Si un denominador tiene un factor primo diferente, entonces no hay nada que podamos hacer.)

Para concluir nuestra excursión, volvemos a la base 2:

0.5 = 1/2 puede ser representado con base 2 porque el denominador ya es una potencia de 2.
0.25 = 1/4 puede ser representado con base 2 porque el denominador ya es una potencia de 2.
0.1 = 1/10 no puede ser representado porque el denominador no puede convertirse en una potencia de 2.
0.2 = 2/10 no puede ser representado porque el denominador no puede convertirse en una potencia de 2.

Ahora podemos ver por qué 0.1 + 0.2 no produce un resultado correcto: internamente, ninguno de los dos operandos puede ser representado con precisión.

La única manera de calcular con precisión con fracciones decimales es cambiando internamente a la base 10. Para muchos lenguajes de programación, la base 2 es el valor predeterminado y la base 10 una opción. Por ejemplo, Java tiene la clase BigDecimal y Python tiene el módulo decimal. Hay planes para agregar algo similar a JavaScript: la propuesta ECMAScript "Decimal".

### 16.9 Números enteros en JavaScript

Los números enteros son números normales (de punto flotante) sin fracciones decimales:

> 1 === 1.0
> true
> Number.isInteger(1.0)
> true

En esta sección, veremos algunas herramientas para trabajar con estos pseudo-enteros. JavaScript también admite bigints, que son enteros reales.

### 16.9.1 Convertir a entero

La forma recomendada de convertir números a enteros es utilizar uno de los métodos de redondeo del objeto Math:

**Math.floor(n): devuelve el mayor entero i ≤ n**

> Math.floor(2.1)
> 2
> Math.floor(2.9)
> 2

**Math.ceil(n): devuelve el menor entero i ≥ n**

> Math.ceil(2.1)
> 3
> Math.ceil(2.9)
> 3

**Math.round(n): devuelve el entero que está "más cerca" de n con \_\_.5 redondeado hacia arriba – por ejemplo:**

> Math.round(2.4)
> 2
> Math.round(2.5)
> 3

**Math.trunc(n): elimina cualquier fracción decimal (después del punto) que tenga n, convirtiéndolo así en un entero.**

> Math.trunc(2.1)
> 2
> Math.trunc(2.9)
> 2

Para obtener más información sobre el redondeo, consulte §17.3 "Redondeo".

### 16.9.2 Rangos de números enteros en JavaScript

Estos son rangos importantes de números enteros en JavaScript:

- **Enteros seguros**: pueden ser representados "de manera segura" por JavaScript (más sobre lo que eso significa en la subsección siguiente)
  - **Precisión**: 53 bits más el signo
  - **Rango**: (−2^53, 2^53)
- **Índices de array**
  - **Precisión**: 32 bits, sin signo
  - **Rango**: [0, 2^32−1) (excluyendo la longitud máxima)
  - Los Arrays Tipados tienen un rango más amplio de 53 bits (seguros y sin signo)
- **Operadores a nivel de bits (bitwise Or, etc.)**
  - **Precisión**: 32 bits
  - **Rango de desplazamiento a la derecha sin signo (>>>): sin signo, [0, 2^32)**
  - **Rango de todos los demás operadores a nivel de bits: con signo, [−2^31, 2^31)**

### 16.9.3 Enteros seguros

Este es el rango de números enteros que son seguros en JavaScript (53 bits más un signo):

[–(2^53)+1, 2^53–1]

Un entero es seguro si se representa con exactamente un número de JavaScript. Dado que los números de JavaScript se codifican como una fracción multiplicada por 2 elevado a la potencia de un exponente, los enteros más altos también pueden representarse, pero entonces hay espacios entre ellos.

Por ejemplo (18014398509481984 es 2^54):

> 18014398509481984
> 18014398509481984
> 18014398509481985
> 18014398509481984
> 18014398509481986
> 18014398509481984
> 18014398509481987
> 18014398509481988

Las siguientes propiedades de Number ayudan a determinar si un entero es seguro:

```javascript
assert.equal(Number.MAX_SAFE_INTEGER, 2 ** 53 - 1);
assert.equal(Number.MIN_SAFE_INTEGER, -Number.MAX_SAFE_INTEGER);

assert.equal(Number.isSafeInteger(5), true);
assert.equal(Number.isSafeInteger("5"), false);
assert.equal(Number.isSafeInteger(5.1), false);
assert.equal(Number.isSafeInteger(Number.MAX_SAFE_INTEGER), true);
assert.equal(Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1), false);
```

**Ejercicio: Detectar enteros seguros**

`exercises/numbers-math/is_safe_integer_test.mjs`

### 16.9.3.1 Computaciones seguras

Veamos cálculos que involucran enteros no seguros.

El siguiente resultado es incorrecto e inseguro, aunque ambos operandos son seguros:

> 9007199254740990 + 3  
> 9007199254740992

El siguiente resultado es seguro, pero incorrecto. El primer operando no es seguro; el segundo operando es seguro:

> 9007199254740995 - 10  
> 9007199254740986

Por lo tanto, el resultado de una expresión a op b es correcto si y solo si:

isSafeInteger(a) && isSafeInteger(b) && isSafeInteger(a op b)  
Es decir, ambos operandos y el resultado deben ser seguros.

### 16.10 Operadores bit a bit

#### 16.10.1 Internamente, los operadores bit a bit trabajan con enteros de 32 bits

Internamente, los operadores bit a bit de JavaScript trabajan con enteros de 32 bits. Producen sus resultados en los siguientes pasos:

1. **Entrada (números en JavaScript)**: Los 1–2 operandos se convierten primero en números de JavaScript (números de punto flotante de 64 bits) y luego en enteros de 32 bits.
2. **Cálculo (enteros de 32 bits)**: La operación real procesa enteros de 32 bits y produce un entero de 32 bits.
3. **Salida (número en JavaScript)**: Antes de devolver el resultado, se convierte nuevamente en un número de JavaScript.

#### 16.10.1.1 Tipos de operandos y resultados

Para cada operador bit a bit, este libro menciona los tipos de sus operandos y su resultado. Cada tipo es siempre uno de los siguientes dos:

| Tipo   | Descripción                 | Tamaño              | Rango       |
| ------ | --------------------------- | ------------------- | ----------- |
| Int32  | Entero con signo de 32 bits | 32 bits incl. signo | [−2³¹, 2³¹) |
| Uint32 | Entero sin signo de 32 bits | 32 bits             | [0, 2³²)    |

Considerando los pasos mencionados anteriormente, recomiendo pretender que los operadores bit a bit internamente trabajan con enteros sin signo de 32 bits (paso “cálculo”) y que Int32 y Uint32 solo afectan cómo los números de JavaScript se convierten hacia y desde enteros (pasos “entrada” y “salida”).

#### 16.10.1.2 Mostrar números en JavaScript como enteros sin signo de 32 bits

Mientras exploramos los operadores bit a bit, ocasionalmente ayuda mostrar los números de JavaScript como enteros sin signo de 32 bits en notación binaria. Eso es lo que hace b32() (cuya implementación se muestra más adelante):

```js
assert.equal(b32(-1), "11111111111111111111111111111111");
assert.equal(b32(1), "00000000000000000000000000000001");
assert.equal(b32(2 ** 31), "10000000000000000000000000000000");
```

### 16.10.2 Not bit a bit

Tabla 9: El operador Not bit a bit.
| Operación | Nombre | Firma de tipo |
|-----------|---------------------|----------------|
| ~num | Not bit a bit, complemento a uno | Int32 → Int32 | ES1 |

El operador Not bit a bit invierte cada dígito binario de su operando:

```js
> b32(~0b100)
'11111111111111111111111111111011'
```

Este llamado complemento a uno es similar a un negativo para algunas operaciones aritméticas. Por ejemplo, sumar un entero a su complemento a uno siempre es -1:

```js
> 4 + ~4
-1
> -11 + ~-11
-1
```

### 16.10.3 Operadores binarios bit a bit

Tabla 10: Operadores binarios bit a bit.
| Operación | Nombre | Firma de tipo |
|---------------|----------------|------------------------|
| num1 & num2 | And bit a bit | Int32 × Int32 → Int32 | ES1 |
| num1 | num2 | Or bit a bit | Int32 × Int32 → Int32 | ES1 |
| num1 ^ num2 | Xor bit a bit | Int32 × Int32 → Int32 | ES1 |

Los operadores binarios bit a bit combinan los bits de sus operandos para producir sus resultados:

```js
> (0b1010 & 0b0011).toString(2).padStart(4, '0')
'0010'
> (0b1010 | 0b0011).toString(2).padStart(4, '0')
'1011'
> (0b1010 ^ 0b0011).toString(2).padStart(4, '0')
'1001'
```

### 16.10.4 Operadores de desplazamiento bit a bit

Tabla 11: Operadores de desplazamiento bit a bit.
| Operación | Nombre | Firma de tipo |
|-----------------|---------------------|------------------------|
| num << count | Desplazamiento a la izquierda | Int32 × Uint32 → Int32 | ES1 |
| num >> count | Desplazamiento a la derecha con signo | Int32 × Uint32 → Int32 | ES1 |
| num >>> count | Desplazamiento a la derecha sin signo | Uint32 × Uint32 → Uint32 | ES1 |

Los operadores de desplazamiento mueven dígitos binarios hacia la izquierda o hacia la derecha:

```js
> (0b10 << 1).toString(2)
'100'
```

> > preserva el bit más alto, >>> no:

```js
> b32(0b10000000000000000000000000000010 >> 1)
'11000000000000000000000000000001'
> b32(0b10000000000000000000000000000010 >>> 1)
'01000000000000000000000000000001'
```

### 16.10.5 b32(): mostrando enteros sin signo de 32 bits en notación binaria

Hemos usado b32() varias veces. El siguiente código es una implementación de ello:

```js
/**
 * Devuelve una cadena que representa n como un entero sin signo de 32 bits,
 * en notación binaria.
 */
function b32(n) {
  // >>> asegura que el bit más alto no se interprete como signo
  return (n >>> 0).toString(2).padStart(32, "0");
}
assert.equal(b32(6), "00000000000000000000000000000110");
```

n >>> 0 significa que estamos desplazando n cero bits a la derecha. Por lo tanto, en principio, el operador >>> no hace nada, pero aún así convierte n en un entero sin signo de 32 bits:

```js
> 12 >>> 0
12
> -12 >>> 0
4294967284
> (2**32 + 1) >>> 0
1
```

### 16.11 Referencia rápida: números

#### 16.11.1 Funciones globales para números

JavaScript tiene las siguientes cuatro funciones globales para números:

- isFinite()
- isNaN()
- parseFloat()
- parseInt()

Sin embargo, es mejor usar los métodos correspondientes de Number (Number.isFinite(), etc.), que tienen menos trampas. Fueron introducidos con ES6 y se discuten a continuación.

#### 16.11.2 Propiedades estáticas de Number

- **.EPSILON: número [ES6]**

  La diferencia entre 1 y el siguiente número de punto flotante representable. En general, una epsilon de máquina proporciona un límite superior para los errores de redondeo en la aritmética de punto flotante.

  Aproximadamente: 2.2204460492503130808472633361816 × 10^-16

- **.MAX_SAFE_INTEGER: número [ES6]**

  El entero más grande que JavaScript puede representar sin ambigüedades (2^53−1).

- **.MAX_VALUE: número [ES1]**

  El número finito positivo más grande en JavaScript.

  Aproximadamente: 1.7976931348623157 × 10^308

- **.MIN_SAFE_INTEGER: número [ES6]**

  El entero más pequeño que JavaScript puede representar sin ambigüedades (−2^53+1).

- **.MIN_VALUE: número [ES1]**

  El número positivo más pequeño en JavaScript. Aproximadamente 5 × 10^-324.

- **.NaN: número [ES1]**

  El mismo que la variable global NaN.

- **.NEGATIVE_INFINITY: número [ES1]**

  Lo mismo que -Number.POSITIVE_INFINITY.

- **.POSITIVE_INFINITY: número [ES1]**

  Lo mismo que la variable global Infinity.

### 16.11.3 Métodos estáticos de Number

**.isFinite(num: número): booleano [ES6]**

Devuelve true si num es un número real (ni Infinity ni -Infinity ni NaN).

```javascript
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
Number.isFinite(NaN); // false
Number.isFinite(123); // true
```

**.isInteger(num: número): booleano [ES6]**

Devuelve true si num es un número y no tiene una fracción decimal.

```javascript
Number.isInteger(-17); // true
Number.isInteger(33); // true
Number.isInteger(33.1); // false
Number.isInteger("33"); // false
Number.isInteger(NaN); // false
Number.isInteger(Infinity); // false
```

**.isNaN(num: número): booleano [ES6]**

Devuelve true si num es el valor NaN:

```javascript
Number.isNaN(NaN); // true
Number.isNaN(123); // false
Number.isNaN("abc"); // false
```

**.isSafeInteger(num: número): booleano [ES6]**

Devuelve true si num es un número y representa de manera inequívoca un número entero.

**.parseFloat(str: cadena): número [ES6]**

Convierte su parámetro a una cadena y lo analiza como un número de punto flotante. Para convertir cadenas a números, Number() (que ignora espacios en blanco al principio y al final) es generalmente una mejor opción que Number.parseFloat() (que ignora los espacios en blanco iniciales y los caracteres ilegales al final y puede ocultar problemas).

```javascript
Number.parseFloat(" 123.4#"); // 123.4
Number(" 123.4#"); // NaN
```

**.parseInt(str: cadena, radix=10): número [ES6]**

Convierte su parámetro a una cadena y lo analiza como un número entero, ignorando los espacios en blanco iniciales y los caracteres ilegales al final:

```javascript
Number.parseInt("  123#"); // 123
```

El parámetro radix especifica la base del número a analizar:

```javascript
Number.parseInt("101", 2); // 5
Number.parseInt("FF", 16); // 255
```

No uses este método para convertir números a enteros: convertir a cadena es ineficiente. Y detenerse antes del primer no dígito no es un buen algoritmo para eliminar la fracción de un número. Aquí hay un ejemplo donde falla:

```javascript
Number.parseInt(1e21, 10); // incorrecto, 1
```

Es mejor usar una de las funciones de redondeo de Math para convertir un número a un entero:

```javascript
Math.trunc(1e21); // correcto, 1e+21
```

### 16.11.4 Métodos de Number.prototype

(Number.prototype es donde se almacenan los métodos de los números.)

**.toExponential(fractionDigits?: número): cadena [ES3]**

Devuelve una cadena que representa el número en notación exponencial. Con fractionDigits, podemos especificar cuántos dígitos deben mostrarse del número que se multiplica con el exponente (el valor predeterminado es mostrar tantos dígitos como sea necesario).

Ejemplo: número demasiado pequeño para obtener un exponente positivo a través de .toString().

```javascript
(1234).toString(); // '1234'
(1234).toExponential(); // '1.234e+3'
(1234).toExponential(5); // '1.23400e+3'
(1234).toExponential(1); // '1.2e+3'
```

Ejemplo: fracción no lo suficientemente pequeña para obtener un exponente negativo a través de .toString().

```javascript
(0.003).toString(); // '0.003'
(0.003).toExponential(); // '3e-3'
```

**.toFixed(fractionDigits=0): cadena [ES3]**

Devuelve una representación sin exponente del número, redondeado a los dígitos especificados por fractionDigits.

```javascript
(0.00000012).toString(); // '1.2e-7'
(0.00000012).toFixed(10); // '0.0000001200'
(0.00000012).toFixed(); // '0'
```

Si el número es 1021 o mayor, incluso .toFixed() usa un exponente:

```javascript
(10 ** 21).toFixed(); // '1e+21'
```

**.toPrecision(precision?: número): cadena [ES3]**

Funciona como .toString(), pero precision especifica cuántos dígitos deben mostrarse. Si precision falta, se usa .toString().

```javascript
(1234).toPrecision(3); // '1.23e+3'
(1234).toPrecision(4); // '1234'
(1234).toPrecision(5); // '1234.0'
(1.234).toPrecision(3); // '1.23'
```

**.toString(radix=10): cadena [ES1]**

Devuelve una representación en cadena del número.

Por defecto, obtenemos un número en base 10 como resultado:

```javascript
(123.456).toString(); // '123.456'
```

Si queremos que el número tenga una base diferente, podemos especificarlo mediante radix:

```javascript
(4).toString(2); // '100'
(4.5).toString(2); // '100.1'
(255).toString(16); // 'ff'
(255.66796875).toString(16); // 'ff.ab'
(1234567890).toString(36); // 'kf12oi'
```

Number.parseInt() proporciona la operación inversa: convierte una cadena que contiene un número entero (¡sin fracción!) con una base dada, a un número.

```javascript
Number.parseInt("kf12oi", 36); // 1234567890
```

### 16.11.5 Fuentes

- Wikipedia
- Tipos integrados de TypeScript
- Documentación web de MDN para JavaScript
- Especificación del lenguaje ECMAScript

**Quiz: avanzado**

Ver aplicación de quiz.
