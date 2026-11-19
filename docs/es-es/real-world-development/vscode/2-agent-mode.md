---
title: "Ejercicio 2 - Añade funciones con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo de trabajo de un desarrollador suele consistir en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, hacer otro cambio… y así sucesivamente.

Para eso sirve el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar con mayor autonomía en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a ver cómo usar el modo de agente para añadir funciones al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en tu IDE y permite ciclos de respuesta rápidos e interacción. El agente en la nube trabaja de forma asíncrona como otro miembro del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- hacer que el modo de agente de Copilot explore el proyecto e identifique los archivos pertinentes.
- usar el modo de agente de GitHub Copilot para implementar funciones en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por el modo de agente antes de combinarlos con el código.

## Escenario

A medida que crece la lista de juegos, quieres que se puedan filtrar por categoría y editor. En el ejercicio anterior añadiste un asistente para editores. Ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio para entender su funcionalidad actual.

El sitio es una plataforma de financiación colectiva de juegos de mesa con una temática para desarrolladores. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los asistentes reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se implementa con scripts Astro pequeños y acotados que usan las API DOM estándar.

### Inicia el sitio

Para facilitar la ejecución del sitio, se ha configurado la tarea `npm run dev`, que inicia el único servidor de desarrollo Astro. Puedes ejecutarlo en tu GitHub Codespace así:

1. Vuelve a tu Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal en Codespaces con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio con el siguiente comando:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un Codespace, seleccionar el enlace localhost en el terminal redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al Codespace, que ahora aloja el servidor web.

### Explora el sitio

Cuando el sitio esté en marcha, explora su funcionalidad:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso los indica cuando corresponde.

La implementación inicial del sitio funciona, pero queremos añadir capacidades. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente una serie de incidencias. Pide a GitHub Copilot que te las muestre.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos de terminal y usa herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo concreto. Durante su desarrollo usamos Claude Sonnet 4.5 y lo indicamos para que la experiencia sea más coherente. Sin embargo, como los modelos de lenguaje son probabilísticos, las sugerencias pueden diferir de las descritas en el taller. Es normal.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje, Copilot puede usar un comando MCP distinto y aun así completar la tarea.

3. Pide a Copilot que consulte las incidencias del backlog con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En Codespaces, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funciones:
   - descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a instrucciones específicas para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia a otros archivos de instrucciones y a los archivos del proyecto. Las rutas son relativas a la ubicación del propio archivo. Así puedes reutilizar directrices, dividir conjuntos grandes en archivos más manejables e indicar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, habrá que realizar al menos tres cambios independientes en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y confirmar que se superan antes de combinar los cambios. El modo de agente de Copilot puede hacer todo esto.

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad descrita en la incidencia del backlog con este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad: los asistentes de la capa de datos, la interfaz y las pruebas. Después empieza a modificar los archivos y ejecutar las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código, puede tardar unos minutos. Durante el proceso quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, podrás aceptar o deshacer todos los cambios; no es necesario seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas o que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Captura de una sesión completada de Copilot Chat en modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio has usado el modo de agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. Has aprendido:

- cómo implementar funciones en la interfaz y la capa de datos con el modo de agente.
- cómo puede explorar el proyecto e identificar archivos pertinentes para coordinar cambios.
- cómo revisar los cambios y las pruebas antes de combinarlos.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido para pedir a Copilot que añada esta función. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas, incluidos los archivos de instrucciones existentes.
- decide cómo se implementará la paginación y si el usuario podrá elegir el tamaño de página o será fijo.
- proporciona a Copilot las indicaciones necesarias para crear la implementación que deseas.
- es posible que tengas que iterar y pedir cambios proporcionando contexto. Es el proceso normal cuando se trabaja con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent; qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
