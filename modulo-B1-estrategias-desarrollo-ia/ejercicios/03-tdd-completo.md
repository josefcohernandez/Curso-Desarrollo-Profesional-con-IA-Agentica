# Ejercicio 3: Ciclo TDD completo

## Contexto

En este ejercicio practicas el ciclo completo Red-Green-Refactor para construir un módulo de autenticación desde cero. El agente implementa, tú diriges y verificas cada fase.

El módulo cubre 4 requisitos en orden de dependencia: primero los más básicos, luego los que dependen de ellos. Sigue el orden — cada fase usa lo construido en la anterior.

**Tiempo estimado**: 20 minutos  
**Lenguaje sugerido**: Python (con pytest) o TypeScript (con Jest). Adapta los prompts a tu lenguaje preferido.  
**Objetivo**: ejecutar el ciclo Red-Green-Refactor de principio a fin con un agente de código

---

## Reglas del ejercicio

- **No escribas código de producción antes de tener tests**. Esto es la regla fundamental de TDD.
- **Espera la aprobación entre fases**. En el prompt de Red, pide los tests y verifica que fallan antes de pedir el Green.
- **Mínimo viable en Green**. El código que hace pasar los tests debe ser el más simple posible. No te adelantes.
- **No combines fases**. Si el agente intenta implementar el código de producción en la fase Red, deténlo.

---

## Requisito 1: Hash de contraseñas

**Especificación**:
- La función `hash_password(password: str) -> str` recibe una contraseña en texto plano y devuelve su hash
- Contraseñas iguales producen hashes diferentes (salt único por hash)
- La función `verify_password(password: str, hash: str) -> bool` comprueba si una contraseña corresponde a un hash
- `verify_password` con la contraseña correcta devuelve `True`
- `verify_password` con contraseña incorrecta devuelve `False`
- No se puede recuperar la contraseña original a partir del hash

### Fase Red — Escribe los tests

```text
Prompt Red - Requisito 1:

Vamos a implementar hash_password() y verify_password() en Python usando bcrypt.
Escribe los tests primero. No implementes las funciones todavía.

Comportamiento esperado:
- hash_password(password) -> str: recibe texto plano, devuelve hash (siempre diferente para la misma entrada)
- verify_password(password, hash) -> bool: devuelve True si coincide, False si no

Tests requeridos:
1. hash_password devuelve un string no vacío
2. Dos llamadas a hash_password con el mismo input devuelven strings diferentes (salt aleatorio)
3. verify_password(password, hash_de_ese_password) devuelve True
4. verify_password(password_distinto, hash) devuelve False
5. verify_password con hash malformado no lanza excepción — devuelve False

Usa pytest. Nombres de tests descriptivos.
```

Verifica que los tests fallan (`pytest` debería reportar `ImportError` o `NameError`). Esto confirma que estás en Red.

### Fase Green — Implementa

```text
Prompt Green - Requisito 1:

Los tests están escritos y fallan (red). Implementa hash_password() y verify_password()
para que todos los tests pasen.
Usa bcrypt.
Código mínimo que hace pasar los tests. Nada más.
```

Ejecuta los tests. Todos deben estar en verde. Si alguno falla, analiza el error antes de continuar.

### Fase Refactor — Mejora el diseño

```text
Prompt Refactor - Requisito 1:

Todos los tests pasan. ¿Hay alguna mejora de diseño que valga la pena aplicar?
Considera: tipado, manejo de errores de inputs vacíos o None, constantes configurables (rounds).
Cualquier cambio debe mantener todos los tests en verde.
Espera mi aprobación antes de aplicar cualquier cambio.
```

---

## Requisito 2: Validación de email

**Especificación**:
- La función `validate_email(email: str) -> bool` valida el formato del email
- Devuelve `True` para emails válidos (usuario@dominio.com, usuario@sub.dominio.co.uk)
- Devuelve `False` para: sin arroba, sin dominio, sin TLD, strings vacíos, `None`
- No hace llamadas externas — solo validación de formato local

### Fase Red — Escribe los tests

```text
Prompt Red - Requisito 2:

Implementaremos validate_email(email) -> bool.
Escribe los tests primero. No implementes la función.

Casos requeridos:
- Válidos: email simple, email con subdominio, email con TLD largo (.museum)
- Inválidos: sin arroba, sin dominio después del arroba, sin TLD, string vacío, None, solo espacios
- Edge cases: espacios al inicio o al final (deben ser inválidos)

Usa pytest.
```

Repite el ciclo Red → Green → Refactor igual que en el Requisito 1.

---

## Requisito 3: Fortaleza de contraseña

**Especificación**:
- La función `check_password_strength(password: str) -> dict` evalúa la fortaleza
- El resultado es un diccionario con `{"score": int, "valid": bool, "issues": list[str]}`
- `score` va de 0 a 4 (un punto por cada criterio cumplido)
- `valid` es `True` solo si `score >= 3`
- Criterios: longitud >= 8, contiene mayúscula, contiene número, contiene carácter especial
- `issues` lista los criterios que no se cumplen

### Fase Red — Escribe los tests

```text
Prompt Red - Requisito 3:

Implementaremos check_password_strength(password) -> dict
con estructura {"score": int, "valid": bool, "issues": list[str]}.

Tests requeridos:
1. Contraseña con todos los criterios: score=4, valid=True, issues=[]
2. Contraseña sin mayúscula: score=3, valid=True, issues=["Debe contener al menos una mayúscula"]
3. Contraseña solo letras minúsculas: score=1 (tiene longitud si es larga), valid=False
4. String vacío: score=0, valid=False, issues contiene todos los criterios
5. Contraseña "Password1!": score=4, valid=True (tiene todo)
6. Contraseña "abc": score=0 o 1, valid=False (falta longitud y demás)

Usa pytest. Un test por caso de uso.
```

Repite el ciclo Red → Green → Refactor.

---

## Requisito 4: Generación y validación de JWT

**Especificación**:
- `generate_token(user_id: str, expires_in_seconds: int = 3600) -> str`: genera un JWT
- `validate_token(token: str) -> dict | None`: valida el token y devuelve el payload, o `None` si no es válido
- Un token expirado devuelve `None`
- Un token con firma modificada devuelve `None`
- El payload contiene al menos `user_id` y `exp`

### Fase Red — Escribe los tests

```text
Prompt Red - Requisito 4:

Implementaremos generate_token() y validate_token() usando PyJWT.

Tests requeridos:
1. generate_token devuelve un string no vacío
2. validate_token con token válido devuelve dict con user_id correcto
3. validate_token con token expirado (expires_in_seconds=0) devuelve None
4. validate_token con token manipulado (string modificado) devuelve None
5. validate_token con string vacío no lanza excepción — devuelve None
6. El payload tiene el campo exp con timestamp correcto

Nota: para probar expiración, usa expires_in_seconds=-1 o mock del tiempo.
Usa pytest.
```

Repite el ciclo Red → Green → Refactor.

---

## Verificación de cobertura final

Después de completar los 4 requisitos, verifica la cobertura:

```bash
# Python
pytest --cov=auth --cov-report=term-missing

# TypeScript/Jest
npx jest --coverage
```

```text
Prompt de cobertura:

Aquí está el informe de cobertura actual:
[pega el output]

Identifica:
1. Las líneas no cubiertas más importantes (lógica de negocio, no boilerplate)
2. Los tests adicionales que cerrarían los gaps más significativos
3. Si hay branches condicionales sin cubrir

Genera los tests faltantes para llegar al 90% de branch coverage.
```

---

## Reto extra: `refresh_token` via TDD

Si completaste los 4 requisitos con tiempo de sobra, implementa `refresh_token(old_token: str) -> str | None`:
- Recibe un token válido (no expirado)
- Devuelve un nuevo token con la misma `user_id` y nueva expiración
- Si el token es inválido o expirado, devuelve `None`
- Escribe los tests antes de implementar

---

## Tabla de evaluación

| Criterio | Puntuación |
|----------|-----------|
| Fase Red completada antes de Green en cada requisito | /4 |
| Tests cubren todos los casos especificados | /4 |
| Fase Green: código mínimo que hace pasar los tests | /4 |
| Fase Refactor aplicada con aprobación | /2 |
| Cobertura final >= 90% de branches | /3 |
| Reto extra: refresh_token implementado via TDD | /3 |
| **Total** | **/20** |

---

[← Anterior: Gherkin desde requisitos](02-gherkin-desde-requisitos.md) | [Siguiente: Writer/Reviewer →](04-writer-reviewer.md)
