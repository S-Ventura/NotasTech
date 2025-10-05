# Guía mínima de instalación y primeros pasos (Ubuntu y macOS)

> **Objetivo**: dejar un entorno limpio (sin conda) desde terminal, con Git, Python, Pipenv, VS Code, y utilidades clave. Incluye uso, validaciones y qué hacer dentro de cada carpeta/proyecto. Pensado para reinstalar rápido en una máquina nueva.

---

## 0. Estructura de carpetas sugerida

```
~/Escritorio/Proyectos/
├─ <repo-o-proyecto-1>/
│  ├─ Pipfile / Pipfile.lock
│  ├─ src/  (código)
│  ├─ data/ (opcional, no versionar datos pesados)
│  ├─ .gitignore
│  └─ README.md
└─ <repo-o-proyecto-2>/
```

- **Un repositorio por proyecto**. Evita `git init` adentro de subcarpetas si el repositorio padre ya existe.
- Mantén **datos grandes** fuera de Git; usa artefactos o almacenamiento externo.

---

## 1. Requisitos del sistema y actualización

### Ubuntu
```bash
sudo apt update && sudo apt upgrade -y
```

### macOS
- Requiere **Homebrew** (gestor de paquetes). Si no lo tienes:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Después:
```bash
brew update && brew upgrade
```

> *Nota:* Homebrew es la forma más limpia de instalar desde CLI en macOS sin descargas manuales.

---

## 2. Git

### Instalar
**Ubuntu**
```bash
sudo apt install -y git
```
**macOS**
```bash
brew install git
```

### Configuración básica
```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu_email@example.com"
```

### Validación
```bash
git --version
git config --list --global
```

---

## 3. Claves SSH para GitHub (recomendado)

### Generar y registrar
```bash
mkdir -p ~/.ssh && chmod 700 ~/.ssh
ssh-keygen -t ed25519 -C "tu_email_de_github@example.com"
# ENTER para ruta por defecto (~/.ssh/id_ed25519) y passphrase opcional

# Agente SSH
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Config opcional para forzar el uso de la llave
cat <<'EOF' >> ~/.ssh/config
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  AddKeysToAgent yes
EOF
chmod 600 ~/.ssh/config

# Copia la clave pública y pégala en GitHub > Settings > SSH and GPG keys > New SSH key
cat ~/.ssh/id_ed25519.pub
```

### Validación
```bash
ssh -T git@github.com
# Esperado: "Hi <usuario>! You've successfully authenticated..."
```

> **Si tu red bloquea el puerto 22**: usa SSH sobre 443
>
> Añade esto a `~/.ssh/config` y prueba de nuevo:
> ```
> Host github.com
>   HostName ssh.github.com
>   User git
>   Port 443
>   IdentityFile ~/.ssh/id_ed25519
>   AddKeysToAgent yes
> ```

---

## 4. Python y Pipenv (entornos por proyecto)

### Python del sistema
**Ubuntu**
```bash
sudo apt install -y python3 python3-venv python3-dev build-essential
python3 --version
```
**macOS**
```bash
brew install python
python3 --version
```

### Instalar Pipenv de forma aislada (recomendado con pipx)
**Ubuntu**
```bash
sudo apt install -y pipx
pipx ensurepath  # cierra y abre la terminal si no se aplica el PATH
pipx install pipenv
```
**macOS**
```bash
brew install pipx
pipx ensurepath
pipx install pipenv
```

### Validación
```bash
pipenv --version
```

> **Por qué pipx**: instala apps CLI de Python (como `pipenv`) en entornos aislados sin tocar el Python del sistema (evita PEP 668).

---

## 5. VS Code (desde CLI)

**Ubuntu** (repositorio oficial de Microsoft):
```bash
sudo apt install -y wget gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | sudo tee /usr/share/keyrings/packages.microsoft.gpg >/dev/null
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | \
  sudo tee /etc/apt/sources.list.d/vscode.list >/dev/null
sudo apt update && sudo apt install -y code
```

**macOS**
```bash
brew install --cask visual-studio-code
```

**Validación**
```bash
code --version
```

> Si `code` no funciona en macOS: abre VS Code una vez y ejecuta el comando de paleta: *Shell Command: Install 'code' command in PATH*.

---

## 6. Elegir versión de Python específica (3.11 en el proyecto)

### Instalar Python 3.11 además del global
**Ubuntu**
```bash
# Intenta primero con repos oficiales
sudo apt update
sudo apt install -y python3.11 python3.11-venv python3.11-dev build-essential

# Si no está disponible, usar PPA Deadsnakes
# sudo apt install -y software-properties-common
# sudo add-apt-repository -y ppa:deadsnakes/ppa
# sudo apt update
# sudo apt install -y python3.11 python3.11-venv python3.11-dev build-essential

/usr/bin/python3.11 --version
```
**macOS**
```bash
brew install python@3.11
/usr/local/opt/python@3.11/bin/python3.11 --version  # Intel
/opt/homebrew/opt/python@3.11/bin/python3.11 --version # Apple Silicon
```

### Crear/Recrear el entorno del proyecto con 3.11 (Pipenv)
Desde la carpeta **del proyecto** (ej.: `~/Escritorio/Proyectos/RT-DETR`):
```bash
# Si ya existe un venv con otra versión
pipenv --rm  # ignora el error si no existe

# Opción A: especificando binario
pipenv --python /usr/bin/python3.11
# macOS (ajusta ruta según tu CPU):
# pipenv --python /opt/homebrew/opt/python@3.11/bin/python3.11

# Instalar dependencias del proyecto
# Si hay Pipfile.lock y quieres respetarlo:
pipenv install --ignore-pipfile
# Si no, calcular desde Pipfile:
# pipenv install

# Activar
pipenv shell
python --version  # Debe mostrar Python 3.11.x
```

### Fijar versión en `Pipfile` (opcional)
```toml
[requires]
python_version = "3.11"
```

---

## 7. Flujo de trabajo por proyecto

### Caso A: clonar un repositorio existente (recomendado)
```bash
cd ~/Escritorio/Proyectos
git clone git@github.com:USUARIO/REPO.git
cd REPO

# Crear/activar entorno
pipenv install --ignore-pipfile   # usa lock si existe
pipenv shell

# Abrir VS Code
code .
```

### Caso B: iniciar un proyecto nuevo
```bash
cd ~/Escritorio/Proyectos
mkdir MiProyecto && cd MiProyecto

# Entorno con versión específica
pipenv --python /usr/bin/python3.11
pipenv install requests  # ejemplo de dependencia
pipenv shell

# Git
git init
cat > .gitignore <<'EOF'
__pycache__/
*.pyc
.env
.venv/
*/.ipynb_checkpoints/
.data/
EOF

git add .
git commit -m "Initial commit"
# Conectar a remoto cuando lo tengas
# git remote add origin git@github.com:USUARIO/MiProyecto.git
# git push -u origin main

# VS Code
code .
```

> **Dentro del venv**: `pip`, `pip3` y `python -m pip` apuntan al mismo intérprete. Fuera del venv, para evitar confusiones, usa `python3 -m pip`.

---

## 8. Validaciones útiles

```bash
# ¿Qué binarios estoy usando?
which python
which pip

# Comprobar intérprete y site-packages
python -c "import sys, site; print(sys.executable); print(sys.version); print(site.getsitepackages())"

# Versiones
python --version
pip --version
pipenv --version

# Git y remotos
git remote -v
git status
```

---

## 9. Troubleshooting rápido

- **PEP 668 / externally-managed-environment (Ubuntu)**: evita instalar apps globales con `pip`. Usa `pipx install <app>` o APT. Para proyectos, usa venv/Pipenv.
- **`pipenv: command not found`**: ejecuta `pipx ensurepath`, cierra/abre terminal; confirma `which pipenv`.
- **SSH `Permission denied (publickey)`**: comprueba `~/.ssh/id_ed25519`, `ssh-add -l`, `~/.ssh/config`, y `ssh -T git@github.com`.
- **Múltiples Pythons**: especifica la ruta del binario al crear el venv (`pipenv --python /usr/bin/python3.11`).
- **`code` no abre**: valida `code --version`. En macOS usa la acción *Install 'code' command in PATH* desde la Paleta de Comandos.

---

## 10. Utilidades opcionales (no invasivas)

```bash
# Formateo y linting
pipx install black
pipx install ruff

# Entorno
pipx install virtualenv

# HTTP CLI
pipx install httpie
```

> Se instalan con `pipx`, quedan aisladas y no contaminan el Python del sistema.

---

## 11. Recordatorio de buenas prácticas

- Un **venv por repositorio**.
- No mezcles `pip install` global con paquetes del sistema; usa `pipx` o venvs.
- Commits pequeños y mensajes claros.
- Mantén actualizado `Pipfile.lock` (`pipenv update` cuando corresponda) y documenta en `README.md` cómo levantar el proyecto.

---

## 12. Mini plantilla de README

```markdown
# Nombre del Proyecto

## Requisitos
- Python 3.11
- Pipenv

## Instalación
```bash
pipenv --python 3.11
pipenv install --ignore-pipfile
pipenv shell
```

## Desarrollo
```bash
code .
python -m <modulo_o_script>
```

## Pruebas
```bash
pytest -q
```
```

---

## 13. Script opcional para configurar SSH (Ubuntu)

Guarda como `~/setup_ssh_github.sh` y ejecútalo con tu e‑mail:

```bash
cat > ~/setup_ssh_github.sh <<'EOS'
#!/usr/bin/env bash
set -euo pipefail
EMAIL="${1:-tu_email_de_github@example.com}"
mkdir -p ~/.ssh && chmod 700 ~/.ssh
if [[ ! -f ~/.ssh/id_ed25519 ]]; then
  ssh-keygen -t ed25519 -C "$EMAIL" -f ~/.ssh/id_ed25519 -N ""
fi
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519 || true
if ! grep -q "Host github.com" ~/.ssh/config 2>/dev/null; then
  {
    echo "Host github.com"
    echo "  HostName github.com"
    echo "  User git"
    echo "  IdentityFile ~/.ssh/id_ed25519"
    echo "  AddKeysToAgent yes"
  } >> ~/.ssh/config
fi
chmod 600 ~/.ssh/config
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub

echo
echo "✅ Copia esta clave pública y pégala en GitHub (Settings > SSH and GPG keys):"
cat ~/.ssh/id_ed25519.pub
echo
echo "Luego prueba: ssh -T git@github.com"
EOS
chmod +x ~/setup_ssh_github.sh
```

Ejecutar:
```bash
bash ~/setup_ssh_github.sh "tu_email_de_github@example.com"
```

---

> **Checklist final para una máquina nueva**
>
> - [ ] Git instalado y configurado
> - [ ] SSH key registrada en GitHub (prueba `ssh -T git@github.com`)
> - [ ] Python 3.11 disponible (`python3.11 --version`)
> - [ ] Pipenv vía `pipx` (`pipenv --version`)
> - [ ] VS Code (`code --version`)
> - [ ] Clonado del repo y `pipenv install --ignore-pipfile` + `pipenv shell`

