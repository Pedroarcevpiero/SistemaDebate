# Dynamic Workflow — round-based-debate

Este archivo especifica cómo Claude Code debe **generar y ejecutar** el Dynamic Workflow
de un debate secuencial por rondas. El debate es **secuencial**, no paralelo: en cada
ronda un agente interviene y luego el otro responde, con contexto acumulado.

Agentes usados:
- `thesis-framer`
- `proponent-debater`
- `opponent-debater`
- `debate-arbiter`

---

## Fase 0 — Recolección de parámetros

Antes de debatir, verifica que tienes:

- Tesis del usuario.
- Número de rondas.
- Quién inicia (proponente / oponente / que el sistema recomiende).
- Uso de evidencia web (sí / no / solo si requiere datos actuales o verificables).

Si falta alguno, **pregunta al usuario**. **No inicies el debate sin número de rondas.**
No repreguntes lo que el usuario ya respondió.

---

## Fase 1 — Enmarcado y creación del archivo `debate-full`

Ejecuta `thesis-framer`.

Salida obligatoria:
- Tesis original.
- Tesis reformulada.
- Pregunta debatible.
- Posición proponente.
- Posición oponente.
- Criterios de evaluación.
- Alcance.
- Exclusiones.
- Riesgos de desviación temática.

> **Regla de persistencia incremental (crítica):** inmediatamente después de
> obtener el enmarcado, **fija el `[slug]` y crea con `Write`** el archivo
> `outputs/debate-full-[fecha]-[slug].md` con el encabezado (tesis, fecha,
> número de rondas, quién inicia) y el enmarcado completo del `thesis-framer`.
> A partir de aquí, **el disco es la fuente de verdad del debate, no el
> contexto del orquestador**. Cada fase posterior **anexa** (`Edit` o
> reescritura append-only) contenido a este mismo archivo apenas se produce.
> Esto evita que una compactación de contexto a mitad del debate borre
> intervenciones ya producidas.

---

## Fase 2 — Apertura del árbitro

Ejecuta `debate-arbiter` para confirmar:
- Reglas del debate.
- Número de rondas.
- Quién inicia.
- Criterios de victoria.
- Condiciones para usar búsqueda web.
- Penalizaciones por desvío, evasión o evidencia débil.

El árbitro **no debe declarar ninguna preferencia inicial**.

**Anexa inmediatamente** la apertura completa del árbitro al `debate-full`
ya creado en Fase 1.

---

## Fase 3 — Debate secuencial por rondas

> **Regla de persistencia incremental (crítica):** tras recibir el texto de
> **cada** intervención (proponent-debater u opponent-debater), el
> orquestador debe, **en el mismo turno y antes de lanzar la siguiente
> ronda**:
> 1. **Anexar el texto verbatim completo** de esa intervención al
>    `debate-full-[fecha]-[slug].md` (con su título "## Intervención del
>    [Proponente/Oponente] — Ronda [N]" y todas sus secciones).
> 2. Verificar (releyendo el archivo o contando) que la intervención quedó
>    escrita antes de continuar.
>
> El contexto para el siguiente debatiente (rondas previas, retos
> pendientes, evidencia usada) debe obtenerse **leyendo el archivo en
> disco** con `Read`, no confiando en la memoria acumulada de la
> conversación. Esto es lo que garantiza que ninguna intervención se pierda
> si el contexto del orquestador se compacta a mitad del debate.
>
> No se guardan resúmenes: cada intervención se anexa palabra por palabra,
> tal como la devuelve el agente.

Ejecuta el debate alternado según quién inicia.

**Si inicia el proponente**, para cada ronda N:
1. `proponent-debater` → anexar al archivo
2. `opponent-debater` → anexar al archivo

**Si inicia el oponente**, para cada ronda N:
1. `opponent-debater` → anexar al archivo
2. `proponent-debater` → anexar al archivo

Repite hasta completar el número de rondas acordado.

Cada agente debe recibir como contexto (leído del archivo en disco):
- Tesis reformulada.
- Criterios de evaluación.
- Intervención anterior del rival.
- Rondas previas completas.
- Evidencia usada hasta el momento.
- Preguntas o retos pendientes.
- Advertencia de no desviarse del tema.

Los agentes pueden buscar evidencia web en cualquier ronda si:
- Hay afirmaciones actuales.
- Hay datos técnicos, financieros, legales, regulatorios o de mercado.
- El rival usó una afirmación verificable.
- La decisión depende de hechos externos.
- La evidencia previa es insuficiente.

Si usan web, deben **citar la fuente** y explicar cómo afecta el argumento.

---

## Fase 4 — Cierre de debate y compuerta de verificación

Tras completar todas las rondas:
- No permitir nuevas intervenciones de proponente u oponente.
- **Compuerta de verificación (obligatoria):** lee `debate-full-[fecha]-[slug].md`
  completo y cuenta los encabezados `## Intervención del Proponente — Ronda [N]`
  y `## Intervención del Oponente — Ronda [N]`. Deben sumar exactamente **2·N**
  (N = número de rondas acordado), con todas las rondas 1..N presentes para ambas
  posiciones.
  - Si falta alguna intervención, **detente y reporta al usuario** qué falta antes
    de continuar. No invoques al árbitro ni guardes un veredicto sobre un debate
    incompleto sin que el usuario lo sepa.
  - Si las 2·N están completas, continúa a Fase 5.

---

## Fase 5 — Veredicto del árbitro

Ejecuta `debate-arbiter`, indicándole que **lea `debate-full-[fecha]-[slug].md`
desde disco** (ya contiene encabezado + enmarcado + apertura + las 2·N
intervenciones verbatim verificadas en Fase 4). El árbitro:

1. Lee el archivo completo.
2. Produce el veredicto con 20 secciones (ver `.claude/agents/debate-arbiter.md`),
   incluyendo tablas de puntaje 0–10 para proponente y oponente, declaración de
   ganador (Proponente / Oponente / Empate técnico), nivel de confianza
   (Alto / Medio / Bajo) y recomendaciones prácticas.
3. **Anexa** (no reescribe) el veredicto al final de `debate-full-[fecha]-[slug].md`.
4. Genera los otros 2 archivos obligatorios, sintéticos:
   - `outputs/debate-result-[fecha]-[slug].json`
   - `knowledge/debate-knowledge-[fecha]-[slug].md`

### Resultado final de `debate-full-[fecha]-[slug].md`

Al terminar, el archivo contiene, en este orden (construido incrementalmente
desde la Fase 1, no al final):

1. Encabezado (tesis, fecha, rondas, quién inició, nota de alto impacto si aplica).
2. Enmarcado completo del `thesis-framer`.
3. Apertura completa del árbitro.
4. **Las 2·N intervenciones verbatim**, en orden, con su título y todas sus secciones
   (Respuesta directa, Argumento/Objeción, Evidencia con fuentes, Ataque, Concesión, Reto),
   tal como las produjeron `proponent-debater` y `opponent-debater`. Nada de paráfrasis.
5. El **veredicto completo de 20 secciones** del árbitro, incluidas las tablas de puntaje.

La compuerta de Fase 4 garantiza que el paso 4 esté completo antes de que el
árbitro escriba el paso 5. El `debate-result` JSON y el `knowledge` md sí son
sintéticos; el `debate-full` no.
