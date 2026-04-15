# Por qué la metodología importa con IA

## La diferencia entre "usar IA" y "desarrollar con IA"

Hay dos formas de trabajar con un agente de código:

1. **Usar IA**: abrir una sesión, describir lo que quieres, esperar el resultado, corregirlo, repetir. El proceso es reactivo. Cada sesión empieza desde cero. No hay estructura que guíe al agente.

2. **Desarrollar con IA**: diseñar el trabajo antes de ejecutarlo, proporcionar al agente el contexto y los criterios correctos, verificar el resultado contra especificaciones objetivas, iterar dentro de un ciclo definido.

La diferencia no está en la herramienta. Está en el proceso. El desarrollador que "usa IA" trabaja con la misma herramienta que el desarrollador que "desarrolla con IA" — pero obtiene resultados radicalmente distintos.

---

## Improvisación vs proceso: el coste oculto

La improvisación con IA parece más rápida a corto plazo. Es tentador: abres una sesión, describes el problema, recibes código, copias y pegas. Sin planificar, sin especificar, sin metodología.

El problema no es inmediato. El problema se acumula:

| Dimensión | Sin metodología | Con metodología |
|-----------|-----------------|-----------------|
| **Primera implementación** | Rápida (30 min) | Más lenta (45 min) |
| **Primera revisión** | Difusa — no sabes si está bien | Clara — tienes criterios |
| **Primer bug** | Dónde buscar, no está claro | El agente tiene el contexto del diseño |
| **Segunda feature** | Empieza de cero | Reutiliza especificaciones |
| **Semana 3** | Código que nadie entiende | Sistema coherente y navegable |
| **Proyecto terminado** | Quizás funciona | Funciona y tienes evidencia |

El coste de no tener metodología no aparece en la sesión 1. Aparece en la sesión 15, cuando el agente no tiene contexto de por qué el sistema fue diseñado así, cuando corriges el mismo tipo de bug por tercera vez, cuando la velocidad de desarrollo cae porque el código es demasiado difícil de modificar.

---

## El "loop de la desesperación"

Existe un patrón de trabajo muy común que merece un nombre propio:

```text
┌─────────────────────────────────────────────────────┐
│                                                     │
│   Prompt vago   →   Resultado malo   →   Frustración│
│       ▲                                      │      │
│       │                                      │      │
│       └──── Otro prompt vago ◄───────────────┘      │
│             (más detallado, pero sin estructura)     │
│                                                     │
└─────────────────────────────────────────────────────┘
```

El "loop de la desesperación" ocurre cuando:
1. El prompt inicial es vago o incompleto
2. El resultado no es lo que querías
3. Añades más instrucciones al mismo prompt
4. El agente acumula contexto contradictorio
5. El resultado empeora o cambia de forma impredecible
6. Repites el ciclo con frustración creciente

**La salida del loop no es un prompt mejor.** Es un proceso mejor.

Un proceso efectivo define qué se va a construir antes de pedirle al agente que lo construya. Eso elimina la ambigüedad que alimenta el loop.

---

## La paradoja de velocidad

La promesa de la IA es velocidad. Y es real: las tareas que antes tomaban horas ahora toman minutos. Pero hay una trampa:

> Ir más rápido no significa llegar antes.

Si vas en la dirección equivocada, la velocidad solo te lleva más lejos de donde quieres estar.

Un agente que implementa sin especificación produce código rápidamente — pero no necesariamente el código correcto. Sin criterios claros de "correcto", el código tiene que ser evaluado subjetivamente, corregido iterativamente y posiblemente descartado.

La paradoja es que **invertir tiempo en metodología antes de codificar reduce el tiempo total** del proyecto:

```text
                     SIN metodología        CON metodología
Especificación:           0 min                 15 min
Primera implementación:  30 min                 20 min
Correcciones:            60 min                 10 min
Tests:                   45 min                 15 min
Integración:             30 min                 10 min
────────────────────────────────────────────────────────
TOTAL:                  165 min                 70 min
```

La diferencia no está en la velocidad de escritura de código. Está en la calidad de las instrucciones y en tener criterios objetivos para evaluar el resultado.

---

## Cómo las metodologías multiplican la efectividad

Las metodologías de desarrollo actúan como multiplicadores de la efectividad del agente de tres formas:

### 1. Convierten la ambigüedad en especificación

El mayor problema del agente no es su capacidad técnica — es la ambigüedad de las instrucciones. Una metodología convierte requisitos vagos ("necesito un sistema de login") en especificaciones precisas que el agente puede implementar de forma determinista.

### 2. Proporcionan criterios objetivos de éxito

Sin metodología: el código "parece bien" o "no parece bien" — un juicio subjetivo. Con metodología (especialmente TDD o Gherkin/BDD): el código pasa o no pasa los criterios definidos. La evaluación es objetiva y automatizable.

### 3. Estructuran la conversación con el agente

Las metodologías definen qué tipo de output pedir en cada fase. No pides "implementa esto" — pides "escribe tests que fallen para estos criterios, luego implementa hasta que pasen". La metodología guía el diálogo.

---

## Resumen: el cambio de modelo mental

El cambio más importante no es técnico. Es conceptual:

| Modelo reactivo | Modelo metodológico |
|----------------|---------------------|
| "Pido código, corrijo lo que está mal" | "Defino qué es correcto, luego pido código" |
| El agente decide cómo | Tú defines qué, el agente decide cómo |
| Evalúo subjetivamente | Evalúo contra criterios |
| Cada sesión empieza de cero | Cada sesión tiene contexto acumulado |
| El proceso depende del agente | El proceso depende de mí |

El siguiente capítulo presenta el panorama completo de estrategias disponibles para aplicar este modelo metodológico en diferentes tipos de proyectos y tareas.

---

[Siguiente: Panorama de estrategias →](02-panorama-estrategias.md)
