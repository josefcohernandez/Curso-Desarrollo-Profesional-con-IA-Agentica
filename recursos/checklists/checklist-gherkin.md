# Checklist de Calidad Gherkin

> Checklist para verificar la calidad de features escritas en Gherkin. Consulta el [Módulo B1](../../modulo-B1-estrategias-desarrollo-ia/README.md) para la teoría completa.

---

## Estructura

- [ ] Cada `Feature` tiene una descripción "Como [rol] quiero [acción] para [beneficio]"
- [ ] Los `Scenario` tienen nombres descriptivos que indican comportamiento, no "Test caso 1"
- [ ] Se usa `Background` para precondiciones comunes (evita repetición)
- [ ] Se usa `Scenario Outline` + `Examples` para variaciones del mismo caso
- [ ] Los archivos `.feature` están organizados por funcionalidad

## Sintaxis Given/When/Then

- [ ] `Given` solo describe estado inicial (precondiciones)
- [ ] `When` solo describe acciones del usuario
- [ ] `Then` solo describe resultados esperados
- [ ] No hay acciones en `Given` ni verificaciones en `When`
- [ ] Se usa `And` para pasos adicionales en la misma sección
- [ ] Se usa `But` para excepciones o condiciones negativas

## Cobertura

- [ ] Cada feature tiene escenarios para el **happy path**
- [ ] Cada feature tiene escenarios para **errores de validación**
- [ ] Se cubren **permisos y autorización** cuando aplica
- [ ] Se cubren **edge cases** (vacío, nulo, límites, caracteres especiales)
- [ ] Los `Scenario Outline` cubren todas las variaciones relevantes

## Calidad

- [ ] Un escenario = un comportamiento (no mezclar múltiples verificaciones)
- [ ] Se usa el lenguaje del dominio, no jerga técnica
- [ ] Los escenarios son independientes entre sí (no dependen del orden)
- [ ] Los datos de ejemplo son realistas (no "foo", "bar", "test123")
- [ ] Se incluyen números concretos cuando aplica (status codes, límites)

## Uso con agentes IA

- [ ] Cada scenario puede mapearse directamente a un test
- [ ] Los `Scenario Outline` con `Examples` generan tests parametrizados
- [ ] La feature es suficiente para que el agente genere tests sin ambigüedad
- [ ] Los criterios de aceptación son objetivos: feature "hecha" = todos los escenarios pasan
