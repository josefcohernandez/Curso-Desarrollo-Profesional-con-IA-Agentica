# Plantilla: User Story en Gherkin

Copia esta plantilla para cada nueva feature. Rellena cada sección y elimina los comentarios entre corchetes antes de usar el fichero con el agente.

---

## Cabecera del Feature

```gherkin
Feature: [Nombre breve de la funcionalidad — sustantivo + verbo]
  [Descripción del valor de negocio en 1-2 oraciones]
  Como [rol del usuario: "usuario registrado", "administrador", "visitante"]
  Quiero [acción concreta]
  Para [beneficio o resultado esperado]
```

---

## Sección de Background

Úsalo solo si más de 2 escenarios comparten las mismas precondiciones.  
Si solo 1 escenario necesita la precondición, ponla en ese escenario directamente.

```gherkin
  Background:
    Given [precondición 1 — estado del sistema, no acción]
    And [precondición 2]
```

---

## Happy Path (escenario principal)

El flujo que ocurre cuando todo va bien. Es el escenario más importante.

```gherkin
  Scenario: [Título descriptivo del flujo exitoso]
    Given [estado inicial relevante para este escenario]
    When [una única acción del usuario]
    Then [resultado observable principal]
    And [resultado observable adicional, si aplica]
```

---

## Validaciones (escenarios de error)

Un escenario por cada tipo de validación. No combines validaciones en un solo escenario.

```gherkin
  Scenario: [Validación 1 — qué pasa cuando X es inválido]
    Given [estado inicial]
    When [acción con el dato inválido]
    Then [mensaje de error específico]
    But [qué NO debe ocurrir — el estado no debe cambiar]

  Scenario: [Validación 2]
    Given [estado inicial]
    When [acción con otro dato inválido]
    Then [mensaje de error específico]
```

---

## Permisos / Autorización

Escenarios que verifican quién puede hacer qué.

```gherkin
  Scenario: [Rol sin permisos no puede realizar la acción]
    Given el usuario es [rol sin permisos]
    When intenta [la acción restringida]
    Then recibe el error "No tienes permisos para realizar esta acción"
    And [qué no debe ocurrir]
```

---

## Scenario Outline (variantes parametrizadas)

Cuando el mismo flujo aplica a múltiples valores de datos.

```gherkin
  Scenario Outline: [Título que describe el patrón general]
    Given [contexto con <variable>]
    When [acción con <variable>]
    Then [resultado con <resultado_esperado>]

    Examples:
      | variable          | resultado_esperado                    |
      | [valor caso 1]    | [resultado caso 1]                    |
      | [valor caso 2]    | [resultado caso 2]                    |
      | [valor caso 3]    | [resultado caso 3]                    |
      | [valor edge case] | [resultado del edge case]             |
```

---

## Edge Cases (casos límite)

Comportamientos en los extremos: límites máximos, entradas vacías, estados concurrentes.

```gherkin
  Scenario: [Edge case 1 — valor en el límite máximo]
    Given [estado en el límite]
    When [acción en el límite]
    Then [comportamiento esperado en el límite]

  Scenario: [Edge case 2 — recurso vacío]
    Given [estado con recurso vacío o inexistente]
    When [acción sobre recurso vacío]
    Then [respuesta apropiada para estado vacío]
```

---

## Ejemplo completo: sistema de autenticación

```gherkin
Feature: Autenticación de usuarios
  El sistema autentica a usuarios con email y contraseña.
  Como usuario registrado
  Quiero iniciar sesión con mis credenciales
  Para acceder a las funcionalidades de la aplicación

  Background:
    Given el usuario "ana@empresa.com" existe con contraseña "Segura#2024"
    And la cuenta no está bloqueada

  Scenario: Login exitoso
    When el usuario envía email "ana@empresa.com" y contraseña "Segura#2024"
    Then recibe un token de acceso válido
    And el token tiene validez de 3600 segundos
    And se registra el evento de login en el log de auditoría

  Scenario: Contraseña incorrecta
    When el usuario envía email "ana@empresa.com" y contraseña "incorrecta"
    Then recibe el mensaje "Credenciales inválidas"
    And no recibe ningún token
    And el contador de intentos fallidos se incrementa en 1

  Scenario: Cuenta bloqueada tras 5 intentos
    Given el usuario ha fallado el login 4 veces consecutivas
    When el usuario falla el login por quinta vez
    Then recibe el mensaje "Cuenta bloqueada temporalmente. Espera 15 minutos."
    And la cuenta queda bloqueada durante 15 minutos

  Scenario: Login con cuenta bloqueada
    Given la cuenta de "ana@empresa.com" está bloqueada
    When el usuario intenta iniciar sesión con credenciales correctas
    Then recibe el mensaje "Cuenta bloqueada temporalmente. Espera 15 minutos."
    And no recibe ningún token

  Scenario Outline: Formatos de email inválidos
    When el usuario envía el email "<email>" como credencial
    Then recibe el mensaje <mensaje>

    Examples:
      | email              | mensaje                            |
      | sin-arroba.com     | "Formato de email inválido"        |
      | @sindominio.com    | "Formato de email inválido"        |
      |                    | "El email es obligatorio"          |
      | espacios @test.com | "Formato de email inválido"        |

  Scenario: Usuario no registrado
    When el usuario intenta iniciar sesión con "noexiste@empresa.com"
    Then recibe el mensaje "Credenciales inválidas"
    But no se revela si el email existe o no en el sistema
```

---

## Checklist de calidad antes de usar con el agente

- [ ] Cada Scenario tiene exactamente 1 `When`
- [ ] Los `Given` describen estado (no son acciones con el sistema)
- [ ] Los títulos de los Scenarios se entienden sin leer el cuerpo
- [ ] No hay implementación técnica en el Gherkin (no "#campo-email", no SQL)
- [ ] El `Background` es realmente compartido por todos los escenarios
- [ ] Los `Scenario Outline` tienen al menos 3 filas en `Examples`
- [ ] Los mensajes de error en los `Then` son exactos (el agente los usará literalmente)
- [ ] Hay al menos 1 escenario de happy path y 1 de error

---

## Prompt para generar tests desde este Feature

```text
Aquí está el fichero Feature del módulo [nombre]:

[pega el contenido del fichero .feature]

Genera tests en [lenguaje/framework] que implementen cada Scenario.
Estructura: un test por Scenario, con secciones comentadas //Given //When //Then.
Los Scenario Outline deben usar tests parametrizados.
No implementes el código de producción todavía.
Los tests deben fallar al ejecutarse (red phase confirmada).
```
