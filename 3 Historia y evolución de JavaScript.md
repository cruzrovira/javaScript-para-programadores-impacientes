### 3 Historia y evolución de JavaScript

#### 3.1 Cómo se creó JavaScript

#### 3.2 Estandarización de JavaScript

#### 3.3 Cronología de las versiones de ECMAScript

#### 3.4 Comité Técnico 39 de Ecma (TC39)

#### 3.5 El proceso de TC39

##### 3.5.1 Consejo: Piensa en características individuales y etapas, no en versiones de ECMAScript

#### 3.6 Preguntas frecuentes: Proceso de TC39

##### 3.6.1 ¿Cómo va [mi característica propuesta favorita]?

##### 3.6.2 ¿Existe una lista oficial de características de ECMAScript?

#### 3.7 Evolucionando JavaScript: No rompas la web

### 3.1 Cómo se creó JavaScript

JavaScript fue creado en mayo de 1995 en 10 días, por Brendan Eich. Eich trabajaba en Netscape e implementó JavaScript para su navegador web, Netscape Navigator.

La idea era que las partes interactivas principales del lado del cliente web se implementaran en Java. JavaScript debía ser un lenguaje de enlace para esas partes y también hacer que HTML fuera ligeramente más interactivo. Dado su papel de asistir a Java, JavaScript tenía que parecerse a Java. Eso descartaba soluciones existentes como Perl, Python, TCL y otros.

Inicialmente, el nombre de JavaScript cambió varias veces:

- Su nombre en clave era Mocha.
- En las betas de Netscape Navigator 2.0 (septiembre de 1995), se llamaba LiveScript.
- En la beta 3 de Netscape Navigator 2.0 (diciembre de 1995), obtuvo su nombre final, JavaScript.

### 3.2 Estandarización de JavaScript

Existen dos estándares para JavaScript:

- ECMA-262 está alojado por Ecma International. Es el estándar principal.
- ISO/IEC 16262 está alojado por la Organización Internacional de Normalización (ISO) y la Comisión Electrotécnica Internacional (IEC). Este es un estándar secundario.

El lenguaje descrito por estos estándares se llama ECMAScript, no JavaScript. Se eligió un nombre diferente porque Sun (ahora Oracle) tenía una marca registrada para el nombre JavaScript. El “ECMA” en “ECMAScript” proviene de la organización que aloja el estándar principal.

El nombre original de esa organización era ECMA, un acrónimo de European Computer Manufacturers Association. Luego se cambió a Ecma International (con “Ecma” siendo un nombre propio, no un acrónimo) porque las actividades de la organización se habían expandido más allá de Europa. El acrónimo original en mayúsculas explica la ortografía de ECMAScript.

En principio, JavaScript y ECMAScript significan lo mismo. A veces se hace la siguiente distinción:

- El término JavaScript se refiere al lenguaje y sus implementaciones.
- El término ECMAScript se refiere al estándar del lenguaje y las versiones del lenguaje.

Por lo tanto, ECMAScript 6 es una versión del lenguaje (su sexta edición).

### 3.3 Cronología de las versiones de ECMAScript

Esta es una breve cronología de las versiones de ECMAScript:

- ECMAScript 1 (junio de 1997): Primera versión del estándar.
- ECMAScript 2 (junio de 1998): Pequeña actualización para mantener ECMA-262 en sincronía con el estándar ISO.
- ECMAScript 3 (diciembre de 1999): Agrega muchas características principales – “[…] expresiones regulares, mejor manejo de cadenas, nuevas declaraciones de control [do-while, switch], manejo de excepciones con try/catch, […]”
- ECMAScript 4 (abandonado en julio de 2008): Habría sido una actualización masiva (con tipado estático, módulos, espacios de nombres y más), pero terminó siendo demasiado ambicioso y dividió a los responsables del lenguaje.
- ECMAScript 5 (diciembre de 2009): Trajo mejoras menores – algunas características de la biblioteca estándar y modo estricto.
- ECMAScript 5.1 (junio de 2011): Otra pequeña actualización para mantener los estándares de Ecma e ISO en sincronía.
- ECMAScript 6 (junio de 2015): Una gran actualización que cumplió muchas de las promesas de ECMAScript 4. Esta versión es la primera cuyo nombre oficial – ECMAScript 2015 – se basa en el año de publicación.
- ECMAScript 2016 (junio de 2016): Primer lanzamiento anual. El ciclo de lanzamiento más corto resultó en menos características nuevas en comparación con el gran ES6.
- ECMAScript 2017 (junio de 2017). Segundo lanzamiento anual.

Las versiones posteriores de ECMAScript (ES2018, etc.) siempre se ratifican en junio.

### 3.4 Comité Técnico 39 de Ecma (TC39)

TC39 es el comité que evoluciona JavaScript. Sus miembros son, estrictamente hablando, empresas: Adobe, Apple, Facebook, Google, Microsoft, Mozilla, Opera, Twitter y otras. Es decir, empresas que usualmente son competidoras feroces están trabajando juntas por el bien del lenguaje.

Cada dos meses, TC39 tiene reuniones a las que asisten delegados designados por los miembros y expertos invitados. Las actas de esas reuniones son públicas en un repositorio de GitHub.

### 3.5 El proceso de TC39

Con ECMAScript 6, se hicieron evidentes dos problemas con el proceso de lanzamiento utilizado en ese momento:

- Si pasa demasiado tiempo entre lanzamientos, las características que están listas temprano tienen que esperar mucho tiempo hasta que puedan ser lanzadas. Y las características que están listas tarde corren el riesgo de ser apresuradas para cumplir con el plazo.
- Las características a menudo se diseñaban mucho antes de ser implementadas y utilizadas. Por lo tanto, las deficiencias de diseño relacionadas con la implementación y el uso se descubrían demasiado tarde.

En respuesta a estos problemas, TC39 instituyó el nuevo proceso de TC39:

- Las características de ECMAScript se diseñan de manera independiente y pasan por etapas, comenzando en 0 (“borrador”), terminando en 4 (“terminado”).
- Especialmente las etapas posteriores requieren implementaciones prototipo y pruebas en el mundo real, lo que lleva a bucles de retroalimentación entre diseños e implementaciones.
- Las versiones de ECMAScript se lanzan una vez al año e incluyen todas las características que han alcanzado la etapa 4 antes de una fecha límite de lanzamiento.

El resultado: lanzamientos más pequeños e incrementales, cuyas características ya han sido probadas en el campo. La Figura 1 ilustra el proceso de TC39.

### 3.5.1 Consejo: Piensa en características individuales y etapas, no en versiones de ECMAScript

Hasta e incluyendo ES6, lo más común era pensar en JavaScript en términos de versiones de ECMAScript – por ejemplo, “¿Este navegador soporta ES6 ya?”

A partir de ES2016, es mejor pensar en características individuales: una vez que una característica alcanza la etapa 4, puedes usarla con seguridad (si es compatible con los motores de JavaScript que estás apuntando). No tienes que esperar hasta el próximo lanzamiento de ECMAScript.

### 3.6 Preguntas frecuentes: Proceso de TC39

#### 3.6.1 ¿Cómo va [mi característica propuesta favorita]?

Si te preguntas en qué etapas se encuentran varias características propuestas, consulta el repositorio de GitHub propuestas.

#### 3.6.2 ¿Existe una lista oficial de características de ECMAScript?

Sí, el repositorio de TC39 enumera las propuestas terminadas y menciona en qué versiones de ECMAScript se introdujeron.

### 3.7 Evolucionando JavaScript: No rompas la web

Una idea que surge ocasionalmente es limpiar JavaScript eliminando características y peculiaridades antiguas. Si bien el atractivo de esa idea es obvio, tiene desventajas significativas.

Supongamos que creamos una nueva versión de JavaScript que no es compatible con versiones anteriores y solucionamos todos sus defectos. Como resultado, encontraríamos los siguientes problemas:

- Los motores de JavaScript se vuelven más pesados: necesitan soportar tanto la versión antigua como la nueva. Lo mismo es cierto para herramientas como IDEs y herramientas de construcción.
- Los programadores necesitan conocer, y estar continuamente conscientes, de las diferencias entre las versiones.
- Puedes migrar todo un código base existente a la nueva versión (lo que puede ser mucho trabajo). O puedes mezclar versiones y la refactorización se vuelve más difícil porque no puedes mover código entre versiones sin cambiarlo.
- De alguna manera tienes que especificar por pieza de código – ya sea un archivo o código incrustado en una página web – en qué versión está escrito. Cada solución concebible tiene pros y contras. Por ejemplo, el modo estricto es una versión ligeramente más limpia de ES5. Una de las razones por las que no fue tan popular como debería haber sido: era un problema optar por él a través de una directiva al comienzo de un archivo o una función.

Entonces, ¿cuál es la solución? ¿Podemos tener el pastel y comérnoslo? El enfoque que se eligió para ES6 se llama “Un JavaScript”:

- Las nuevas versiones siempre son completamente compatibles con versiones anteriores (pero ocasionalmente puede haber pequeñas limpiezas
