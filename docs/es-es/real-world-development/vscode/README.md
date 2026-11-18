---
slug: es-es/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** incorpora GitHub Copilot al editor de código que ya utilizas. En Visual Studio Code y GitHub Codespaces, guiarás Copilot Chat en el modo de agente, conectarás herramientas externas mediante MCP y usarás agentes personalizados, todo sin salir del IDE, donde Copilot puede acceder a tus archivos, al terminal y a los problemas.

Empezarás añadiendo instrucciones personalizadas y observando cómo las sigue Copilot. Después, usarás el modo de agente para crear una funcionalidad de filtrado que afecta a la interfaz, la capa de datos y las pruebas. A continuación, conectarás el servidor Playwright MCP y dejarás que Copilot controle un navegador para probar la funcionalidad antes de abrir una solicitud de incorporación de cambios. Por último, revisarás y usarás un agente personalizado para tareas de accesibilidad y supervisarás, guiarás e iterarás sobre los cambios de Copilot sin salir del editor.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y Codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade y valida instrucciones personalizadas en VS Code |
| [2. Modo de agente][ex2] | Generación de código | Desarrolla una funcionalidad de filtrado con el modo de agente |
| [3. Prueba con Playwright MCP][ex3] | Herramientas externas | Prueba la funcionalidad en un navegador con el servidor Playwright MCP |
| [4. Agentes personalizados][ex4] | Agentes especializados | Revisa y usa agentes personalizados |
| [5. Supervisar y gestionar agentes][ex5] | Supervisión | Supervisa y guía las sesiones del agente |
| [6. Iterar][ex6] | Revisión | Revisa el trabajo de Copilot en local y decide los siguientes pasos |
| [Opcional: Incorpora Foundry][foundry-toolkit] | Agentes de IA | Prepara un modelo, implementa un agente y conéctalo al sitio en tres módulos con VS Code y Foundry Toolkit |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Acceso a GitHub Codespaces

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que se usan en este taller, por lo que podrás completar todos los recorridos.

[callout-student-plan-education]: https://github.com/education/students

## Primeros pasos

**[Empieza con el ejercicio 0: Requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
