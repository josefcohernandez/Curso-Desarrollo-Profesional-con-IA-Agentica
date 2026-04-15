# 06 — SDD Aplicado a Diferentes Contextos

## SDD no es igual en todos los proyectos

El ciclo SDD (entrevista → spec → implementación → verificación) es el mismo en todos los contextos, pero el énfasis en cada fase varía según el tipo de proyecto. En un proyecto greenfield, la entrevista exploratoria es crítica. En una feature para código legacy, el trabajo de ingeniería inversa del estado actual es tan importante como la especificación del estado deseado.

Este fichero mapea cómo adaptar SDD a los cinco contextos más comunes.

---

## Greenfield: SDD como fundamento del proyecto

En un proyecto greenfield, SDD no es una opción — es la diferencia entre una arquitectura coherente y una "arquitectura emergente caótica" donde cada sesión del agente toma decisiones de diseño independientes que se contradicen entre sí.

**El problema sin SDD en greenfield**: en la primera sesión, el agente crea una estructura de archivos; en la segunda, añade código que no sigue esa estructura; en la tercera, introduce patrones que contradicen los de la primera. El resultado es un codebase que nadie entiende y que acumula deuda técnica desde el día 1.

**Flujo SDD para greenfield**:

```text
1. Entrevista exploratoria (20 min)
   → Descubre requisitos funcionales, no funcionales, restricciones técnicas

2. SPEC.md (15 min)
   → Especificación completa con modelo de datos, APIs, fases

3. ARCHITECTURE.md (10 min)
   → Decisiones de arquitectura: estructura de directorios, frameworks, patrones
   → Se genera desde la spec, no independientemente

4. Scaffolding (5 min)
   → Estructura de directorios, archivos placeholder, configuración
   → El agente sigue ARCHITECTURE.md exactamente

5. Implementación por fases
   → Una feature funcional al final de cada fase

6. Verificación
   → Al final de cada fase, no solo al final del proyecto
```

El archivo ARCHITECTURE.md es específico de greenfield. Es una extensión de la spec que documenta las decisiones técnicas que la spec deja abiertas:

```markdown
# ARCHITECTURE.md

## Stack
- Runtime: Node.js 22 con TypeScript strict
- Framework: Fastify (por performance y schema validation integrado)
- Base de datos: PostgreSQL 16 con Drizzle ORM
- Testing: Vitest + Supertest para tests de integración

## Estructura de directorios
src/
  routes/        → Definición de endpoints
  handlers/      → Lógica de manejo de requests
  services/      → Lógica de negocio
  repositories/  → Acceso a base de datos
  models/        → Tipos y esquemas de Zod
  middleware/    → Auth, rate limiting, logging

## Convenciones
- Un archivo por recurso en cada capa
- Tests en __tests__/ junto a cada módulo
- Variables de entorno solo en src/config.ts
```

---

## Legacy: SDD como diff entre estados

Con código legacy, la spec tiene dos partes: el estado actual (documentado mediante ingeniería inversa) y el estado deseado (especificado como en un proyecto normal). La implementación es el camino entre ambos.

**El problema sin SDD en legacy**: el agente modifica el código sin entender el comportamiento actual, rompe invariantes no documentadas, y produce regresiones que nadie detecta porque no hay tests.

**Flujo SDD para legacy**:

```text
1. Reverse-spec: documentar el estado actual (30 min)
   "Lee src/legacy/payment-processor.js.
   Documenta en SPEC-ACTUAL.md:
   - Qué hace exactamente (comportamiento observable)
   - Inputs y outputs con tipos
   - Side effects (base de datos, eventos, llamadas externas)
   - Edge cases implícitos que encuentres en el código"

2. Tests del estado actual (antes de cambiar nada)
   "Escribe tests para el comportamiento ACTUAL documentado en SPEC-ACTUAL.md.
   No cambies el código. Solo añade tests."

3. SPEC-DESEADO.md: especificar el estado objetivo
   → La nueva feature, el refactoring, la migración

4. Gap analysis
   "Compara SPEC-ACTUAL.md con SPEC-DESEADO.md.
   Lista las diferencias: qué cambia, qué se añade, qué se elimina."

5. Implementación incremental
   → Pequeños pasos, verificando en cada paso que los tests anteriores siguen pasando
```

La reverse-spec es el artefacto más valioso del trabajo con legacy — es la primera vez que existe documentación del comportamiento real del sistema.

---

## APIs: OpenAPI como complemento de SPEC.md

Para APIs REST o GraphQL, SPEC.md se complementa con una especificación formal de la API (OpenAPI/Swagger). La secuencia es:

```text
SPEC.md define el comportamiento → OpenAPI define el contrato → Código implementa el contrato
```

**El flujo**:

```text
1. Entrevista de requisitos (normal)

2. SPEC.md con sección de API
   → Los endpoints, modelos de datos y comportamiento en lenguaje natural

3. Generar OpenAPI desde la spec
   "Genera el archivo openapi.yaml desde la sección de API de SPEC.md.
   Usa OpenAPI 3.1. Incluye: paths, requestBodies, responses, schemas y ejemplos."

4. Revisar OpenAPI antes de implementar
   "Valida que openapi.yaml es sintácticamente correcto.
   Verifica que cubre todos los endpoints de SPEC.md."

5. Implementar siguiendo OpenAPI como contrato
   → El agente tiene dos fuentes de verdad: SPEC.md (comportamiento) y openapi.yaml (contrato)

6. Verificación doble
   → Tests de integración contra los endpoints
   → Validación automática de que las respuestas cumplen el schema de OpenAPI
```

Este enfoque es especialmente valioso cuando hay múltiples equipos consumiendo la API — el OpenAPI es el contrato que todos los equipos pueden verificar independientemente.

---

## Frontend: specs de componentes y flujos de usuario

En frontend, SDD se aplica a dos niveles:

**Nivel de componente** (specs de componentes):

```markdown
# Spec: Componente TaskCard

## Props
- task: Task (obligatorio)
- onComplete: () => void
- onDelete: () => void
- isLoading?: boolean (default: false)

## Estados visuales
- Estado normal: muestra título, fecha, prioridad como badge de color
- Estado cargando: skeleton loader sobre todo el componente
- Estado completada: título con strikethrough, badge "Completada" en verde

## Comportamiento
- Click en checkbox → llama onComplete()
- Click en papelera → confirmación modal → llama onDelete()
- La card no es clickeable mientras isLoading === true
```

**Nivel de flujo de usuario** (specs de user journeys):

```markdown
# Spec: Flujo de Creación de Tarea

## Actores: usuario autenticado, sistema

## Flujo principal
1. Usuario hace click en "Nueva tarea"
2. Se abre modal con formulario
3. Usuario rellena título (obligatorio) y opcionales
4. Usuario hace click en "Crear"
5. El sistema valida los campos
6. El sistema crea la tarea y cierra el modal
7. La nueva tarea aparece en la lista con animación de entrada

## Flujos alternativos
- Validación falla (2a): se muestran errores bajo cada campo incorrecto
- Error de red (2b): toast "Error al crear la tarea. Inténtalo de nuevo."
- Click en "Cancelar" (2c): modal se cierra, no se crea nada
```

La combinación con Visual-Driven Development (VDD): si tienes un mockup o screenshot del diseño, inclúyelo como referencia en la spec y en los prompts de implementación. El agente puede ver imágenes y usarlas como referencia de diseño junto a la spec.

---

## Microservicios: specs por servicio + spec de integración

En arquitecturas de microservicios, SDD opera en dos niveles:

**Spec por servicio**: cada servicio tiene su SPEC.md propio, que incluye la API que expone y las APIs que consume.

**Spec de integración**: documenta cómo interactúan los servicios entre sí.

```markdown
# Spec de Integración: Order Service ↔ Inventory Service

## Contrato de comunicación
- El Order Service llama a Inventory Service al crear un pedido
- Endpoint: POST /internal/inventory/reserve
- Timeout: 2 segundos
- Retry policy: 2 reintentos con backoff exponencial

## Comportamientos en caso de fallo
- Si Inventory Service no responde: Order Service devuelve 503 al cliente
- Si el inventario es insuficiente: 422 con lista de items sin stock
- Si hay timeout en el reintento: la orden queda en estado "pending_inventory"
  y un worker reintenta cada 5 minutos durante 30 minutos

## Formato de mensajes
[schemas de request/response con ejemplos]
```

---

## Monorepos: specs que referencian múltiples packages

En monorepos, la spec puede referenciar código de múltiples packages. La clave es hacer explícitas las dependencias:

```markdown
## Dependencias entre packages

Este cambio afecta a:
- packages/api: nuevos endpoints de tags
- packages/shared: nuevo tipo Tag en los tipos compartidos
- packages/web: componentes de UI para gestionar tags
- packages/mobile: mismo, para la app móvil

## Orden de implementación
1. packages/shared: definir el tipo Tag y las interfaces
2. packages/api: implementar los endpoints
3. packages/web y packages/mobile: consumir los endpoints (en paralelo)
```

---

## Cuándo SDD es excesivo

| Tarea | Justificación para omitir SDD |
|-------|-------------------------------|
| Bug fix de una línea | El comportamiento correcto es obvio y verificable inmediatamente |
| Prototipo que se va a tirar | La spec no va a sobrevivir al prototipo |
| Script de un uso | Sin mantenimiento futuro, la inversión no se amortiza |
| Cambio cosmético (CSS, copy) | Sin lógica de negocio, sin edge cases |
| Exploración técnica | El objetivo es aprender, no producir código de producción |

La regla práctica: si el resultado del trabajo va a ser revisado por alguien más (code review, QA, stakeholder), SDD añade valor. Si es solo para ti y se descarta en 24 horas, es excesivo.

---

## El triple stack metodológico: SDD + Gherkin + TDD

La combinación más robusta para desarrollo profesional con IA agéntica:

```text
SDD define el QUÉ
  ↓
Gherkin define el CÓMO SE VERIFICA
  ↓
TDD implementa el CÓMO

Resultado: código que hace exactamente lo especificado,
verificado de forma legible, implementado incrementalmente.
```

Esta combinación no es para todos los proyectos — es el nivel profesional para código crítico que va a vivir en producción durante años. Para proyectos más simples, SDD solo (sin Gherkin y TDD formales) es suficiente.

---

> **Referencia**: el [Módulo B1 - Estrategias de Desarrollo con IA](../../modulo-B1-estrategias-desarrollo-ia/README.md) de este curso profundiza en la combinación SDD + Gherkin + TDD con ejemplos de implementación detallados y casos de uso por tipo de proyecto.
