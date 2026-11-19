---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa en conjunto el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora toca revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Combinaste manualmente la PR de la valoración con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- habilitarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

Durante el trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR sin quitar al desarrollador el control sobre la autorización de la combinación.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Al habilitarlo, Copilot resuelve los impedimentos que bloquean la PR: corrige las comprobaciones fallidas de integración continua (CI), responde a comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando sea necesario. Después habilita la combinación automática de GitHub si el repositorio la permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Deja que Agent Merge gestione la PR

Ya has creado todo el código. Vamos a revisarlo, crear la PR y dejar que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo de agentes con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se complete correctamente, realizará la combinación.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos necesarios. Si el proceso se bloquea, revisa el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y pasos siguientes

Has automatizado varios aspectos del desarrollo: la generación de código, las pruebas y la validación del código, y ahora el proceso de solicitud de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionaste todos los cambios de la funcionalidad y las pruebas de verificación.
- creaste la PR de filtrado.
- habilitaste Agent Merge solo después de la revisión y confirmaste que la PR se había combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
