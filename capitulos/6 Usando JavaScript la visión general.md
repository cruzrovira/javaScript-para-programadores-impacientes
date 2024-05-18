# 6 Usando JavaScript: la visión general

## 6.1 ¿Qué estás aprendiendo en este libro?

## 6.2 La estructura de los navegadores y Node.js

## 6.3 Referencias de JavaScript

## 6.4 Lecturas adicionales

En este capítulo, me gustaría pintar la visión general: ¿qué estás aprendiendo en este libro y cómo encaja en el panorama general del desarrollo web?

## 6.1 ¿Qué estás aprendiendo en este libro?

Este libro enseña el lenguaje JavaScript. Se enfoca solo en el lenguaje, pero ofrece miradas ocasionales a dos plataformas donde se puede usar JavaScript:

- Navegador web
- Node.js

Node.js es importante para el desarrollo web de tres maneras:

- Puedes usarlo para escribir software del lado del servidor en JavaScript.
- También puedes usarlo para escribir software para la línea de comandos (piensa en el shell de Unix, Windows PowerShell, etc.). Muchas herramientas relacionadas con JavaScript están basadas en (y se ejecutan a través de) Node.js.
- El registro de software de Node, npm, se ha convertido en la forma dominante de instalar herramientas (como compiladores y herramientas de construcción) y bibliotecas, incluso para el desarrollo del lado del cliente.

## 6.2 La estructura de los navegadores y Node.js

Figura 2: La estructura de las dos plataformas de JavaScript, navegador web y Node.js. Las API "biblioteca estándar" y "API de plataforma" están alojadas sobre una capa fundamental con un motor de JavaScript y un "núcleo" específico de la plataforma.
Figura 2: La estructura de las dos plataformas de JavaScript, navegador web y Node.js. Las API "biblioteca estándar" y "API de plataforma" están alojadas sobre una capa fundamental con un motor de JavaScript y un "núcleo" específico de la plataforma.

Las estructuras de las dos plataformas de JavaScript, navegador web y Node.js, son similares (fig. 2):

- La capa fundamental consiste en el motor de JavaScript y la funcionalidad "núcleo" específica de la plataforma.
- Dos API están alojadas sobre esta base:
  - La biblioteca estándar de JavaScript es parte del propio JavaScript y se ejecuta sobre el motor.
  - La API de la plataforma también está disponible desde JavaScript y proporciona acceso a la funcionalidad específica de la plataforma. Por ejemplo:
    - En los navegadores, necesitas usar la API específica de la plataforma si quieres hacer algo relacionado con la interfaz de usuario: reaccionar a los clics del ratón, reproducir sonidos, etc.
    - En Node.js, la API específica de la plataforma te permite leer y escribir archivos, descargar datos a través de HTTP, etc.

## 6.3 Referencias de JavaScript

Cuando tienes una pregunta sobre JavaScript, una búsqueda en la web generalmente ayuda. Puedo recomendar las siguientes fuentes en línea:

- Documentos web de MDN: cubren varias tecnologías web como CSS, HTML, JavaScript y más. Una excelente referencia.
- Documentos de Node.js: documentan la API de Node.js.
- ExploringJS.com: Mis otros libros sobre JavaScript profundizan más en detalle que este libro y son gratuitos para leer en línea. Puedes buscar características por versión de ECMAScript:
  - ES1–ES5: Speaking JavaScript
  - ES6: Exploring ES6
  - ES2016–ES2017: Exploring ES2016 and ES2017
  - Etc.

## 6.4 Lecturas adicionales

Un capítulo adicional proporciona una visión más completa del desarrollo web.
