# 5 FAQ: JavaScript

## 5.1 ¿Cuáles son buenas referencias para JavaScript?

Consulta el §6.3 "Referencias de JavaScript".

## 5.2 ¿Cómo puedo averiguar qué características de JavaScript son compatibles y dónde?

Este libro generalmente menciona si una característica es parte de ECMAScript 5 (como se requiere para navegadores más antiguos) o una versión más reciente. Para obtener información más detallada (incluyendo versiones anteriores a ES5), hay varias buenas tablas de compatibilidad disponibles en línea:

- Tablas de compatibilidad de ECMAScript para varios motores (por kangax, webbedspace, zloirock)
- Tablas de compatibilidad de Node.js (por William Kapke)
- Los documentos web de MDN de Mozilla tienen tablas para cada característica que describen las versiones relevantes de ECMAScript y la compatibilidad con navegadores.
- "Can I use…" documenta qué características (incluyendo características del lenguaje JavaScript) son compatibles con los navegadores web.

## 5.3 ¿Dónde puedo buscar qué características están planificadas para JavaScript?

Consulta las siguientes fuentes:

- §3.5 "El proceso TC39" describe cómo se planifican las características futuras.
- §3.6 "FAQ: Proceso TC39" responde varias preguntas sobre las características futuras.

## 5.4 ¿Por qué JavaScript falla en silencio tan a menudo?

JavaScript a menudo falla en silencio. Veamos dos ejemplos.

Primer ejemplo: Si los operandos de un operador no tienen los tipos apropiados, se convierten según sea necesario.

```javascript
"3" * "5";
```

15

Segundo ejemplo: Si una operación aritmética falla, obtienes un valor de error, no una excepción.

```javascript
1 / 0;
```

Infinity

La razón de los fallos silenciosos es histórica: JavaScript no tenía excepciones hasta ECMAScript 3. Desde entonces, sus diseñadores han intentado evitar los fallos silenciosos.

## 5.5 ¿Por qué no podemos limpiar JavaScript eliminando rarezas y características desactualizadas?

Esta pregunta se responde en §3.7 "Evolucionando JavaScript: No rompas la web".

## 5.6 ¿Cómo puedo probar rápidamente un fragmento de código JavaScript?

§8.1 "Probar código JavaScript" explica cómo hacerlo.
