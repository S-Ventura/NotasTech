# 03 · Tu primer repositorio local

## 1. Crear el repositorio
```bash
mkdir primer_git
cd primer_git
git init
```
Git creará una carpeta oculta `.git` con todo el historial. Ahora estás en la rama `main`.

## 2. Agregar tu primer archivo
```bash
echo "Hola Git" > README.md
```

Revisa el estado del repositorio:
```bash
git status
```
Debes ver `README.md` como archivo sin seguimiento (`untracked`).

## 3. Preparar y confirmar cambios
```bash
git add README.md
# git add .  # agrega todos los archivos nuevos y modificados en staging
```
Esto mueve el archivo al área de preparación (*staging area*). Ahora crea tu primer commit:
```bash
git commit -m "Primer commit"
```

## 4. Revisar historial de commits
```bash
git log --oneline
```
Verás una línea con el identificador del commit y el mensaje "Primer commit".

## 5. Conectar con un remoto (opcional)
Si tienes un repositorio vacío en GitHub:
```bash
git remote add origin git@github.com:usuario/primer_git.git
git branch -M main              # `-M` fuerza el renombrado aunque ya exista en el remoto
git push -u origin main         # `-u` (o `--set-upstream`) vincula la rama local con la remota
```
> Si estás configurando claves SSH, revisa la sección dedicada en el capítulo anterior antes de ejecutar estos comandos.

> Tip: usa commits frecuentes con mensajes claros; te ayudarán a entender la evolución del proyecto.

## 6. Contribuir a un repositorio colaborativo
1. Clona el repositorio y revisa sus remotos:
   ```bash
   git clone git@github.com:organizacion/proyecto.git
   cd proyecto
   git remote -v                 # confirma URLs de fetch/push
   ```
2. Asegúrate de trabajar sobre la rama principal actualizada:
   ```bash
   git switch main               # usa el nombre real de la rama (main, master, etc.)
   git pull --rebase origin main # sincroniza tu copia local
   ```
   - `git fetch origin` descarga cambios sin mezclarlos; puedes usarlo antes de `pull`.
   - Si el repositorio usa otra rama base, cámbiala por `develop`, `trunk`, etc.
3. Crea una rama para tu aporte:
   ```bash
   git switch -c feature/nueva-funcionalidad
   ```
   Trabaja siempre en ramas descriptivas; evita modificar directamente `main`.
4. Realiza cambios, revisa el estado y confirma:
   ```bash
   git status -sb
   git add .
   git commit -m "Describe brevemente tu cambio"
   ```
5. Envía la rama al remoto y prepara el Pull Request:
   ```bash
   git push -u origin feature/nueva-funcionalidad
   ```
   - Abre un Pull Request desde la rama recién publicada hacia la rama principal del proyecto.
   - Responde a comentarios y añade commits extra si es necesario.
6. Mantén tu rama alineada mientras revisan tu PR:
   ```bash
   git fetch origin
   git rebase origin/main        # o merge si el flujo del equipo así lo prefiere
   git push --force-with-lease origin feature/nueva-funcionalidad
   ```
   - `--force-with-lease` evita sobrescribir trabajo ajeno al actualizar la rama remota.
   - Si el repositorio es un fork, añade el remoto original una vez:
     ```bash
     git remote add upstream git@github.com:upstream/proyecto.git
     git fetch upstream
     git rebase upstream/main
     ```
     Luego envía tus cambios a tu fork (`origin`) y crea el PR hacia `upstream`.

### Notas sobre ramas
- `git branch` lista las ramas locales; la marca `*` indica cuál está activa.
- Cambiar de rama:
  - Modo clásico: `git checkout nombre-rama`
  - Sintaxis moderna: `git switch nombre-rama`
  ```bash
  git switch -c mi-rama          # crea y cambia a una rama nueva (`-c` = create)
  git switch main               # vuelve a la rama principal
  ```
- Crear y subir una rama al remoto por primera vez:
  ```bash
  git push --set-upstream origin mi-rama   # equivalente a `git push -u origin mi-rama`
  ```
  Después bastará con `git push`/`git pull`.
- Alternativa con comandos tradicionales:
  ```bash
  git checkout -b mi-rama
  git push -u origin mi-rama
  ```
- Otros flags útiles:
  - `-m` en `git branch` renombra localmente sin forzar (`git branch -m main`).
  - `-d` elimina ramas locales ya fusionadas (`git branch -d mi-rama`).
  - `--delete` borra la rama remota (`git push origin --delete mi-rama`).
- Sustituye `mi-rama` por un nombre descriptivo del trabajo (por ejemplo `notas`).

## Resumen rápido
- `git init` crea el repositorio local y la estructura interna.
- `git status` te muestra qué archivos están listos para versionar.
- `git add` mueve cambios al área de preparación.
- `git commit` guarda un snapshot permanente con un mensaje descriptivo.
- Configurar un remoto (`git remote add`, `git push -u origin main`) te permite respaldar y colaborar en línea.
