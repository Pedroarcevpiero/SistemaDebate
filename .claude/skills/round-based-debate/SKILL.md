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

Esta skill es **autocontenida**: ejecuta la orquestación con la tool `Task`, lanzando los
subagentes `thesis-framer`, `proponent-debater`, `opponent-debater` y `debate-arbiter`.
La especificación detallada está en `.claude/workflows/round-based-debate.md`; **léela con
`Read` al iniciar** para seguir el detalle de cada fase (este archivo no se autocarga, es
una spec de referencia).

El debate es **secuencial, no paralelo**: en cada ronda interviene un agente y luego el
otro responde, con contexto acumulado. Nunca lances proponente y oponente en paralelo.

Fases:
0. **Recolección de parámetros** — confirmar tesis, número de rondas, quién inicia y uso de
   evidencia web. No iniciar sin número de rondas.
1. **Enmarcado y creación del archivo** — `Task` → `thesis-framer`; luego crear de inmediato
   `outputs/debate-full-[fecha]-[slug].md` con encabezado + enmarcado (`Write`).
2. **Apertura del árbitro** — `Task` → `debate-arbiter` (confirma reglas, sin preferencia);
   anexar su apertura al archivo.
3. **Debate secuencial por rondas** — para cada ronda, lanzar primero al agente que inicia
   y luego al rival. Tras CADA intervención, **anexarla verbatim al archivo
   inmediatamente** (antes de lanzar la siguiente). Repetir hasta completar las rondas.
4. **Cierre del debate y compuerta de verificación** — no más intervenciones de los
   debatientes; releer el archivo y contar que existan exactamente 2·N intervenciones
   (todas las rondas, ambas posiciones). Si falta alguna, detenerse y reportarlo.
5. **Veredicto del árbitro** — `Task` → `debate-arbiter`, que lee el archivo completo
   desde disco, anexa su veredicto de 20 secciones, y genera los otros 2 archivos.

## Salidas obligatorias (3 archivos por debate)

1. `outputs/debate-full-[fecha]-[slug].md` — debate completo **verbatim** (no resumen).
2. `outputs/debate-result-[fecha]-[slug].json` — resultado estructurado.
3. `knowledge/debate-knowledge-[fecha]-[slug].md` — fuente de conocimiento reutilizable.

`[fecha]` en formato `YYYY-MM-DD`; `[slug]` es un identificador corto del tema en
minúsculas con guiones.

**Regla crítica de contenido:** `debate-full` es el **registro literal (verbatim)** del
debate: encabezado + enmarcado + apertura + las 2·N intervenciones completas tal cual las
produjeron los agentes (todas sus secciones y fuentes) + el veredicto de 20 secciones. **No
guardes resúmenes en `debate-full`.**

**Persistencia incremental (crítica):** el archivo `debate-full` se crea al inicio del
debate (Fase 1) y se **anexa** tras cada paso (apertura, cada intervención, veredicto). El
disco es la fuente de verdad, no el contexto del orquestador — así una compactación de
contexto a mitad del debate no puede borrar intervenciones ya producidas. Antes de invocar
al árbitro (Fase 4), verificar que el archivo contiene las 2·N intervenciones completas.
Solo `debate-result.json` y `knowledge.md` son sintéticos.

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
