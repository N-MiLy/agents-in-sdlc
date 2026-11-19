---
title: "Ejercicio 3 - Prueba la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En vez de probar la aplicación manualmente, conectarás el **servidor MCP de Playwright** y pedirás a Copilot que controle un navegador para probarla. Después enviarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas.
- añadirás el servidor MCP de Playwright al espacio de trabajo.
- pedirás a Copilot que lo use para probar manualmente la funcionalidad en un navegador.
- enviarás la rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a recursos externos al editor. Copilot usa Model Context Protocol (MCP) para comunicarse con herramientas y servicios externos mediante un estándar común.

![Diagrama del funcionamiento del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las de servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite a los agentes de IA comunicarse con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y los servicios externos, como API existentes o herramientas locales como paquetes NPM. Cada servidor MCP representa un conjunto distinto de herramientas y recursos disponibles para el agente.

Algunos servidores MCP populares son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a varias API para gestionar repositorios de GitHub. Permite, por ejemplo, crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: ofrece automatización del navegador con Playwright. Permite navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> En materia de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y evalúa las implicaciones de seguridad. Usa únicamente servidores en los que confíes y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Deberías ver que ya hay un servidor `github` configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye esta entrada `github`, por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Usa el [servidor remoto de GitHub MCP][remote-github-mcp-server], así que no hay que instalar nada localmente: VS Code se conecta por HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`, de modo que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es **local**: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` hace que Playwright ejecute el navegador sin una ventana visible, necesario en un Codespace que no tiene escritorio.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, así que normalmente el navegador necesario ya está instalado. Si Copilot informa más adelante de que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot requiere las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, aparecerá la acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y selecciona la opción para confiar en él.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code lo iniciará y te pedirá que confirmes que confías en él la primera vez que Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] permite que Copilot controle un navegador real. En vez de que tú selecciones elementos de la aplicación para comprobar el trabajo, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados. Después, puede resumir lo observado. Es una forma rápida de verificar el comportamiento sin salir de la conversación.

El servidor MCP de Playwright utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página, no capturas de pantalla. Por eso el agente trabaja con elementos estructurados y etiquetados (botones, enlaces y elementos de lista), de forma parecida a las tecnologías de asistencia. Así, una comprobación funcional rápida también ofrece una comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en marcha, pide a Copilot que pruebe la funcionalidad de filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, realizará cada paso y comunicará lo que haya observado. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona, haz preguntas de seguimiento o pide que corrija el código antes de abrir una solicitud de incorporación de cambios.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot usa el servidor MCP de Playwright, es posible que tenga que descargar un navegador. Si informa de que falta uno, pídele que ejecute `npx playwright install chromium` y lo vuelva a intentar.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Envía la rama y crea una solicitud de incorporación de cambios

Ya has confirmado que la funcionalidad funciona y puedes abrir una solicitud de incorporación de cambios (PR) para que el equipo la revise. Primero publicarás la rama `filtering-vscode`.

1. Abre el panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una PR, por ejemplo, desde github.com o con la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, pídele que la cree. Puede buscar la incidencia pertinente y relacionarla con la PR.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en el menú desplegable de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice operaciones. La primera vez que use una herramienta de GitHub, quizá debas autenticarte; sigue las indicaciones.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace que genera Copilot para revisar la PR, pero **no la combines todavía**.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio:

- aprendiste qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas.
- añadiste el servidor MCP de Playwright al espacio de trabajo.
- lo usaste para probar manualmente la funcionalidad en un navegador antes de publicarla.
- enviaste la rama y abriste una solicitud de incorporación de cambios.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
