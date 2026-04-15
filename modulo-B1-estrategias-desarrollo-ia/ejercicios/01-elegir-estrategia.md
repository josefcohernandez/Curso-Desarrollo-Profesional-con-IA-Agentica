# Ejercicio 1: Elegir la estrategia correcta

## Contexto

Saber qué estrategia aplicar es una habilidad de juicio que se desarrolla con práctica. En este ejercicio analizas 5 escenarios reales, eliges la metodología más adecuada y describes cómo empezarías.

**Tiempo estimado**: 15 minutos  
**Objetivo**: desarrollar el juicio para seleccionar estrategias de desarrollo con IA

---

## Instrucciones generales

Para cada escenario:
1. Elige la estrategia principal (y secundaria si aplica)
2. Justifica en 2-3 oraciones por qué es la más adecuada
3. Describe los primeros 3 pasos concretos que darías

---

## Escenario 1: API REST para e-commerce desde cero

**Situación**: el equipo de producto ha aprobado construir la API de un e-commerce: catálogo de productos, carrito de compras, órdenes y pagos con Stripe. Tienes los requisitos en una presentación de PowerPoint. Empiezas tú solo. El plazo es 3 semanas.

**Pregunta**: ¿qué estrategia usas y cómo empiezas?

<details>
<summary>Pista (solo si llevas más de 5 minutos en este escenario)</summary>

Considera: ¿cuántos módulos hay? ¿Son interdependientes? ¿El coste de un mal diseño inicial es alto o bajo?

</details>

---

## Escenario 2: Bug fix en sistema de pagos con tests existentes

**Situación**: en producción hay un bug: algunas transacciones con tarjetas Visa de empresas españolas no procesan el 3DS correctamente y dan error 402. El módulo de pagos tiene una cobertura de tests del 87%. El bug es reproducible con ciertos BINs específicos.

**Pregunta**: ¿qué estrategia usas y cómo empiezas?

<details>
<summary>Pista</summary>

El código ya existe. Los tests ya existen. El comportamiento correcto está definido (las transacciones deben completarse). ¿Necesitas especificar o necesitas reproducir y aislar?

</details>

---

## Escenario 3: Migración de 200 ficheros de CommonJS a ESM

**Situación**: la empresa tiene un monorepo de 200 módulos en Node.js escritos con `require()`/`module.exports`. Hay que migrarlos a ES modules (`import`/`export`) para poder usar top-level await y mejorar el tree-shaking. El patrón de migración está bien definido y es mecánico en la mayoría de los casos. Hay unos 15 ficheros con dependencias circulares que necesitan atención especial.

**Pregunta**: ¿qué estrategia usas y cómo organizas el trabajo?

<details>
<summary>Pista</summary>

El patrón de transformación es repetitivo y conocido. El volumen es alto. ¿Qué patrón de workflow está diseñado exactamente para este tipo de tarea?

</details>

---

## Escenario 4: Landing page desde mockup de Figma

**Situación**: el equipo de diseño ha entregado un mockup de Figma de alta fidelidad para la nueva landing page de marketing. Incluye versión desktop, tablet y mobile. Los colores, tipografía y espaciado están en el sistema de diseño de la empresa. No hay lógica de negocio — es una página estática con un formulario de contacto.

**Pregunta**: ¿qué estrategia usas y cómo empiezas?

<details>
<summary>Pista</summary>

El "qué construir" ya está completamente definido — visualmente. ¿Hay alguna estrategia que convierta directamente esta especificación visual en código?

</details>

---

## Escenario 5: Nueva feature en proyecto legacy sin tests

**Situación**: tienes un proyecto de 4 años con 80.000 líneas de código, prácticamente sin tests unitarios. El módulo de notificaciones (emails, push, SMS) necesita una nueva feature: notificaciones en tiempo real via WebSockets. El módulo actual tiene muchas dependencias internas y nadie recuerda exactamente cómo funciona.

**Pregunta**: ¿qué estrategia usas? ¿Por qué este escenario requiere una combinación?

<details>
<summary>Pista</summary>

Hay dos problemas separados: (1) el código existente sin tests que tendrás que tocar, y (2) la feature nueva que hay que diseñar. ¿Qué estrategia aplicas a cada problema?

</details>

---

## Entregables

Para cada escenario, escribe:

```markdown
### Escenario N

**Estrategia elegida**: [nombre]
**Estrategia secundaria** (si aplica): [nombre]

**Justificación**: [2-3 oraciones]

**Primeros 3 pasos**:
1. [paso concreto y accionable]
2. [paso concreto y accionable]
3. [paso concreto y accionable]
```

---

## Tabla de evaluación

| Criterio | Puntuación |
|----------|-----------|
| Estrategia principal correctamente identificada (1 pt por escenario) | /5 |
| Justificación coherente con las características del escenario | /5 |
| Los 3 pasos son concretos y correctamente ordenados | /5 |
| Identificación de la combinación necesaria en el escenario 5 | /2 |
| **Total** | **/17** |

---

## Respuestas de referencia

Revisa tus respuestas una vez completado el ejercicio:

- **Escenario 1**: SDD. Múltiples módulos interdependientes, plazo ajustado — el coste de retrabajar un mal diseño inicial es alto. Los primeros pasos son: (1) convertir el PowerPoint en una lista estructurada de entidades y sus relaciones, (2) escribir la spec de cada módulo en orden de dependencia, (3) validar la spec antes de implementar el primer endpoint.

- **Escenario 2**: TDD Inverso (o simplemente: debugging sistemático con test de regresión). El código existe, los tests existen — el workflow es reproducir el bug con un test específico para esos BINs, aislar la causa, corregir, verificar que el nuevo test pasa y los existentes no regresionan.

- **Escenario 3**: Fan-out. El patrón de migración es mecánico y repetitivo. Organiza los 200 ficheros en batches de 15-20, excluye los 15 con dependencias circulares para tratamiento manual, y lanza un batch de prueba de 5 ficheros para calibrar la calidad antes de escalar.

- **Escenario 4**: Visual-Driven Development. El mockup de Figma es la especificación — adjunta las imágenes al agente con instrucciones de fidelidad visual, sistema de diseño existente y requisitos de responsive.

- **Escenario 5**: Combinación TDD Inverso + SDD (o Gherkin). El TDD Inverso se aplica al código existente que vas a tocar (primero cobertura, luego seguridad para modificar). El SDD o Gherkin se aplica al diseño de la nueva feature de WebSockets, que es suficientemente compleja para justificar especificación antes de implementación.

---

[Siguiente ejercicio: Gherkin desde requisitos →](02-gherkin-desde-requisitos.md)
