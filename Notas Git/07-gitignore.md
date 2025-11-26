# 07 · .gitignore: excluir archivos del repositorio

El archivo `.gitignore` indica a Git qué archivos o carpetas debe ignorar. Esto evita subir al repositorio archivos innecesarios, sensibles o generados automáticamente.

## ¿Por qué ignorar archivos?

- **Dependencias**: `node_modules/`, `vendor/`, `venv/` se regeneran con el gestor de paquetes
- **Archivos compilados**: `.class`, `.o`, `.pyc`, `dist/`, `build/`
- **Configuración local**: `.env`, `config.local.js`, credenciales
- **Archivos del sistema**: `.DS_Store`, `Thumbs.db`
- **Archivos de IDE**: `.idea/`, `.vscode/` (settings personales)
- **Logs y caché**: `*.log`, `.cache/`, `tmp/`

## Crear un .gitignore

Crea el archivo en la raíz del repositorio:

```bash
touch .gitignore
```

Ejemplo básico para un proyecto Node.js:
```gitignore
# Dependencias
node_modules/

# Variables de entorno
.env
.env.local
.env.*.local

# Logs
*.log
npm-debug.log*

# Build
dist/
build/

# Sistema operativo
.DS_Store
Thumbs.db

# IDE
.idea/
.vscode/
*.swp
```

## Sintaxis de patrones

### Archivos y extensiones
```gitignore
archivo.txt             # archivo específico
*.log                   # todos los .log
*.py[cod]               # .pyc, .pyo, .pyd
```

### Directorios
```gitignore
logs/                   # carpeta logs y todo su contenido
/logs/                  # solo logs/ en la raíz (no subcarpetas/logs/)
```

### Comodines
```gitignore
*                       # cualquier cosa excepto /
**                      # cualquier ruta (incluyendo subdirectorios)
?                       # un solo carácter
```

### Ejemplos con comodines
```gitignore
*.log                   # cualquier .log en cualquier nivel
/**.log                 # .log solo en la raíz
logs/**/*.log           # .log dentro de logs/ a cualquier profundidad
doc/*.txt               # .txt directamente en doc/ (no subdirectorios)
doc/**/*.txt            # .txt en doc/ y subdirectorios
```

### Negación (excepciones)
```gitignore
*.log                   # ignora todos los .log
!importante.log         # excepto este
```

```gitignore
build/                  # ignora build/
!build/.gitkeep         # pero mantiene este archivo
```

> Nota: No puedes negar un archivo si su directorio padre ya está ignorado.

### Comentarios y líneas en blanco
```gitignore
# Esto es un comentario
# Las líneas en blanco se ignoran

*.tmp
```

## Ignorar archivos ya rastreados

Si un archivo ya tiene commits, agregarlo a `.gitignore` no lo elimina del historial. Debes quitarlo del índice:

```bash
# Quitar del índice pero mantener localmente
git rm --cached archivo.txt
git rm --cached -r carpeta/

# Luego commitear
git add .gitignore
git commit -m "Ignorar archivo.txt"
```

El archivo permanecerá en tu disco pero Git dejará de rastrearlo.

## .gitignore global

Para ignorar archivos en todos tus repositorios (como `.DS_Store`):

```bash
# Crear archivo global
touch ~/.gitignore_global

# Configurar Git para usarlo
git config --global core.excludesfile ~/.gitignore_global
```

Contenido típico de `~/.gitignore_global`:
```gitignore
# macOS
.DS_Store
.AppleDouble
.LSOverride
._*

# Windows
Thumbs.db
ehthumbs.db
Desktop.ini

# Linux
*~

# Editores
*.swp
*.swo
*~
.idea/
.vscode/
*.sublime-project
*.sublime-workspace
```

## .gitignore por lenguaje

### Python
```gitignore
__pycache__/
*.py[cod]
*$py.class
.Python
venv/
.env
*.egg-info/
dist/
build/
.pytest_cache/
.coverage
htmlcov/
```

### JavaScript/Node.js
```gitignore
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*
.npm
dist/
build/
.env
.env.local
coverage/
```

### Java
```gitignore
*.class
*.jar
*.war
*.ear
target/
.gradle/
build/
.idea/
*.iml
```

### Go
```gitignore
*.exe
*.exe~
*.dll
*.so
*.dylib
*.test
*.out
vendor/
```

## Verificar qué se ignora

```bash
# Ver si un archivo específico está ignorado
git check-ignore -v archivo.txt

# Listar todos los archivos ignorados
git status --ignored

# Ver qué regla ignora cada archivo
git check-ignore -v *
```

## Plantillas de .gitignore

GitHub mantiene una colección de plantillas:
- https://github.com/github/gitignore

Puedes generarlos automáticamente:
```bash
# Usando gitignore.io desde la terminal
curl -sL https://www.toptal.com/developers/gitignore/api/node,macos,vscode
```

## .git/info/exclude

Para ignorar archivos solo en tu copia local (sin afectar a otros desarrolladores):

```bash
# Editar el archivo exclude
nano .git/info/exclude
```

Útil para:
- Archivos de configuración personal
- Scripts locales de desarrollo
- Notas personales del proyecto

## Errores comunes

### El archivo no se ignora
```bash
# Probablemente ya está rastreado
git rm --cached archivo.txt
```

### Quiero ignorar todo excepto algunos archivos
```gitignore
# Ignora todo
*

# Excepto estos
!.gitignore
!src/
!src/**
!README.md
```

### Quiero mantener una carpeta vacía
Git no rastrea carpetas vacías. Solución:
```bash
# Crear archivo vacío en la carpeta
touch carpeta/.gitkeep
```

```gitignore
# En .gitignore, permitir .gitkeep
!.gitkeep
```

## Resumen de patrones

| Patrón | Qué ignora |
|--------|------------|
| `archivo.txt` | Ese archivo en cualquier nivel |
| `/archivo.txt` | Solo en la raíz |
| `*.log` | Todos los .log |
| `logs/` | Carpeta logs y contenido |
| `**/logs` | Carpeta logs en cualquier nivel |
| `logs/**` | Todo dentro de logs |
| `!archivo` | Excepción (no ignorar) |
| `#` | Comentario |

> Tip: Configura `.gitignore` al inicio del proyecto. Agregar archivos después de que ya tienen historial es más complejo y puede requerir reescribir la historia si contienen información sensible.
