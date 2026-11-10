# envshield

[![CI](https://img.shields.io/badge/build-passing-brightgreen)](#)
[![Python 3.9+](https://img.shields.io/badge/python-3.9%2B-blue)](#)
[![Zero Dependencies](https://img.shields.io/badge/dependencies-0-success)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**envshield** is a lightweight, zero-dependency Python library for declarative environment variable validation, `.env` variable interpolation, automatic secret redaction in logs/tracebacks, and `.env.example` generation.

It replaces brittle startup boilerplate and heavy configuration dependencies with a single, strictly typed module that fails fast on misconfiguration and prevents accidental secret leakage.

## Features

- **Zero Runtime Dependencies**: Pure Python 3.9+ standard library implementation.
- **Fail-Fast Schema Validation**: Aggregates all missing, malformed, or out-of-range environment variables into a single human-readable startup report instead of failing one variable at a time.
- **Leak-Proof `SecretStr`**: Wraps API keys, tokens, and database credentials so `str()`, `repr()`, and dictionary dumps automatically mask sensitive values (`***REDACTED***`) unless `.get_secret_value()` is explicitly called.
- **Built-in `.env` Loader with Interpolation**: Parses `.env` files with support for quoted multiline strings, inline comments, `export` prefixes, and `${VAR:-default}` variable expansion.
- **Auto-Generated `.env.example`**: Generate documented `.env.example` templates directly from your Python schema class via CLI or API.

## Quick Start

```python
from envshield import EnvSchema, Str, Int, Bool, Url, Enum, Secret, ListField

class AppConfig(EnvSchema):
    APP_ENV = Enum(["development", "staging", "production"], default="development", help="Runtime environment")
    PORT = Int(default=8080, min_val=1, max_val=65535, help="HTTP listen port")
    DEBUG = Bool(default=False, help="Enable verbose debug logging")
    DATABASE_URL = Url(schemes=["postgres", "postgresql", "sqlite"], help="Primary database connection URL")
    API_KEY = Secret(min_length=16, help="External service API key")
    ALLOWED_HOSTS = ListField(Str(), default=["localhost", "127.0.0.1"], separator=",", help="CORS allowed hosts")

# Load from environment + optional .env file
config = AppConfig.load(dotenv_path=".env")

print(config.PORT)                  # 8080
print(config.API_KEY)               # SecretStr('***REDACTED***')
print(config.API_KEY.get_secret_value())  # actual secret string
print(config.to_dict(redact=True))  # safe dictionary for structured logging
```

## CLI Usage

Validate an environment file against a schema or export a clean `.env.example`:

```bash
# Generate a documented .env.example from a schema class
envshield example myapp.config:AppConfig -o .env.example

# Validate current environment or a .env file against a schema
envshield check myapp.config:AppConfig --dotenv .env

# Print redacted configuration summary for debugging
envshield dump myapp.config:AppConfig --dotenv .env
```

## Running Tests

```bash
python -m unittest discover -s tests -v
```

## License

MIT
