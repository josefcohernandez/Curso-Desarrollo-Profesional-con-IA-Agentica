# SPEC.md — [Nombre del proyecto / feature]

> **Instrucciones de uso**: sustituye todos los placeholders entre corchetes por el contenido real.
> Borra las instrucciones en cursiva antes de compartir la spec con el equipo.
> Versión: 1.0 | Fecha: [YYYY-MM-DD] | Autor: [nombre]

---

## Contexto

> _Explica por qué se construye esto: qué problema real resuelve, quién lo usa, y qué valor aporta.
> No copies la descripción del ticket — escribe la motivación real._

[2-4 frases explicando el problema y la solución propuesta]

**Sistema/proyecto**: [nombre del sistema donde se integra esto]
**Usuarios/actores**: [quién usa esto directamente]
**Versión**: [v1, v2, feature X, etc.]

---

## Requisitos Funcionales

> _Cada requisito debe ser verificable — debe tener un criterio de éxito observable.
> Usa el ID (RF-XX) para referenciarte desde otras secciones (edge cases, tests, verificación)._

- **RF-01**: [descripción verificable del requisito]
- **RF-02**: [descripción verificable del requisito]
- **RF-03**: [descripción verificable del requisito]
- **RF-04**: [descripción verificable del requisito]
- **RF-05**: [descripción verificable del requisito]

> _Añade los RF que necesites. Para sistemas complejos, agrúpalos por subsistema:_
> ```
> ### [Subsistema A]
> - RF-01: ...
> - RF-02: ...
> ### [Subsistema B]
> - RF-03: ...
> ```

---

## Requisitos No Funcionales

> _Los RNF deben tener números concretos, no descripciones vagas.
> "El sistema debe ser rápido" no es un RNF — "respuesta < 200ms en p95" sí lo es._

- **RNF-01**: [rendimiento] — [métrica concreta, ej: tiempo de respuesta < Xms en pXX bajo Y req/s]
- **RNF-02**: [seguridad] — [descripción concreta, ej: autenticación con JWT, tokens válidos X horas]
- **RNF-03**: [disponibilidad/escalabilidad] — [métrica, ej: rate limiting X req/min por usuario]
- **RNF-04**: [otros] — [ej: API versioning, formato de respuesta, logging]

---

## Diseño Técnico

### Arquitectura

> _Describe la estructura del sistema a alto nivel. Solo lo necesario para que el agente tome
> las decisiones de implementación correctas. No especifiques detalles de implementación
> a menos que sean restricciones reales._

[Descripción de la arquitectura: capas, módulos principales, flujo de datos]

### Modelo de Datos

> _Define las entidades principales y sus relaciones. Para cada tabla/entidad, lista los campos
> con tipo y restricciones. Para relaciones, especifica cardinalidad y comportamiento en cascada._

**[Entidad 1]**
- `id`: [tipo] (PK)
- `[campo]`: [tipo] ([restricciones])
- `[campo_fk]`: [tipo] (FK → [tabla_referenciada], [ON DELETE BEHAVIOR])
- `created_at`: TIMESTAMPTZ
- `updated_at`: TIMESTAMPTZ

**[Entidad 2]**
- `id`: [tipo] (PK)
- `[campo]`: [tipo] ([restricciones])

### API / Interfaces

> _Si es una API REST, lista los endpoints con método, path y propósito.
> Para los más complejos, añade ejemplo de request y response._

```text
[MÉTODO]  /[path]          → [descripción breve]
[MÉTODO]  /[path]/:id      → [descripción breve]
[MÉTODO]  /[path]          → [descripción breve]
```

> _Para el endpoint principal o el más complejo, añade el ejemplo request/response:_

**Ejemplo: [nombre del endpoint]**

Request:
```json
{
  "[campo]": "[valor de ejemplo]",
  "[campo]": "[valor de ejemplo]"
}
```

Response exitosa ([código]):
```json
{
  "id": "[ejemplo]",
  "[campo]": "[valor]",
  "[campo]": "[valor]"
}
```

Response de error ([código]):
```json
{
  "error": "[código de error]",
  "message": "[mensaje legible]"
}
```

### Integraciones

> _Lista los sistemas externos con los que interactúa, qué operaciones y qué pasa si fallan._

| Sistema | Tipo de integración | Operaciones | Comportamiento en fallo |
|---------|---------------------|-------------|------------------------|
| [sistema] | [REST / evento / SDK] | [qué hace] | [qué ocurre si no responde] |

> _Si no hay integraciones externas, elimina esta sección._

---

## Edge Cases y Manejo de Errores

> _Para cada situación límite identificada en la entrevista o el análisis, documenta
> el comportamiento esperado. Usa el formato "Situación → Comportamiento esperado"._

- **EC-01**: [situación] → [comportamiento: código HTTP, mensaje de error, acción del sistema]
- **EC-02**: [situación] → [comportamiento]
- **EC-03**: [situación] → [comportamiento]

> _Los edge cases más importantes de documentar son:_
> - Inputs inválidos o malformados
> - Recursos que no existen o pertenecen a otro usuario
> - Operaciones sobre estados inválidos
> - Fallos de sistemas externos
> - Condiciones de carrera o concurrencia (si aplica)

---

## Plan de Testing

> _Especifica QUÉ se va a testear, no solo "hacer tests".
> Incluye: tipo de test (unitario/integración/e2e), qué cubre cada tipo, y criterio mínimo._

**Tipo de tests**: [integración con DB real / unitarios / e2e]

**Cobertura mínima requerida**:
- Todos los RF con al menos un test de happy path
- Todos los edge cases documentados
- Autenticación: [token válido, expirado, ausente, malformado]
- Autorización: [un usuario no puede acceder a datos de otro]

**Casos específicos a cubrir**:
- [caso 1: descripción]
- [caso 2: descripción]
- [caso 3: descripción]

---

## Fases de Implementación

> _Divide la implementación en fases ordenadas por dependencias.
> Cada fase debe producir un sistema funcionando (aunque incompleto).
> Al final de cada fase, los tests de esa fase deben pasar._

### Fase 1: [nombre]
- [ítem 1]
- [ítem 2]
- Tests: [qué se testea en esta fase]

### Fase 2: [nombre]
> _Prerequisito: Fase 1 completa y sus tests pasando_
- [ítem 1]
- [ítem 2]
- Tests: [qué se testea en esta fase]

### Fase 3: [nombre]
> _Prerequisito: Fase 2 completa y sus tests pasando_
- [ítem 1]
- [ítem 2]
- Tests: [qué se testea en esta fase]

---

## Exclusiones

> _Lista explícita de lo que NO incluye esta versión.
> Las exclusiones previenen el scope creep durante la implementación.
> Si el agente propone añadir algo que no está en la spec, esta lista es la referencia._

Esta versión NO incluye:
- [exclusión 1: descripción y por qué se excluye]
- [exclusión 2]
- [exclusión 3]
- [exclusión 4]

---

## Criterios de Éxito

> _Cómo se demuestra que la implementación está completa._

La implementación se considera completa cuando:
- [ ] Todos los RF tienen al menos un test que los verifica
- [ ] Los tests de integración pasan en el entorno de CI
- [ ] VERIFICACION.md muestra todos los RF en estado CUMPLIDO o PARCIAL con plan
- [ ] [criterio específico del proyecto]
- [ ] [criterio específico del proyecto]

---

## Checklist de Calidad de la Spec

Antes de comenzar la implementación, verifica:

```markdown
Requisitos
- [ ] Todos los RF son verificables (tienen criterio de éxito observable)
- [ ] Los RNF tienen números concretos
- [ ] Las exclusiones están presentes y son específicas

Diseño técnico
- [ ] El modelo de datos cubre todos los RF
- [ ] Los ejemplos de request/response están incluidos para endpoints no triviales
- [ ] Las integraciones externas tienen comportamiento en fallo documentado

Edge cases
- [ ] Los edge cases de la entrevista están documentados
- [ ] Los casos de error (4xx, 5xx) tienen mensajes de error definidos

Plan de implementación
- [ ] Las fases están ordenadas por dependencias
- [ ] Cada fase tiene criterios de tests definidos
- [ ] La Fase 1 es implementable sin dependencias externas

Calidad
- [ ] La spec no mezcla el QUÉ con el CÓMO en los RF
- [ ] No hay requisitos contradictorios
- [ ] La spec puede leerse sin el contexto de la entrevista
```
