# 03 — Fase 2: Escribir SPEC.md

## El artefacto más importante del proyecto

SPEC.md no es documentación — es una herramienta de trabajo activa. Es el documento al que el agente va a referirse en cada decisión durante la implementación, y al que tú vas a referirte cuando alguien te pregunte "¿por qué funciona así?" o "¿era este el comportamiento esperado?".

Una buena spec tiene tres propiedades:

1. **Verificable**: cada requisito tiene un criterio de éxito observable
2. **Completa en las decisiones clave**: no deja al agente elegir libremente en áreas que importen
3. **Concisa en los detalles irrelevantes**: no especifica cómo implementar lo que el agente puede inferir del contexto

---

## Estructura de SPEC.md

```markdown
# [Nombre del proyecto / feature]

## Contexto
Por qué se construye, qué problema resuelve, quién lo usa.

## Requisitos Funcionales
Lista verificable de lo que el sistema debe hacer.

## Requisitos No Funcionales
Rendimiento, seguridad, disponibilidad, escalabilidad.

## Diseño Técnico
### Arquitectura
### Modelo de Datos
### API / Interfaces
### Integraciones

## Edge Cases y Manejo de Errores
Comportamiento esperado en situaciones límite.

## Plan de Testing
Qué se va a testear y cómo.

## Fases de Implementación
Plan ordenado por dependencias.

## Exclusiones
Qué NO incluye esta versión (explícito).
```

Cada sección tiene un propósito específico. Las omisiones no son neutrales — si no defines cómo manejar un error, el agente elegirá por ti.

---

## Las cinco reglas de calidad

### Regla 1: Cada requisito debe ser verificable

Un requisito verificable tiene un criterio de éxito observable sin ambigüedad.

| Requisito vago (no verificable) | Requisito específico (verificable) |
|---------------------------------|------------------------------------|
| "El sistema debe ser rápido" | "El endpoint GET /products responde en menos de 200ms para el percentil 95 con 1000 productos" |
| "El formulario debe validar los datos" | "El formulario valida: email con formato válido, password de 8+ caracteres con al menos una mayúscula y un número, nombre de 2-100 caracteres" |
| "Manejar errores adecuadamente" | "En caso de error de red, mostrar mensaje 'Error de conexión. Inténtalo de nuevo.' y botón de reintento" |
| "El usuario debe poder autenticarse" | "El usuario puede autenticarse con email y password. Un token JWT válido por 24h se devuelve en la respuesta" |

### Regla 2: Incluir exclusiones explícitas

Las exclusiones previenen el scope creep durante la implementación. Sin exclusiones, el agente puede decidir añadir features "razonables" que no pediste.

```markdown
## Exclusiones

Esta versión NO incluye:
- Autenticación con proveedores externos (Google, GitHub) — se añadirá en v2
- Notificaciones por SMS — solo email y push
- API pública para terceros — solo uso interno
- Internacionalización — solo español
- Modo offline — requiere conexión permanente
```

### Regla 3: Separar el QUÉ del CÓMO

La spec define el comportamiento observable, no la implementación interna. Si especificas el cómo, estás tomando decisiones de diseño que el agente puede hacer mejor con su conocimiento del stack.

| Mezcla QUÉ y CÓMO (malo) | Solo el QUÉ (bueno) |
|--------------------------|---------------------|
| "Usar Redis para cachear los productos con TTL de 5 minutos" | "Los resultados de búsqueda de productos se sirven en menos de 100ms" |
| "Usar una tabla notifications con columnas id, user_id, type, read_at, created_at" | "El sistema mantiene un historial de notificaciones por usuario, con estado leído/no leído" |

Excepción: cuando tienes restricciones técnicas reales (el cliente tiene Redis ya instalado, el sistema hereda una base de datos existente), sí se incluyen en la sección de restricciones técnicas.

### Regla 4: Incluir ejemplos concretos

Los ejemplos eliminan ambigüedad mejor que las descripciones. Especialmente en APIs, los ejemplos de request/response son más claros que cualquier descripción textual.

Ejemplo de cómo documentar un endpoint en la spec:

**Sección a añadir en SPEC.md — Endpoint: Crear tarea**

Request (POST /api/v1/tasks):

```json
{
  "title": "Preparar presentación Q4",
  "due_date": "2025-01-15",
  "priority": "high",
  "tags": ["trabajo", "presentación"]
}
```

Response exitosa (201):

```json
{
  "id": "task_01jk2m3n4p",
  "title": "Preparar presentación Q4",
  "due_date": "2025-01-15",
  "priority": "high",
  "tags": ["trabajo", "presentación"],
  "completed": false,
  "created_at": "2025-01-10T09:30:00Z"
}
```

Error de validación (400):

```json
{
  "error": "validation_error",
  "fields": {
    "title": "El título es obligatorio",
    "due_date": "La fecha debe ser futura"
  }
}
```

### Regla 5: Definir criterios de éxito medibles

La spec debe indicar cómo se va a demostrar que el sistema funciona correctamente. Esto enlaza directamente con el plan de testing.

```markdown
## Criterios de Éxito

El sistema se considera completo cuando:
- [ ] Todos los endpoints responden según los ejemplos de la spec
- [ ] Los tests de integración cubren el 80% de los requisitos funcionales
- [ ] El tiempo de respuesta del endpoint más lento es < 500ms bajo carga normal
- [ ] Los edge cases documentados tienen comportamiento verificado
- [ ] VERIFICACION.md muestra 100% de requisitos en estado CUMPLIDO o PARCIAL (con plan)
```

---

## Revisión de la spec antes de implementar

Antes de pasar a implementación, la spec debe pasar por tres niveles de revisión:

### Auto-revisión con checklist

```markdown
- [ ] ¿Todos los requisitos funcionales son verificables?
- [ ] ¿Hay exclusiones explícitas?
- [ ] ¿Los edge cases de la entrevista están documentados?
- [ ] ¿El plan de testing es concreto?
- [ ] ¿Las fases de implementación están ordenadas por dependencias?
- [ ] ¿Hay ejemplos de request/response para las APIs?
- [ ] ¿Los requisitos no funcionales tienen números concretos?
```

### Revisión por el agente

```text
Revisa esta spec antes de que empiece la implementación.
Busca:
1. Requisitos ambiguos o no verificables
2. Contradicciones entre secciones
3. Edge cases que no están cubiertos
4. Dependencias entre fases que no están en el orden correcto
5. Exclusiones que debería añadir para prevenir scope creep
Reporta como lista priorizada.
```

### Revisión cruzada

Para proyectos importantes, abre una sesión nueva (sin el contexto de la entrevista) y pide al agente que revise la spec como si fuera un revisor externo:

```text
Eres un revisor de especificaciones. Lee este SPEC.md y dime:
1. ¿Qué información falta para implementar esto correctamente?
2. ¿Qué decisiones técnicas importantes no están tomadas?
3. ¿Hay requisitos contradictorios?
4. ¿Qué asumiría un agente implementador sin que lo hubieras pedido?
```

---

## Ejemplo completo anotado: API de tareas

```markdown
# API de Gestión de Tareas v1

## Contexto
<!-- Por qué existe: el problema real que resuelve -->
API REST interna para la app móvil de productividad. Los usuarios necesitan
gestionar tareas con categorías, prioridades y fechas de vencimiento.
El equipo móvil consume esta API — no hay clientes externos.

## Requisitos Funcionales
<!-- Lista verificable — cada ítem puede marcarse como ✓ o ✗ -->

### Tareas
- RF-01: CRUD completo sobre tareas (crear, leer, actualizar, eliminar)
- RF-02: Una tarea tiene: título (obligatorio), descripción (opcional),
         fecha de vencimiento (opcional), prioridad (low/medium/high),
         estado (pending/in_progress/completed), categoría (opcional)
- RF-03: Las tareas se pueden marcar como completadas
- RF-04: Las tareas completadas permanecen accesibles (no se borran)
- RF-05: El usuario solo puede ver sus propias tareas

### Categorías
- RF-06: CRUD sobre categorías personales del usuario
- RF-07: Cada tarea puede pertenecer a una o ninguna categoría
- RF-08: Borrar una categoría no borra las tareas que la usan
         (las tareas quedan sin categoría)

### Búsqueda y filtrado
- RF-09: Filtrar tareas por estado, prioridad y categoría (combinables)
- RF-10: Ordenar por fecha de vencimiento, prioridad o fecha de creación
- RF-11: Paginación: 20 tareas por página por defecto, máximo 100

## Requisitos No Funcionales
<!-- Números concretos, no descripciones vagas -->
- RNF-01: Tiempo de respuesta < 200ms en p95 bajo carga normal (hasta 100 req/s)
- RNF-02: Autenticación con JWT, tokens válidos por 24 horas
- RNF-03: Rate limiting: 100 requests/minuto por usuario autenticado
- RNF-04: Los datos de usuario no se comparten entre usuarios bajo ninguna circunstancia
- RNF-05: API versioning: prefijo /api/v1/

## Diseño Técnico

### Modelo de Datos

**tasks**
- id: UUID (PK)
- user_id: UUID (FK → users)
- title: VARCHAR(200) NOT NULL
- description: TEXT
- due_date: DATE
- priority: ENUM(low, medium, high) DEFAULT medium
- status: ENUM(pending, in_progress, completed) DEFAULT pending
- category_id: UUID (FK → categories, nullable)
- created_at: TIMESTAMPTZ
- updated_at: TIMESTAMPTZ

**categories**
- id: UUID (PK)
- user_id: UUID (FK → users)
- name: VARCHAR(100) NOT NULL
- color: VARCHAR(7) (hex color, opcional)

### Endpoints principales

POST   /api/v1/tasks          → Crear tarea
GET    /api/v1/tasks          → Listar tareas (con filtros)
GET    /api/v1/tasks/:id      → Obtener tarea
PATCH  /api/v1/tasks/:id      → Actualizar tarea
DELETE /api/v1/tasks/:id      → Borrar tarea

POST   /api/v1/categories     → Crear categoría
GET    /api/v1/categories     → Listar categorías del usuario
PATCH  /api/v1/categories/:id → Actualizar categoría
DELETE /api/v1/categories/:id → Borrar categoría

## Edge Cases y Manejo de Errores

- Intentar ver/editar/borrar tarea de otro usuario → 404 (no revelar existencia)
- Borrar categoría con tareas asociadas → 200, las tareas quedan con category_id = null
- Fecha de vencimiento en el pasado → se acepta (permite registrar tareas atrasadas)
- Filtros con valores no válidos → 400 con mensaje descriptivo
- Paginación: página fuera de rango → 200 con lista vacía

## Plan de Testing

- Tests de integración con base de datos real (no mocks)
- Cobertura mínima: todos los endpoints + todos los edge cases documentados
- Tests de autenticación: token válido, token expirado, token ausente, token malformado
- Tests de autorización: usuario A no puede ver datos de usuario B

## Fases de Implementación

### Fase 1: Estructura base (sin auth)
- Setup de base de datos y migraciones
- CRUD básico de tareas sin autenticación
- Tests de los endpoints básicos

### Fase 2: Autenticación y autorización
- Middleware de JWT
- Filtrado automático por user_id
- Tests de autenticación y autorización

### Fase 3: Categorías
- CRUD de categorías
- Asociación tarea-categoría
- Comportamiento al borrar categoría

### Fase 4: Filtrado y paginación
- Query parameters de filtro
- Paginación
- Ordenación

## Exclusiones

Esta v1 NO incluye:
- Subtareas o dependencias entre tareas
- Compartir tareas con otros usuarios
- Notificaciones por fecha de vencimiento
- Attachments (archivos adjuntos)
- Comentarios en tareas
- Etiquetas (tags) — distinto de categorías
- Integración con calendarios externos
```

---

## La spec como documento vivo

La spec no es un documento que se escribe una vez y se archiva. Cuando durante la implementación descubres que un requisito es incorrecto o incompleto, lo corriges en la spec **antes** de continuar:

1. Para la implementación
2. Actualiza SPEC.md con el cambio
3. Anota la razón del cambio
4. Continúa la implementación

Este hábito mantiene SPEC.md como la fuente de verdad real del sistema, no un documento que diverge del código desde el primer día.

---

> **Anti-patrones frecuentes**:
> - **Spec demasiado vaga**: "El sistema debe gestionar usuarios" — sin requisitos funcionales específicos
> - **Over-specification**: especificar cada nombre de columna, cada índice de base de datos, cada función — dejando al agente sin espacio para tomar decisiones técnicas que es perfectamente capaz de tomar
> - **Mezclar QUÉ y CÓMO**: "Usar Redux para gestionar el estado" en lugar de "El estado de la aplicación debe persistir entre navegación de pantallas"
> - **Spec sin exclusiones**: invitación abierta al scope creep
