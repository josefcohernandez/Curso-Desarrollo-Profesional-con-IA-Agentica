# Patrones avanzados de workflow con IA

## Por qué los patrones de workflow importan

Las estrategias de los capítulos anteriores (Gherkin, TDD) son metodologías de especificación — definen qué construir. Los patrones de workflow son metodologías de ejecución — definen cómo organizar las sesiones del agente para maximizar la efectividad.

---

## Patrón Writer/Reviewer

### El concepto

El patrón Writer/Reviewer separa el trabajo en dos sesiones con roles radicalmente distintos:

- **Sesión Writer**: el agente implementa. Su objetivo es producir código funcional.
- **Sesión Reviewer**: el agente critica. Su objetivo es encontrar problemas en el código del Writer.

La clave es que son **sesiones separadas**. Un agente que implementa código tiene sesgo de confirmación: tiende a no encontrar los problemas que él mismo introdujo. Una sesión nueva, con rol de revisor explícito, llega sin ese sesgo.

```text
┌─────────────────┐         ┌─────────────────┐
│  Sesión Writer  │         │ Sesión Reviewer │
│                 │         │                 │
│ "Implementa     │  código │ "Aquí está el   │
│  este servicio" │────────▶│  código. Busca  │
│                 │         │  problemas."    │
│ Produce código  │         │                 │
│ funcional       │         │ Produce informe │
│                 │         │ de problemas    │
└─────────────────┘         └─────────────────┘
                                     │
                                     ▼
                            ┌─────────────────┐
                            │ Sesión Fix      │
                            │                 │
                            │ Aplica las      │
                            │ correcciones    │
                            └─────────────────┘
```

### Por qué el Reviewer es más efectivo que el Writer corrigiéndose

En una sesión única de implementación + revisión:
- El agente tiene el contexto de por qué eligió ese enfoque
- Tiende a defender sus decisiones de diseño
- No "ve" los edge cases que no consideró al implementar

En una sesión separada de revisión:
- El agente llega al código sin saber por qué se tomaron las decisiones
- Evalúa el resultado, no las intenciones
- Aplica su capacidad crítica sin sesgos previos

### 5 variantes del patrón

**Variante 1: Revisión de calidad general**
```text
[Sesión Reviewer]
Eres un revisor de código senior. Aquí está el código del servicio de autenticación:
[código]

Analiza y reporta:
1. Bugs potenciales o casos no manejados
2. Problemas de seguridad
3. Violaciones de principios SOLID
4. Legibilidad y mantenibilidad
5. Tests faltantes o insuficientes

Sé específico y cita líneas de código. No propongas fixes todavía — solo el diagnóstico.
```

**Variante 2: Revisión de seguridad**
```text
[Sesión Reviewer - Security Audit]
Eres un experto en seguridad. Analiza este código buscando:
- Vulnerabilidades OWASP Top 10
- SQL/NoSQL injection
- Autenticación y autorización incorrectas
- Exposición de datos sensibles en logs o respuestas
- Dependencias con vulnerabilidades conocidas

Reporta con severidad: CRÍTICO / ALTO / MEDIO / BAJO.
```

**Variante 3: Revisión de rendimiento**
```text
[Sesión Reviewer - Performance]
Analiza este código buscando:
- N+1 queries
- Operaciones O(n²) o peor donde hay alternativas mejores
- Falta de índices en queries críticos
- Memoria que no se libera
- Operaciones síncronas que deberían ser asíncronas

Para cada problema, estima el impacto en milisegundos o en operaciones de base de datos.
```

**Variante 4: Revisión de tests**
```text
[Sesión Reviewer - Test Quality]
Aquí están los tests del módulo de pagos:
[tests]

Evalúa:
1. ¿Qué comportamientos importantes NO están cubiertos?
2. ¿Hay tests que prueban la implementación en lugar del comportamiento?
3. ¿Hay tests frágiles que se romperán con cualquier refactor?
4. ¿La cobertura de branches es suficiente?
```

**Variante 5: TDD Cruzado**
```text
[Sesión Reviewer - TDD Cross-check]
El Writer implementó este código para pasar los siguientes tests:
[tests]
[código]

Verifica:
1. ¿El código realmente hace lo que los tests comprueban, o los pasa por casualidad?
2. ¿Hay comportamientos en el código que los tests no comprueban?
3. ¿El código tiene lógica adicional que no está especificada en ningún test?
```

---

## Visual-Driven Development

### Cuándo usar imágenes como especificación

Los agentes con visión pueden analizar mockups, wireframes y screenshots y generar código directamente desde ellos. Esto elimina la ambigüedad de describir interfaces en texto.

**Cuándo es más efectivo**:
- Reproducciones fieles de diseños de Figma o Sketch
- Landing pages y páginas de marketing
- Componentes UI con layout complejo
- Refactorización visual (tengo esto, quiero esto)

**Prompt para Visual-Driven Development**:
```text
[adjunta la imagen del mockup]

Implementa este componente en [React/Vue/HTML+CSS].
Prioridades:
1. Layout y estructura exactos al mockup
2. Colores y tipografía del sistema de diseño (usa variables CSS existentes si las hay)
3. Responsive para mobile/tablet/desktop
4. Accesibilidad básica (aria-labels, roles semánticos)

No añadas interactividad que no sea visible en el mockup.
Preguntas antes de empezar: [lista cualquier ambigüedad del mockup]
```

---

## Fan-out: operaciones a escala

### El concepto

El patrón Fan-out distribuye trabajo repetitivo en múltiples invocaciones del agente en lugar de pedirle que haga todo en una sola sesión. Es esencial para tareas de migración o transformación masiva.

```text
Un fichero solo:          ┌────────────────────────────┐
                          │ Migra 200 ficheros de A a B │
                          └──────────────┬─────────────┘
                                         │
                               Overwhelm del agente
                               Errores en ficheros tardíos
                               Pérdida de foco

Fan-out:                  ┌──────────┐ ┌──────────┐ ┌──────────┐
                          │ Batch 1  │ │ Batch 2  │ │ Batch 3  │
                          │ 50 fich. │ │ 50 fich. │ │ 50 fich. │
                          └──────────┘ └──────────┘ └──────────┘
                               │             │            │
                          Calidad uniforme en todos los lotes
```

### Ejemplos de aplicación

**Migración de imports (CommonJS a ESM)**:
```text
Prompt para cada batch:

Migra los siguientes ficheros de CommonJS (require/module.exports) a ESM (import/export).
Ficheros de este batch:
- src/utils/helpers.js
- src/utils/validators.js
- src/utils/formatters.js
[... hasta 20 ficheros]

Reglas:
- require() → import ... from
- module.exports = → export default o named exports
- No cambies la lógica, solo la sintaxis de módulos
- Si un fichero tiene dependencias circulares, reporta sin modificar

Al finalizar el batch, lista qué ficheros modificaste y cuáles reportaron problemas.
```

**Documentación masiva**:
```text
Batch de documentación:

Añade JSDoc a las funciones de estos ficheros.
Solo funciones exportadas. Solo los parámetros y el retorno.
No documentes la implementación interna.

Ficheros: [lista]
```

**Revisión de seguridad en lote**:
```text
Batch de auditoría:

Analiza estos endpoints en busca de problemas de seguridad:
[lista de controladores]

Para cada endpoint reporta: endpoint, problema, severidad, línea.
Formato de salida: tabla Markdown.
No corrijas — solo reporta.
```

### Control de costos en Fan-out

Antes de lanzar un fan-out grande, estima:
- Número de batches necesarios
- Tamaño típico de cada batch (tokens de entrada + salida)
- Coste por batch en tu herramienta

Para migraciones de 200+ ficheros, considera lanzar un batch de prueba con 5 ficheros, revisar la calidad y luego escalar.

---

## Git Worktrees: sesiones paralelas aisladas

### El concepto

Git Worktrees permite tener múltiples copies del repositorio checkout en paralelo, cada una en una rama diferente. Combinado con agentes de código, permite ejecutar trabajos en paralelo de forma aislada.

### Cuándo usar Worktrees con agentes

- Implementar dos features simultáneamente sin interferencia
- Mantener una rama de "investigación" mientras desarrollas en la principal
- Ejecutar el Writer en una rama y el Reviewer en otra simultáneamente

### Flujo básico

```bash
# Crear worktrees para trabajo paralelo
git worktree add ../proyecto-feature-a feature/feature-a
git worktree add ../proyecto-feature-b feature/feature-b

# Lanzar agentes en cada worktree (en terminales separadas)
# Terminal 1: cd ../proyecto-feature-a && [tu agente]
# Terminal 2: cd ../proyecto-feature-b && [tu agente]

# Ver worktrees activos
git worktree list

# Limpiar cuando termines
git worktree remove ../proyecto-feature-a
git worktree remove ../proyecto-feature-b
```

> En Claude Code: lanza instancias separadas de Claude Code en cada directorio de worktree.
> En Cursor: abre ventanas separadas del editor, cada una apuntando al directorio del worktree.
> En Copilot: misma lógica — ventanas separadas del editor.

---

## Tabla de selección de metodología por tipo de tarea

| Tipo de tarea | Metodología recomendada | Patrón de workflow |
|---------------|------------------------|-------------------|
| Nuevo módulo con requisitos claros | TDD o Gherkin + TDD | Writer secuencial + Reviewer |
| Nueva feature en producto existente | Gherkin + TDD | Writer + Reviewer de seguridad |
| Migración masiva (>20 ficheros) | Fan-out por batches | Control de calidad por sampling |
| Bug fix en código con tests | TDD Inverso | Sesión única con foco |
| Componente UI desde mockup | Visual-Driven | Iteración rápida |
| Revisión de seguridad | — | Reviewer variante 2 |
| Refactor sin cambio de comportamiento | TDD Inverso → Refactor | Writer + Reviewer de tests |
| Trabajo en paralelo (2 features) | Cualquier estrategia | Git Worktrees |
| Documentación masiva | — | Fan-out por batches |

---

[← Anterior: TDD con IA](04-tdd-con-ia.md) | [Siguiente: Otras estrategias →](06-otras-estrategias.md)
