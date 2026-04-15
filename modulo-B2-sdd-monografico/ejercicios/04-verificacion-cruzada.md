# Ejercicio 04 — Verificación Cruzada

**Tiempo estimado**: 10 minutos  
**Dificultad**: Enfocado  
**Prerequisito**: haber leído el fichero de teoría 05 (Fase de Verificación)

---

## Contexto

La verificación cruzada es la fase que más frecuentemente se omite en proyectos reales — y la que más errores silenciosos detecta. En este ejercicio recibes una SPEC.md completa y una implementación con gaps intencionales. Tu trabajo es usar el agente como Reviewer para encontrar exactamente qué no se implementó, qué se implementó de forma incorrecta, y qué quedó a medias.

Este ejercicio simula una situación real: te unes a un proyecto en marcha, recibes el código y la spec, y necesitas saber con precisión en qué estado está la implementación.

---

## SPEC.md del proyecto (proporcionada)

```markdown
# API de Gestión de Tareas — Spec v1.1

## Requisitos Funcionales

- RF-01: CRUD completo de tareas (crear, leer, actualizar, borrar)
- RF-02: Una tarea tiene: título (obligatorio, 1-200 chars), descripción (opcional),
         prioridad (low/medium/high, default: medium), estado (pending/in_progress/completed)
- RF-03: Los usuarios solo pueden ver y modificar sus propias tareas.
         Intentar acceder a tarea de otro usuario devuelve 404 (no revelar existencia)
- RF-04: Al listar tareas, soportar filtro por estado y por prioridad (combinables)
- RF-05: Paginación en GET /tasks: 20 por defecto, máximo 100, con metadatos
         (total, page, per_page, total_pages)
- RF-06: Los títulos de tarea se almacenan normalizados (trim de espacios, sin dobles espacios)
- RF-07: Una tarea completada (status=completed) no puede volver a pending o in_progress
- RF-08: El endpoint DELETE /tasks/:id borra físicamente la tarea (no soft delete)

## Requisitos No Funcionales

- RNF-01: Autenticación con JWT, tokens válidos 24 horas
- RNF-02: Todos los inputs se validan y sanean antes de guardar en base de datos
- RNF-03: Rate limiting: 60 requests/minuto por usuario autenticado
- RNF-04: Los endpoints devuelven JSON con Content-Type: application/json siempre

## Edge Cases

- EC-01: Crear tarea con título que solo contiene espacios → 400 "El título no puede estar vacío"
- EC-02: Filtrar por estado con valor inválido (ej: ?status=done) → 400 con mensaje descriptivo
- EC-03: Página fuera de rango → 200 con lista vacía y metadatos correctos
- EC-04: Intentar cambiar estado de completed a pending → 422 "No se puede reabrir una tarea completada"
```

---

## Implementación a verificar

La implementación está en los siguientes fragmentos de código (simplificados para el ejercicio):

```javascript
// src/routes/tasks.js

router.post('/tasks', auth, async (req, res) => {
  const { title, description, priority } = req.body;

  if (!title) {
    return res.status(400).json({ error: 'El título es obligatorio' });
  }

  const task = await db.query(
    `INSERT INTO tasks (user_id, title, description, priority, status)
     VALUES ($1, $2, $3, $4, 'pending') RETURNING *`,
    [req.user.id, title, description, priority || 'medium']
  );

  res.status(201).json(task.rows[0]);
});

router.get('/tasks', auth, async (req, res) => {
  const { status, priority, page = 1 } = req.query;

  let query = 'SELECT * FROM tasks WHERE user_id = $1';
  const params = [req.user.id];

  if (status) {
    params.push(status);
    query += ` AND status = $${params.length}`;
  }

  if (priority) {
    params.push(priority);
    query += ` AND priority = $${params.length}`;
  }

  const tasks = await db.query(query, params);
  res.json(tasks.rows);
});

router.get('/tasks/:id', auth, async (req, res) => {
  const task = await db.query(
    'SELECT * FROM tasks WHERE id = $1',
    [req.params.id]
  );

  if (!task.rows[0]) {
    return res.status(404).json({ error: 'Tarea no encontrada' });
  }

  res.json(task.rows[0]);
});

router.patch('/tasks/:id', auth, async (req, res) => {
  const { title, description, status, priority } = req.body;

  const task = await db.query(
    `UPDATE tasks SET title = COALESCE($1, title),
     description = COALESCE($2, description),
     status = COALESCE($3, status),
     priority = COALESCE($4, priority)
     WHERE id = $5 AND user_id = $6 RETURNING *`,
    [title, description, status, priority, req.params.id, req.user.id]
  );

  if (!task.rows[0]) {
    return res.status(404).json({ error: 'Tarea no encontrada' });
  }

  res.json(task.rows[0]);
});

router.delete('/tasks/:id', auth, async (req, res) => {
  await db.query('DELETE FROM tasks WHERE id = $1', [req.params.id]);
  res.status(204).send();
});
```

---

## Instrucciones

### Paso 1: Identificar gaps visualmente (3 minutos)

Antes de usar el agente, lee el código y la spec por tu cuenta. Anota los gaps que encuentres tú mismo. Esto entrena el ojo crítico que el ejercicio busca desarrollar.

### Paso 2: Verificación con el agente (5 minutos)

Usa este prompt:

```text
Eres un revisor de código. Tienes una spec (SPEC.md) y una implementación.

SPEC.md:
[pega el SPEC.md de este ejercicio]

IMPLEMENTACIÓN:
[pega los fragmentos de código de este ejercicio]

Para cada requisito (RF-XX, RNF-XX, EC-XX) de la spec:
1. Busca evidencia en el código
2. Clasifica: CUMPLIDO / PARCIAL / NO IMPLEMENTADO
3. Para PARCIAL: describe exactamente qué falta
4. Para NO IMPLEMENTADO: confirma que no hay ningún código que lo cubra

Genera VERIFICACION.md con la tabla completa y un resumen con:
- Total CUMPLIDO, PARCIAL, NO IMPLEMENTADO
- Plan de corrección priorizado por impacto
```

### Paso 3: Comparación (2 minutos)

Compara los gaps que encontraste tú en el Paso 1 con los que encontró el agente. ¿Cuáles encontraste primero? ¿Cuáles encontró el agente que tú no viste?

---

## Solución de referencia

Los gaps intencionales en esta implementación son:

1. **RF-03 parcial**: GET /tasks/:id no verifica que la tarea pertenece al usuario actual — cualquier usuario autenticado puede ver cualquier tarea por ID.
2. **RF-05 no implementado**: GET /tasks no tiene paginación — devuelve todas las tareas.
3. **RF-06 no implementado**: el título no se normaliza (trim, dobles espacios).
4. **RF-07 no implementado**: PATCH no valida que completed no puede volver a pending/in_progress.
5. **EC-01 parcial**: valida que title no esté vacío, pero no valida títulos de solo espacios.
6. **EC-02 no implementado**: no se valida que los valores de status y priority sean válidos.
7. **EC-03 no implementado**: no hay lógica de paginación, así que el edge case no aplica.
8. **EC-04 no implementado**: no hay validación de transiciones de estado.
9. **RNF-03 no implementado**: no hay rate limiting.
10. **DELETE sin autorización**: DELETE /tasks/:id borra sin verificar que pertenece al usuario.

---

## Entregables

1. **Lista de gaps identificados por ti** (Paso 1)
2. **VERIFICACION.md** generado por el agente (Paso 2)
3. **Comparación**: qué gaps encontraste tú vs. el agente

---

## Criterios de Evaluación

| Criterio | Excelente | Aceptable | Necesita mejora |
|----------|-----------|-----------|-----------------|
| Gaps identificados manualmente | 6+ gaps antes del agente | 3-5 gaps | Menos de 3 gaps |
| VERIFICACION.md | Todos los gaps documentados con evidencia | La mayoría documentados | Menos de la mitad |
| Clasificación correcta | CUMPLIDO/PARCIAL/NO IMPLEMENTADO preciso | Algún error de clasificación | Clasificación incorrecta frecuente |
| Plan de corrección | Priorizado por impacto (seguridad primero) | Sin priorización clara | Sin plan de corrección |
| Detección del bug de seguridad | RF-03 y DELETE sin autorización identificados | Uno de los dos identificado | Ninguno identificado |

---

> **El gap más crítico de todos**: el bug de seguridad en DELETE /tasks/:id — cualquier usuario autenticado puede borrar tareas de cualquier otro usuario. Este tipo de vulnerabilidad pasa desapercibida sin una verificación sistemática, especialmente cuando los tests solo cubren el happy path (el usuario borrando sus propias tareas).
