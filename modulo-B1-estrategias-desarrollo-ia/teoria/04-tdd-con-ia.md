# TDD con agentes de código

## El ciclo clásico: Red, Green, Refactor

TDD (Test-Driven Development) es una disciplina de desarrollo en la que los tests se escriben antes que el código de producción. El ciclo tiene tres fases:

```text
┌──────────┐     ┌──────────┐     ┌──────────┐
│   RED    │────▶│  GREEN   │────▶│ REFACTOR │
│          │     │          │     │          │
│ Test que │     │ Código   │     │ Mejora   │
│ falla    │     │ que pasa │     │ del      │
│          │     │ el test  │     │ diseño   │
└──────────┘     └──────────┘     └──────────┘
     ▲                                  │
     └──────────────────────────────────┘
                  (siguiente test)
```

- **Red**: escribes un test que describe el comportamiento esperado. El test falla porque el código no existe todavía.
- **Green**: implementas el código mínimo necesario para que el test pase. No más.
- **Refactor**: mejoras el diseño del código sin cambiar su comportamiento. Los tests garantizan que no rompes nada.

La clave es el orden: **test primero, implementación después**.

---

## Flujo TDD con agentes: tú defines el QUÉ, el agente ejecuta el CÓMO

Sin agente, TDD tiene un coste: escribir tests es tiempo. Con agente, ese coste cae drásticamente — el agente puede generar suites de tests completas desde una descripción.

El flujo cambia de la siguiente forma:

| Fase | Sin agente | Con agente |
|------|-----------|-----------|
| **Red** | Tú escribes los tests | Tú defines el comportamiento, el agente genera los tests |
| **Green** | Tú implementas el código | El agente implementa el código |
| **Refactor** | Tú mejoras el diseño | El agente propone mejoras, tú apruebas |
| **Tu rol** | Implementador | Diseñador y revisor |

Tu responsabilidad sigue siendo crítica: defines qué se prueba, evalúas si los tests son correctos, apruebas el refactor. El agente ejecuta, tú diriges.

---

## Patrón 1: Test-First básico

El patrón más simple. Describes el comportamiento en lenguaje natural y pides tests primero.

**Ejemplo: validación de email**

```text
Prompt para la fase Red:

Vamos a implementar una función validateEmail() en TypeScript.
Comportamiento esperado:
- Devuelve true para emails válidos (usuario@dominio.com, usuario@sub.dominio.co.uk)
- Devuelve false para: sin arroba, sin dominio, sin TLD, campo vacío, null, undefined
- No hace llamadas externas — solo validación de formato

Escribe los tests primero. No implementes la función todavía.
Usa Jest. Cada caso debe ser un test independiente con nombre descriptivo.
```

```typescript
// Tests generados por el agente (fase Red)
describe('validateEmail', () => {
    // Happy path
    test('devuelve true para email simple válido', () => {
        expect(validateEmail('usuario@dominio.com')).toBe(true);
    });

    test('devuelve true para email con subdominio', () => {
        expect(validateEmail('usuario@sub.dominio.co.uk')).toBe(true);
    });

    // Casos de fallo
    test('devuelve false para email sin arroba', () => {
        expect(validateEmail('usuariodominio.com')).toBe(false);
    });

    test('devuelve false para email sin dominio', () => {
        expect(validateEmail('usuario@')).toBe(false);
    });

    test('devuelve false para string vacío', () => {
        expect(validateEmail('')).toBe(false);
    });

    test('devuelve false para null', () => {
        expect(validateEmail(null)).toBe(false);
    });

    test('devuelve false para undefined', () => {
        expect(validateEmail(undefined)).toBe(false);
    });
});
```

```text
Prompt para la fase Green (después de verificar que los tests están bien):

Los tests están correctos. Ahora implementa validateEmail() para que todos pasen.
Implementación mínima — no añadas funcionalidad no requerida por los tests.
```

```text
Prompt para la fase Refactor:

Todos los tests pasan. ¿Hay alguna mejora de diseño que valga la pena aplicar?
Considera: legibilidad, mantenibilidad, rendimiento.
Cualquier cambio debe mantener todos los tests en verde.
```

---

## Patrón 2: TDD desde Gherkin

Cuando tienes escenarios Gherkin (capítulo 03), puedes usarlos directamente como punto de partida para TDD. Los escenarios se convierten en tests de aceptación, y TDD garantiza la implementación a nivel unitario.

```text
Prompt combinado Gherkin + TDD:

Tengo estos escenarios Gherkin para el módulo de autenticación:

[escenarios Gherkin]

Proceso:
1. Genera tests de aceptación que implementen cada escenario (fase Red)
2. A continuación, genera los tests unitarios para los componentes internos
3. Implementa el código para hacer pasar ambos niveles de tests (fase Green)
4. Finalmente, propón mejoras de diseño (fase Refactor)

Empieza por el paso 1. Espera mi aprobación antes de continuar.
```

Este patrón combina la especificación de alto nivel (Gherkin) con la precisión de bajo nivel (TDD unitario), produciendo un sistema bien especificado en dos capas.

---

## Patrón 3: TDD Inverso (Code-First, Test-Verify)

Para código legacy sin tests, el flujo se invierte: primero existe el código, y usas TDD para añadir cobertura de forma sistemática.

```text
Prompt TDD Inverso:

Aquí está el código del módulo de pagos existente:
[código]

El código no tiene tests. Antes de modificarlo necesito cobertura.
Proceso:
1. Analiza el código e identifica todos los comportamientos observables
2. Genera tests que documenten el comportamiento actual (no el ideal)
3. Ejecuto los tests — me dices cuáles esperas que fallen y por qué
4. Después podemos refactorizar con seguridad

Empieza por identificar los comportamientos.
```

El TDD Inverso es especialmente útil antes de:
- Refactorizar código legacy
- Añadir una nueva feature a un módulo sin tests
- Preparar una migración

**Regla**: los tests del TDD Inverso documentan el comportamiento actual, no el correcto. Algunos pueden descubrir bugs — eso es bueno, pero no los corrijas todavía. Primero cobertura, luego corrección.

---

## Patrón 4: TDD con objetivo de cobertura

Cuando necesitas cobertura alta (90%+), un agente puede sistemáticamente identificar los gaps y generar los tests faltantes.

```text
Prompt cobertura:

Ejecuta los tests actuales y revisa el informe de cobertura.
Identifica:
1. Las líneas y branches no cubiertos
2. Los casos de uso que esas líneas representan
3. Los tests que faltarían para llegar al 90% de branch coverage

Genera los tests en orden de importancia (prioriza la lógica de negocio sobre el boilerplate).
```

---

## Buenas prácticas en TDD con IA

**Un test = una cosa**. Si el test tiene más de un `assert` lógico (no cuenta verificar múltiples propiedades del mismo resultado), probablemente está probando demasiado.

**Tests independientes**. Cada test debe poder ejecutarse de forma aislada. El orden no debe importar. Usa `beforeEach` para setup, no dependencias entre tests.

**Nombres descriptivos**. El nombre del test debe ser suficiente para entender qué falla si falla. `test('falla')` es inútil. `test('devuelve 401 cuando el token ha expirado')` es valioso.

**Separa unit tests de integration tests**. Los tests unitarios son rápidos y no tienen efectos externos. Los tests de integración prueban múltiples componentes juntos. Mantenlos en carpetas separadas.

**El test verifica comportamiento, no implementación**. Si tu test tiene que saber que internamente usas un `Set` en lugar de un `Array`, el test está acoplado a la implementación y se romperá en cualquier refactor. Prueba el resultado, no el mecanismo.

---

## Cuándo usar TDD (y cuándo no)

| Situación | TDD | Razón |
|-----------|-----|-------|
| Lógica de negocio con reglas | Sí | Las reglas son los tests |
| API REST con validaciones | Sí | Cada endpoint tiene comportamiento definido |
| Módulo nuevo con requisitos claros | Sí | Los requisitos se convierten en tests |
| Prototipo exploratorio | No | Demasiado overhead para código desechable |
| Script de uso único | No | No justifica el coste |
| UI puramente visual | No (o poco) | Difícil de testear, valor bajo |
| Código legacy sin tests | TDD Inverso | Primero cobertura, luego refactor |
| Integración con API externa desconocida | Spike primero | Entiende la API antes de especificar |

---

## Prompt plantilla TDD genérico

```text
Contexto: [descripción del módulo/función a implementar]

Comportamiento esperado:
- [comportamiento 1]
- [comportamiento 2]
- [edge case 1]
- [edge case 2]

Restricciones técnicas: [lenguaje, framework de tests, dependencias disponibles]

Proceso TDD:
1. Fase Red: escribe los tests. No implementes el código de producción.
   Espera mi confirmación antes de continuar.
2. Fase Green: implementa el código mínimo para que todos los tests pasen.
   Espera mi confirmación antes de continuar.
3. Fase Refactor: propón mejoras de diseño. Lista qué cambiarías y por qué.
   No apliques nada sin mi aprobación.
```

La clave del prompt está en las instrucciones de espera entre fases. Sin ellas, el agente tiende a ejecutar las tres fases seguidas, lo que elimina tu capacidad de verificar cada una.

> Para técnicas avanzadas como property-based testing y mutation testing, ver el [Módulo E3: Testing Avanzado y AI Pair Programming](../../modulo-E3-testing-pair-programming/README.md).

---

[← Anterior: Gherkin y BDD](03-gherkin-bdd.md) | [Siguiente: Patrones avanzados de workflow →](05-patrones-avanzados.md)
