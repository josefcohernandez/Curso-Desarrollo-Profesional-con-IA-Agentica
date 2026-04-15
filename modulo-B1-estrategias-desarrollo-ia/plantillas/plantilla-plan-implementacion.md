# Plantilla: Plan de Implementación

Usa esta plantilla para organizar el trabajo de implementación antes de empezar a dar prompts al agente. Un plan de implementación bien definido evita el "loop de la desesperación" y permite al agente trabajar de forma más efectiva.

---

## Cabecera del plan

```markdown
# Plan de implementación: [Nombre del módulo/feature]

Fecha: YYYY-MM-DD
Estrategia: [SDD / TDD / Gherkin+TDD / Visual-Driven / Fan-out / otra]
Estimación total: [N horas / N días]
Spec o PRD de referencia: [enlace o nombre del fichero]
```

---

## Estructura de fases

Divide el trabajo en fases de máximo 4 horas cada una. Cada fase debe tener un resultado verificable.

### Plantilla de fase

```markdown
## Fase N: [Nombre descriptivo]

**Objetivo**: [Una oración que describe qué debe existir al final de esta fase]

**Dependencias**: [qué debe estar completado antes de empezar esta fase]

**Tareas**:
- [ ] Tarea 1 — [descripción breve]
- [ ] Tarea 2 — [descripción breve]
- [ ] Tarea 3 — [descripción breve]

**Sesiones del agente previstas**:
| # | Tipo de sesión | Prompt principal | Resultado esperado |
|---|----------------|------------------|--------------------|
| 1 | Writer | [resumen del prompt] | [qué produce] |
| 2 | Reviewer | [resumen del prompt] | [qué produce] |

**Verificación de la fase**:
- [ ] [criterio 1 verificable objetivamente]
- [ ] [criterio 2 verificable objetivamente]
- [ ] Los tests de esta fase están en verde

**Estimación**: [N horas]
```

---

## Ejemplo completo: módulo de autenticación JWT

```markdown
# Plan de implementación: Módulo de autenticación JWT

Fecha: 2026-04-15
Estrategia: TDD
Estimación total: 6 horas
Spec de referencia: docs/spec-autenticacion.md

## Fase 1: Hashing de contraseñas

**Objetivo**: hash_password() y verify_password() implementadas y con tests en verde

**Dependencias**: ninguna (es la base del módulo)

**Tareas**:
- [ ] Instalar dependencia bcrypt
- [ ] Tests de hash_password (red phase)
- [ ] Implementación de hash_password (green phase)
- [ ] Tests de verify_password (red phase)
- [ ] Implementación de verify_password (green phase)
- [ ] Refactor si aplica

**Sesiones del agente previstas**:
| # | Tipo de sesión | Prompt principal | Resultado esperado |
|---|----------------|------------------|--------------------|
| 1 | Writer (Red) | Tests de hash_password y verify_password | Suite de tests que falla |
| 2 | Writer (Green) | Implementación de ambas funciones | Tests en verde |
| 3 | Reviewer | Review del módulo de hashing | Informe de problemas |
| 4 | Fix | Correcciones del reviewer | Código final |

**Verificación de la fase**:
- [ ] pytest pasa sin errores
- [ ] Dos llamadas a hash_password con el mismo input producen hashes diferentes
- [ ] verify_password con hash manipulado devuelve False sin excepción
- [ ] Cobertura de la fase: 90%+

**Estimación**: 1.5 horas

---

## Fase 2: Validación de credenciales

**Objetivo**: validate_email() y check_password_strength() con tests en verde

**Dependencias**: Fase 1 completada

**Tareas**:
- [ ] Tests de validate_email — todos los formatos válidos e inválidos (red)
- [ ] Implementación de validate_email (green)
- [ ] Tests de check_password_strength — todos los criterios (red)
- [ ] Implementación de check_password_strength (green)
- [ ] Refactor

**Sesiones del agente previstas**:
| # | Tipo de sesión | Prompt principal | Resultado esperado |
|---|----------------|------------------|--------------------|
| 1 | Writer (Red+Green) | Tests + implementación de validate_email | Módulo con tests en verde |
| 2 | Writer (Red+Green) | Tests + implementación de check_password_strength | Módulo con tests en verde |
| 3 | Reviewer (security) | Revisión enfocada en bypass de validaciones | Informe de seguridad |

**Verificación de la fase**:
- [ ] validate_email maneja None sin excepción
- [ ] check_password_strength devuelve issues descriptivos, no solo un booleano
- [ ] Cobertura: 90%+ en ambos módulos

**Estimación**: 1.5 horas

---

## Fase 3: Generación y validación de JWT

**Objetivo**: generate_token() y validate_token() con tests en verde

**Dependencias**: Fase 2 completada

**Tareas**:
- [ ] Tests de generate_token (red)
- [ ] Implementación de generate_token (green)
- [ ] Tests de validate_token — válido, expirado, manipulado (red)
- [ ] Implementación de validate_token (green)
- [ ] Refactor + configuración por variables de entorno

**Sesiones del agente previstas**:
| # | Tipo de sesión | Prompt principal | Resultado esperado |
|---|----------------|------------------|--------------------|
| 1 | Writer (Red) | Tests de generate_token y validate_token | Suite que falla |
| 2 | Writer (Green) | Implementación JWT | Tests en verde |
| 3 | Reviewer (security) | Review enfocado en secretos y algoritmos | Informe de problemas |

**Verificación de la fase**:
- [ ] Token expirado devuelve None, no excepción
- [ ] Secret JWT viene de variable de entorno, no hardcodeado
- [ ] Algoritmo HS256 (no "none")

**Estimación**: 1.5 horas

---

## Fase 4: Integración y tests de sistema

**Objetivo**: todos los módulos funcionan juntos correctamente en el flujo de login

**Dependencias**: Fases 1-3 completadas

**Tareas**:
- [ ] Test de integración del flujo completo: registro → login → acceso con token
- [ ] Test de flujo de error: contraseña débil → rechazo con mensaje descriptivo
- [ ] Test de flujo de expiración: token expirado → reautenticación
- [ ] Documentación del módulo (docstrings)

**Verificación de la fase**:
- [ ] El flujo completo de registro+login funciona end-to-end
- [ ] La cobertura total del módulo es 90%+
- [ ] No hay imports circulares entre submódulos

**Estimación**: 1.5 horas
```

---

## Estimación de costos (opcional)

Si tu herramienta tiene costos por token, usa esta sección para estimar antes de empezar:

```markdown
## Estimación de costos

| Fase | Sesiones previstas | Tokens estimados por sesión | Costo estimado |
|------|-------------------|----------------------------|----------------|
| Fase 1 | 4 | ~5.000 | ~$0.10 |
| Fase 2 | 3 | ~4.000 | ~$0.08 |
| Fase 3 | 3 | ~6.000 | ~$0.12 |
| Fase 4 | 2 | ~8.000 | ~$0.15 |
| **Total** | **12** | | **~$0.45** |

Nota: estas estimaciones son para orientación. Los costos reales dependen
del modelo usado y de la longitud real de las conversaciones.
```

---

## Checklist de cierre del plan

Al finalizar toda la implementación, verifica:

- [ ] Todas las fases completadas
- [ ] Tests de todas las fases en verde
- [ ] Cobertura total >= 90%
- [ ] Reviewer ejecutado al menos una vez por módulo crítico
- [ ] Documentación actualizada (README, docstrings)
- [ ] No hay TODO o FIXME sin resolver
- [ ] El código funciona en el entorno de staging/CI

---

## Prompt para generar el plan con el agente

Si la spec ya existe, el agente puede ayudarte a generar el plan de implementación:

```text
Aquí está la spec del módulo [nombre]:
[pega la spec]

Genera un plan de implementación en fases usando TDD como estrategia.
Cada fase debe:
- Tener un objetivo verificable
- Durar máximo 4 horas
- Listar las sesiones del agente necesarias (Writer, Reviewer, Fix)
- Tener criterios de verificación objetivos

Identifica las dependencias entre fases y el orden correcto.
```
