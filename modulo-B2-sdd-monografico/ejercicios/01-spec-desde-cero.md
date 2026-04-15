# Ejercicio 01 — Spec desde Cero: App de Notas

**Tiempo estimado**: 25 minutos  
**Dificultad**: Introducción  
**Prerequisito**: haber leído los ficheros de teoría 01, 02 y 03 de este módulo

---

## Contexto

Vas a practicar las dos primeras fases de SDD: entrevista y especificación. El proyecto es una aplicación de notas con categorías, búsqueda y sincronización en la nube. Una descripción suficientemente real para tener edge cases no triviales, pero lo bastante acotada para completarla en 25 minutos.

El objetivo no es escribir código — es producir una spec de calidad a partir de una entrevista bien conducida. Al final compararás qué requisitos descubrió la entrevista que no habías anticipado.

---

## Descripción del proyecto

> "Quiero una app de notas. El usuario puede crear, editar y borrar notas. Las notas pueden tener categorías. Hay búsqueda. Las notas se sincronizan en la nube para poder acceder desde varios dispositivos."

Esta descripción tiene al menos 10 decisiones de diseño sin tomar. Tu trabajo es descubrirlas antes de que el agente las tome por ti.

---

## Instrucciones paso a paso

### Paso 1: Preparación (2 minutos)

Antes de iniciar la entrevista, anota manualmente en papel o en un archivo de texto:

**¿Qué creo que necesita esta app?** — Lista libre de 5-8 requisitos que crees que son obvios.

Guarda esta lista. Al final del ejercicio, la compararás con lo que descubrió la entrevista.

### Paso 2: Iniciar la entrevista (10 minutos)

Usa este prompt para iniciar la entrevista con tu agente:

```text
Voy a construir una aplicación de notas con las siguientes características básicas:
- Crear, editar y borrar notas
- Categorías para organizar notas
- Búsqueda de notas
- Sincronización en la nube para acceso desde múltiples dispositivos

Entrevístame sobre los requisitos antes de escribir ningún código.
Haz una pregunta a la vez y espera mi respuesta antes de continuar.
Cubre especialmente: edge cases, requisitos no funcionales, y comportamiento 
en situaciones límite (sin conexión, conflictos de sincronización, etc.).
Al final, genera un resumen de las decisiones tomadas.
```

**Sugerencias de respuesta para las preguntas que probablemente hará el agente**:

- *¿La app es web, móvil o desktop?* → Empieza como web (React) con idea de añadir mobile después. No es un requisito ahora.
- *¿Se pueden compartir notas con otros usuarios?* → No en v1. Solo notas privadas.
- *¿Qué pasa con los conflictos de sincronización?* → Piénsalo antes de responder. Esta es una decisión de diseño real.
- *¿Hay límite de tamaño de nota?* → Sin límite explícito por ahora.
- *¿Markdown en las notas?* → Sí, soporte de Markdown básico.
- *¿Adjuntos (imágenes, archivos)?* → No en v1.

Para preguntas sobre offline, sincronización y conflictos: toma una decisión durante la entrevista aunque sea provisional. La spec documentará esa decisión y el porqué.

### Paso 3: Generar SPEC.md (8 minutos)

Al terminar la entrevista, pide al agente que genere la spec:

```text
Con las decisiones tomadas en la entrevista, genera SPEC.md para esta app de notas.
Sigue la estructura estándar:
- Contexto
- Requisitos Funcionales (con IDs: RF-01, RF-02, ...)
- Requisitos No Funcionales (con IDs: RNF-01, ...)
- Diseño Técnico (Modelo de Datos, API/Interfaces)
- Edge Cases y Manejo de Errores
- Plan de Testing
- Fases de Implementación
- Exclusiones

Cada requisito debe ser verificable. Incluye ejemplos concretos donde ayuden a la claridad.
```

### Paso 4: Revisión con checklist (5 minutos)

Revisa el SPEC.md generado con este checklist:

```markdown
Requisitos
- [ ] Cada RF tiene un criterio de éxito verificable
- [ ] Los RNF tienen números concretos (no "rápido", sino "< 200ms")
- [ ] Las exclusiones están presentes y son explícitas

Diseño técnico
- [ ] El modelo de datos cubre todos los RF
- [ ] Los edge cases de la entrevista aparecen en la spec

Completitud
- [ ] La sección de sincronización y conflictos está cubierta
- [ ] El comportamiento offline está definido (o explícitamente excluido)
- [ ] El plan de testing es específico (qué testear, no solo "hacer tests")

Calidad
- [ ] La spec no mezcla el QUÉ con el CÓMO en los requisitos funcionales
- [ ] Las fases de implementación están ordenadas por dependencias
```

Para cada casilla no marcada, decide si actualizar la spec o si el punto no aplica a este proyecto.

---

## Entregables

1. **Lista inicial** (Paso 1): los requisitos que anticipaste antes de la entrevista
2. **SPEC.md** generado en el Paso 3
3. **Reflexión** (ver abajo)

---

## Reflexión

Responde estas preguntas después del ejercicio:

1. ¿Cuántos requisitos de la spec NO estaban en tu lista inicial? ¿Cuáles son los más importantes?
2. ¿Qué preguntas de la entrevista fueron más reveladoras? ¿Por qué?
3. ¿Hay alguna decisión en la spec con la que no estás de acuerdo? ¿Qué cambiarías?
4. ¿Cuánto tiempo crees que habrías perdido si hubieras empezado a implementar sin la entrevista?

---

## Criterios de Evaluación

| Criterio | Excelente | Aceptable | Necesita mejora |
|----------|-----------|-----------|-----------------|
| Completitud de la spec | 10+ RF verificables + RNF con números | 6-9 RF + algunos RNF | Menos de 6 RF o sin RNF concretos |
| Calidad de requisitos | Todos verificables, con ejemplos | La mayoría verificables | Muchos vagos o no verificables |
| Cobertura de edge cases | Sincronización, offline, conflictos cubiertos | Algunos edge cases cubiertos | Solo happy path |
| Exclusiones explícitas | 4+ exclusiones concretas | 2-3 exclusiones | Sin exclusiones |
| Requisitos descubiertos en entrevista | 4+ requisitos no anticipados | 2-3 no anticipados | Menos de 2 |

---

> **Nota**: no hay una spec "correcta" para este ejercicio. Lo que importa es el proceso: la calidad de las preguntas de la entrevista, la verificabilidad de los requisitos, y la reflexión sobre qué descubriste que no habías anticipado.
