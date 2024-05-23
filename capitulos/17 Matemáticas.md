### 17 Math

### 17.1 Propiedades de datos

### 17.2 Exponentes, raíces, logaritmos

### 17.3 Redondeo

### 17.4 Funciones trigonométricas

### 17.5 Varias otras funciones

### 17.6 Fuentes

Math es un objeto con propiedades de datos y métodos para procesar números. Puedes verlo como un módulo básico: fue creado mucho antes de que JavaScript tuviera módulos.

### 17.1 Propiedades de datos

**Math.E: número [ES1]**

El número de Euler, base de los logaritmos naturales, aproximadamente 2.7182818284590452354.

**Math.LN10: número [ES1]**

El logaritmo natural de 10, aproximadamente 2.302585092994046.

**Math.LN2: número [ES1]**

El logaritmo natural de 2, aproximadamente 0.6931471805599453.

**Math.LOG10E: número [ES1]**

El logaritmo de e en base 10, aproximadamente 0.4342944819032518.

**Math.LOG2E: número [ES1]**

El logaritmo de e en base 2, aproximadamente 1.4426950408889634.

**Math.PI: número [ES1]**

La constante matemática π, relación de la circunferencia de un círculo con su diámetro, aproximadamente 3.1415926535897932.

**Math.SQRT1_2: número [ES1]**

La raíz cuadrada de 1/2, aproximadamente 0.7071067811865476.

**Math.SQRT2: número [ES1]**

La raíz cuadrada de 2, aproximadamente 1.4142135623730951.

### 17.2 Exponentes, raíces, logaritmos

**Math.cbrt(x: número): número [ES6]**

Devuelve la raíz cúbica de x.

```javascript
Math.cbrt(8); // 2
```

**Math.exp(x: número): número [ES1]**

Devuelve e^x (e siendo el número de Euler). El inverso de Math.log().

```javascript
Math.exp(0); // 1
Math.exp(1) === Math.E; // true
```

**Math.expm1(x: número): número [ES6]**

Devuelve Math.exp(x)-1. El inverso de Math.log1p(). Los números muy pequeños (fracciones cercanas a 0) se representan con una mayor precisión. Por lo tanto, esta función devuelve valores más precisos siempre que .exp() devuelva valores cercanos a 1.

**Math.log(x: número): número [ES1]**

Devuelve el logaritmo natural de x (en base e, el número de Euler). El inverso de Math.exp().

```javascript
Math.log(1); // 0
Math.log(Math.E); // 1
Math.log(Math.E ** 2); // 2
```

**Math.log1p(x: número): número [ES6]**

Devuelve Math.log(1 + x). El inverso de Math.expm1(). Los números muy pequeños (fracciones cercanas a 0) se representan con una mayor precisión. Por lo tanto, puedes proporcionar a esta función un argumento más preciso siempre que el argumento para .log() esté cerca de 1.

**Math.log10(x: número): número [ES6]**

Devuelve el logaritmo de x en base 10. El inverso de 10^x.

```javascript
Math.log10(1); // 0
Math.log10(10); // 1
Math.log10(100); // 2
```

**Math.log2(x: número): número [ES6]**

Devuelve el logaritmo de x en base 2. El inverso de 2^x.

```javascript
Math.log2(1); // 0
Math.log2(2); // 1
Math.log2(4); // 2
```

**Math.pow(x: número, y: número): número [ES1]**

Devuelve x^y, x elevado a la potencia de y. Lo mismo que x \*\* y.

```javascript
Math.pow(2, 3); // 8
Math.pow(25, 0.5); // 5
```

**Math.sqrt(x: número): número [ES1]**

Devuelve la raíz cuadrada de x. El inverso de x^2.

```javascript
Math.sqrt(9); // 3
```

### 17.3 Redondeo

Redondear significa convertir un número arbitrario en un entero (un número sin fracción decimal). Las siguientes funciones implementan diferentes enfoques para redondear.

**Math.ceil(x: número): número [ES1]**

Devuelve el menor entero (más cercano a -∞) i con x ≤ i.

```javascript
Math.ceil(2.1); // 3
Math.ceil(2.9); // 3
```

**Math.floor(x: número): número [ES1]**

Devuelve el mayor entero (más cercano a +∞) i con i ≤ x.

```javascript
Math.floor(2.1); // 2
Math.floor(2.9); // 2
```

**Math.round(x: número): número [ES1]**

Devuelve el entero más cercano a x. Si la fracción decimal de x es .5, entonces .round() redondea hacia arriba (al entero más cercano a infinito positivo):

```javascript
Math.round(2.4); // 2
Math.round(2.5); // 3
```

**Math.trunc(x: número): número [ES6]**

Elimina la fracción decimal de x y devuelve el entero resultante.

```javascript
Math.trunc(2.1); // 2
Math.trunc(2.9); // 2
```

Tabla 12 muestra los resultados de las funciones de redondeo para algunas entradas representativas.

**Tabla 12: Funciones de redondeo de Math. Nota cómo cambian las cosas con números negativos porque "mayor" siempre significa "más cercano a infinito positivo".**

|            | -2.9 | -2.5 | -2.1 | 2.1 | 2.5 | 2.9 |
| ---------- | ---- | ---- | ---- | --- | --- | --- |
| Math.floor | -3   | -3   | -3   | 2   | 2   | 2   |
| Math.ceil  | -2   | -2   | -2   | 3   | 3   | 3   |
| Math.round | -3   | -2   | -2   | 2   | 3   | 3   |
| Math.trunc | -2   | -2   | -2   | 2   | 2   | 2   |

### 17.4 Funciones trigonométricas

Todos los ángulos se especifican en radianes. Usa las siguientes dos funciones para convertir entre grados y radianes.

```javascript
function degreesToRadians(degrees) {
  return (degrees / 180) * Math.PI;
}
assert.equal(degreesToRadians(90), Math.PI / 2);

function radiansToDegrees(radians) {
  return (radians / Math.PI) * 180;
}
assert.equal(radiansToDegrees(Math.PI), 180);
```

**Math.acos(x: número): número [ES1]**

Devuelve el arcocoseno (coseno inverso) de x.

```javascript
Math.acos(0); // 1.5707963267948966
Math.acos(1); // 0
```

**Math.acosh(x: número): número [ES6]**

Devuelve el coseno hiperbólico inverso de x.

**Math.asin(x: número): número [ES1]**

Devuelve el arcoseno (seno inverso) de x.

```javascript
Math.asin(0); // 0
Math.asin(1); // 1.5707963267948966
```

**Math.asinh(x: número): número [ES6]**

Devuelve el seno hiperbólico inverso de x.

**Math.atan(x: número): número [ES1]**

Devuelve el arcotangente (tangente inversa) de x.

**Math.atanh(x: número): número [ES6]**

Devuelve la tangente hiperbólica inversa de x.

**Math.atan2(y: número, x: número): número [ES1]**

Devuelve el arcotangente del cociente y/x.

**Math.cos(x: número): número [ES1]**

Devuelve el coseno de x.

```javascript
Math.cos(0); // 1
Math.cos(Math.PI); // -1
```

**Math.cosh(x: número): número [ES6]**

Devuelve el coseno hiperbólico de x.

**Math.hypot(...values: número[]): número [ES6]**

Devuelve la raíz cuadrada de la suma de los cuadrados de values (teorema de Pitágoras):

```javascript
Math.hypot(3, 4); // 5
```

**Math.sin(x: número): número [ES1]**

Devuelve el seno de x.

```javascript
Math.sin(0); // 0
Math.sin(Math.PI / 2); // 1
```

**Math.sinh(x: número): número [ES6]**

Devuelve el seno hiperbólico de x.

**Math.tan(x: número): número [ES1]**

Devuelve la tangente de x.

```javascript
Math.tan(0); // 0
Math.tan(1); // 1.5574077246549023
```

**Math.tanh(x: número): número [ES6]**

Devuelve la tangente hiperbólica de x.

### 17.5 Varias otras funciones

**Math.abs(x: número): número [ES1]**

Devuelve el valor absoluto de x.

```javascript
Math.abs(3); // 3
Math.abs(-3); // 3
Math.abs(0); // 0
```

**Math.clz32(x: número): número [ES6]**

Cuenta los bits cero a la izquierda en el entero de 32 bits x. Usado en algoritmos DSP.

```javascript
Math.clz32(0b01000000000000000000000000000000); // 1
Math.clz32(0b00100000000000000000000000000000); // 2
Math.clz32(2); // 30
Math.clz32(1); // 31
```

**Math.max(...values: número[]): número [ES1]**

Convierte los valores a números y devuelve el mayor de ellos.

```javascript
Math.max(3, -5, 24); // 24
```

**Math.min(...values: número[]): número [ES1]**

Convierte los valores a números y devuelve el menor de ellos.

```javascript
Math.min(3, -5, 24); // -5
```

**Math.random(): número [ES1]**

Devuelve un número pseudoaleatorio n donde 0 ≤ n < 1.

```javascript
/** Devuelve un entero aleatorio i con 0 <= i < max */
function getRandomInteger(max) {
  return Math.floor(Math.random() * max);
}
```

**Math.sign(x: número): número [ES6]**

Devuelve el signo de un número:

```javascript
Math.sign(-8); // -1
Math.sign(0); // 0
Math.sign(3); // 1
```

### 17.6 Fuentes

- Wikipedia
- Tipos integrados de TypeScript
- Documentos web de MDN para JavaScript
- Especificación del lenguaje ECMAScript
