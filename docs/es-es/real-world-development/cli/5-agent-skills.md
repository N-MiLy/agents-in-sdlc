---
title: "Lección 5 - Personaliza y usa la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar implica mucho más que escribir código. Ya hemos comprobado manualmente que el código funciona y hemos usado archivos de instrucciones para asegurarnos de que sigue nuestros estándares. Pero ¿qué ocurre con las pruebas, lint y los demás elementos de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para estas tareas. Ayudan a Copilot a entender cómo ejecutar operaciones como estas.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de sus resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que un agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la habilidad y un archivo Markdown llamado `SKILL.md`. El Markdown contiene metadatos de nombre y descripción, una explicación de lo que hace la habilidad y directrices sobre cuándo invocarla. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> No es obligatorio añadir archivos ni carpetas adicionales a una habilidad. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque usa los comandos existentes del proyecto.

Las habilidades se pueden guardar en `.github/skills` del proyecto para compartirlas y reutilizarlas en el equipo, o en la carpeta de habilidades del usuario `~/.copilot/skills`.

## Explora la habilidad

Vamos a explorar la habilidad que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos: `quality-checks`.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a determinar cuándo invocar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot por el proceso de pruebas y lint.
4. Fíjate en que la habilidad aún no tiene una sección **Results output formatting**.

## Ejecuta la habilidad antes de cambiarla

Las habilidades se pueden invocar directamente desde Copilot CLI o mediante lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Usa el siguiente prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final.

## Personaliza el informe

Queremos que el informe indique qué se ha ejecutado, si se ha superado y qué han comunicado realmente las herramientas. Actualiza la habilidad para generar ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Veamos cómo funciona el cambio. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Usa exactamente el mismo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final y compáralo con el primero.

## Resumen y pasos siguientes

Has personalizado y usado una habilidad de agente existente. En esta lección:

- exploraste la habilidad `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos.
- personalizaste el formato de sus resultados.
- volviste a cargar y ejecutaste la habilidad.

Este cambio se incluirá con el filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
