# 04 · Ramas en profundidad

Las ramas son una de las características más potentes de Git. Permiten trabajar en funcionalidades, correcciones o experimentos de forma aislada sin afectar la rama principal.

## ¿Qué es una rama?

Una rama es simplemente un puntero móvil a un commit. Cuando creas una rama nueva, Git genera un nuevo puntero que puedes mover independientemente de otras ramas.

```
main:       A---B---C
                     \
feature:              D---E
```

## Crear y cambiar de rama

### Sintaxis moderna (recomendada)
```bash
git switch -c nueva-rama        # crea y cambia a la rama
git switch main                 # vuelve a main
git switch -                    # vuelve a la rama anterior (como cd -)
```

### Sintaxis clásica
```bash
git checkout -b nueva-rama      # crea y cambia
git checkout main               # cambia de rama
```

### Solo crear sin cambiar
```bash
git branch nueva-rama           # crea la rama pero no cambia a ella
```

## Listar y gestionar ramas

```bash
git branch                      # lista ramas locales (* indica la activa)
git branch -v                   # incluye último commit de cada rama
git branch -vv                  # muestra también el remoto vinculado
git branch -a                   # lista locales y remotas
git branch -r                   # solo remotas
```

### Renombrar una rama
```bash
git branch -m nombre-antiguo nombre-nuevo
git branch -m nuevo-nombre      # renombra la rama actual
```

### Eliminar ramas
```bash
git branch -d rama-fusionada    # elimina si ya fue fusionada
git branch -D rama-sin-fusionar # fuerza eliminación
git push origin --delete rama   # elimina rama remota
```

Riesgos:
- `git branch -D` y `git push --delete` eliminan ramas; confirma antes de ejecutar.

## Fusionar ramas (merge)

Cuando terminas el trabajo en una rama, normalmente la fusionas de vuelta a `main`.

```bash
git switch main
git merge feature/nueva-funcionalidad
```

### Tipos de merge

#### Fast-forward
Si `main` no avanzó desde que creaste la rama, Git simplemente mueve el puntero:
```
Antes:
main:     A---B
               \
feature:        C---D

Después (fast-forward):
main:     A---B---C---D
```

```bash
git merge feature               # fast-forward automático si es posible
git merge --ff-only feature     # falla si no puede hacer fast-forward
```

#### Merge commit
Si `main` avanzó, Git crea un commit de fusión con dos padres:
```
Antes:
main:     A---B---E
               \
feature:        C---D

Después:
main:     A---B---E---F (merge commit)
               \     /
feature:        C---D
```

```bash
git merge --no-ff feature       # fuerza merge commit aunque sea posible ff
```

> Tip: Algunos equipos prefieren siempre `--no-ff` para mantener el historial de cada feature visible.

## Rebase: alternativa al merge

`git rebase` reescribe la historia moviendo los commits de tu rama sobre la punta de otra rama.

```bash
git switch feature
git rebase main
```

Riesgos:
- `git rebase` reescribe historial; evita usarlo en ramas compartidas.

```
Antes:
main:     A---B---E
               \
feature:        C---D

Después del rebase:
main:     A---B---E
                   \
feature:            C'---D'
```

### ¿Cuándo usar rebase?

| Situación | Recomendación |
|-----------|---------------|
| Actualizar rama local antes de PR | `git rebase origin/main` |
| Mantener historial lineal | Rebase |
| Rama ya compartida con otros | Merge (evita reescribir historia pública) |
| Fusionar feature terminada | Merge (preserva contexto) |

### Rebase interactivo
Permite reorganizar, combinar o editar commits antes de fusionar:
```bash
git rebase -i HEAD~3            # últimos 3 commits
git rebase -i main              # commits desde que te separaste de main
```

Opciones comunes en el editor:
- `pick`: mantiene el commit
- `reword`: cambia el mensaje
- `squash`: combina con el commit anterior
- `drop`: elimina el commit

## Estrategias de branching

### GitHub Flow (simple)
1. `main` siempre desplegable
2. Crear rama por cada feature/fix
3. Abrir Pull Request
4. Revisar, aprobar, fusionar
5. Desplegar desde `main`

### Git Flow (completo)
- `main`: producción
- `develop`: integración
- `feature/*`: nuevas funcionalidades
- `release/*`: preparación de versión
- `hotfix/*`: correcciones urgentes

### Trunk-Based Development
- Ramas muy cortas (horas, no días)
- Integración continua a `main`
- Feature flags para código incompleto

## Ramas remotas

### Publicar una rama local
```bash
git push -u origin mi-rama      # -u establece el seguimiento
```

### Traer ramas remotas
```bash
git fetch origin                        # actualiza referencias remotas
git switch -c local-rama origin/rama    # crea local desde remota
git switch rama                         # Git 2.23+ lo hace automáticamente
```

### Limpiar ramas remotas obsoletas
```bash
git fetch --prune                       # elimina referencias a ramas borradas
git remote prune origin                 # alternativa
```

Riesgos:
- `--prune` elimina referencias locales a ramas remotas borradas; confirma que no las necesitas.

## Buenas prácticas

- **Nombres descriptivos**: `feature/login-oauth`, `fix/header-overflow`, `docs/api-endpoints`
- **Ramas cortas**: fusiona pronto y frecuentemente
- **Una tarea por rama**: facilita revisión y rollback
- **Sincroniza antes de crear rama**: `git pull origin main && git switch -c nueva-rama`
- **Elimina ramas fusionadas**: mantén el repositorio limpio

## Resumen de comandos

| Acción | Comando |
|--------|---------|
| Crear y cambiar | `git switch -c rama` |
| Cambiar de rama | `git switch rama` |
| Listar todas | `git branch -a` |
| Fusionar | `git merge rama` |
| Rebase | `git rebase main` |
| Eliminar local | `git branch -d rama` |
| Eliminar remota | `git push origin --delete rama` |
| Publicar | `git push -u origin rama` |

## Referencias oficiales (Git)

- https://git-scm.com/docs/git-branch
- https://git-scm.com/docs/git-switch
- https://git-scm.com/docs/git-checkout
- https://git-scm.com/docs/git-merge
- https://git-scm.com/docs/git-rebase
- https://git-scm.com/docs/git-fetch
- https://git-scm.com/docs/git-push
- https://git-scm.com/docs/git-remote
