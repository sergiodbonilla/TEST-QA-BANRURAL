Error 1: ReferenceError: lowOrHi is not defined (Identificado en Consola)
Descripción del Error: Al ejecutar el código, la consola del navegador mostraba un error ReferenceError: lowOrHi is not defined. Esto indicaba que la variable lowOrHi no estaba siendo encontrada en el scope donde se intentaba utilizar.
Pasos para Reproducir:

Abrir el archivo index.html en el navegador.
Inspeccionar la consola del navegador (Herramientas de desarrollador > Consola).
Intentar ingresar un número y hacer clic en "Ingresar el número aleatorio".
Observar el error ReferenceError en la consola. Solución: El error en el código era un selector CSS incorrecto. Se corrigió la línea:
JavaScript

const lowOrHi = document.querySelector('lowOrHi');
a:
JavaScript

const lowOrHi = document.querySelector('.lowOrHi');
para seleccionar correctamente el elemento HTML con la clase lowOrHi.
Error 2: TypeError: Cannot read properties of null (reading 'addeventListener') (Potencialmente en Consola si el botón no se crea correctamente)
Descripción del Error: Si el botón de reinicio no se creaba o adjuntaba al DOM correctamente, la consola podría mostrar un TypeError al intentar agregarle un listener.
Pasos para Reproducir:

Jugar una partida hasta el final (ganar o perder).
Inspeccionar la consola del navegador. Si el botón de reinicio no aparece o no funciona, podría haber un error relacionado con su creación o el intento de agregar el listener. Solución: Se verificó y corrigió la lógica de creación y adición del botón de reinicio en la función setGameOver(). También se corrigió el error de escritura en addEventListener en ambas asignaciones de listeners (guessSubmit y resetButton). La ortografía correcta es addEventListener.
Error 3: Lógica Invertida en los Mensajes de Ganar y Perder (Deducido por Comportamiento, no directamente un error de JavaScript en consola)
Descripción del Error: Aunque la consola no mostraba un error de JavaScript directamente relacionado con esta lógica, el comportamiento incorrecto del juego (mostrar "Perdistes" al adivinar y felicitar al perder todos los intentos) indicaba un error lógico.
Pasos para Reproducir:

Abrir el juego en el navegador.
Ingresar el número correcto. Observar el mensaje incorrecto de derrota.
Fallar intencionalmente todos los intentos. Observar el mensaje incorrecto de felicitaciones. Solución: Se corrigió la lógica condicional (if y else if) dentro de la función checkGuess() para mostrar los mensajes correctos y aplicar las clases CSS adecuadas (verde para ganar, rojo para perder).
Error 4: Generación Incorrecta del Rango del Número Aleatorio (Deducido por Comportamiento)
Descripción del Error: No había un error de JavaScript directo en la generación del número aleatorio que se mostrara en la consola, pero el rango limitado (0-9) era evidente al jugar.
Pasos para Reproducir:

Abrir el juego en el navegador.
Jugar varias partidas, notando que el número a adivinar parecía estar siempre dentro de un rango pequeño. Solución: Se corrigió la fórmula para generar el número aleatorio a Math.floor(Math.random() * 100) + 1; para asegurar un rango entre 1 y 100.
Error 5: Falta de Validación de Entrada (Deducido por Comportamiento)
Descripción del Error: La consola no mostraba un error cuando se ingresaba texto, pero el juego no manejaba esta situación correctamente según los requisitos.
Pasos para Reproducir:

Abrir el juego en el navegador.
Ingresar texto en el campo de adivinanza.
Hacer clic en "Ingresar el número aleatorio".
Observar que no se mostraba ninguna alerta y el juego intentaba procesar la entrada no numérica. Solución: Se añadió la validación con isNaN(parseInt(userGuess)) dentro de checkGuess() para mostrar una alerta si la entrada no es un número entero.
Error 6: Reinicio Incorrecto del Número Aleatorio (Deducido por Comportamiento)
Descripción del Error: Después de reiniciar el juego, el número aleatorio generado parecía ser consistentemente 1, lo que no generaba un error en la consola pero sí un comportamiento incorrecto.
Pasos para Reproducir:

Jugar una partida hasta el final.
Hacer clic en "Comienza un nuevo juego".
Intentar adivinar el nuevo número, notando que casi siempre era 1. Solución: Se corrigió la lógica de generación del número aleatorio en la función resetGame() para utilizar la misma fórmula correcta que al inicio del juego.

### Mejora: Validación de Entrada para Números Enteros negativos 
**Descripción del Requisito:** El juego no debe admitir números negativos.
**Comportamiento Anterior:** El código permitía ingresar números negativos, `parseInt()` solo tomaba la parte entera.
**Pasos para Probar:**
1. Abrir el juego en el navegador.
2. Ingresar un número negativo (ej. -10) y hacer clic en "Ingresar el número aleatorio".
3. Observar que el juego procesaba la entrada de forma incorrecta (o tomaba solo la parte entera).
**Solución Implementada:** Se agregaron validaciones en la función `checkGuess()` para:
* Mostrar una alerta si el número entero parseado es menor que 1 (para evitar negativos y cero).
* (Opcional) Mostrar una alerta si el número entero parseado es mayor que 100.
En estos casos, se muestra un mensaje de alerta al usuario, se limpia el campo de entrada y no se incrementa el contador de intentos.

### Reporte de Error y Solución: Fallo en el Restablecimiento de Colores al Reiniciar el Juego

**Descripción del Error:**
Al hacer clic en el botón "Comienza un nuevo juego", el juego se reiniciaba correctamente en términos de contador de intentos y generación de un nuevo número aleatorio. Sin embargo, el color de fondo del mensaje de resultado (`<p class="lastResult">`) no se restablecía adecuadamente. Si el juego anterior terminaba con un mensaje verde o rojo, este color persistía al comenzar un nuevo juego, impidiendo que los nuevos mensajes de "Incorrecto!", "Felicitaciones!", o "!!!Pérdistes!!!" se mostraran con sus colores correspondientes (negro, verde, rojo respectivamente).

**Pasos para Reproducir el Error (Antes de la Corrección):**
1. Jugar una partida hasta adivinar el número (mensaje verde) o agotar los intentos (mensaje rojo).
2. Hacer clic en el botón "Comienza un nuevo juego".
3. Ingresar una nueva adivinanza.
4. Observar que el color de fondo del mensaje de resultado (`<p class="lastResult">`) mantenía el color del juego anterior (verde o rojo) en lugar de mostrar el color negro para un intento incorrecto inicial.

**Impacto del Error:**
Este error afectaba la retroalimentación visual al usuario, ya que los colores de los mensajes de resultado no indicaban correctamente el estado del nuevo juego después de un reinicio. Esto podía generar confusión sobre si el juego realmente se había restablecido por completo.

**Solución Implementada:**
Se modificó la función `resetGame()` en el archivo `script.js` para asegurar que el estilo de fondo en línea del elemento `lastResult` se elimine al reiniciar el juego. Se añadió la siguiente línea de código:

```javascript
lastResult.style.backgroundColor = '';