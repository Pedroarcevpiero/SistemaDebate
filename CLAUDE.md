# CLAUDE.md — round-based-debate-council

## Propósito del proyecto

- Este proyecto **solo** sirve para debates por rondas.
- **No** usarlo como asistente general.

## Cuándo usar la skill

- Cuando el usuario pida **debatir, evaluar, refutar, contrastar o decidir** una tesis,
  usar la skill `round-based-debate`.
- Antes de iniciar, **preguntar número de rondas y quién inicia** si no fue indicado.
  No iniciar el debate sin número de rondas.

## Estructura del debate

- El debate tiene **dos posiciones principales**: proponente y oponente.
- El **árbitro no debate**; solo enmarca, controla el foco y decide el ganador.
- El debate es **secuencial por rondas** (un agente interviene, el otro responde), no
  un panel paralelo.

## Evidencia

- Los agentes pueden buscar evidencia web en cualquier etapa si la necesitan.
- Toda evidencia externa debe citar la fuente. **No inventar fuentes.**

## Salidas

El sistema debe guardar, por cada debate:
1. El debate completo — `outputs/debate-full-[fecha]-[slug].md`
2. El resultado JSON — `outputs/debate-result-[fecha]-[slug].json`
3. La fuente de conocimiento — `knowledge/debate-knowledge-[fecha]-[slug].md`

## Estilo y objetivo

- Estilo **adversarial fuerte**, pero **sin ataques personales**.
- El objetivo es **crear conocimiento reutilizable**, no solo opiniones.

## Componentes

- Skill: `.claude/skills/round-based-debate/SKILL.md`
- Workflow: `.claude/workflows/round-based-debate.md`
- Agentes: `.claude/agents/thesis-framer.md`, `proponent-debater.md`,
  `opponent-debater.md`, `debate-arbiter.md`
