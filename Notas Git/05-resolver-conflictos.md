# 05 · Resolver conflictos

Los conflictos ocurren cuando Git no puede fusionar automáticamente cambios porque dos ramas modificaron las mismas líneas de un archivo. No son un error: son parte normal del trabajo colaborativo.

## ¿Cuándo aparecen los conflictos?

- Durante un `git merge`
- Durante un `git rebase`
- Durante un `git pull` (que internamente hace fetch + merge)
- Al aplicar un `git stash pop` o `git cherry-pick`

## Anatomía de un conflicto

Cuando Git detecta un conflicto, marca el archivo así:

```
<<<<<<< HEAD
código de tu rama actual
=======
código de la rama que intentas fusionar
>>>>>>> feature/otra-rama
```

- **`<<<<<<< HEAD`**: inicio del bloque con tu versión actual
- **`=======`**: separador entre las dos versiones
- **`>>>>>>>`**: fin del bloque con la versión entrante

## Flujo para resolver conflictos

### 1. Identificar archivos en conflicto
```bash
git status
```
Verás los archivos marcados como `both modified` o `Unmerged paths`.

### 2. Abrir y editar los archivos
Abre cada archivo conflictivo y decide qué código mantener:
- Quedarte con tu versión
- Quedarte con la versión entrante
- Combinar ambas manualmente
- Reescribir completamente esa sección

### 3. Eliminar los marcadores de conflicto
Borra las líneas `<<<<<<<`, `=======` y `>>>>>>>` dejando solo el código final.
No commitees archivos que todavía tengan estos marcadores.

### 4. Marcar como resuelto
```bash
git add archivo-resuelto.txt
```
Si agregaste algo por error:
```bash
git restore --staged archivo-resuelto.txt
```

### 5. Continuar la operación
```bash
# Si estabas en un merge:
git commit                      # Git propone un mensaje de merge

# Si estabas en un rebase:
git rebase --continue

# Si era un cherry-pick:
git cherry-pick --continue
```

## Ejemplo práctico

Supongamos que `main` y `feature` modificaron la misma función:

**Estado del archivo tras intentar merge:**
```javascript
function saludar(nombre) {
<<<<<<< HEAD
    return `¡Hola, ${nombre}!`;
=======
    return `Buenos días, ${nombre}`;
>>>>>>> feature/saludo-formal
}
```

**Resolución (combinando ambas):**
```javascript
function saludar(nombre, formal = false) {
    if (formal) {
        return `Buenos días, ${nombre}`;
    }
    return `¡Hola, ${nombre}!`;
}
```

Después:
```bash
git add archivo.js
git commit -m "Merge feature/saludo-formal: agregar opción formal"
```

## Herramientas visuales

### Usar el mergetool configurado
```bash
git mergetool
```
Esto abre la herramienta configurada (VS Code, vimdiff, meld, etc.).

### Configurar VS Code como mergetool
```bash
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

### Alternativa: extensiones de VS Code
La extensión GitLens o el soporte nativo de VS Code muestran los conflictos con botones para:
- **Accept Current Change**: tu versión
- **Accept Incoming Change**: versión entrante
- **Accept Both Changes**: ambas secuencialmente
- **Compare Changes**: ver diferencias lado a lado

## Abortar una operación con conflictos

Si prefieres cancelar y volver al estado anterior:

```bash
git merge --abort           # cancela el merge en curso
git rebase --abort          # cancela el rebase
git cherry-pick --abort     # cancela el cherry-pick
```

## Estrategias de merge

Git permite especificar qué versión preferir automáticamente:

```bash
# Preferir siempre nuestra versión en conflictos
git merge -X ours feature

# Preferir siempre la versión entrante
git merge -X theirs feature
```

> Cuidado: estas opciones resuelven conflictos automáticamente, lo que puede descartar cambios importantes.

### Estrategia `ours` vs opción `-X ours`

```bash
git merge -s ours feature       # estrategia: ignora completamente los cambios de feature
git merge -X ours feature       # opción: en conflictos, prefiere nuestra versión
```

Riesgos:
- `-s ours` descarta todos los cambios entrantes; revisa antes de usarlo.

## Prevenir conflictos

- **Comunicación**: coordina con tu equipo quién trabaja en qué archivos
- **Ramas cortas**: fusiona frecuentemente para reducir divergencia
- **Pull antes de push**: `git pull --rebase origin main` antes de trabajar
- **Commits pequeños**: cambios atómicos son más fáciles de fusionar

## Conflictos en archivos binarios

Git no puede fusionar automáticamente imágenes, PDFs u otros binarios. Debes elegir una versión:

```bash
# Mantener nuestra versión
git checkout --ours imagen.png
git add imagen.png

# Usar la versión entrante
git checkout --theirs imagen.png
git add imagen.png
```

## Ver diferencias durante conflictos

```bash
git diff                        # muestra los conflictos pendientes
git diff --ours archivo.txt     # compara con nuestra versión original
git diff --theirs archivo.txt   # compara con la versión entrante
git diff --base archivo.txt     # compara con el ancestro común
```

## Conflictos durante rebase

En un rebase, los conflictos pueden aparecer en cada commit que se re-aplica:

```bash
git rebase main
# Conflicto en el commit 1
# Resolver...
git add .
git rebase --continue
# Conflicto en el commit 2
# Resolver...
git add .
git rebase --continue
# Completado
```

Si un commit ya no tiene sentido tras resolver:
```bash
git rebase --skip               # salta el commit actual y continúa
```

## Resumen de comandos

| Situación | Comando |
|-----------|---------|
| Ver archivos en conflicto | `git status` |
| Abrir herramienta visual | `git mergetool` |
| Marcar como resuelto | `git add archivo` |
| Continuar merge | `git commit` |
| Continuar rebase | `git rebase --continue` |
| Cancelar merge | `git merge --abort` |
| Cancelar rebase | `git rebase --abort` |
| Usar nuestra versión | `git checkout --ours archivo` |
| Usar versión entrante | `git checkout --theirs archivo` |

> Tip: Los conflictos parecen intimidantes al principio, pero con práctica se vuelven rutinarios. Tómate el tiempo de entender ambas versiones antes de decidir.

## Referencias oficiales (Git)

- https://git-scm.com/docs/git-merge
- https://git-scm.com/docs/git-rebase
- https://git-scm.com/docs/git-pull
- https://git-scm.com/docs/git-stash
- https://git-scm.com/docs/git-cherry-pick
- https://git-scm.com/docs/git-status
- https://git-scm.com/docs/git-add
- https://git-scm.com/docs/git-diff
- https://git-scm.com/docs/git-mergetool
- https://git-scm.com/docs/git-restore
