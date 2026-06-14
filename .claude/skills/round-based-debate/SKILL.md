---
name: round-based-debate
description: Ejecuta debates adversariales por rondas entre dos posiciones principales y un árbitro. Pregunta cuántas rondas tendrá el debate, quién inicia, permite búsqueda web cuando se requiera evidencia, guarda el debate completo y produce un veredicto con ganador.
allowed-tools: Read, Write, Edit, Glob, Grep, WebSearch, WebFetch, Task, Bash
---

# Round-Based Debate

Esta skill ejecuta debates **adversariales por rondas** entre dos posiciones principales
(proponente y oponente) supervisados por un árbitro. No es un sistema de investigación
general ni un panel paralelo de agentes: es un debate secuencial, fuerte y enfocado, cuyo
objetivo es **crear conocimiento reutilizable**.

## Cuándo se activa

Activa esta skill cuando el usuario diga cosas como:

- "haz un debate"
- "somete esto a debate"
- "debate por rondas"
- "debate council"
- "evalúa esta tesis con debate"
- "usa dynamic workflow para debatir"
- "ultracode debate"
- "proponente contra oponente"
- "quiero que uno inicie y el otro responda"

## Flujo inicial obligatorio

Antes de iniciar cualquier debate, pregunta al usuario (no repreguntes lo ya respondido):

1. **¿Cuántas rondas quieres para el debate?**
2. **¿Quién debe iniciar?**
   - Proponente
   - Oponente
   - Que el sistema recomiende quién debe iniciar según la tesis
3. **¿Quieres que el sistema busque evidencia web cuando algún agente la requiera?**
   - Sí
   - No
   - Solo si el tema requiere datos actuales o verificables

**No inicies el debate sin el número de rondas.**

## Definición de ronda

Una ronda equivale a **una intervención de cada posición**.

```
Ronda 1: Proponente abre  → Oponente responde
Ronda 2: Proponente replica → Oponente contrarreplica
Ronda 3: Proponente refuerza → Oponente refuta de nuevo
```

Si el usuario pide 3 rondas, deben existir **6 intervenciones principales**, más el
veredicto del árbitro.

## Orquestación

Esta skill usa el Dynamic Workflow definido en `.claude/workflows/round-based-debate.md`,
con los agentes `thesis-framer`, `proponent-debater`, `opponent-debater` y `debate-arbiter`.

Fases:
0. Recolección de parámetros.
1. Enmarcado (`thesis-framer`).
2. Apertura del árbitro (`debate-arbiter`).
3. Debate secuencial por rondas (alternancia según quién inicia).
4. Cierre del debate.
5. Veredicto del árbitro (`debate-arbiter`) y persistencia.

## Salidas obligatorias (3 archivos por debate)

1. `outputs/debate-full-[fecha]-[slug].md` — debate completo.
2. `outputs/debate-result-[fecha]-[slug].json` — resultado estructurado.
3. `knowledge/debate-knowledge-[fecha]-[slug].md` — fuente de conocimiento reutilizable.

`[fecha]` en formato `YYYY-MM-DD`; `[slug]` es un identificador corto del tema en
minúsculas con guiones.

## Reglas de calidad

- No inventar fuentes.
- No presentar opiniones como hechos.
- No desviarse de la tesis.
- No permitir ataques personales.
- No usar retórica sin contenido.
- No declarar ganador antes del final.
- Penalizar evasiones, evidencia débil usada como decisiva y cambios de tesis.
- Empate técnico permitido si la evidencia es insuficiente.
- Temas financieros, legales, médicos, regulatorios o de alto impacto requieren
  revisión humana.
