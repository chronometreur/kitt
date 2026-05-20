# KITT

KITT es un framework CLI para inicializar proyectos optimizados para desarrollo asistido por IA.

Su objetivo es generar automáticamente la estructura, contexto, convenciones, workflows y memoria contextual necesarios para trabajar eficientemente con herramientas como Cursor, Claude Code, Codex, Copilot y otras plataformas de AI-assisted development.

## Objetivos

- Generar contexto reutilizable para herramientas AI
- Estandarizar workflows de desarrollo asistido por IA
- Soportar múltiples stacks y arquitecturas
- Facilitar spec-driven development
- Reducir fricción al iniciar nuevos proyectos
- Centralizar prompts, conventions, skills y memoria contextual
- Permitir composición modular de templates y workflows

## Ejemplo

```bash
kitt init \
  --language ruby \
  --framework rails \
  --type api \
  --db postgres \
  --workflow spec-driven \
  --testing rspec
```

## Estructura generada

```txt
.ai/
  prompts/
  workflows/
  conventions/
  skills/
  memory/

docs/
  specs/
  architecture/
  decisions/
```

## Conceptos principales

Kitt funciona mediante composición modular de:

- languages
- frameworks
- databases
- workflows
- prompts
- conventions
- skills
- AI tool integrations

Cada módulo puede contribuir:

- archivos
- templates
- reglas
- convenciones
- configuración
- contexto AI
- estrategias de merge

## Arquitectura

El proyecto está dividido en:

```txt
packages/
  cli/
  core/
  template-sdk/
  templates-base/
  plugins/
```

## Estado del proyecto

KITT se encuentra actualmente en etapa experimental y de diseño arquitectónico.

## Licencia

Business Source License 1.1 (BUSL-1.1)

Ver el archivo LICENSE para más información.
