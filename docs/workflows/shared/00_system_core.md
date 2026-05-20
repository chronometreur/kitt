# 00_System_Core (Estructura Global del SDLC Asistido por IA)

**Propósito:** Este documento define el núcleo operativo, la arquitectura de persistencia, la estructura de directorios y el mecanismo de control de estado que rigen a TODOS los workflows de desarrollo del proyecto. Cualquier workflow individual debe apegarse a estas reglas globales.

---

## 📂 1. Estructura de Directorios y Persistencia de Contexto

Para garantizar la resiliencia del contexto ante sesiones de chat efímeras (cierres de pestaña, reinicios de IDE o CLI), todos los artefactos intermedios y finales generados por los flujos de trabajo se organizan bajo un subdirectorio con el nombre o número del ticket/card dentro de la carpeta central de IA.

### Árbol de Directorios Estándar del Proyecto
```text
.ai/
├── workflows
|   ├── 00_system_core.md                   <-- Este archivo (Reglas globales)
|   ├── [01_XX_workflow_name].md            <-- Definición de Workflows específicos
└── features/
    └── [ID_DEL_TICKET_O_CARD]/
        ├── session.md                      <-- Bitácora mínima de avance inter-workflow
        ├── [XX_Y].md                       <-- Artefactos intermedios (Workflow XX, Paso Y)
        └── [ENTREGABLE_FINAL_DEL_WF]       <-- Contrato final de cada workflow
```

---

## 🔄 2. Mecanismo de Control de Estado (`session.md`)

El archivo `session.md` vive exclusivamente dentro de la carpeta de cada funcionalidad (`.ia/features/[ID_DEL_TICKET]/`). Actúa como la bitácora centralizada que permite el paso de estafeta entre pasos de un mismo workflow, así como la transición entre diferentes flujos del ciclo de vida (SDLC).

### Reglas de Sintaxis
1. **Minimalismo Extremo:** Su sintaxis es intencionalmente compacta (estilo clave-valor o tags sencillos) para optimizar el consumo de tokens y permitir que cualquier IA comprenda el estado del desarrollo en milisegundos.
2. **Estados Mutables:** Cada nodo/paso en ejecución tiene la obligación de reescribir este archivo para actualizar el progreso actual.

### Ciclo de Estados Estándar:

*   **Estado: Esperando Revisión Humana (Bloqueo en Paso Y del Workflow XX)**
    ```markdown
    ticket: [ID_DEL_TICKET]
    w[XX]_p[Y]: generado pendiente de aprobacion
    w[XX]_p[Y+1]: pendiente
    ```
*   **Estado: Aprobado y En Progreso Técnico**
    ```markdown
    ticket: [ID_DEL_TICKET]
    w[XX]_p[Y]: generado y aprobado
    w[XX]_p[Y+1]: en progreso
    ```
*   **Estado: Cierre de Workflow / Listo para Siguiente Etapa**
    ```markdown
    ticket: [ID_DEL_TICKET]
    w[XX]: completado
    w[XX+1]_p1: pendiente
    ```

---

## 🛠️ 3. Directiva Global para la IA (System Instructions Core)

Cuando un desarrollador invoque cualquier flujo mediante la instrucción de activación:
> *"Usa el workflow [NOMBRE/NUMERO] para trabajar con el ticket/card [ID_DEL_TICKET]"*

La IA ejecutora tiene la obligación estricta de:
1. **Validar la existencia del subdirectorio:** Si la carpeta `.ai/features/[ID_DEL_TICKET]/` no existe, debe crearla de inmediato junto con el archivo `session.md` inicial con el siguiente contenido:
    ```markdown
    ticket: [ID_DEL_TICKET]
    w[XX]_p1: pendiente
    ```
2. **Consultar el Estado Actual:** Leer `.ai/features/[ID_DEL_TICKET]/session.md` antes de responder. Si el archivo indica que un paso previo ya fue `generado y aprobado`, la IA no debe recalcular ese paso; debe saltar directamente al estado `pendiente` o `en progreso` indicado.
3. **Escritura antes de Responder:** Guardar los resultados en el formato indexado correspondiente (`[Workflow]_[Paso].md`) en `.ai/features/[ID_DEL_TICKET]/` en el disco local antes de renderizar la respuesta final en el chat.
