# Debate completo — Modelos abiertos vs cerrados de IA

- Fecha: 2026-06-14
- Rondas: 8 (16 intervenciones)
- Inició: Oponente (Agente B, cierre/control)
- Tema de alto impacto (política tecnológica / seguridad nacional) → REVISIÓN HUMANA OBLIGATORIA

## Tesis original
Modelos abiertos vs cerrados de IA.

## Tesis reformulada (thesis-framer)
¿La política por defecto para modelos fundacionales avanzados debería ser apertura
(cierre solo si evaluaciones independientes demuestran riesgo sistémico concreto —
Proponente/Agente A) o cierre/control (apertura solo bajo seguridad verificada —
Oponente/Agente B)?

## Reglas confirmadas
- Debate secuencial por rondas, dos posiciones, árbitro no debate.
- Evidencia web permitida con cita de fuente; no inventar fuentes.
- Estilo adversarial fuerte, sin ataques personales.

## Desarrollo por rondas

### R1
- Oponente: pesos abiertos irreversibles ("gemelo malvado" vía fine-tuning,
  Jailbreak-Tuning arXiv 2507.11630); evaluación previa no detecta riesgo post-publicación.
- Proponente: modelos cerrados igualmente vulnerables vía API fine-tuning (<$50,
  arXiv 2510.01342); cierre = single point of failure; apertura = auditoría distribuida
  (Model Openness Framework). Concedió pérdida de control de revocación.

### R2
- Oponente: "capacidad de respuesta post-incidente" (cierre revoca/parchea; abierto sin
  kill switch). Concedió que SPOF institucional es gobernanza, no apertura.
- Proponente: "kill switch = control switch" (riesgo de censura/captura); TAR safeguards
  (arXiv 2408.00761), Deep Ignorance (EleutherAI). Concedió que no son perfectas.

### R3
- Oponente: TAR tiene paper de bypass del MIT; Deep Ignorance solo validado en biorriesgo;
  asimetría temporal favorece cierre. Concedió que Deep Ignorance es avance real.
- Proponente: pivote a soberanía/dependencia estratégica (arXiv 2604.06217;
  "International AI Safety Report 2026" arXiv 2602.21012, cifra "50-60% adopción china" —
  PARCIALMENTE NO VERIFICABLE). Concedió que v1 abierto no "desaprende".

### R4
- Oponente: verificó existencia de papers pero no la cifra ni cita específica de kill
  switches; distinguió "soberanía de cómputo" de "control de pesos".
- Proponente: CONCEDIÓ honestamente la cita no verificable; reforzó con datos reales:
  Dataconomy feb 2026, modelos chinos = 61% tokens OpenRouter (de 1.2% en 2024),
  41% descargas HuggingFace vs 36.5% EEUU, 80% startups (a16z) con stacks chinos.

### R5
- Oponente: reencuadró "abierto vs cerrado" (no China vs Occidente); Nature Communications
  (88.1% jailbreak vía persuasión social en GPT-4o/DeepSeek/Gemini). Concedió adopción china real.
- Proponente: Cisco/HarmBench — DeepSeek R1 100% ASR vs GPT-4o 86% bloqueo / Gemini 64%;
  sesgo pro-China 91.2% en DeepSeek-R1 (arXiv 2506.01814) + reporte Estonia 2026.
  Concedió que ningún modelo es invulnerable; tesis basada en grado/tipo, no absoluto.

### R6
- Oponente: FAR.AI "Illusory Safety" (guardrails cerrados removibles vía fine-tuning),
  WildJailbreak; sesgo de R1 detectable PORQUE es abierto. Concedió 100% ASR como fricción inicial.
- Proponente: "Heretic" (desactiva salvaguardas de cualquier modelo abierto en <10 min);
  reversibilidad del control (GPT-4o sycophancy rollback abril 2025). Concedió que su tesis
  real es "control de la decisión de liberar pesos" = apertura condicionada.

### R7
- Oponente: Heretic afectó Llama 3.3/Gemma 3 que YA tenían escrutinio pre-publicación —
  refuta que "apertura condicionada" conductual sea solución. Concedió rollback como corrección real.
- Proponente: distinguió escrutinio conductual (frágil) de su propuesta (restringir solo
  doble uso concreto: bio-uplift, ciber-ofensivo autónomo); "falacia del vacío seguro".
  Concedió que RLHF/filtrado conductual es insuficiente como única salvaguarda.

### R8 (cierres)
- Oponente: "control por defecto + apertura condicionada por verificación, vía gobernanza
  de cómputo (controles de exportación de chips) y estándares de auditoría (Brussels
  Effect/EU AI Act)". Carga de prueba en quien publica (irreversible). Dicotomía burda.
- Proponente: "apertura por defecto para capacidades generales + restricción específica
  solo para doble uso concreto demostrable". Reconoció cita R3 no verificable, TAR/Deep
  Ignorance no definitivos, RLHF insuficiente; el vacío seguro nunca fue refutado.

## Convergencia final
Régimen mixto: apertura/cierre por defecto con excepción quirúrgica basada en capacidades
específicas de doble uso verificadas. Diferencias: (a) carga de la prueba (Oponente: en
quien publica por irreversibilidad; Proponente: apertura es norma salvo riesgo demostrado);
(b) peso del "vacío seguro" geopolítico (Proponente: decisivo; Oponente: secundario).

## Nota de evidencia
Varias citas arXiv "26xx.xxxxx" usadas por ambos lados solo fueron confirmadas en
título/existencia, no en contenido específico. Evidencia débil simétrica.
