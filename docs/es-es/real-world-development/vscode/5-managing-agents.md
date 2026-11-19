---
title: "Ejercicio 5 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando GitHub Copilot trabaja en modo de agente, explora el código, propone cambios, edita archivos y ejecuta comandos de forma autónoma. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat de VS Code muestra cada llamada a herramienta y cada edición a medida que ocurren. Puedes revisar cada diff en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento para mejorar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- indicarás al agente durante la sesión que añada un selector de modo claro junto al de contraste alto.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de contraste alto. Mientras trabaja, el equipo de diseño añade otra solicitud: también quiere un modo claro para las personas a quienes les resulta demasiado oscuro el tema predeterminado. En vez de empezar otra conversación, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver las llamadas a herramientas. Aparecerán lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, aparecerá su diff:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y pedir al agente que lo intente de nuevo.
4. También puedes abrir **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta requiere permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará un aviso **Allow**. Aprueba únicamente los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de contraste alto. Deberías ver un mensaje final que resuma los cambios.

## Pide al agente que añada el modo claro

Ahora que ya está listo el modo de contraste alto, amplía la misma conversación para añadir un selector de modo claro. Como continuarás en la misma sesión, el agente conserva el contexto del trabajo anterior: los componentes que modificó, el enfoque de estilos y el uso de almacenamiento local.

1. Escribe este prompt de seguimiento en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde el trabajo anterior, revisará los cambios y propondrá cómo añadir el modo claro.
3. Revisa cada edición nueva a medida que aparezca, igual que hiciste con el modo de contraste alto. Usa **Keep** o **Undo** en cada diff.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta rediseñar los controles de alternancia—, detenlo con el botón **Stop** junto al campo de chat y envíale una indicación para aclarar lo que necesitas.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para pedir ajustes, como «simplifica el CSS y usa variables CSS en lugar de duplicar selectores», corregir el rumbo, como «no añadas un tercer selector; integra el modo claro en el panel de configuración», o ampliar el alcance, por ejemplo, «añade también métodos abreviados de teclado para los dos selectores».

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos modificados por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa sus diff. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y almacenamiento local para conservar las preferencias.
4. Cuando estés conforme, prepara y confirma los cambios en el panel Source Control. Los enviarás en [la siguiente lección][next-lesson].

## Resumen y pasos siguientes

Has usado Copilot Chat en modo de agente para supervisar y dirigir una sesión en curso: has revisado cada edición al aparecer, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- supervisaste el trabajo de un agente en tiempo real.
- aceptaste y rechazaste ediciones individuales.
- dirigiste la sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios realizados por el agente personalizado durante este taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
