# Round-Based Debate Council

## 1. Qué es

Round-Based Debate Council es un sistema Claude Code con **Dynamic Workflow** dedicado
exclusivamente a **debates secuenciales por rondas**. Dos posiciones principales
(proponente y oponente) debaten de forma adversarial fuerte, y un árbitro imparcial
controla el foco, evalúa y declara un ganador. Cada debate se guarda como conocimiento
reutilizable.

## 2. Para qué sirve

Para evaluar tesis, hipótesis, decisiones, estrategias, políticas, inversiones, propuestas
de negocio, decisiones de procurement, contratos o argumentos, sometiéndolos a presión
adversarial estructurada y produciendo un veredicto trazable.

## 3. Para qué NO sirve

- No es un asistente general.
- No es un sistema de investigación libre.
- No es un panel paralelo de agentes (el debate es secuencial, por turnos).
- No produce decisiones vinculantes: en temas de alto impacto requiere revisión humana.

## 4. Cómo instalarlo

Clona el repositorio y ábrelo con Claude Code. La estructura `.claude/` (agents, skills,
workflows) se carga automáticamente.

```bash
git clone <repo-url>
cd SistemaDebate
claude
```

## 5. Cómo usarlo en Claude Code

Invoca la skill con `/round-based-debate` o pide directamente un debate ("haz un debate",
"somete esto a debate", "ultracode debate", "proponente contra oponente").

## 6. Cómo iniciar un debate

Da la tesis. El sistema te preguntará (si no lo indicaste):
1. Cuántas rondas.
2. Quién inicia (proponente / oponente / que el sistema recomiende).
3. Uso de evidencia web (sí / no / solo si requiere datos actuales o verificables).

## 7. Qué significa una ronda

Una ronda = una intervención de cada posición. Con 3 rondas hay 6 intervenciones
principales más el veredicto del árbitro.

## 8. Roles de agentes

- **thesis-framer** — enmarca la tesis; no decide.
- **proponent-debater** — defiende la tesis.
- **opponent-debater** — ataca la tesis.
- **debate-arbiter** — enmarca reglas, controla foco, puntúa y declara ganador.

## 9. Cómo funciona el Dynamic Workflow

Fases: (0) parámetros, (1) enmarcado, (2) apertura del árbitro, (3) debate secuencial por
rondas con contexto acumulado, (4) cierre, (5) veredicto y persistencia. Detalle en
`.claude/workflows/round-based-debate.md`.

## 10. Cómo se guarda el conocimiento

Cada debate genera 3 archivos:
- `outputs/debate-full-[fecha]-[slug].md` — debate completo.
- `outputs/debate-result-[fecha]-[slug].json` — resultado estructurado.
- `knowledge/debate-knowledge-[fecha]-[slug].md` — fuente reutilizable.

## 11. Ejemplos

Ver `examples/`: procurement, inversión, estrategia de negocio y política interna.

## 12. Limitaciones

- No decide por ti; produce análisis y veredicto.
- Depende de la evidencia disponible; si es insuficiente, puede declarar empate técnico.
- No reemplaza juicio profesional en temas financieros, legales, médicos o regulatorios.

## 13. Reglas de seguridad

- No inventar fuentes.
- No presentar opiniones como hechos.
- No desviarse de la tesis.
- Sin ataques personales.
- Revisión humana obligatoria en temas de alto impacto.

Detalle en `docs/safety-and-quality-rules.md`.
