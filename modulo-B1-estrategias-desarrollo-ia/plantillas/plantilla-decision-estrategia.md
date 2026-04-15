# Plantilla: Matriz de Decisión de Estrategia

Usa esta plantilla cuando no tengas claro qué estrategia aplicar. Puntúa cada criterio, suma los puntos por columna y elige la estrategia con mayor puntuación.

---

## Cómo usar esta plantilla

1. Lee la descripción de cada criterio
2. Asigna la puntuación correspondiente según tu situación (los valores de cada fila son excluyentes)
3. Suma los puntos por columna (estrategia)
4. La estrategia con más puntos es la recomendada
5. Si hay empate, considera la columna "Tiempo de setup" como desempate

---

## Matriz de decisión

### Criterio 1: Complejidad del problema

| Situación | SDD | TDD | Gherkin | Visual | Fan-out | Iterativo | Spike |
|-----------|-----|-----|---------|--------|---------|-----------|-------|
| Muchos módulos interdependientes | 3 | 1 | 2 | 0 | 1 | 0 | 1 |
| Módulo único con lógica compleja | 1 | 3 | 2 | 0 | 0 | 1 | 1 |
| Componente UI/visual | 0 | 1 | 1 | 3 | 0 | 1 | 0 |
| Tarea repetitiva a escala | 0 | 0 | 0 | 0 | 3 | 0 | 0 |
| Tarea simple o script | 0 | 1 | 0 | 0 | 0 | 3 | 0 |
| Tecnología desconocida | 0 | 0 | 0 | 0 | 0 | 0 | 3 |

**Mi puntuación** (elige una fila): SDD: ___ / TDD: ___ / Gherkin: ___ / Visual: ___ / Fan-out: ___ / Iterativo: ___ / Spike: ___

---

### Criterio 2: Archivos afectados

| Situación | SDD | TDD | Gherkin | Visual | Fan-out | Iterativo | Spike |
|-----------|-----|-----|---------|--------|---------|-----------|-------|
| Más de 20 archivos | 2 | 0 | 0 | 0 | 3 | 0 | 0 |
| 5-20 archivos | 3 | 2 | 2 | 0 | 1 | 0 | 1 |
| 2-5 archivos | 1 | 3 | 2 | 1 | 0 | 1 | 1 |
| 1 archivo | 0 | 2 | 1 | 2 | 0 | 3 | 1 |

**Mi puntuación**: SDD: ___ / TDD: ___ / Gherkin: ___ / Visual: ___ / Fan-out: ___ / Iterativo: ___ / Spike: ___

---

### Criterio 3: Tests existentes

| Situación | SDD | TDD | Gherkin | Visual | Fan-out | Iterativo | Spike |
|-----------|-----|-----|---------|--------|---------|-----------|-------|
| Sin tests (código nuevo) | 2 | 3 | 3 | 1 | 1 | 1 | 1 |
| Sin tests (código existente a modificar) | 1 | 3 | 1 | 0 | 0 | 0 | 1 |
| Con tests parciales | 1 | 2 | 2 | 0 | 1 | 1 | 0 |
| Con tests robustos | 0 | 1 | 1 | 0 | 2 | 2 | 0 |

**Mi puntuación**: SDD: ___ / TDD: ___ / Gherkin: ___ / Visual: ___ / Fan-out: ___ / Iterativo: ___ / Spike: ___

---

### Criterio 4: Tipo de tarea

| Situación | SDD | TDD | Gherkin | Visual | Fan-out | Iterativo | Spike |
|-----------|-----|-----|---------|--------|---------|-----------|-------|
| Proyecto greenfield (desde cero) | 3 | 2 | 2 | 0 | 0 | 0 | 1 |
| Feature nueva en producto existente | 2 | 2 | 3 | 0 | 0 | 1 | 0 |
| Bug fix | 0 | 3 | 1 | 0 | 0 | 1 | 0 |
| Refactor sin cambio de comportamiento | 1 | 3 | 1 | 0 | 1 | 0 | 0 |
| Migración / transformación | 1 | 1 | 0 | 0 | 3 | 0 | 1 |
| UI / componente visual | 0 | 1 | 1 | 3 | 0 | 1 | 0 |
| Prototipo / validación de idea | 0 | 0 | 0 | 1 | 0 | 2 | 3 |

**Mi puntuación**: SDD: ___ / TDD: ___ / Gherkin: ___ / Visual: ___ / Fan-out: ___ / Iterativo: ___ / Spike: ___

---

### Criterio 5: Stakeholders involucrados

| Situación | SDD | TDD | Gherkin | Visual | Fan-out | Iterativo | Spike |
|-----------|-----|-----|---------|--------|---------|-----------|-------|
| Solo desarrolladores | 2 | 3 | 2 | 1 | 2 | 2 | 2 |
| Desarrolladores + producto/negocio | 2 | 1 | 3 | 1 | 0 | 0 | 1 |
| Múltiples equipos | 3 | 1 | 2 | 0 | 1 | 0 | 0 |

**Mi puntuación**: SDD: ___ / TDD: ___ / Gherkin: ___ / Visual: ___ / Fan-out: ___ / Iterativo: ___ / Spike: ___

---

### Criterio 6: Urgencia / tiempo disponible

| Situación | SDD | TDD | Gherkin | Visual | Fan-out | Iterativo | Spike |
|-----------|-----|-----|---------|--------|---------|-----------|-------|
| Tengo tiempo para hacerlo bien | 3 | 3 | 3 | 2 | 2 | 1 | 2 |
| Tiempo ajustado — plazo en días | 1 | 2 | 2 | 2 | 2 | 2 | 1 |
| Urgente — plazo en horas | 0 | 1 | 1 | 2 | 1 | 3 | 1 |

**Mi puntuación**: SDD: ___ / TDD: ___ / Gherkin: ___ / Visual: ___ / Fan-out: ___ / Iterativo: ___ / Spike: ___

---

## Resumen de puntuaciones

Suma las puntuaciones de todos los criterios:

| Estrategia | Puntuación total | Tiempo de setup |
|------------|-----------------|-----------------|
| SDD | | Alto (30-60 min) |
| TDD | | Medio (15-30 min) |
| Gherkin/BDD | | Medio (20-40 min) |
| Visual-Driven | | Muy bajo (5 min) |
| Fan-out | | Bajo (10-20 min) |
| Desarrollo iterativo | | Ninguno |
| Spike | | Bajo (empieza de inmediato) |

**Estrategia ganadora**: ___________________

**Estrategia secundaria** (si la ganadora tiene empate o puntuación similar): ___________________

---

## Sección de contexto adicional

Completa si la matriz no da un ganador claro:

**¿El error en producción de esta implementación tiene alto impacto?**
- Sí (sistema de pagos, autenticación, datos críticos) → penaliza el desarrollo iterativo, favorece TDD + Reviewer
- No (feature de bajo riesgo) → el desarrollo iterativo puede ser suficiente

**¿Este código lo va a tocar otra persona?**
- Sí → favorece estrategias con documentación implícita (Gherkin, TDD, SDD)
- No (script personal) → el desarrollo iterativo es aceptable

**¿El dominio del problema es claro para ti?**
- Sí → puedes especificar directamente (SDD, Gherkin, TDD)
- No → considera Spike primero

---

## Ejemplo rellenado: nuevo servicio de notificaciones

**Situación**: necesito implementar un servicio de notificaciones (email + SMS) para una aplicación SaaS. 3 módulos: templates, envío, registro de entrega. El equipo de producto tiene requisitos en Confluence. Plazo: 2 semanas.

| Criterio | Situación elegida | SDD | TDD | Gherkin | Visual | Fan-out | Iterativo | Spike |
|----------|-------------------|-----|-----|---------|--------|---------|-----------|-------|
| Complejidad | Módulos interdependientes | **3** | 1 | 2 | 0 | 1 | 0 | 1 |
| Archivos | 5-20 archivos | **3** | 2 | 2 | 0 | 1 | 0 | 1 |
| Tests | Sin tests (código nuevo) | 2 | **3** | **3** | 1 | 1 | 1 | 1 |
| Tipo de tarea | Feature nueva en producto | 2 | 2 | **3** | 0 | 0 | 1 | 0 |
| Stakeholders | Devs + producto | 2 | 1 | **3** | 1 | 0 | 0 | 1 |
| Urgencia | Tengo tiempo | **3** | **3** | **3** | 2 | 2 | 1 | 2 |
| **TOTAL** | | **15** | **12** | **16** | **4** | **5** | **3** | **6** |

**Resultado**: Gherkin/BDD (16 puntos) ganador, SDD (15) segunda opción.

**Decisión**: Usar Gherkin para definir los escenarios con el equipo de producto, luego TDD para la implementación. Combinación Gherkin + TDD.
