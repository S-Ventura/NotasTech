# 08 · Reescribir el historial

Git permite modificar commits pasados para corregir errores, reorganizar cambios o limpiar el historial antes de compartirlo. Estas operaciones son poderosas pero requieren precaución.

## Regla de oro

> **Nunca reescribas historial que ya ha sido pusheado y compartido con otros.**

Reescribir commits públicos causa problemas a todos los que ya tienen esos commits. Usa estas técnicas solo en ramas locales o ramas personales que nadie más ha descargado.

## git commit --amend

Modifica el último commit sin crear uno nuevo.

### Cambiar el mensaje
```bash
git commit --amend -m "Nuevo mensaje corregido"
```

### Agregar archivos olvidados
```bash
git add archivo_olvidado.js
git commit --amend --no-edit    # mantiene el mensaje original
```

### Cambiar autor
```bash
git commit --amend --author="Nombre <email@ejemplo.com>"
```

> Nota: `--amend` crea un commit nuevo con distinto hash. El commit original queda huérfano.

## git rebase interactivo

Permite reorganizar, editar, combinar o eliminar múltiples commits.

```bash
git rebase -i HEAD~5            # últimos 5 commits
git rebase -i abc1234           # desde un commit específico
git rebase -i main              # commits desde que divergiste de main
```

### El editor de rebase
Git abre un editor con los commits listados (del más antiguo al más reciente):

```
pick abc1234 Primer commit
pick def5678 Segundo commit
pick ghi9012 Tercer commit

# Comandos:
# p, pick = usar commit
# r, reword = usar commit, pero editar el mensaje
# e, edit = usar commit, pero parar para modificar
# s, squash = combinar con el commit anterior
# f, fixup = como squash, pero descarta el mensaje
# d, drop = eliminar commit
```

### Reordenar commits
Simplemente cambia el orden de las líneas:
```
pick def5678 Segundo commit
pick abc1234 Primer commit
pick ghi9012 Tercer commit
```

### Combinar commits (squash)
```
pick abc1234 Primer commit
squash def5678 Segundo commit
squash ghi9012 Tercer commit
```
Git combinará los tres en uno y te pedirá escribir un nuevo mensaje.

### Fixup: squash silencioso
```
pick abc1234 Implementar feature
fixup def5678 fix typo
fixup ghi9012 otro fix
```
Combina todo usando solo el mensaje del primer commit.

### Editar un commit antiguo
```
pick abc1234 Primer commit
edit def5678 Quiero modificar este     # <-- cambiar pick por edit
pick ghi9012 Tercer commit
```

Git pausará en ese commit. Haz tus cambios y:
```bash
git add .
git commit --amend
git rebase --continue
```

### Eliminar un commit
```
pick abc1234 Primer commit
drop def5678 Este commit no sirve      # o simplemente borra la línea
pick ghi9012 Tercer commit
```

### Abortar un rebase
Si algo sale mal:
```bash
git rebase --abort
```

## git reset

Mueve la rama actual a un commit anterior, con diferentes efectos en el área de trabajo.

### Modos de reset

```bash
git reset --soft HEAD~1         # deshace commit, mantiene cambios en staging
git reset --mixed HEAD~1        # deshace commit, mantiene cambios en working dir (default)
git reset --hard HEAD~1         # deshace commit y ELIMINA los cambios
```

Riesgos:
- `git reset --hard` elimina cambios locales; crea backup si no estás seguro.

| Modo | Commit | Staging | Working dir |
|------|--------|---------|-------------|
| `--soft` | Deshace | Mantiene | Mantiene |
| `--mixed` | Deshace | Limpia | Mantiene |
| `--hard` | Deshace | Limpia | Limpia |

### Ejemplos prácticos

```bash
# Deshacer el último commit pero mantener los cambios
git reset --soft HEAD~1

# Deshacer los últimos 3 commits
git reset --soft HEAD~3

# Volver a un commit específico (cuidado: pierde cambios)
git reset --hard abc1234
```

### Reset vs Revert

| `git reset` | `git revert` |
|-------------|--------------|
| Elimina commits del historial | Crea un nuevo commit que deshace |
| Solo para commits no publicados | Seguro para commits públicos |
| Modifica el historial | Preserva el historial |

```bash
# Revert: deshace un commit de forma segura
git revert abc1234              # crea commit que revierte abc1234
git revert HEAD                 # revierte el último commit
git revert HEAD~3..HEAD         # revierte los últimos 3 commits
```

## git cherry-pick

Aplica commits específicos de otra rama a la rama actual.

```bash
git cherry-pick abc1234                 # aplica un commit
git cherry-pick abc1234 def5678         # varios commits
git cherry-pick abc1234..def5678        # rango de commits
```

### Opciones útiles
```bash
git cherry-pick -n abc1234      # aplica cambios sin commitear
git cherry-pick -x abc1234      # añade referencia al commit original
git cherry-pick --abort         # cancela si hay conflictos
```

### Caso de uso típico
Tienes un fix en `develop` que necesitas urgentemente en `main`:
```bash
git switch main
git cherry-pick abc1234         # el hash del commit con el fix
```

## git reflog

El reflog es tu red de seguridad. Registra todos los movimientos de HEAD, incluso después de reset o rebase.

```bash
git reflog                      # ver historial de movimientos
git reflog show main            # movimientos de una rama específica
```

Salida típica:
```
abc1234 HEAD@{0}: commit: Último cambio
def5678 HEAD@{1}: reset: moving to HEAD~1
ghi9012 HEAD@{2}: commit: Commit que "perdí"
```

### Recuperar commits "perdidos"
```bash
# Encontrar el commit perdido
git reflog

# Restaurar
git reset --hard HEAD@{2}       # volver a ese punto
git branch recuperado HEAD@{2}  # o crear rama desde ahí
```

> El reflog mantiene referencias por 90 días por defecto. Después, los commits huérfanos pueden ser eliminados por `git gc`.

## Casos de uso comunes

### Limpiar historial antes de PR
```bash
# En tu rama feature, combina todos los commits en uno
git rebase -i main
# Marca todos menos el primero como 'squash'
```

### Separar un commit grande
```bash
git rebase -i HEAD~1
# Cambiar 'pick' por 'edit'
git reset HEAD~1                # deshace el commit pero mantiene cambios
git add archivo1.js
git commit -m "Cambio 1"
git add archivo2.js
git commit -m "Cambio 2"
git rebase --continue
```

### Mover commits a otra rama
```bash
# Estás en main pero los commits debían ir en feature
git branch feature              # crea rama en el punto actual
git reset --hard HEAD~3         # retrocede main
git switch feature              # los commits están aquí
```

Riesgos:
- `git reset --hard` descarta cambios; crea una rama de respaldo antes.

### Eliminar un archivo del historial completo
Si commiteaste accidentalmente un archivo sensible:
```bash
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch archivo_sensible.txt" \
  --prune-empty --tag-name-filter cat -- --all
```

O usando `git-filter-repo` (herramienta externa recomendada):
```bash
git filter-repo --path archivo_sensible.txt --invert-paths
```

> Después de esto, necesitarás `git push --force` y todos los colaboradores deberán re-clonar.

Riesgos:
- `git filter-branch` y `git push --force` reescriben historial; coordina con el equipo.

## Resumen de comandos

| Acción | Comando |
|--------|---------|
| Modificar último commit | `git commit --amend` |
| Rebase interactivo | `git rebase -i HEAD~n` |
| Deshacer commits (mantener cambios) | `git reset --soft HEAD~n` |
| Deshacer commits (perder cambios) | `git reset --hard HEAD~n` |
| Revertir commit publicado | `git revert <hash>` |
| Aplicar commit de otra rama | `git cherry-pick <hash>` |
| Ver historial de movimientos | `git reflog` |
| Recuperar commit perdido | `git reset --hard HEAD@{n}` |
| Abortar rebase | `git rebase --abort` |

> Tip: Antes de operaciones peligrosas, crea una rama de respaldo: `git branch backup`. Si algo sale mal, siempre puedes volver a ella.

## Referencias oficiales (Git)

- https://git-scm.com/docs/git-commit
- https://git-scm.com/docs/git-rebase
- https://git-scm.com/docs/git-reset
- https://git-scm.com/docs/git-revert
- https://git-scm.com/docs/git-cherry-pick
- https://git-scm.com/docs/git-reflog
- https://git-scm.com/docs/git-filter-branch
- https://git-scm.com/docs/git-branch
