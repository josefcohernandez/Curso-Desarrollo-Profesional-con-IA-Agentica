# Ejercicio 2: Gherkin desde requisitos vagos

## Contexto

Los requisitos raramente llegan bien especificados. Lo más común es recibir una descripción vaga de negocio y tener que convertirla en algo que el agente pueda implementar y que tú puedas verificar objetivamente.

En este ejercicio recibes 3 requisitos vagos, los conviertes en features Gherkin, pides al agente que genere tests desde ellos, y verificas que la implementación pasa todos los escenarios.

**Tiempo estimado**: 20 minutos  
**Objetivo**: transformar requisitos vagos en especificaciones ejecutables y usarlas para guiar la implementación

---

## Requisito 1: Carrito de compras

**Requisito vago**:
> "Los usuarios pueden añadir productos al carrito. El carrito debe manejar cantidades. Hay un límite por producto. El carrito se puede vaciar."

**Ambigüedades detectadas** (no las resuelvas con suposiciones — escríbelas como preguntas en el Feature):
- ¿Cuál es el límite máximo de cantidad por producto?
- ¿Puede el carrito tener 0 productos? ¿O el mínimo es 1?
- ¿"Vaciar" elimina todos los productos o solo reduce cantidades a 0?
- ¿Qué pasa si se intenta añadir un producto que no existe?

**Tu tarea**: escribe un fichero `carrito.feature` con al menos:
- 1 Background con las precondiciones comunes
- 3 Scenarios del happy path
- 2 Scenarios de validación/error
- 1 Scenario Outline para los límites de cantidad

---

## Requisito 2: Registro de usuarios

**Requisito vago**:
> "Los usuarios se pueden registrar con email y contraseña. El email debe ser único. La contraseña tiene que ser segura. Al registrarse, el usuario recibe un email de bienvenida."

**Tu tarea**: escribe `registro.feature` con al menos:
- 1 Scenario del happy path completo
- Scenarios para: email duplicado, formato de email inválido, contraseña débil
- 1 Scenario Outline para validar al menos 4 variantes de contraseñas débiles con tabla Examples
- 1 Scenario para verificar que el email de bienvenida se envía

**Criterio de contraseña segura** (defínelo tú — el requisito no lo especifica): usa las convenciones más comunes del sector (longitud mínima, mayúsculas, números, caracteres especiales) y documenta tu decisión al inicio del fichero como un comentario.

---

## Requisito 3: Búsqueda de productos

**Requisito vago**:
> "Hay una búsqueda de productos. Funciona por nombre. También se puede filtrar. Los resultados son paginados."

**Tu tarea**: escribe `busqueda.feature` con al menos:
- 1 Scenario de búsqueda exitosa con resultados
- 1 Scenario de búsqueda sin resultados
- 1 Scenario de búsqueda con filtro (elige el criterio de filtro más útil: precio, categoría, etc.)
- 1 Scenario Outline para la paginación con diferentes tamaños de página

---

## Flujo completo con el agente

Una vez tengas los 3 ficheros Feature escritos, sigue este flujo:

### Paso 1: Generar los tests

```text
Prompt para el agente:

Tengo estos escenarios Gherkin para el módulo de [nombre]:

[pega el contenido del fichero .feature]

Genera tests en [lenguaje/framework de tu elección: Jest, pytest, Go testing, etc.].
Un test por Scenario. Los Scenario Outline deben usar tests parametrizados.
No implementes el código de producción — solo los tests.
Los tests deben fallar inicialmente (red phase).
```

### Paso 2: Implementar el código

```text
Prompt para el agente:

Los tests están escritos y fallan. Implementa el código de producción para que todos pasen.
Empieza por la implementación más simple posible.
No añadas funcionalidad que no esté especificada en los tests.
```

### Paso 3: Verificar

Ejecuta los tests. Para cada escenario del fichero Feature, verifica que:
- Existe un test correspondiente
- El test pasa
- El nombre del test es descriptivo

### Paso 4: Documentar gaps

Anota cualquier comportamiento que descubras durante la implementación que no estaba en el Feature pero que debería estarlo. Añádelo como un Scenario nuevo.

---

## Entregables

Al completar el ejercicio deberías tener:

- [ ] `carrito.feature` con todos los escenarios requeridos
- [ ] `registro.feature` con todos los escenarios requeridos
- [ ] `busqueda.feature` con todos los escenarios requeridos
- [ ] Tests generados para cada Feature
- [ ] Código de implementación que hace pasar todos los tests
- [ ] Lista de gaps o comportamientos descubiertos durante la implementación

---

## Checklist de calidad Gherkin

Revisa cada Feature con esta lista antes de darlo por completo:

- [ ] Cada Scenario tiene exactamente 1 `When` (una única acción del usuario)
- [ ] Los `Given` describen estado, no acciones pasadas
- [ ] Los títulos de los Scenarios son autoexplicativos
- [ ] No hay referencias a elementos técnicos en el Gherkin (no "#email-input", no "tabla users")
- [ ] El Background solo tiene precondiciones comunes (no lógica de negocio específica)
- [ ] Los Scenario Outline tienen al menos 3 filas en la tabla Examples

---

## Tabla de evaluación

| Criterio | Puntuación |
|----------|-----------|
| Carrito: mínimo de escenarios completos y bien formados | /4 |
| Registro: contraseña definida con criterio explícito + Scenario Outline correcto | /4 |
| Búsqueda: paginación cubierta con Scenario Outline | /3 |
| Tests generados correctamente mapeados a escenarios | /4 |
| Implementación pasa todos los tests | /3 |
| Gaps documentados correctamente | /2 |
| **Total** | **/20** |

---

[← Anterior: Elegir estrategia](01-elegir-estrategia.md) | [Siguiente: TDD completo →](03-tdd-completo.md)
