# 🧪 Urban Grocers — API Test Automation

**TripleTen QA Engineering Bootcamp · Cohort 23**
👤 Luis Manco · Medellín, Colombia

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=flat-square&logo=python)](.)
[![pytest](https://img.shields.io/badge/pytest-0A9EDC?style=flat-square&logo=pytest)](.)
[![API Testing](https://img.shields.io/badge/API-Postman%20%7C%20REST-2E5FA3?style=flat-square)](.)
[![Status](https://img.shields.io/badge/Status-Complete-green?style=flat-square)](.)

🇺🇸 [English](#-english) · 🇪🇸 [Español](#-español)

---

## 🇺🇸 English

### Project Description

This project contains automated API tests for the **kit creation endpoint** of the Urban Grocers application. Tests are written in Python using `pytest` and `requests`, covering positive and negative cases based on equivalence classes and boundary values applied to the `name` field.

This is the first automation project of the bootcamp — the transition from manual testing to repeatable, code-driven validation.

### Tech Stack

| Tool | Purpose |
|------|---------|
| Python 3 | Primary language |
| pytest | Test runner and assertions |
| requests | HTTP client for API calls |
| Git / GitHub | Version control |

### Project Structure

```
qa-project-Urban-Grocers-app/
├── configuration.py          # Base URL and endpoint paths
├── data.py                   # Test data: user body, kit body, headers
├── sender_stand_request.py   # HTTP request functions (POST user, POST kit)
├── create_kit_name_kit_test.py  # pytest test cases
├── .gitignore
└── README.md
```

### File Responsibilities

**`configuration.py`** — Single source of truth for the server URL and endpoint paths. Changing the environment only requires editing this file.

```python
URL_SERVICE    = "https://<server>.tripleten-services.com"
CREATE_USER_PATH = "/api/v1/users/"
KITS_PATH        = "/api/v1/kits/"
```

**`data.py`** — Test data bodies and headers. Keeping data separate from logic makes tests easier to maintain.

**`sender_stand_request.py`** — Wraps the `requests` library into two reusable functions so test files never call `requests` directly.

**`create_kit_name_kit_test.py`** — All pytest test cases organized by equivalence class and boundary value scenarios.

### Endpoint Under Test

```
POST /api/v1/kits/
Authorization: Bearer <token>
Content-Type: application/json

{ "name": "<string>" }
```

**Business rule:** `name` must be a non-empty string between 1 and 511 characters.

### Test Design

Tests apply **equivalence class partitioning** and **boundary value analysis** to the `name` field.

| Class | Range | Representative Value |
|-------|-------|----------------------|
| Valid — minimum boundary | 1 char | `"a"` |
| Valid — within range | 2–510 chars | `"El valor de prueba..."` |
| Valid — special characters | Any valid length | `"N%@*"` |
| Valid — spaces | With leading/trailing | `" A Aaa "` |
| Valid — numeric string | Digits only | `"123"` |
| Invalid — empty string | 0 chars | `""` |
| Invalid — exceeds max | > 511 chars | `"a" * 512` |
| Invalid — missing field | No `name` key | `{}` |
| Invalid — wrong type | Integer | `123` |

### Test Cases

| Test Function | Type | Input | Expected Status |
|--------------|------|-------|----------------|
| `test_create_kit_valid_cases` | Positive ×5 | Valid class values | 201 Created |
| `test_create_kit_invalid_cases` | Negative ×4 | Empty, too long, missing, wrong type | 400 Bad Request |
| `test_missing_token` | Negative | Valid body, no auth token | 401 Unauthorized |
| `test_get_new_user_token` | Smoke | Valid user body | Token is not None |

### How to Run

```bash
# 1. Clone the repository
git clone https://github.com/AMluisXVI/api-automation.git
cd API-Test-Automation-Suite-for-Urban-Grocers

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate  # macOS / Linux
# venv\Scripts\activate   # Windows

# 3. Install dependencies
pip install pytest requests

# 4. Run all tests
pytest create_kit_name_kit_test.py -v
```

**Expected output:**
```
PASSED  create_kit_name_kit_test.py::test_get_new_user_token
PASSED  create_kit_name_kit_test.py::test_create_kit_valid_cases
PASSED  create_kit_name_kit_test.py::test_create_kit_invalid_cases
PASSED  create_kit_name_kit_test.py::test_missing_token
```

### Key Concepts Applied

| Concept | Where Used |
|---------|-----------|
| Equivalence class partitioning | `name` field test data selection |
| Boundary value analysis | 1-char min, 511-char max, 512-char over-limit |
| Separation of concerns | data / config / requests / tests in separate files |
| DRY principle | `get_kit_body()` and assertion helpers avoid code repetition |
| Auth token lifecycle | New user created per test via `get_new_user_token()` |
| HTTP status code validation | 201, 400, 401 assertions |

---

## 🇪🇸 Español

### Descripción del Proyecto

Este proyecto contiene pruebas automatizadas de API para el **endpoint de creación de kits** de la aplicación Urban Grocers. Las pruebas están escritas en Python usando `pytest` y `requests`, cubriendo casos positivos y negativos basados en clases de equivalencia y valores límite aplicados al campo `name`.

Este es el primer proyecto de automatización del bootcamp — la transición de pruebas manuales a validación repetible basada en código.

### Stack Tecnológico

| Herramienta | Propósito |
|-------------|-----------|
| Python 3 | Lenguaje principal |
| pytest | Ejecutor de pruebas y aserciones |
| requests | Cliente HTTP para llamadas API |
| Git / GitHub | Control de versiones |

### Estructura del Proyecto

```
qa-project-Urban-Grocers-app/
├── configuration.py          # URL base y rutas de endpoints
├── data.py                   # Datos de prueba: body de usuario, kit, headers
├── sender_stand_request.py   # Funciones de solicitud HTTP
├── create_kit_name_kit_test.py  # Casos de prueba pytest
├── .gitignore
└── README.md
```

### Endpoint Bajo Prueba

```
POST /api/v1/kits/
Authorization: Bearer <token>
Content-Type: application/json

{ "name": "<string>" }
```

**Regla de negocio:** `name` debe ser un string no vacío entre 1 y 511 caracteres.

### Diseño de Pruebas

Las pruebas aplican **partición de clases de equivalencia** y **análisis de valores límite** al campo `name`.

| Clase | Rango | Valor Representativo |
|-------|-------|---------------------|
| Válido — límite mínimo | 1 carácter | `"a"` |
| Válido — dentro del rango | 2–510 caracteres | `"El valor de prueba..."` |
| Válido — caracteres especiales | Cualquier longitud | `"N%@*"` |
| Válido — con espacios | Con iniciales/finales | `" A Aaa "` |
| Válido — numérico | Solo dígitos | `"123"` |
| Inválido — vacío | 0 caracteres | `""` |
| Inválido — excede máximo | > 511 caracteres | `"a" * 512` |
| Inválido — campo ausente | Sin clave `name` | `{}` |
| Inválido — tipo incorrecto | Entero | `123` |

### Cómo Ejecutar

```bash
# 1. Clonar el repositorio
git clone https://github.com/AMluisXVI/api-automation.git
cd API-Test-Automation-Suite-for-Urban-Grocers

# 2. Crear y activar entorno virtual
python -m venv venv
source venv/bin/activate

# 3. Instalar dependencias
pip install pytest requests

# 4. Ejecutar todas las pruebas
pytest create_kit_name_kit_test.py -v
```

### Conceptos Clave Aplicados

| Concepto | Dónde se Usó |
|----------|-------------|
| Partición de clases de equivalencia | Selección de datos de prueba para campo `name` |
| Análisis de valores límite | Mínimo 1, máximo 511, exceso 512 caracteres |
| Separación de responsabilidades | datos / configuración / solicitudes / tests en archivos separados |
| Principio DRY | `get_kit_body()` y helpers de aserción evitan repetición |
| Ciclo de vida del token | Usuario nuevo creado por prueba con `get_new_user_token()` |
| Validación de códigos HTTP | Aserciones 201, 400, 401 |

---

> 📚 Project developed as part of the **TripleTen QA Engineering Bootcamp** · Sprint 8 · Cohort 23
