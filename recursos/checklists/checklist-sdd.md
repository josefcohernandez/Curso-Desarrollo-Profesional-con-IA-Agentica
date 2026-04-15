# Checklist SDD (Spec-Driven Development)

> Checklist rápido para las 4 fases de SDD. Consulta el [Módulo B2](../../modulo-B2-sdd-monografico/README.md) para la teoría completa.

---

## Fase 1: Entrevista

- [ ] Describir el proyecto en 2-3 líneas al agente
- [ ] Pedir al agente que entreviste sobre requisitos (no escribir la spec tú solo)
- [ ] Cubrir las 6 categorías: funcionalidad core, edge cases, requisitos no funcionales, integraciones, restricciones, UX
- [ ] Documentar las decisiones tomadas y sus razones
- [ ] Señal de parada: las preguntas del agente ya no descubren información nueva

## Fase 2: Especificación (SPEC.md)

- [ ] **Contexto**: por qué se construye, qué problema resuelve
- [ ] **Requisitos funcionales**: cada uno es verificable (no ambiguo)
- [ ] **Requisitos no funcionales**: rendimiento, seguridad, escalabilidad con métricas concretas
- [ ] **Diseño técnico**: arquitectura, modelo de datos, API, integraciones
- [ ] **Edge cases**: qué pasa cuando las cosas salen mal
- [ ] **Plan de testing**: cómo se verifica que funciona
- [ ] **Fases de implementación**: orden de construcción con dependencias
- [ ] **Exclusiones**: qué NO incluye esta versión
- [ ] Revisar la spec con el agente buscando ambigüedades y contradicciones
- [ ] Incluir ejemplos concretos (request/response, input/output)

## Fase 3: Implementación

- [ ] Iniciar sesión nueva (contexto limpio)
- [ ] Implementar fase por fase siguiendo el plan de implementación
- [ ] Escribir tests al final de cada fase
- [ ] Ejecutar tests antes de pasar a la siguiente fase
- [ ] Limpiar contexto entre fases no relacionadas
- [ ] No modificar la spec durante la implementación (documentar desviaciones para después)

## Fase 4: Verificación

- [ ] Iniciar sesión nueva para verificación (ojos frescos)
- [ ] Para cada requisito funcional: CUMPLIDO / PARCIAL / NO IMPLEMENTADO
- [ ] Incluir referencia al archivo/función que implementa cada requisito
- [ ] Generar VERIFICACION.md con el resultado
- [ ] Identificar y priorizar gaps
- [ ] Corregir gaps críticos antes de dar por terminado

---

## Señales de alerta

| Señal | Problema | Solución |
|-------|----------|----------|
| La entrevista dura más de 20 minutos | Scope demasiado amplio | Dividir en fases más pequeñas |
| La spec supera las 5 páginas | Over-specification | Separar qué/cómo, mover detalles técnicos al plan |
| El agente se desvía de la spec | Spec ambigua o incompleta | Volver a Fase 2, refinar la sección problemática |
| Más del 30% de requisitos NO IMPLEMENTADOS | Estimación incorrecta o spec irealista | Repriorizar, dividir en iteraciones |
