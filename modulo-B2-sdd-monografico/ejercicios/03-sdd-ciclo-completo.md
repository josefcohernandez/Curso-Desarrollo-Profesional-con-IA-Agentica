# Ejercicio 03 — Ciclo SDD Completo

**Tiempo estimado**: 30 minutos  
**Dificultad**: Avanzado  
**Prerequisito**: haber completado los Ejercicios 01 y 02, y leído los ficheros de teoría 04 y 05

---

## Contexto

Este es el ejercicio central del módulo. Ejecutarás el ciclo SDD completo — las cuatro fases — para un proyecto realista. El objetivo es experimentar el flujo end-to-end y producir cuatro artefactos concretos: SPEC.md, features Gherkin, código con tests, y VERIFICACION.md.

El proyecto es una API de gestión de tareas con usuarios, categorías y prioridades. Es deliberadamente más complejo que los ejercicios anteriores para que las cuatro fases tengan peso real.

**Nota sobre tiempo**: 30 minutos es el tiempo mínimo si el agente es eficiente y tú tienes clara la dirección. Si en alguna fase te atascas más de 10 minutos, pasa a la siguiente con lo que tengas — el objetivo es practicar el flujo completo, no la perfección en cada fase.

---

## Proyecto: API de gestión de tareas

Una API REST con:
- Gestión de usuarios (registro, login, perfil)
- Tareas con título, descripción, prioridad (low/medium/high/urgent), fecha de vencimiento
- Categorías propias del usuario
- Las tareas pueden tener una categoría (o ninguna)
- Las tareas se listan por usuario, filtrables por categoría y prioridad

---

## Fase 1: Entrevista (5 minutos)

Inicia la entrevista con este prompt:

```text
Voy a construir una API REST de gestión de tareas con usuarios, categorías y prioridades.
Características básicas: registro/login de usuarios, CRUD de tareas con prioridad y 
fecha de vencimiento, categorías del usuario, filtrado por categoría y prioridad.

Entrevístame para descubrir los requisitos que no he mencionado.
Limita la entrevista a 5-6 preguntas enfocadas en edge cases y requisitos no obvios.
Al terminar, genera un resumen de las decisiones tomadas.
```

Responde con decisiones concretas. Si no sabes una respuesta, elige la opción más simple y documenta esa elección.

### Decisiones que debes tomar durante la entrevista

Prepárate para responder:

- ¿Qué pasa con las tareas cuando se borra una categoría? (Recomendación: quedan sin categoría)
- ¿Las categorías se comparten entre usuarios o son privadas? (Recomendación: privadas)
- ¿Qué pasa con las tareas de un usuario si se borra el usuario? (Recomendación: se borran en cascada)
- ¿Hay rate limiting? (Recomendación: sí, 100 req/min por usuario — es buena práctica desde v1)
- ¿Los tokens JWT tienen refresh token? (Recomendación: no en v1)

---

## Fase 2: Spec (5 minutos)

Pide al agente que genere SPEC.md:

```text
Con las decisiones de la entrevista, genera SPEC.md conciso pero completo.
Incluye: RF (5-8 requisitos clave), RNF (3-4 concretos), modelo de datos
(3 tablas: users, tasks, categories), endpoints principales (lista, sin ejemplos extensos),
edge cases, plan de testing breve, 3 fases de implementación, y exclusiones.
Prioriza la claridad sobre la exhaustividad — esto es para un ejercicio de 30 minutos.
```

Revisa SPEC.md con el checklist de 2 minutos:

```markdown
- [ ] ¿Hay al menos 5 RF verificables?
- [ ] ¿Los RNF tienen números (no "rápido")?
- [ ] ¿El modelo de datos está completo?
- [ ] ¿Las exclusiones están presentes?
```

Si falta algo crítico, pide al agente que lo añada. Si es un detalle menor, sigue adelante.

---

## Fase 3: Implementación (15 minutos)

### 3a: Generar features Gherkin (3 minutos)

```text
Genera features Gherkin para los requisitos más críticos de SPEC.md.
Crea al menos 2 features: una para autenticación y otra para CRUD de tareas.
Incluye escenarios para: happy path y el edge case más importante de cada feature.
Guarda en features/auth.feature y features/tasks.feature
```

### 3b: Iniciar implementación con sesión limpia (12 minutos)

**Importante**: abre una sesión nueva de tu agente (nueva conversación, nueva pestaña) antes de este paso.

```text
[NUEVA SESIÓN]

Lee SPEC.md completo antes de escribir código.

Implementa la Fase 1 de la spec:
- Setup de proyecto (Node.js/Express o el stack que prefieras con TypeScript)
- Modelo de base de datos y migraciones (pueden ser scripts SQL, no necesita Prisma/Drizzle)
- Autenticación: registro y login con JWT
- Tests de autenticación: registro exitoso, login correcto, login con credenciales incorrectas,
  token válido, token ausente

No implementes las fases 2 y 3 todavía.
Al terminar, ejecuta los tests y muéstrame el output.
```

Si en 12 minutos no has terminado la Fase 1 completa, para donde estés. El ejercicio sigue.

---

## Fase 4: Verificación (5 minutos)

Abre una tercera sesión nueva para el reviewer.

```text
[NUEVA SESIÓN — REVIEWER]

Eres un revisor de código. Revisa la implementación actual contra SPEC.md.

Para cada requisito (RF-XX, RNF-XX) de la spec:
1. Busca evidencia de implementación
2. Clasifica: CUMPLIDO / PARCIAL / NO IMPLEMENTADO
3. Para PARCIAL: qué falta
4. Para NO IMPLEMENTADO: confirma que no existe

Recuerda: solo está implementada la Fase 1 (autenticación). Las fases 2 y 3 serán
NO IMPLEMENTADO — es esperado.

Genera VERIFICACION.md con la tabla completa y un resumen.
```

---

## Entregables

1. **SPEC.md** — con los 3 requisitos mínimos cubiertos (RF, RNF, modelo de datos)
2. **features/*.feature** — al menos 2 features con happy path + 1 edge case cada una
3. **Código de Fase 1** — estructura del proyecto + autenticación funcionando + tests pasando
4. **VERIFICACION.md** — clasificación de todos los requisitos de la spec (incluyendo los NO IMPLEMENTADOS de fases 2 y 3)

---

## Reflexión

1. ¿En qué fase invertiste más tiempo? ¿Fue el tiempo bien invertido?
2. ¿La sesión limpia de implementación (sin el contexto de la entrevista) cambió algo? ¿Qué preguntas hizo el agente que no habría hecho si hubiera tenido el contexto de la entrevista?
3. ¿La verificación encontró algo que no habías notado?
4. ¿El flujo completo de 30 minutos te pareció más o menos eficiente que cómo normalmente empezarías este tipo de proyecto?

---

## Criterios de Evaluación

| Criterio | Excelente | Aceptable | Necesita mejora |
|----------|-----------|-----------|-----------------|
| SPEC.md | RF verificables, RNF concretos, modelo de datos, exclusiones | La mayoría de secciones completas | Spec incompleta o con requisitos vagos |
| Features Gherkin | 2+ features con happy path y edge case | 1-2 features solo con happy path | Sin features o inválidas |
| Implementación Fase 1 | Tests pasan, autenticación completa, estructura correcta | Tests pasan pero estructura mejorable | Tests no pasan o implementación incompleta |
| VERIFICACION.md | Todos los RF clasificados con evidencia concreta | Clasificación sin evidencia | Sin VERIFICACION.md |
| Ciclo completo | 4 fases en 4 sesiones/contextos distintos | 3-4 fases completadas | Menos de 3 fases |
