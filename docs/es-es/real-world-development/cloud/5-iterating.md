---
title: "Ejercicio 5: Iteración sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Pediste a Copilot que documentara el código, desarrollara una función de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera mejoras de accesibilidad, incluidos los modos de contraste alto y claro. Vamos a revisar los cambios y, si es necesario, dar indicaciones a Copilot para mejorarlos.

### Escenario

Como hemos visto varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisaremos las sugerencias de GitHub Copilot para añadir documentación, juegos relacionados y mejoras de accesibilidad antes de permitir que el resto del equipo revise los cambios.

## Seguridad y el agente de Copilot en la nube

El agente de Copilot en la nube trabaja de forma asíncrona y sin supervisión, por lo que existen medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama en la que trabaja.
- El agente se ejecuta en GitHub Actions, dentro de un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) que ha generado el agente de Copilot en la nube: añadir documentación al código. Usarás la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, es posible que veas un aviso de que el firewall ha bloqueado a GitHub Copilot. Es **normal**: Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente de Copilot en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation** o algo parecido.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo aprendido hasta ahora.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que ahora incluye comentarios TSDoc y otra documentación. Los cambios concretos pueden variar. Busca los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario: proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la cola de comprobaciones de la PR. Si todo va bien, verás que se superan las comprobaciones del proyecto, que ahora es una aplicación Astro única. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una PR no es una interacción unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios insertados en el código, igual que harías con otra persona del equipo. Copilot verá los comentarios e iniciará otra sesión para resolverlos. Como los resultados son probabilísticos, no podemos recomendar un texto concreto. Puedes pedirle, por ejemplo:

- que añada encabezados de comentario al principio de cada archivo de código con una breve descripción.
- que añada comentarios TSDoc a los archivos TypeScript y Astro.
- que cree un README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario en la PR con una solicitud de cambio y menciona a **@copilot**, como harías con cualquier usuario. Puedes elegir una de las ideas anteriores o pedir otra mejora de documentación.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Iniciará una sesión nueva para realizar los cambios.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot haya terminado, aparecerá una confirmación nueva en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Volvamos a la PR que Copilot ha creado para resolver la incidencia sobre los juegos relacionados en la página de detalles de un juego.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que avisa de que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos aparecerán en la sección de comprobaciones de la PR. Si todo va bien, verás que se superan las comprobaciones de la aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace y probar manualmente la función de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente en la nube y solicitar cambios, por ejemplo, ajustar el número de juegos relacionados o añadir encabezados de comentario y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Añade un comentario en la pestaña **Conversation** para pedir los cambios; Copilot lo verá e iniciará otra sesión.

## Revisa las mejoras de accesibilidad

Por último, revisaremos las mejoras de accesibilidad implementadas mediante el agente personalizado de accesibilidad. Esta PR debe incluir tanto el modo de contraste alto que asignaste en el ejercicio 3 como el modo claro solicitado desde mission control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot aún está trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz para alternar entre modos.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios CSS o de estilos para los modos de contraste alto y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - el código JavaScript/TypeScript que gestiona los cambios de modo.
6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos aparecerán en la sección de comprobaciones de la PR. Si todo va bien, verás que se superan las comprobaciones de la aplicación Astro. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las mejoras de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los selectores de contraste alto y modo claro en el navegador. Confirma que funcionan y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas problemas o mejoras posibles, menciona a **@copilot** en un comentario y solicita los cambios, como hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil cuando puedes asignarle incidencias reales del backlog y ocuparte de otras tareas. Para adquirir el hábito, crea más incidencias en el repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario de interés para patrocinadores en la página de detalles del juego.
- añadir paginación a la lista de juegos.
- añadir validación de entradas y gestión de errores a los asistentes de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizados del repositorio** para reconocer su efecto en los resultados del agente.
- **Asignaste incidencias al agente de Copilot en la nube** y observaste cómo configuraba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y usaste un agente personalizado** de accesibilidad para añadir los modos de contraste alto y claro.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad.
- **Revisaste e iteraste sobre las solicitudes de incorporación de cambios del agente**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Revisión y pasos siguientes

Has completado el itinerario del agente en la nube. Si quieres seguir explorando, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal mediante los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y la opción `/delegate` para volver a delegar al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente una incidencia de refactorización o cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Información sobre los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para configurar el flujo de trabajo del agente de Copilot en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Supervisión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
