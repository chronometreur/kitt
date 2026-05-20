# Workflow 01: Descubrimiento y Especificación Técnica (Versión Alfa)

**Estrategia:** Spec-Driven Development (SDD)  
**Dependencia Global:** Requiere cumplir con las especificaciones de `00_system_core.md`. Si `00_system_core.md` no está en el mismo directorio de este archivo detente e indica que faltan las especificaciones de control de sesión y persistencia de avance del workflow.
**Instrucción de Activación:**  
> *"Usa el workflow 01 para trabajar con el ticket/card [ID_DEL_TICKET]"*

---

## 📋 Secuencia Específica del Workflow

Este flujo opera de forma interactiva y secuencial en la misma sesión del chat. Utiliza los estados de la sesión local para coordinar la transición entre el análisis funcional y el diseño técnico.

```
 [Entrada: ID del Ticket]
            │
            ▼
  ┌───────────────────┐
  │      NODO 1       │ ──> Genera: `.ai/features/[ID]/01_1.md`
  └───────────────────┘     Actualiza: `session.md` -> `w01_p1: generado pendiente de aprobacion`
    SystemPrompt: ProductAnalystPersona
    Capabilities: ReadTask, ReadKnowledgeBase
            │
            ▼ [Aprobación Humana: Desarrollador escribe "Aprobado"]
            │
  ┌───────────────────┐
  │  NODO 2 [PEND.]  │ ──> Genera: `.ai/features/[ID]/TECHNICAL_SPEC.md`
  └───────────────────┘     Actualiza: `session.md` -> `w01: completado`
    SystemPrompt: TechnicalArchitectPersona [falta crear]
    Capabilities: ReadCodebase, DrawArchitecture
```

---

## 🛠️ Definición de Nodos y Prompts

### Nodo 1: Analista de Producto (Product Analyst Node)
* **Objetivo:** Refinar el requerimiento de negocio, validar criterios funcionales y eliminar ambigüedades.
* **Prompt Operativo:** `ProductAnalystPersona` (Especializado en desglose funcional y estructuración en escenarios Gherkin).
* **Capacidades Activas:** `ReadTask`, `ReadKnowledgeBase`.
* **Comportamiento Específico:** 
    1. Lee el contenido del ticket y cruza con la documentación existente.
    2. Escribe el desglose de reglas funcionales en `.ai/features/[ID_DEL_TICKET]/01_1.md`.
    3. Registra en `session.md`: `w01_p1: generado pendiente de aprobacion`.
    4. Detiene la ejecución y solicita confirmación explícita al desarrollador en el chat.

### Nodo 2: Arquitecto de Software (Software Architect Node) — **[PENDIENTE: falta `TechnicalArchitectPersona`]**
* **Objetivo:** Traducir los requerimientos funcionales aprobados en un plano técnico ejecutable y acoplado a la arquitectura del proyecto.
* **Prompt Operativo:** `TechnicalArchitectPersona` — **aún no existe en `prompts/people/`. No ejecutar este nodo hasta que el archivo esté disponible.**
* **Capacidades Activas:** `ReadCodebase`, `DrawArchitecture`.
* **Comportamiento Específico:**
    1. Se activa únicamente tras recibir la confirmación humana del paso anterior.
    2. Actualiza `session.md` a: `w01_p1: generado y aprobado` y `w01_p2: en progreso`.
    3. Lee el archivo local `01_1.md` e inspecciona el estado actual del repositorio.
    4. Genera el entregable final en `.ai/features/[ID_DEL_TICKET]/TECHNICAL_SPEC.md`.
    5. Finaliza actualizando `session.md` a: `w01: completado`.

---

## 📑 Contrato del Entregable Final (`TECHNICAL_SPEC.md`)

El documento final producido por el Nodo 2 en la carpeta del ticket debe cumplir estrictamente con la siguiente estructura:

```markdown
# TECHNICAL_SPEC: [ID_DEL_TICKET] - [Título del Requerimiento]

## 1. Resumen y Contexto Funcional
* Breve descripción del cambio desde la perspectiva del usuario (derivado de `01_1.md`).

## 2. Arquitectura de Datos y Modelo de Entidades
* Modificaciones a tablas existentes o creación de nuevos modelos (incluye diagramas Mermaid.js).

## 3. Contrato de Interfaz / API (Si aplica)
* Definición de nuevos endpoints, payloads de petición y códigos de respuesta.

## 4. Plan de Impacto en Archivos (Impact Analysis)
* **Archivos a modificar:** Ruta exacta de archivos existentes a alterar.
* **Archivos a crear:** Nuevos archivos requeridos por la arquitectura.

## 5. Estrategia de QA y Casos de Prueba (Test Plan)
* Lista explícita de pruebas unitarias y de integración necesarias en lenguaje Gherkin.
```
