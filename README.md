# API Automation — Urban Grocers

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=flat-square&logo=python)](.)
[![pytest](https://img.shields.io/badge/pytest-0A9EDC?style=flat-square&logo=pytest)](.)
[![API Testing](https://img.shields.io/badge/API-Postman%20%7C%20REST-2E5FA3?style=flat-square)](.)
[![Status](https://img.shields.io/badge/Status-Complete-green?style=flat-square)](.)

🇺🇸 [English](#-english) · 🇪🇸 [Español](#-español)

---

## 🇺🇸 English

### The Problem

**Manual API testing found 25 bugs in Sprint 4.** But every time the backend is deployed, someone needs to run those 43 cases again. Manually. For two hours.

That's not sustainable. The kit creation endpoint had clear validation rules (name: 1–511 characters), clear equivalence classes, and clear boundaries. It was a perfect candidate for automation.

The problem wasn't "write tests." The problem was: **how do you make these tests repeatable so they run on every deploy without human effort?**

### Why This Matters

This was the first automation project of the bootcamp — the transition from manual testing to **repeatable, code-driven validation**.

A manual API test takes 2–3 minutes per case. 43 cases = ~2 hours of repetitive clicking. Automation runs the same checks in under 10 seconds. It catches regressions the same day the code is deployed, not a week later when someone finds time to test.

The business value isn't "we have tests." It's **"we have tests that run without us."**

### The Approach — Scope & Cuts

I automated **one endpoint** — `POST /api/v1/kits/` — the kit creation feature. The scope was deliberately narrow:

| Scope | Detail |
|---|---|
| Endpoint | `POST /api/v1/kits/` |
| Field under test | `name` (1–511 chars, non-empty, valid type) |
| Test framework | `pytest` + `requests` |
| Language | Python 3 |
| Test cases | 9 (5 positive, 4 negative) + auth + smoke |
| Test functions | 4 |

**What was cut:**
- Only 1 of 3 endpoints from Sprint 4 — automation starts with the most critical endpoint
- No CI/CD integration — the test suite is ready for it, but the pipeline wasn't in scope
- No PUT/DELETE for kits — POST was the primary creation flow
- No other fields — `name` was chosen because it had clear equivalence classes and boundaries

> 1 endpoint, 1 field, 4 test functions. If it looks small, it is — and that's the point. Automation starts small, proves it works, then expands.

### Tools — Chosen by Need

| Tool | Why, Not Just What |
|---|---|
| **Python 3** | Industry standard for API test automation. The team already knew it. |
| **pytest** | Lightweight, powerful assertions, readable output. No need for a heavy framework. |
| **requests** | The de facto HTTP library for Python. No wrapper, no abstraction — just direct API calls. |
| **Git / GitHub** | Version control for collaboration and traceability. |

Everything was chosen because it was **the simplest tool that solved the problem**. No test management tool, no UI framework, no CI/CD — just Python, pytest, and the HTTP library.

### How It Was Broken Down

4 test functions, each covering a logical group:

1. **`test_get_new_user_token`** — Smoke test. Ensures the auth flow works before testing the endpoint.
2. **`test_create_kit_valid_cases`** — 5 positive scenarios: 1 char, 2–510 chars, special characters, spaces, numeric string. All expected: 201 Created.
3. **`test_create_kit_invalid_cases`** — 4 negative scenarios: empty string, over 511 chars, missing field, wrong type (integer). All expected: 400 Bad Request.
4. **`test_missing_token`** — Auth error. Valid body, no token. Expected: 401 Unauthorized.

Each test function is independent — no shared state, no test ordering dependencies. Run them in any order, run them individually.

### Project Structure — Separation of Concerns

```
qa-project-Urban-Grocers-app/
├── configuration.py            # Base URL and endpoint paths
├── data.py                     # Test data: bodies, headers
├── sender_stand_request.py     # HTTP request functions
├── create_kit_name_kit_test.py # pytest test cases
├── .gitignore
└── README.md
```

| File | Responsibility |
|---|---|
| `configuration.py` | Single source of truth for the server URL. Change environments here, not in tests. |
| `data.py` | Test data separated from test logic. Reusable across scenarios. |
| `sender_stand_request.py` | Wraps `requests` into reusable functions. Tests never call `requests` directly. |
| `create_kit_name_kit_test.py` | Only test logic — arrange, act, assert. |

### The Results

**All 4 test functions pass:**
```
PASSED  create_kit_name_kit_test.py::test_get_new_user_token
PASSED  create_kit_name_kit_test.py::test_create_kit_valid_cases
PASSED  create_kit_name_kit_test.py::test_create_kit_invalid_cases
PASSED  create_kit_name_kit_test.py::test_missing_token
```

| Category | Tests | Expected Status |
|---|---|---|
| ✅ Positive — valid values | 5 | 201 Created |
| ✅ Negative — invalid values | 4 | 400 Bad Request |
| ✅ Auth — missing token | 1 | 401 Unauthorized |
| ✅ Smoke — auth flow | 1 | Token obtained |

The tests are repeatable, run in seconds, and catch regressions the same day a deploy happens.

---

## 🇪🇸 Español

### El Problema

**Las pruebas manuales de API encontraron 25 bugs en el Sprint 4.** Pero cada vez que el backend se despliega, alguien tiene que ejecutar esos 43 casos otra vez. Manualmente. Durante dos horas.

Eso no es sostenible. El endpoint de creación de kits tenía reglas de validación claras (name: 1–511 caracteres), clases de equivalencia claras, y límites claros. Era un candidato perfecto para automatización.

El problema no era "escribir tests." El problema era: **cómo hacer que estas pruebas sean repetibles para que se ejecuten en cada deploy sin esfuerzo humano.**

### Por Qué Importa

Este fue el primer proyecto de automatización del bootcamp — la transición de pruebas manuales a **validación repetible basada en código**.

Una prueba de API manual toma 2–3 minutos por caso. 43 casos = ~2 horas de clics repetitivos. La automatización ejecuta los mismos checks en menos de 10 segundos. Atrapa regresiones el mismo día que se despliega el código, no una semana después cuando alguien encuentra tiempo para probar.

El valor de negocio no es "tenemos tests." Es **"tenemos tests que se ejecutan sin nosotros."**

### El Enfoque — Alcance y Recortes

Automaticé **un endpoint** — `POST /api/v1/kits/` — la creación de kits. El alcance fue deliberadamente acotado:

| Alcance | Detalle |
|---|---|
| Endpoint | `POST /api/v1/kits/` |
| Campo bajo prueba | `name` (1–511 caracteres, no vacío, tipo válido) |
| Framework de pruebas | `pytest` + `requests` |
| Lenguaje | Python 3 |
| Casos de prueba | 9 (5 positivos, 4 negativos) + auth + smoke |
| Funciones de prueba | 4 |

**Lo que se recortó:**
- Solo 1 de 3 endpoints del Sprint 4 — la automatización empieza por el más crítico
- Sin CI/CD — el suite está listo, pero el pipeline no estaba en alcance
- Sin PUT/DELETE — POST era el flujo de creación principal
- Sin otros campos — `name` fue elegido por tener clases de equivalencia y límites claros

> 1 endpoint, 1 campo, 4 funciones de prueba. Si parece poco, lo es — y ese es el punto. La automatización empieza chica, demuestra que funciona, y luego se expande.

### Stack — Elegido por Necesidad

| Herramienta | Por Qué, No Solo Qué |
|---|---|
| **Python 3** | Estándar de la industria para automatización de APIs. El equipo ya lo conocía. |
| **pytest** | Liviano, aserciones potentes, salida legible. Sin necesidad de un framework pesado. |
| **requests** | La biblioteca HTTP de facto para Python. Sin wrapper, sin abstracción — llamadas directas. |
| **Git / GitHub** | Control de versiones para colaboración y trazabilidad. |

Todo fue elegido porque era **la herramienta más simple que resolvía el problema**. Sin herramienta de gestión de pruebas, sin framework UI, sin CI/CD — solo Python, pytest, y la biblioteca HTTP.

### Cómo se Fragmentó

4 funciones de prueba, cada una cubriendo un grupo lógico:

1. **`test_get_new_user_token`** — Smoke. Verifica que el flujo de auth funciona antes de probar el endpoint.
2. **`test_create_kit_valid_cases`** — 5 escenarios positivos: 1 carácter, 2–510, especiales, espacios, numérico. Todos esperados: 201 Created.
3. **`test_create_kit_invalid_cases`** — 4 escenarios negativos: vacío, >511 caracteres, campo ausente, tipo incorrecto. Todos esperados: 400 Bad Request.
4. **`test_missing_token`** — Error de auth. Body válido, sin token. Esperado: 401 Unauthorized.

Cada función es independiente — sin estado compartido, sin dependencias de orden.

### Estructura del Proyecto — Separación de Responsabilidades

```
qa-project-Urban-Grocers-app/
├── configuration.py            # URL base y rutas de endpoints
├── data.py                     # Datos de prueba: bodies, headers
├── sender_stand_request.py     # Funciones de solicitud HTTP
├── create_kit_name_kit_test.py # Casos de prueba pytest
├── .gitignore
└── README.md
```

| Archivo | Responsabilidad |
|---|---|
| `configuration.py` | Fuente única de verdad para la URL del servidor. Cambiás entornos acá, no en los tests. |
| `data.py` | Datos separados de la lógica de prueba. Reutilizables entre escenarios. |
| `sender_stand_request.py` | Wrapper de `requests`. Los tests nunca llaman `requests` directamente. |
| `create_kit_name_kit_test.py` | Solo lógica de prueba — arrange, act, assert. |

### Los Resultados

**Las 4 funciones de prueba pasan:**
```
PASSED  create_kit_name_kit_test.py::test_get_new_user_token
PASSED  create_kit_name_kit_test.py::test_create_kit_valid_cases
PASSED  create_kit_name_kit_test.py::test_create_kit_invalid_cases
PASSED  create_kit_name_kit_test.py::test_missing_token
```

| Categoría | Tests | Estado esperado |
|---|---|---|
| ✅ Positivos — valores válidos | 5 | 201 Created |
| ✅ Negativos — valores inválidos | 4 | 400 Bad Request |
| ✅ Auth — token faltante | 1 | 401 Unauthorized |
| ✅ Smoke — flujo de auth | 1 | Token obtenido |

Los tests son repetibles, se ejecutan en segundos, y atrapan regresiones el mismo día del deploy.
