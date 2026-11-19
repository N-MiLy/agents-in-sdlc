---
title: "Ejercicio 4 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, pediste al agente de Copilot en la nube que se encargara de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. El agente trabaja de forma asíncrona y autónoma, pero sigue siendo importante supervisar estas tareas.

Dispones de varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas, explorar las operaciones realizadas e incluso dirigir una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de contraste alto, el equipo se dio cuenta de que también era un buen momento para añadir un modo claro. Como ya se estaba trabajando en actualizar los estilos del sitio y añadir una función de alternancia, parecía lógico incluirlo. Dirigirás el trabajo del agente para asegurarte de que incorpore tanto el modo claro como el de contraste alto.

## Revisa las tareas del agente de Copilot en la nube

Vamos a consultar el estado actual de las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de contraste alto al sitio web.
3. Selecciona una de las tareas en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar distintas operaciones para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de PR, que aparece a la derecha y permite consultar la PR y los archivos modificados para seguir el trabajo.

## Dirige al agente en la nube

Ya has visto las tareas en curso. Ahora pide a Copilot que añada una alternancia de modo claro mientras trabaja en el modo de contraste alto.

1. Selecciona la sesión relacionada con el modo de contraste alto. El título exacto dependerá del nombre que Copilot haya asignado y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes similares a los que se muestran a continuación.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso que estaba ejecutando al recibir el mensaje.

Como antes, el proceso tardará varios minutos. Puedes aprovechar para repasar lo que has aprendido hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y dirigir las tareas del agente de Copilot en la nube. Puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para cambiar su alcance o proporcionarle más instrucciones.

Has aprendido:

- a usar mission control y la página de agentes para supervisar las tareas del agente en la nube.
- a redirigir una sesión en curso para solicitar funcionalidades adicionales.

Mientras Copilot termina de trabajar en las mejoras de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Información sobre los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
