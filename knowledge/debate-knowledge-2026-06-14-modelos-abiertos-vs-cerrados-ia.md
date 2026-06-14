# Conocimiento reutilizable — Modelos abiertos vs cerrados de IA

- Fecha: 2026-06-14 | Rondas: 8 | Ganador: Oponente (cierre/control) por margen estrecho
- Confianza: Media | REVISIÓN HUMANA OBLIGATORIA (política tecnológica / seguridad nacional)

## Pregunta central
¿Política por defecto de apertura (cierre solo con riesgo sistémico demostrado) o de
cierre/control (apertura solo bajo seguridad verificada) para modelos fundacionales?

## Ejes de decisión que importaron
1. **Asimetría de reversibilidad** (eje rector del ganador): publicar pesos es
   irreversible y sin kill switch; el cierre es reversible (revocar, parchear, rollback).
   La carga de la prueba tiende a recaer en quien realiza la acción irreversible.
2. **Vacío seguro / contención unilateral** (argumento más fuerte del perdedor, no
   refutado de fondo): el cierre occidental no detiene el desarrollo en otros polos; solo
   determina quién fija las normas. Respaldado por datos reales de adopción.
3. **Seguridad por diseño es ilusoria en ambos formatos**: guardrails cerrados removibles
   vía API fine-tuning (<$50); salvaguardas abiertas desactivables en <10 min (Heretic),
   incluso en modelos con escrutinio previo (Llama 3.3, Gemma 3).

## Hallazgos transferibles
- La dicotomía "abierto vs cerrado" es demasiado burda; lo decisivo es **qué capacidades
  específicas de doble uso** (bio-uplift, ciber-ofensivo autónomo) requieren verificación previa.
- El filtrado conductual (RLHF) es insuficiente como única salvaguarda. La contención
  estructural (TAR, Deep Ignorance) es prometedora pero no definitiva ni estándar industrial.
- "Soberanía de cómputo" (chips, exportación) ≠ "control de pesos": son ejes distintos.
- Convergencia probable de expertos: régimen mixto con excepción quirúrgica por capacidad
  verificada, vía gobernanza de cómputo y estándares de auditoría (Brussels Effect / EU AI Act).

## Datos citados (verificar antes de usar)
- Adopción (verificables, Feb 2026): modelos chinos 61% tokens OpenRouter (de 1.2% en 2024),
  41% descargas HuggingFace vs 36.5% EEUU, 80% startups (a16z) con stacks chinos.
- Seguridad: DeepSeek R1 100% ASR (Cisco/HarmBench) vs GPT-4o 86% bloqueo / Gemini 64%;
  88.1% jailbreak por persuasión social en GPT-4o/DeepSeek/Gemini (Nature Communications);
  sesgo pro-China 91.2% DeepSeek-R1.
- ADVERTENCIA: múltiples citas arXiv "26xx.xxxxx" no fueron verificadas en contenido.
  Tratar como no confirmadas hasta verificación independiente.

## Lecciones de método de debate
- La honestidad al conceder evidencia no verificable preserva credibilidad pero no
  compensa la pérdida de consistencia si la tesis se mueve.
- Mantener un eje rector estable las N rondas supera a acumular argumentos dispersos.
- Evidencia débil simétrica (ambos lados) reduce el nivel de confianza del veredicto y
  empuja hacia empate técnico cuando lo demás está parejo.
