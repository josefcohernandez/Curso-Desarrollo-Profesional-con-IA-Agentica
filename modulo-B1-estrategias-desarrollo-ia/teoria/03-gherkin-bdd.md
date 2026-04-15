# Gherkin y BDD con agentes de código

## Qué es BDD y por qué funciona tan bien con IA

BDD (Behavior-Driven Development) es una metodología que extiende TDD desplazando el foco desde la implementación técnica hacia el comportamiento observable del sistema desde la perspectiva del usuario.

La herramienta central de BDD es **Gherkin**: un lenguaje estructurado de especificación que describe comportamientos en lenguaje natural pero con una sintaxis precisa. Los agentes de código lo procesan excepcionalmente bien por dos razones:

1. **Precisión sin ambigüedad técnica**: Gherkin elimina la vaguedad del lenguaje natural sin requerir que quien escribe la especificación sepa cómo se va a implementar.
2. **Generación directa de tests**: un agente puede transformar escenarios Gherkin en tests ejecutables de forma casi mecánica — la estructura Given/When/Then mapea directamente a la estructura Arrange/Act/Assert de los tests.

---

## Sintaxis completa de Gherkin

### Keywords principales

| Keyword | Propósito | Ejemplo |
|---------|-----------|---------|
| `Feature` | Describe la funcionalidad completa | `Feature: Gestión de tareas` |
| `Background` | Pasos comunes a todos los escenarios | `Background: Given el usuario está autenticado` |
| `Scenario` | Un caso de uso específico | `Scenario: Crear una tarea` |
| `Scenario Outline` | Escenario parametrizado (múltiples variantes) | `Scenario Outline: Validar contraseña` |
| `Given` | Estado inicial / precondición | `Given el carrito contiene 2 productos` |
| `When` | Acción que realiza el usuario | `When el usuario hace click en "Comprar"` |
| `Then` | Resultado esperado | `Then se muestra el resumen del pedido` |
| `And` | Continúa el paso anterior | `And el stock se reduce en 2 unidades` |
| `But` | Excepción o contrapunto | `But el descuento no se aplica` |
| `Examples` | Tabla de datos para Scenario Outline | Ver ejemplo completo abajo |

### Estructura de un fichero Feature

```gherkin
Feature: [Nombre de la funcionalidad]
  [Descripción breve del valor de negocio]
  Como [rol del usuario]
  Quiero [acción]
  Para [beneficio]

  Background:
    Given [precondición compartida por todos los escenarios]

  Scenario: [Nombre descriptivo del happy path]
    Given [estado inicial específico]
    When [acción del usuario]
    Then [resultado observable]
    And [resultado adicional]

  Scenario: [Caso de validación o error]
    Given [estado inicial]
    When [acción inválida]
    Then [mensaje de error esperado]
    But [qué NO debe ocurrir]

  Scenario Outline: [Escenario con múltiples variantes]
    Given [el campo <campo> tiene el valor <valor>]
    When [el usuario envía el formulario]
    Then [se muestra <resultado>]

    Examples:
      | campo    | valor              | resultado               |
      | email    | sin-arroba.com     | "Formato de email inválido" |
      | email    | vacio              | "El email es obligatorio"   |
      | password | 123                | "La contraseña es muy corta" |
```

---

## Ejemplo completo: sistema de gestión de tareas

```gherkin
Feature: Gestión de tareas
  Los usuarios pueden crear, completar y eliminar tareas
  organizadas por proyecto.

  Background:
    Given el usuario "ana@ejemplo.com" está autenticado
    And el proyecto "Proyecto Alpha" existe
    And el proyecto contiene las tareas:
      | titulo              | estado     |
      | Preparar propuesta  | pendiente  |
      | Revisar presupuesto | completada |

  Scenario: Crear una nueva tarea
    When el usuario crea la tarea "Enviar informe" en "Proyecto Alpha"
    Then la tarea "Enviar informe" aparece en la lista con estado "pendiente"
    And el total de tareas pendientes del proyecto es 2

  Scenario: Completar una tarea existente
    When el usuario marca "Preparar propuesta" como completada
    Then la tarea "Preparar propuesta" tiene estado "completada"
    And el total de tareas completadas del proyecto es 2

  Scenario: No se puede crear una tarea sin título
    When el usuario intenta crear una tarea con título vacío
    Then se muestra el mensaje "El título de la tarea es obligatorio"
    And no se crea ninguna tarea nueva

  Scenario: Eliminar una tarea completada
    When el usuario elimina la tarea "Revisar presupuesto"
    Then "Revisar presupuesto" ya no aparece en la lista
    And el total de tareas del proyecto es 1

  Scenario Outline: Validar longitud del título de la tarea
    When el usuario intenta crear una tarea con título de <longitud> caracteres
    Then se muestra el mensaje <mensaje>

    Examples:
      | longitud | mensaje                                          |
      | 0        | "El título de la tarea es obligatorio"           |
      | 1        | "El título debe tener al menos 3 caracteres"     |
      | 201      | "El título no puede superar 200 caracteres"      |
      | 50       | ""                                               |
```

---

## Gherkin para diferentes tipos de proyecto

### API REST

```gherkin
Feature: API de autenticación

  Scenario: Login exitoso
    Given el usuario "dev@empresa.com" existe con contraseña "Segura#2024"
    When se envía POST /auth/login con body:
      """json
      {
        "email": "dev@empresa.com",
        "password": "Segura#2024"
      }
      """
    Then la respuesta tiene status 200
    And la respuesta contiene "access_token"
    And el token tiene validez de 3600 segundos

  Scenario: Login con contraseña incorrecta
    When se envía POST /auth/login con email "dev@empresa.com" y password "incorrecta"
    Then la respuesta tiene status 401
    And la respuesta contiene "message": "Credenciales inválidas"
```

### Frontend / UI

```gherkin
Feature: Formulario de registro

  Scenario: Registro exitoso
    Given el usuario está en la página /register
    When rellena "nombre" con "Ana García"
    And rellena "email" con "ana@ejemplo.com"
    And rellena "password" con "Segura#2024"
    And hace click en "Crear cuenta"
    Then se muestra el mensaje "Cuenta creada correctamente"
    And se redirige a /dashboard

  Scenario: Email ya registrado
    Given "ana@ejemplo.com" ya existe en el sistema
    When el usuario intenta registrarse con ese email
    Then se muestra "Este email ya está registrado"
    And el formulario permanece visible con los datos actuales
```

### CLI

```gherkin
Feature: Herramienta de línea de comandos

  Scenario: Listar proyectos
    Given existen 3 proyectos en la base de datos
    When el usuario ejecuta "mytool projects list"
    Then la salida muestra 3 líneas con nombre y estado
    And el código de salida es 0

  Scenario: Proyecto no encontrado
    When el usuario ejecuta "mytool projects show --id=999"
    Then la salida muestra "Error: proyecto 999 no encontrado"
    And el código de salida es 1
```

---

## Gherkin como criterio de aceptación objetivo

Una de las propiedades más valiosas de Gherkin es que transforma la pregunta "¿está bien implementado?" en una pregunta binaria:

> ¿Pasan todos los escenarios? Sí / No.

Esto elimina la subjetividad del proceso de verificación. Cuando usas Gherkin correctamente:

- Los criterios de aceptación están escritos antes de que empiece la implementación
- El agente implementa hasta que todos los escenarios pasan
- Tú verificas los resultados contra los escenarios, no contra una impresión subjetiva
- El equipo comparte un lenguaje común sobre qué significa "terminado"

---

## 7 buenas prácticas

**1. Un escenario = un comportamiento**. Si tu escenario tiene 3 `When`, probablemente está probando 3 cosas. Divídelo.

**2. Los escenarios son independientes**. Cada escenario debe poder ejecutarse de forma aislada. No crees dependencias entre escenarios.

**3. Escribe en presente, desde la perspectiva del usuario**. `When el usuario hace click` — no `When el sistema procesa la solicitud`.

**4. Los `Given` describen estado, no acciones**. `Given el carrito tiene 2 productos` — no `Given el usuario ha añadido 2 productos` (eso es una acción).

**5. Evita detalles de implementación**. No escribas `Given el campo #email-input tiene el valor "ana@..."` — escribe `Given el email del formulario es "ana@..."`. El cómo es responsabilidad del agente.

**6. Usa `Background` para precondiciones comunes**, no para lógica de negocio. El `Background` es contexto, no parte del escenario.

**7. Los títulos de escenario deben ser autoexplicativos**. Si necesitas leer el cuerpo para entender qué prueba el escenario, el título es insuficiente.

---

## Flujo completo con agente de código

```text
┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│   Features   │    │    Tests     │    │Implementación│    │ Verificación │
│  en Gherkin  │───▶│  generados   │───▶│  del agente  │───▶│  escenarios  │
│              │    │  por agente  │    │              │    │              │
│ Tú defines   │    │ Automático   │    │ Agente impl. │    │ Tú verificas │
│ el QUÉ       │    │              │    │ el CÓMO      │    │ binariamente │
└──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
```

**Prompt para generar tests desde Gherkin:**

```text
Estos son los escenarios Gherkin para la feature de [nombre]:

[pegar el fichero .feature]

Genera tests de integración en [lenguaje/framework] que implementen cada escenario.
Usa la estructura Given/When/Then como secciones Arrange/Act/Assert.
Cada Scenario debe ser un test independiente.
Los Scenario Outline deben generar tests parametrizados.
No implementes el código de producción todavía — solo los tests.
```

---

## Tabla resumen

| Elemento | Propósito | Cuándo usar |
|----------|-----------|-------------|
| `Feature` | Contexto y valor de negocio | Siempre — es la unidad mínima |
| `Background` | Precondiciones compartidas | Cuando todos los escenarios comparten más de 2 `Given` |
| `Scenario` | Caso de uso específico | Un comportamiento = un escenario |
| `Scenario Outline` | Variantes del mismo comportamiento | Cuando el flujo es igual pero los datos cambian |
| `Examples` | Datos para Scenario Outline | Siempre que uses Scenario Outline |
| User story | Contexto de negocio | Al inicio del Feature para dar contexto |

---

[← Anterior: Panorama de estrategias](02-panorama-estrategias.md) | [Siguiente: TDD con IA →](04-tdd-con-ia.md)
