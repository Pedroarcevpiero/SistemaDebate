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

## Fase 1 — Enmarcado

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

---

## Fase 3 — Debate secuencial por rondas

> **Regla de persistencia (crítica):** el orquestador debe **conservar el texto íntegro
> y literal (verbatim) de cada intervención** tal como la devuelve cada agente. No se
> guardan resúmenes. A medida que avanza el debate, acumula las intervenciones completas
> para construir luego `outputs/debate-full-[fecha]-[slug].md`. Pasar resúmenes en lugar
> del texto completo es un error: el archivo `debate-full` debe permitir releer el debate
> palabra por palabra.

Ejecuta el debate alternado según quién inicia.

**Si inicia el proponente**, para cada ronda N:
1. `proponent-debater`
2. `opponent-debater`

**Si inicia el oponente**, para cada ronda N:
1. `opponent-debater`
2. `proponent-debater`

Repite hasta completar el número de rondas acordado.

Cada agente debe recibir como contexto:
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

## Fase 4 — Cierre de debate

Tras completar todas las rondas:
- No permitir nuevas intervenciones de proponente u oponente.
- Entregar al árbitro **el historial completo y literal (verbatim)** de las 2·N
  intervenciones, más el enmarcado y la apertura. No entregues resúmenes.
- El árbitro evalúa el debate completo.

---

## Fase 5 — Veredicto del árbitro

Ejecuta `debate-arbiter`. Debe producir el veredicto con 20 secciones (ver
`.claude/agents/debate-arbiter.md`), incluyendo tablas de puntaje 0–10 para proponente
y oponente, y la declaración de ganador (Proponente / Oponente / Empate técnico), nivel
de confianza (Alto / Medio / Bajo) y recomendaciones prácticas.

Luego guarda los **3 archivos obligatorios**:
1. `outputs/debate-full-[fecha]-[slug].md`
2. `outputs/debate-result-[fecha]-[slug].json`
3. `knowledge/debate-knowledge-[fecha]-[slug].md`

### Contenido obligatorio de `debate-full-[fecha]-[slug].md`

Este archivo es el **registro completo y literal** del debate, no un resumen. Debe contener,
en este orden:

1. Encabezado (tesis, fecha, rondas, quién inició, nota de alto impacto si aplica).
2. Enmarcado completo del `thesis-framer`.
3. Apertura completa del árbitro.
4. **Las 2·N intervenciones verbatim**, en orden, con su título y todas sus secciones
   (Respuesta directa, Argumento/Objeción, Evidencia con fuentes, Ataque, Concesión, Reto),
   tal como las produjeron `proponent-debater` y `opponent-debater`. Nada de paráfrasis.
5. El **veredicto completo de 20 secciones** del árbitro, incluidas las tablas de puntaje.

Si el archivo no permite releer cada intervención palabra por palabra, está incompleto y
debe regenerarse. El `debate-result` JSON y el `knowledge` md sí son sintéticos; el
`debate-full` no.
