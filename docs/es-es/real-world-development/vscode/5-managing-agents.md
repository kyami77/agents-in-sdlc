---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat de VS Code muestra cada llamada a herramientas y cada edición mientras ocurren. Puedes revisar cada diff, aceptar o rechazar cambios y orientar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- orientarás el agente durante la sesión para que añada un conmutador para el modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para las personas a quienes les resulte demasiado oscuro el tema predeterminado. En vez de iniciar una conversación nueva, guiarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada cambio aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal que pide ejecutar.
3. Cuando el agente proponga un cambio, aparecerá un diff:
   - selecciona **Keep** para aceptar el cambio.
   - selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará la solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Pide al agente que añada el modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la misma conversación para añadir un conmutador para el modo claro. Como continuarás en la misma sesión, el agente conserva el contexto de su trabajo: los componentes que modificó, el enfoque de estilo y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento desde el cuadro de Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde donde lo dejó, explorará sus cambios anteriores y propondrá los cambios necesarios para el modo claro.
3. Revisa cada cambio nuevo cuando aparezca, como hiciste con el modo de alto contraste. Utiliza **Keep** o **Undo** para decidir si lo aceptas.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si intenta reorganizar la interfaz de los conmutadores—, detenlo con el botón **Stop** junto al cuadro de chat y envíale una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Utiliza prompts de seguimiento para solicitar ajustes («simplifica el CSS: utiliza variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer conmutador; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos conmutadores»).

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa brevemente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver actualizaciones de componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa los diffs. Confirma que se aplican las prácticas del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y almacenamiento de preferencias local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y próximos pasos

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición al aparecer, has aceptado o rechazado cambios y has enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- supervisaste el trabajo del agente en tiempo real.
- aceptaste y rechazaste cambios individuales.
- orientaste la sesión con prompts de seguimiento.

En la [lección siguiente][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que ha generado el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Lección siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
