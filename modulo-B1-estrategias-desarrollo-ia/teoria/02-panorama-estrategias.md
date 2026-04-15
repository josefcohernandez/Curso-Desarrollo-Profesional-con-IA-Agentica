# Panorama de estrategias de desarrollo con IA

## Mapa completo de estrategias

No existe una única metodología "correcta" para desarrollar con agentes de código. Cada estrategia tiene su contexto ideal, sus fortalezas y sus limitaciones. El desarrollador profesional conoce el mapa completo y elige la herramienta adecuada para cada situación.

Este capítulo presenta todas las estrategias disponibles. Los capítulos 03 y 04 profundizan en Gherkin/BDD y TDD respectivamente, que son las dos más utilizadas. El capítulo 05 cubre patrones avanzados de workflow. El capítulo 06 completa el panorama con el resto de estrategias.

---

## Las estrategias principales

### SDD — Spec-Driven Development

**Concepto**: diseñar antes de codificar. Producir un documento de especificación completo antes de la primera línea de código. El agente implementa contra la spec, no contra una descripción vaga.

**Flujo básico**:
```text
Entrevista de requisitos → Spec → Validación humana → Implementación → Verificación vs spec
```

**Cuándo es la elección correcta**: proyectos nuevos con más de 3 módulos interdependientes, sistemas donde el coste de cambios tardíos es alto, proyectos en equipo donde todos necesitan compartir la misma visión.

**El capítulo B2 está dedicado íntegramente a SDD.** Este es solo un resumen de posicionamiento.

---

### TDD — Test-Driven Development

**Concepto**: los tests como especificación ejecutable. Defines el comportamiento esperado en forma de tests antes de implementar. El agente implementa código que hace pasar los tests.

**Flujo básico**:
```text
Test que falla (Red) → Implementación mínima (Green) → Mejora del diseño (Refactor) → repetir
```

**Cuándo es la elección correcta**: lógica de negocio con reglas claras, módulos con múltiples edge cases, cualquier componente que deba ser robusto ante cambios futuros.

**El capítulo 04 está dedicado a TDD con IA.**

---

### BDD/Gherkin — Behavior-Driven Development

**Concepto**: especificaciones de comportamiento en lenguaje natural estructurado. Gherkin (Given/When/Then) actúa como puente entre los requisitos de negocio y los tests técnicos. Los agentes de código lo entienden excepcionalmente bien.

**Flujo básico**:
```text
Feature en Gherkin → Tests desde features → Implementación → Verificación de escenarios
```

**Cuándo es la elección correcta**: cuando los requisitos vienen de negocio (no solo técnicos), cuando necesitas que los criterios de aceptación sean verificables, cuando hay colaboración entre desarrolladores y producto.

**El capítulo 03 está dedicado a Gherkin/BDD.**

---

### PRD — Product Requirements Document

**Concepto**: documento de requisitos orientado a producto, no a implementación técnica. Define el qué y el para qué, no el cómo. Menos formal que una spec técnica, más orientado al usuario.

**Estructura típica**:
```markdown
# PRD: [Nombre del producto/feature]

## Problema que resuelve
## Usuarios objetivo
## Casos de uso principales
## Requisitos funcionales
## Requisitos no funcionales
## Métricas de éxito
## Out of scope
```

**Cuándo es la elección correcta**: features nuevas en productos existentes, proyectos con stakeholders no técnicos, cuando necesitas alinear equipo antes de la fase técnica.

---

### Design Docs / RFC

**Concepto**: documentos de diseño técnico detallados que exploran alternativas, evalúan trade-offs y registran la decisión tomada. Los RFC (Request for Comments) invitan revisión externa antes de comprometerse con una implementación.

**Estructura típica**:
```markdown
# Design Doc: [Sistema/componente]

## Contexto y motivación
## Objetivos y no-objetivos
## Diseño propuesto
## Alternativas consideradas
## Trade-offs
## Plan de implementación
## Preguntas abiertas
```

**Cuándo es la elección correcta**: cambios arquitectónicos significativos, decisiones con impacto en múltiples equipos, sistemas distribuidos, migraciones de infraestructura.

---

### ADR — Architecture Decision Records

**Concepto**: registros breves y cronológicos de decisiones arquitectónicas. Cada ADR documenta una única decisión con su contexto, la decisión tomada y las consecuencias esperadas.

**Formato estándar**:
```markdown
# ADR-001: [Título de la decisión]

## Estado: Aceptado | Propuesto | Deprecado | Sustituido por ADR-XXX

## Contexto
[Situación que requirió la decisión]

## Decisión
[Lo que se decidió hacer]

## Consecuencias
### Positivas
- [beneficio 1]

### Negativas
- [trade-off 1]
```

**Cuándo es la elección correcta**: decisiones de tecnología (qué base de datos, qué framework), decisiones de arquitectura (monolito vs microservicios), cualquier decisión que en el futuro alguien se va a preguntar "¿por qué hicimos esto así?".

---

### Visual-Driven Development

**Concepto**: usar imágenes (mockups, wireframes, screenshots) como especificación. Los agentes modernos con visión pueden analizar imágenes y generar código directamente desde ellas.

**Flujo básico**:
```text
Mockup/screenshot → [imagen al agente] → Código → Ajustes visuales iterativos
```

**Cuándo es la elección correcta**: componentes de UI, landing pages, reproducciones de diseño existente, cuando el diseño ya está hecho y necesitas implementarlo.

> En Claude Code: adjunta la imagen al inicio de la sesión con `@ruta/mockup.png`.
> En Cursor: arrastra la imagen al chat.
> En Copilot Chat: no disponible en todos los planes — verifica tu versión.

---

### Desarrollo iterativo

**Concepto**: ciclos rápidos de prompt-implementación-revisión-corrección sin especificación previa. Apropiado cuando el problema es pequeño o cuando necesitas explorar antes de comprometerte con un diseño.

**Flujo básico**:
```text
Prompt → Resultado → Revisión → Corrección → ... → Resultado aceptable
```

**Cuándo es la elección correcta**: scripts de uso único, tareas de menos de 50 líneas, prototipado rápido para validar ideas, bugs pequeños y aislados.

**Cuándo escala mal**: proyectos de más de 3 archivos, cualquier cosa que vaya a producción, código que otras personas van a leer o mantener.

---

### Spike-then-spec

**Concepto**: exploración técnica primero, especificación después. Un "spike" es una implementación experimental de bajo compromiso para entender el problema antes de comprometerse con un diseño.

**Flujo básico**:
```text
Spike (exploración sin tests) → Aprendizaje → Spec basada en lo aprendido → Implementación real
```

**Cuándo es la elección correcta**: tecnologías que no conoces, integraciones con APIs externas desconocidas, cuando el dominio del problema es incierto.

**Regla clave**: el spike es desechable. Si el spike va a producción, ya no es un spike — es deuda técnica.

---

## Tabla de decisión: qué estrategia elegir

| Situación | Estrategia recomendada | Segunda opción |
|-----------|------------------------|----------------|
| Proyecto greenfield, múltiples módulos | SDD | TDD + Gherkin |
| Feature nueva en producto existente | Gherkin + TDD | PRD + TDD |
| Bug fix en código con tests | TDD inverso | Desarrollo iterativo |
| Bug fix en código legacy sin tests | TDD inverso | Debug + test de regresión |
| Componente UI desde mockup | Visual-Driven | Desarrollo iterativo |
| Migración masiva (>20 archivos) | Fan-out (ver cap. 05) | SDD por lotes |
| Decisión arquitectónica | ADR + Design Doc | RFC |
| Integración API desconocida | Spike-then-spec | Desarrollo iterativo |
| Script de uso único | Desarrollo iterativo | — |
| Módulo con lógica de negocio compleja | TDD + Gherkin | SDD |
| Colaboración con producto/negocio | BDD/Gherkin | PRD |

---

## Diagrama de decisión

```text
¿Sabes exactamente qué construir?
│
├─ No ────► ¿Necesitas explorar primero?
│               │
│               ├─ Sí ──► Spike-then-spec
│               │
│               └─ No ──► ¿Viene de negocio/producto?
│                              │
│                              ├─ Sí ──► PRD → Gherkin/BDD
│                              │
│                              └─ No ──► Design Doc / RFC
│
└─ Sí ────► ¿Cuántos archivos afecta?
                │
                ├─ < 3 archivos ──► Desarrollo iterativo / TDD
                │
                └─ ≥ 3 archivos ──► ¿Es greenfield o feature nueva?
                                        │
                                        ├─ Greenfield ──► SDD
                                        │
                                        └─ Feature ──► Gherkin + TDD
```

---

## Combinaciones frecuentes

Las estrategias no son mutuamente excluyentes. Las combinaciones más comunes en proyectos reales:

- **SDD + TDD**: la spec define el qué, los tests garantizan el cómo. La combinación más robusta para proyectos nuevos.
- **Gherkin + TDD**: los escenarios Gherkin se convierten directamente en tests de aceptación, los tests unitarios los complementan.
- **ADR + cualquier estrategia**: los ADR son complementarios — documentan las decisiones tomadas durante el desarrollo.
- **Spike + SDD**: exploras primero, luego especificas con el conocimiento adquirido.
- **Visual-Driven + TDD**: para componentes UI con comportamiento complejo (formularios, validaciones).

---

En los próximos capítulos profundizamos en las dos estrategias más utilizadas y más transformadoras cuando se combinan con agentes de código:

- **Capítulo 03**: Gherkin/BDD — especificaciones ejecutables
- **Capítulo 04**: TDD con IA — el ciclo Red-Green-Refactor amplificado

---

[← Anterior: Por qué la metodología importa](01-por-que-metodologias.md) | [Siguiente: Gherkin y BDD →](03-gherkin-bdd.md)
