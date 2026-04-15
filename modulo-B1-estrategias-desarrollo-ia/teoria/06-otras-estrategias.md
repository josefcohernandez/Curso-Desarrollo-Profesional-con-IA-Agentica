# Otras estrategias complementarias

## Estrategias que completan el mapa

Los capítulos anteriores cubrieron en profundidad Gherkin/BDD, TDD y los patrones avanzados de workflow. Este capítulo completa el panorama con las estrategias que no tienen capítulo propio pero que son herramientas valiosas en situaciones específicas.

---

## PRD — Product Requirements Document

### Qué es y cuándo usarlo

Un PRD es un documento que describe qué debe hacer un producto o feature desde la perspectiva del usuario y del negocio. A diferencia de una spec técnica, no prescribe la implementación — describe el problema y los criterios de éxito.

**Cuándo es la elección correcta**:
- Features nuevas con múltiples stakeholders
- Cuando necesitas alinear al equipo antes de entrar en lo técnico
- Cuando quien hace el pedido no es técnico
- Como primer paso antes de producir una spec técnica (PRD → Spec → Implementación)

### Estructura de un PRD para desarrollo con IA

```markdown
# PRD: [Nombre de la feature]

## Problema
[Qué problema tiene el usuario actualmente. Una oración.]

## Usuarios objetivo
[Quién usará esta feature. Sé específico: "usuarios admin del plan Enterprise".]

## Solución propuesta
[Qué vamos a construir. Sin implementación técnica todavía.]

## Casos de uso principales
1. [Usuario puede hacer X]
2. [Usuario puede hacer Y]
3. [Cuando ocurre Z, el usuario ve W]

## Requisitos funcionales
- RF01: [requisito concreto y verificable]
- RF02: [requisito concreto y verificable]

## Requisitos no funcionales
- Tiempo de respuesta: [criterio medible]
- Disponibilidad: [criterio medible]

## Métricas de éxito
- [Métrica 1 con valor objetivo]
- [Métrica 2 con valor objetivo]

## Fuera de alcance (Out of scope)
- [Qué explícitamente NO vamos a construir en esta iteración]
```

**Prompt para usar un PRD con el agente**:
```text
Aquí está el PRD de la feature de notificaciones:
[PRD]

Basándote en este PRD:
1. Lista las preguntas técnicas abiertas que necesitas que resuelva antes de empezar
2. Propón la estructura técnica de alto nivel (qué módulos, qué APIs)
3. Identifica cualquier requisito del PRD que sea ambiguo o contradictorio
```

---

## Design Docs / RFC

### Qué son y cuándo usarlos

Los Design Docs son documentos de diseño técnico que exploran el espacio de soluciones antes de comprometerse con una implementación. Los RFC (Request for Comments) son una variante más formal que invita revisión del equipo.

**Cuándo son la elección correcta**:
- Cambios arquitectónicos que afectan a más de un servicio o equipo
- Introducción de nueva infraestructura o tecnología crítica
- Decisiones irreversibles o de alto coste de cambio
- Sistemas distribuidos con componentes complejos de coordinación

**Lo que los distingue de una spec**: una spec dice "vamos a construir X de esta forma". Un Design Doc dice "necesitamos resolver X, aquí hay tres formas posibles, y vamos a elegir la segunda por estas razones".

### Estructura de un Design Doc

```markdown
# Design Doc: [Título]
Fecha: YYYY-MM-DD | Autor: [nombre] | Estado: Borrador / En revisión / Aceptado

## Contexto y motivación
[Por qué este problema necesita solución ahora. Métricas actuales si aplica.]

## Objetivos
- [Objetivo 1 medible]
- [Objetivo 2 medible]

## No objetivos
- [Qué explícitamente no resuelve este diseño]

## Diseño propuesto
[Descripción detallada. Diagramas si aplica. Flujo de datos.]

## Alternativas consideradas

### Alternativa A: [Nombre]
Descripción breve.
Pros: ...
Contras: ...
Motivo de descarte: ...

### Alternativa B: [Nombre]
...

## Trade-offs y riesgos
- [Riesgo 1 y mitigación]
- [Riesgo 2 y mitigación]

## Plan de implementación
Fase 1: [qué, estimación]
Fase 2: [qué, estimación]

## Preguntas abiertas
- [ ] [Pregunta pendiente de resolución]
```

**Prompt para generar un Design Doc con el agente**:
```text
Necesito un Design Doc para migrar nuestra autenticación de sesiones a JWT.

Contexto técnico: [stack actual, escala, restricciones]

Genera el Design Doc incluyendo al menos 3 alternativas de implementación con sus trade-offs.
Sé específico sobre los riesgos de seguridad de cada alternativa.
No elijas la solución — deja las preguntas abiertas marcadas para revisión del equipo.
```

---

## ADR — Architecture Decision Records

### Qué son y por qué son valiosos

Los ADR son registros breves de decisiones arquitectónicas tomadas. Su valor principal no es la documentación en sí — es que responden la pregunta que se hace todo equipo cuando llega a código legacy: **"¿por qué está hecho así?"**

Un repositorio con buenos ADR permite que un agente entienda el contexto histórico de las decisiones, lo que mejora drásticamente la calidad de sus sugerencias.

### Formato estándar

```markdown
# ADR-{NNN}: {Título de la decisión}
Fecha: YYYY-MM-DD
Estado: Propuesto | Aceptado | Deprecado | Sustituido por ADR-{NNN}

## Contexto
[Situación técnica o de negocio que requirió tomar esta decisión.
Qué opciones existían. Qué restricciones había.]

## Decisión
[Lo que se decidió. Una oración clara: "Hemos decidido usar X para Y."]

## Consecuencias

### Positivas
- [Beneficio 1]
- [Beneficio 2]

### Negativas o trade-offs
- [Limitación 1]
- [Deuda técnica introducida, si aplica]

### Neutrales
- [Cambio de proceso necesario]
```

### Ejemplo real

```markdown
# ADR-003: Usar PostgreSQL como única base de datos
Fecha: 2024-03-15
Estado: Aceptado

## Contexto
El sistema necesita almacenar tanto datos relacionales (usuarios, pedidos) como
datos semiestructurados (preferencias, metadatos de productos). Se evaluaron
PostgreSQL, MongoDB y una combinación de ambos.

El equipo tiene experiencia sólida en SQL. El presupuesto de infraestructura
no permite mantener dos sistemas de bases de datos en producción.

## Decisión
Usamos PostgreSQL como única base de datos, aprovechando jsonb para los datos
semiestructurados en lugar de introducir MongoDB.

## Consecuencias

### Positivas
- Un solo sistema para operar y monitorizar
- Las queries pueden cruzar datos relacionales y semiestructurados
- El equipo no necesita aprender MongoDB

### Negativas o trade-offs
- Las queries sobre jsonb son más complejas que en MongoDB
- Rendimiento inferior a MongoDB en consultas de documentos muy anidados

### Neutrales
- Necesitamos documentar las convenciones de uso de jsonb en el proyecto
```

**Dónde guardar los ADR**: en el repositorio, típicamente en `docs/adr/` o `docs/decisions/`. Al dar este contexto al agente al inicio de cada sesión, mejora su comprensión del sistema.

---

## Spike-then-Spec

### Cuándo la exploración debe preceder a la especificación

El spike es una implementación experimental de bajo compromiso para validar una hipótesis técnica o aprender sobre un dominio desconocido antes de comprometerse con el diseño.

**Señales de que necesitas un spike antes de especificar**:
- Vas a integrar una API externa que no conoces
- Usas una tecnología nueva para el equipo
- El dominio del problema tiene incertidumbre técnica significativa
- No sabes qué es factible hasta probarlo

**El spike tiene estas características**:
1. **Objetivo acotado**: probar exactamente una hipótesis técnica
2. **Código desechable**: no va a producción, no tiene tests exhaustivos
3. **Tiempo fijo**: máximo 2-4 horas de trabajo
4. **Entregable**: conocimiento, no código

```text
Prompt para fase spike:

Necesito entender cómo funciona la API de Stripe para pagos en cuotas.
Este es un spike — el código es experimental y desechable.

Objetivos del spike:
1. ¿Cómo se crea un PaymentIntent con cuotas en Stripe?
2. ¿Qué webhooks necesito manejar para cuotas?
3. ¿Hay limitaciones que afecten a mi caso de uso? [describir caso]

Implementa el código mínimo para responder estas preguntas.
No te preocupes por manejo de errores ni tests todavía.
Al finalizar, dame un resumen de lo aprendido y las implicaciones para el diseño real.
```

Después del spike, usas lo aprendido para escribir una spec o un Design Doc sólido.

---

## Desarrollo iterativo: cuándo es suficiente y cuándo escala mal

El desarrollo iterativo (prompt → resultado → corrección → repetir) es la forma más natural de trabajar con un agente. También es la que más frecuentemente produce problemas a escala.

**Cuándo el desarrollo iterativo es la elección correcta**:
- Tareas menores de 2-3 horas de trabajo
- Cambios en un único fichero o componente aislado
- Scripts y herramientas de uso interno
- Prototipado para validar una idea

**Señales de que el desarrollo iterativo está escalando mal**:
- Has corregido el mismo tipo de problema 3 veces en la misma sesión
- El agente "olvidó" una instrucción dada al inicio de la sesión
- El código nuevo rompe algo que ya funcionaba
- No sabes si el resultado final cumple los requisitos originales

Cuando aparecen estas señales, detente. Invierte 20 minutos en escribir una spec, unos escenarios Gherkin o un conjunto de tests. Esos 20 minutos recuperarán horas de iteración ineficiente.

---

## Matriz de decisión consolidada — todas las estrategias

| Estrategia | Cuándo usar | Cuándo NO usar | Tiempo de setup | Calidad del resultado |
|------------|-------------|----------------|-----------------|----------------------|
| SDD | Proyectos nuevos, +3 módulos | Tareas pequeñas | Alto | Muy alta |
| TDD | Lógica de negocio, módulos complejos | UI puramente visual, scripts | Medio | Alta |
| BDD/Gherkin | Requisitos de negocio, colaboración | Implementación interna sin UI | Medio | Alta |
| PRD | Features con stakeholders no técnicos | Implementación técnica directa | Bajo-Medio | Depende del equipo |
| Design Doc/RFC | Cambios arquitectónicos, decisiones críticas | Tareas cotidianas | Alto | Muy alta |
| ADR | Decisiones que se quieren recordar | Decisiones triviales | Muy bajo | — (no es código) |
| Visual-Driven | UI desde mockup | Lógica de negocio | Muy bajo | Alta para UI |
| Fan-out | Migraciones y transformaciones masivas | Tareas únicas | Bajo | Alta si los batches son homogéneos |
| Spike-then-Spec | Tecnología desconocida, API externa nueva | Dominio conocido | Bajo (2-4h) | — (es exploración) |
| Writer/Reviewer | Cualquier código de producción | Prototipos desechables | Bajo | Muy alta |
| Desarrollo iterativo | Tareas pequeñas, prototipado | Proyectos complejos | Ninguno | Variable |

---

Con este capítulo completamos el panorama de estrategias. Los ejercicios de este módulo te darán práctica real eligiendo y aplicando cada una de ellas.

---

[← Anterior: Patrones avanzados](05-patrones-avanzados.md) | [Volver al índice →](../README.md)
