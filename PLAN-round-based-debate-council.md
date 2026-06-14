# Plan — round-based-debate-council

## Objetivo
Construir un sistema Claude Code con Dynamic Workflow dedicado **exclusivamente** a
debates secuenciales por rondas: un debate adversarial fuerte entre dos posiciones
(proponente y oponente) supervisado por un árbitro. No es un sistema general de
investigación ni un panel paralelo de agentes.

## Principio central
Crear conocimiento reutilizable, no solo opiniones. Cada debate guarda debate completo,
evidencia, argumentos, refutaciones, concesiones, puntos no resueltos, veredicto, ganador
y un resumen estructurado.

## Estructura a crear
```
.claude/
  agents/
    thesis-framer.md
    proponent-debater.md
    opponent-debater.md
    debate-arbiter.md
  skills/round-based-debate/SKILL.md
  workflows/round-based-debate.md
docs/
  debate-methodology.md
  safety-and-quality-rules.md
  knowledge-base-rules.md
examples/
  procurement-debate.md
  investment-debate.md
  business-strategy-debate.md
  policy-debate.md
outputs/            (vacío, salidas por debate)
knowledge/          (vacío, fuentes de conocimiento)
CLAUDE.md
README.md
```

## Componentes

### Skill: round-based-debate
- Frontmatter con allowed-tools: Read, Write, Edit, Glob, Grep, WebSearch, WebFetch, Task, Bash.
- Se activa con frases como "haz un debate", "debate por rondas", "debate council",
  "ultracode debate", "proponente contra oponente", etc.
- Flujo inicial obligatorio: preguntar (1) número de rondas, (2) quién inicia,
  (3) uso de evidencia web. No repreguntar lo ya respondido.
- Definición de ronda = una intervención de cada posición. 3 rondas = 6 intervenciones + veredicto.

### Agentes (4)
1. **thesis-framer** (sonnet, maxTurns 3) — enmarca tesis, no decide ganador.
2. **proponent-debater** (sonnet, maxTurns 6) — defiende la tesis, formato fijo por intervención.
3. **opponent-debater** (sonnet, maxTurns 6) — ataca la tesis, formato fijo por intervención.
4. **debate-arbiter** (opus, maxTurns 5) — enmarca reglas, controla foco, puntúa y declara ganador.

### Dynamic Workflow (fases 0-5)
0. Recolección de parámetros (no iniciar sin número de rondas).
1. Enmarcado (thesis-framer).
2. Apertura del árbitro (reglas, sin preferencia).
3. Debate secuencial por rondas (alternancia según quién inicia, con contexto acumulado).
4. Cierre (no más intervenciones, todo al árbitro).
5. Veredicto del árbitro (20 secciones, tablas de puntaje 0-10, ganador).

### Salidas obligatorias por debate (3 archivos)
1. `outputs/debate-full-[fecha]-[slug].md`
2. `outputs/debate-result-[fecha]-[slug].json`
3. `knowledge/debate-knowledge-[fecha]-[slug].md`

### Reglas de calidad y seguridad
No inventar fuentes; no desviarse de la tesis; penalizar evasiones/evidencia débil/cambio
de tesis; empate técnico si evidencia insuficiente; temas financieros/legales/médicos/
regulatorios/alto impacto requieren revisión humana.

## Pasos de ejecución
1. Crear directorios. ✅
2. Escribir los 4 agentes.
3. Escribir SKILL.md.
4. Escribir workflow.
5. Escribir docs (3).
6. Escribir ejemplos (4).
7. Escribir CLAUDE.md y README.md.
8. Añadir .gitkeep a outputs/ y knowledge/.
9. Validación final + lista de archivos + prompt de ejemplo.
10. NO hacer commit (pendiente de autorización).
