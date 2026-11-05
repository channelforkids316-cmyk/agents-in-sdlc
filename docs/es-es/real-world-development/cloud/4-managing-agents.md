---
title: "Ejercicio 4 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que realice tres tareas diferentes para mejorar la experiencia y añadir funcionalidad. Aunque trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, como la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver todas las tareas del agente que tienen solicitudes de incorporación de cambios (PR) abiertas, consultar las operaciones realizadas y orientar las sesiones en curso.

En esta lección:

- Explorarás la página de agentes para supervisar las tareas del agente en la nube.
- Orientarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de pedir al agente que creara un modo de alto contraste, el equipo decidió que sería buen momento para añadir también un modo claro. Como ya se están actualizando los estilos del sitio y añadiendo controles de selección, parece lógico incluirlo. Orientarás al agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Consulta el estado de las tareas que tiene asignadas el agente.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel de la izquierda. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - Actualizar la documentación del código.
   - Generar API para modificar juegos.
   - Añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en ejecución. Revisa las operaciones realizadas por Copilot, que pueden incluir:
   - Obtener una copia del código del repositorio.
   - Crear el entorno de trabajo de Copilot.
   - Configurar servidores MCP.
   - Realizar varias operaciones para completar la tarea.

> [!NOTE]
> Los pasos exactos de la lista dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de PR de la derecha. Permite ver la solicitud y los archivos modificados.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que Copilot haya usado y del estado del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot se ocupará de la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso que estaba ejecutando al recibir el mensaje.

Como el proceso puede tardar varios minutos, aprovecha para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente en la nube. Permite seguir las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para cambiar el alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- Usar Mission Control y la página de agentes para supervisar las tareas del agente en la nube.
- Redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot completa el trabajo de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
