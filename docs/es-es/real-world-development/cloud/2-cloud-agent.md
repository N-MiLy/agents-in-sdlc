---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muy pocas organizaciones —si es que hay alguna— no tengan dificultades con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan mucho tiempo en el backlog por falta de tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea un borrador de solicitud de incorporación de cambios (PR) para que lo revise una persona. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

Con el agente de Copilot en la nube explorarás:

- cómo personalizar el entorno de generación de código.
- cómo asegurarte de que las operaciones se realicen de forma segura.
- por qué es importante definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: en realidad, no hay documentación. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego, para que las personas puedan seguir explorando. Por ahora no necesita una implementación pulida; solo algo que pueda utilizar para probar y aceptar la experiencia de usuario. Este trabajo está bloqueado, pero hay otras incidencias de mayor prioridad.

Ambos casos son ejemplos de tareas que pueden acabar relegadas y que resultan adecuadas para el agente de Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, puede revisar el trabajo de Copilot.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Igual que con una persona, puedes asignarle trabajo de distintas maneras, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después de la asignación, Copilot crea un borrador de PR para registrar el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras sigue en curso o una vez terminada. Cuando la solución esté lista para revisión, Copilot te mencionará en la PR.

## La importancia de definir bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el trabajo de pensar. Incluso las tareas aparentemente sencillas pueden ser complejas al examinarlas en detalle. Por eso, [define cuidadosamente el alcance de las tareas para el agente de Copilot en la nube][cloud-agent-best-practices]. Considéralo un programador colaborativo con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores hemos presentado las instrucciones personalizadas y su función. Si ya completaste el ejercicio práctico de instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, este es un buen momento para consultarlo. Antes de asignar trabajo al agente de Copilot en la nube, revisa los archivos de instrucciones que incluye el repositorio. La revisión es de solo lectura y te ayudará a reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace que ya esté activo:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los requisitos de comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que limita estas instrucciones a los archivos que coinciden con `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, observa si la PR incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Estas prácticas proceden de los archivos de instrucciones. Volveremos a señalarlas al revisar las PR en una lección posterior.

## Configura el entorno de desarrollo del agente de Copilot en la nube

Crear código suele requerir un entorno específico y ejecutar scripts de configuración para asegurarse de que está todo listo. Lo mismo se aplica cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas y dependencias de desarrollo necesarias. Este flujo ya está configurado para el taller y garantiza que Copilot tenga acceso a Node.js, las dependencias del proyecto, los navegadores necesarios para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada para la aplicación Astro única:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Para clonar el repositorio como parte de la configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona correctamente sin tener que esperar a que Copilot lo ejecute.

## Añade documentación

Todo el mundo entiende la importancia de la documentación, pero muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar sin resolver, lo que ralentiza el trabajo y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot destaca en la generación de documentación, así que esta es una buena tarea para asignarle al agente de Copilot en la nube. El agente trabajará en segundo plano y generará la documentación necesaria. En un ejercicio posterior revisarás su trabajo.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nuevas incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que se acaba de crear. Tendrá un título parecido a `[WIP]: Code lacks documentation`. Si aún no aparece en la lista, espera un momento y vuelve a cargar la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando su entorno mediante el flujo de trabajo descrito antes, analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que realizará.
14. Desplázate hacia abajo por la cronología de la PR. Deberías ver que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que volver a cargar la página para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en tiempo real para ver cómo Copilot resuelve el problema: explora el código y su estado, se detiene a pensar y decidir el enfoque y, finalmente, crea código.

El trabajo puede tardar varios minutos. Uno de los objetivos principales del agente de Copilot en la nube es permitir que realice tareas de forma asíncrona y liberarte para ocuparte de otras cosas. Aprovecharemos esa función asignándole otra tarea y, después, nos centraremos en crear código para añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente de Copilot en la nube es que permite dividir el trabajo: tú puedes centrarte en unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño no debería llevar mucho tiempo, puedes dedicar ese tiempo a otras tareas. ¡Vamos a asignársela al agente!

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nuevas incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás el emoji 👀 en el primer comentario de la incidencia. Indica que Copilot ya está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente de Copilot en la nube trabaja de forma parecida a un ingeniero de software: no hace falta supervisarlo continuamente, puedes revisar su trabajo cuando termine. Ahora vamos a crear y usar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona. Copilot puede ocuparse de deuda técnica, crear funcionalidades o ayudar a migrar código entre frameworks.

Has explorado estos conceptos:

- cómo personalizar el entorno de generación de código.
- cómo asegurarte de que las operaciones se realicen de forma segura.
- por qué es importante definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, vamos a centrarnos en crear y usar agentes personalizados. El [agente en la nube también puede usar servidores MCP][cloud-agent-mcp] y puede acceder a instrucciones personalizadas, que hemos explorado en módulos anteriores.

## Recursos

- [Información sobre el agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para configurar el agente de Copilot en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
