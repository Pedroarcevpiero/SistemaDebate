# Plan de corrección — persistencia incremental del debate

## Problema detectado

En el debate "modelo económico de Perú" (2026-06-14), 3 de 12 intervenciones
(R1 Proponente, R1 Oponente, R2 Proponente) se perdieron del `debate-full`
porque el diseño anterior dependía de que el orquestador **retuviera el texto
íntegro de cada intervención en su propio contexto de conversación** y lo
entregara todo al árbitro **al final** del debate. Una compactación automática
de contexto a mitad del debate resumió esas intervenciones tempranas antes de
que se persistieran.

## Causa raíz

Persistencia "todo al final" → el disco solo se toca en el último paso → la
memoria del orquestador es punto único de fallo ante compactación de contexto.

## Corrección: persistencia incremental en disco

El `debate-full-[fecha]-[slug].md` se crea **al inicio** del debate y se va
**anexando** (encabezado, enmarcado, apertura, y cada intervención apenas se
produce, y finalmente el veredicto). El disco es la fuente de verdad; la
memoria del orquestador deja de serlo.

## Cambios aplicados

1. `.claude/workflows/round-based-debate.md`:
   - Fase 1: fijar `[slug]` y crear `debate-full` con encabezado + enmarcado.
   - Fase 2: anexar apertura del árbitro inmediatamente.
   - Fase 3: tras cada intervención, anexarla verbatim al archivo antes de
     lanzar la siguiente ronda; el contexto para el siguiente debatiente se
     lee del archivo en disco, no de memoria.
   - Fase 5: compuerta de verificación — contar intervenciones en el archivo
     (deben ser 2·N) antes de invocar al árbitro; si faltan, detener y
     reportar en vez de guardar incompleto.

2. `.claude/agents/debate-arbiter.md`:
   - El árbitro lee `debate-full` completo desde disco y solo anexa su
     veredicto de 20 secciones.

3. `.claude/skills/round-based-debate/SKILL.md`:
   - Reflejar persistencia incremental (escribir-tras-cada-intervención) en
     vez de "conservar en contexto y guardar al final".
