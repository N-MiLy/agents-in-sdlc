---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definirlos mediante archivos Markdown en la carpeta `.github/agents` del repositorio, proporcionas a Copilot instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos para que pueda realizar determinados tipos de trabajo de forma más eficaz. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown en `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que incluye al menos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] comparten algunos aspectos: ambos suelen definirse mediante archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La diferencia más sencilla es que el **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes para realizar el trabajo. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad. Como parte del trabajo, podría invocar habilidades como la de gestión de solicitudes de incorporación de cambios o la de ejecución y gestión de pruebas.

> [!NOTE]
> No hay una única manera correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar la que mejor se adapte al entorno y a las tareas.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados explorarás estos conceptos:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, con independencia de sus capacidades visuales o preferencias. Los comentarios de usuarios indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al contraste insuficiente entre el texto y el fondo. Para solucionar el problema, el equipo de diseño ha solicitado un modo de contraste alto que se pueda activar y desactivar.

La accesibilidad es importante, así que quieres implementar esta mejora cuanto antes. Para generar la funcionalidad usarás un agente personalizado.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisemos su contenido para entender cómo guiará a Copilot.

Vuelve a tu Codespace y revisa el archivo del agente:

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` son obligatorios en el frontmatter de los agentes personalizados.

3. Lee las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio accesible.
   - las prácticas recomendadas para la accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Crea y asigna una incidencia

Mission control es el lugar central para gestionar todos los agentes de tu entorno. Desde allí puedes asignar tareas al agente de Copilot en la nube, supervisarlas y redirigir una sesión en curso o proporcionarle indicaciones adicionales. Vamos a asignar a Copilot la tarea de crear el modo de contraste alto.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nuevas incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Captura del cuadro de asignación del agente en la nube, con los campos del agente personalizado y de accesibilidad resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en la tarea en segundo plano.

## Resumen y pasos siguientes

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot: asistentes de IA especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has aprendido:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

Mientras Copilot trabaja en el modo de contraste alto, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde mission control.

## Recursos

- [Información sobre los agentes personalizados][custom-agents]
- [Preparación de una organización para usar agentes personalizados][org-custom-agents]
- [Preparación de una empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisión y gestión de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
