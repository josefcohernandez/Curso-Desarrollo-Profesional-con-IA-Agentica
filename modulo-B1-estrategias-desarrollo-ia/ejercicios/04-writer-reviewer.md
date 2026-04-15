# Ejercicio 4: El patrón Writer/Reviewer

## Contexto

El patrón Writer/Reviewer es uno de los más valiosos para código de producción: separar la sesión de implementación de la sesión de revisión elimina el sesgo de confirmación del agente. Este ejercicio te lleva por el ciclo completo para que experimentes de primera mano qué encuentra el Reviewer que el Writer no vio.

**Tiempo estimado**: 25 minutos  
**Objetivo**: ejecutar el ciclo Writer → Reviewer → Fix y comparar el resultado con la implementación inicial

---

## La tarea: servicio de rate limiting

Implementarás un servicio de rate limiting (limitación de tasa de peticiones) para proteger endpoints de API. El requisito es claro pero tiene edge cases que un Writer sin cuidado pasará por alto.

### Especificación

```text
RateLimiter(max_requests: int, window_seconds: int)
  - Permite max_requests peticiones en una ventana de window_seconds segundos
  - is_allowed(client_id: str) -> bool
    - Devuelve True si la petición está permitida
    - Devuelve False si el cliente ha superado el límite en la ventana actual
  - get_remaining(client_id: str) -> int
    - Devuelve el número de peticiones restantes para ese cliente
  - reset(client_id: str) -> None
    - Reinicia el contador del cliente

Comportamiento de la ventana:
  - Ventana deslizante: no se reinicia cada N segundos de forma fija, sino que
    las peticiones "caducan" pasados window_seconds segundos desde que se hicieron
```

---

## Sesión 1: Writer

Abre una sesión nueva con tu agente de código y usa este prompt:

```text
Prompt Writer:

Implementa un servicio de rate limiting en [lenguaje de tu elección].

Especificación:
- Clase RateLimiter(max_requests: int, window_seconds: int)
- is_allowed(client_id: str) -> bool
  - True si el cliente no ha superado max_requests en los últimos window_seconds segundos
  - False si ha superado el límite
- get_remaining(client_id: str) -> int
  - Peticiones restantes para ese cliente en la ventana actual
- reset(client_id: str) -> None
  - Reinicia el contador del cliente

Implementación con ventana deslizante: las peticiones caducan pasados window_seconds
desde que se hicieron — no es una ventana fija que se reinicia cada N segundos.

Incluye tests básicos: happy path (dentro del límite), al alcanzar el límite,
después de reset, y verificación de que get_remaining es correcto.
```

Guarda la implementación que produce el Writer. No la modifiques.

---

## Sesión 2: Reviewer

Abre una sesión **completamente nueva** con tu agente. No uses la sesión del Writer. Copia el código producido en el Writer y usa este prompt:

```text
Prompt Reviewer:

Eres un revisor de código senior. Aquí está la implementación de un servicio de rate limiting:

[pega el código del Writer aquí]

Analiza el código buscando:

1. **Edge cases no manejados**: inputs inesperados, condiciones de borde
2. **Race conditions**: ¿es seguro para uso concurrente? ¿hay condiciones de carrera?
3. **Memory leaks**: ¿los datos de clientes inactivos se limpian alguna vez?
4. **Comportamiento incorrecto de la ventana deslizante**: ¿realmente implementa
   una ventana deslizante o una ventana fija?
5. **Tests faltantes**: ¿qué casos importantes no están cubiertos por los tests?
6. **Problemas de rendimiento**: ¿escala correctamente con miles de clientes?

Para cada problema encontrado:
- Describe el problema específicamente (cita el código si aplica)
- Explica el impacto real (qué pasa en producción)
- Clasifica la severidad: CRÍTICO / ALTO / MEDIO / BAJO

No propongas fixes todavía — solo el diagnóstico.
```

---

## Sesión 3: Fix

En la misma sesión del Reviewer (o una nueva), aplica las correcciones:

```text
Prompt Fix:

Aquí están los problemas identificados en el review:
[pega el informe del Reviewer]

Prioriza y corrige los problemas CRÍTICO y ALTO primero.
Para cada corrección:
1. Explica el cambio
2. Añade el test que demuestre que el problema está resuelto

Muéstrame los cambios antes de aplicarlos.
```

---

## Comparación final

Una vez completado el ciclo, responde estas preguntas por escrito:

### 1. ¿Qué encontró el Reviewer que el Writer no vio?

Lista los problemas encontrados por el Reviewer. Para cada uno, indica si el Writer podría haberlo visto si hubiera prestado más atención, o si era un punto ciego estructural.

### 2. ¿La ventana deslizante estaba correctamente implementada?

El requisito específica ventana deslizante, no fija. ¿El Writer implementó la diferencia correctamente? Describe qué es exactamente la diferencia y cómo afecta al comportamiento.

### 3. Comportamiento concurrente

¿El código del Writer es thread-safe? ¿Qué race condition puede ocurrir si dos peticiones del mismo `client_id` llegan al mismo tiempo? ¿Cómo lo resolvió el Reviewer?

### 4. Memory leak

¿Los datos de un cliente que no hace peticiones se mantienen en memoria indefinidamente? ¿Cuál es el impacto en producción si hay 10.000 clientes distintos que hicieron peticiones hace una semana?

### 5. Reflexión sobre el patrón

¿Vale la pena la sesión extra del Reviewer? Calcula en minutos: el tiempo del Writer + el tiempo del Reviewer frente al impacto de los bugs encontrados si hubieran llegado a producción.

---

## Entregables

- [ ] Código del Writer (sin modificar después de la sesión)
- [ ] Informe del Reviewer con problemas categorizados por severidad
- [ ] Código final corregido con tests adicionales
- [ ] Respuestas a las 5 preguntas de la sección de comparación

---

## Tabla de evaluación

| Criterio | Puntuación |
|----------|-----------|
| Sesiones Writer y Reviewer fueron realmente separadas | /2 |
| Reviewer encontró al menos 3 problemas distintos | /3 |
| Correcciones aplicadas para todos los problemas CRÍTICO y ALTO | /4 |
| Tests añadidos que demuestran cada corrección | /4 |
| Análisis de la ventana deslizante correcto | /3 |
| Reflexión sobre el valor del patrón | /2 |
| Análisis de concurrencia | /2 |
| **Total** | **/20** |

---

> **Nota**: El patrón Writer/Reviewer no requiere dos herramientas distintas. Requiere dos sesiones distintas — el contexto limpio es lo que hace la diferencia. En Claude Code, esto significa nueva sesión (nuevo chat). En Cursor, abre una ventana nueva. En Copilot Chat, inicia un nuevo thread. Lo importante es que el Reviewer llega al código sin saber por qué se tomaron las decisiones.

---

[← Anterior: TDD completo](03-tdd-completo.md) | [Volver al índice del módulo →](../README.md)
