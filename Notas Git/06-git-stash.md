# 06 · Git stash: guardar cambios temporalmente

`git stash` permite guardar cambios en progreso sin hacer commit, ideal para cuando necesitas cambiar de rama urgentemente o limpiar tu directorio de trabajo.

## ¿Cuándo usar stash?

- Tienes cambios sin terminar y necesitas cambiar de rama
- Quieres hacer `git pull` pero tienes modificaciones locales
- Necesitas probar algo en limpio sin perder tu trabajo
- Quieres mover cambios a otra rama

## Uso básico

### Guardar cambios
```bash
git stash                       # guarda cambios tracked (modificados)
git stash -u                    # incluye archivos untracked (nuevos)
git stash -a                    # incluye también archivos ignorados
git stash -k                    # conserva lo que ya está en staging
```

### Recuperar cambios
```bash
git stash pop                   # aplica y elimina el stash más reciente
git stash apply                 # aplica pero mantiene el stash guardado
```

Riesgos:
- `git stash pop` puede generar conflictos; si falla, el stash no se elimina.

### Ver stashes guardados
```bash
git stash list
```
Salida típica:
```
stash@{0}: WIP on main: abc1234 Último commit
stash@{1}: WIP on feature: def5678 Otro commit
```

## Stash con mensaje descriptivo

Por defecto, Git usa "WIP on rama" como descripción. Puedes personalizarlo:

```bash
git stash push -m "trabajo parcial en login"
git stash save "descripción"    # sintaxis antigua, equivalente
```

## Trabajar con múltiples stashes

### Aplicar un stash específico
```bash
git stash apply stash@{2}       # aplica el tercer stash (índice 2)
git stash pop stash@{1}         # aplica y elimina el segundo stash
```

### Eliminar stashes
```bash
git stash drop                  # elimina el stash más reciente
git stash drop stash@{1}        # elimina un stash específico
git stash clear                 # elimina todos los stashes
```

Riesgos:
- `git stash drop` y `git stash clear` son destructivos; revisa con `git stash list`.

## Ver contenido de un stash

```bash
git stash show                  # resumen de archivos modificados
git stash show -p               # diff completo del stash
git stash show stash@{1} -p     # diff de un stash específico
```

## Stash parcial: solo algunos archivos

### Modo interactivo
```bash
git stash push -p               # selecciona hunks interactivamente
```
Git te preguntará por cada cambio si quieres incluirlo.

### Archivos específicos
```bash
git stash push archivo1.js archivo2.js
git stash push -m "solo estilos" src/styles/
```

## Crear rama desde un stash

Si decides que tus cambios guardados merecen su propia rama:

```bash
git stash branch nueva-rama             # crea rama desde stash más reciente
git stash branch nueva-rama stash@{2}   # desde un stash específico
```

Esto:
1. Crea la rama desde el commit donde hiciste el stash
2. Aplica los cambios del stash
3. Elimina el stash si se aplicó sin conflictos

## Stash y archivos nuevos (untracked)

Por defecto, `git stash` solo guarda archivos que Git ya conoce (tracked). Para incluir archivos nuevos:

```bash
git stash -u                    # --include-untracked
git stash --include-untracked
```

Para incluir absolutamente todo (incluso ignorados):
```bash
git stash -a                    # --all
```

## Flujo típico: cambiar de rama con cambios pendientes

```bash
# Estás trabajando en feature pero surge algo urgente
git stash -u -m "WIP: formulario de registro"

# Cambias a otra rama
git switch main
git switch -c hotfix/bug-critico

# Arreglas el bug, commiteas, vuelves
git switch feature

# Recuperas tu trabajo
git stash pop
```

## Conflictos al aplicar stash

Si el código cambió desde que guardaste el stash, pueden surgir conflictos:

```bash
git stash pop
# Auto-merging archivo.js
# CONFLICT (content): Merge conflict in archivo.js
```

Resuelve los conflictos igual que en un merge:
1. Edita los archivos conflictivos
2. `git add archivo.js`
3. El stash no se elimina automáticamente tras conflictos; usa `git stash drop` manualmente

## Stash vs commit temporal

| Stash | Commit temporal |
|-------|-----------------|
| No aparece en el historial | Aparece en el log |
| Local, no se puede pushear | Se puede compartir |
| Fácil de perder si no tienes cuidado | Más visible |
| Ideal para pausas cortas | Mejor para WIP que quieras guardar |

> Tip: Para trabajo en progreso que quieras mantener, considera un commit con mensaje `WIP: ...` que luego puedes hacer `amend` o `squash`.

## Comandos útiles adicionales

### Ver diferencias entre stash y HEAD actual
```bash
git diff stash@{0}
```

### Aplicar solo un archivo del stash
```bash
git checkout stash@{0} -- ruta/archivo.js
```

### Listar archivos en un stash
```bash
git stash show stash@{0} --name-only
```

## Resumen de comandos

| Acción | Comando |
|--------|---------|
| Guardar cambios | `git stash` |
| Guardar con mensaje | `git stash push -m "mensaje"` |
| Incluir archivos nuevos | `git stash -u` |
| Ver lista de stashes | `git stash list` |
| Aplicar y eliminar | `git stash pop` |
| Aplicar sin eliminar | `git stash apply` |
| Ver contenido | `git stash show -p` |
| Eliminar stash | `git stash drop` |
| Eliminar todos | `git stash clear` |
| Crear rama desde stash | `git stash branch nombre` |

> Tip: Usa stash para interrupciones cortas. Si vas a dejar el trabajo por más tiempo, considera hacer un commit WIP que es más difícil de perder accidentalmente.

## Referencias oficiales (Git)

- https://git-scm.com/docs/git-stash
- https://git-scm.com/docs/git-checkout
- https://git-scm.com/docs/git-diff
- https://git-scm.com/docs/git-switch
