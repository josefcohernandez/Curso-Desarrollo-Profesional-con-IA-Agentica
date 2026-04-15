# VERIFICACION.md — [Nombre del proyecto / feature]

> **Instrucciones de uso**: completa este documento durante la fase de verificación (Fase 4 de SDD).
> Usa una sesión limpia del agente (sin contexto de implementación) para mayor objetividad.
> Este documento es el artefacto de salida de la verificación — no se modifica la spec ni el código.

**Fecha de verificación**: [YYYY-MM-DD]
**Commit/versión verificada**: [hash del commit o tag de versión]
**Revisado por**: [nombre o "sesión Reviewer — sin contexto de implementación"]
**Spec de referencia**: [enlace a SPEC.md o versión]

---

## Requisitos Funcionales

> _Para cada RF de la spec, documenta el estado y la evidencia. La evidencia debe ser
> concreta: archivo y número de línea, función, o test que lo verifica.
> "Parece que funciona" no es evidencia válida._

| ID | Requisito | Estado | Evidencia | Notas |
|----|-----------|--------|-----------|-------|
| RF-01 | [descripción breve del requisito] | [CUMPLIDO / PARCIAL / NO IMPLEMENTADO] | [archivo:línea, función, test] | [observaciones] |
| RF-02 | [descripción breve] | | | |
| RF-03 | [descripción breve] | | | |
| RF-04 | [descripción breve] | | | |
| RF-05 | [descripción breve] | | | |

> _Añade filas para todos los RF de la spec._

---

## Requisitos No Funcionales

| ID | Requisito | Estado | Evidencia | Notas |
|----|-----------|--------|-----------|-------|
| RNF-01 | [descripción breve] | | | |
| RNF-02 | [descripción breve] | | | |
| RNF-03 | [descripción breve] | | | |

---

## Edge Cases

| ID | Edge Case | Estado | Evidencia | Notas |
|----|-----------|--------|-----------|-------|
| EC-01 | [descripción breve] | | | |
| EC-02 | [descripción breve] | | | |
| EC-03 | [descripción breve] | | | |

---

## Problemas Adicionales Encontrados

> _Problemas detectados durante la verificación que no están en la spec:
> bugs, vulnerabilidades de seguridad, inconsistencias de comportamiento.
> No bloquea el cierre de la verificación, pero deben documentarse._

| # | Tipo | Descripción | Severidad | Recomendación |
|---|------|-------------|-----------|---------------|
| 1 | [Bug / Seguridad / Inconsistencia] | [descripción] | [Alta / Media / Baja] | [acción recomendada] |

> _Si no hay problemas adicionales, escribe: "No se encontraron problemas adicionales durante la verificación."_

---

## Resumen

| Estado | Cantidad | Porcentaje |
|--------|----------|------------|
| CUMPLIDO | [N] | [X%] |
| PARCIAL | [N] | [X%] |
| NO IMPLEMENTADO | [N] | [X%] |
| **Total requisitos** | **[N]** | |

**Valoración general**: [Una frase sobre el estado general de la implementación]

---

## Plan de Corrección

> _Para cada ítem PARCIAL o NO IMPLEMENTADO, documenta qué se necesita hacer para cerrarlo.
> Ordena por prioridad: primero los problemas de seguridad, luego los funcionales bloqueantes,
> luego los que afectan al rendimiento, finalmente los opcionales._

### Prioridad Alta (bloquean el lanzamiento o son problemas de seguridad)

| # | ID Requisito | Descripción del gap | Acción correctiva | Esfuerzo estimado |
|---|-------------|---------------------|-------------------|-------------------|
| 1 | [RF-XX] | [qué falta] | [qué implementar] | [Xh / Xd] |
| 2 | [RNF-XX] | [qué falta] | [qué implementar] | [Xh / Xd] |

### Prioridad Media (mejoran la calidad o cierran casos parciales)

| # | ID Requisito | Descripción del gap | Acción correctiva | Esfuerzo estimado |
|---|-------------|---------------------|-------------------|-------------------|
| 1 | [RF-XX] | [qué falta] | [qué implementar] | [Xh / Xd] |

### Prioridad Baja (mejoras para versiones futuras)

| # | ID Requisito | Descripción del gap | Acción correctiva | Esfuerzo estimado |
|---|-------------|---------------------|-------------------|-------------------|
| 1 | [RF-XX] | [qué falta] | [qué implementar] | [Xh / Xd] |

> _Si no hay ítems en alguna categoría de prioridad, elimina esa sección._

---

## Decisión de Verificación

> _Marca la decisión final sobre el estado de esta implementación._

- [ ] **APROBADA**: todos los requisitos críticos están CUMPLIDOS. Puede pasar a producción.
- [ ] **APROBADA CON OBSERVACIONES**: requisitos no críticos están PARCIALES o NO IMPLEMENTADOS. Puede pasar a producción con el plan de corrección aprobado para el siguiente ciclo.
- [ ] **RECHAZADA**: hay requisitos críticos PARCIALES o NO IMPLEMENTADOS, o problemas de seguridad sin resolver. No puede pasar a producción hasta que se corrijan los ítems de Prioridad Alta.

**Justificación**: [razón de la decisión, especialmente si es RECHAZADA]

---

## Historial de Verificaciones

> _Si esta no es la primera verificación de este proyecto, registra el historial._

| Fecha | Versión | % CUMPLIDO | Decisión | Notas |
|-------|---------|------------|----------|-------|
| [YYYY-MM-DD] | [commit] | [X%] | [APROBADA / RECHAZADA] | [resumen] |
| [fecha actual] | [commit actual] | [X%] | [decisión actual] | |

> _Si es la primera verificación, elimina esta sección._
