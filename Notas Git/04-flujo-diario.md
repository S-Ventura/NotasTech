# Flujo diario con Git

Guia practica para mantener tu rama al dia con main y evitar conflictos.

## Al iniciar el dia

```bash
git fetch origin
git rebase origin/main         # o git merge origin/main si tu equipo usa merge
```

Docs:
- https://git-scm.com/docs/git-fetch
- https://git-scm.com/docs/git-rebase
- https://git-scm.com/docs/git-merge

Riesgos:
- `git rebase` reescribe historial; si ya publicaste commits, puedes causar conflictos en otros clones.
- `git merge` puede introducir commits de merge extra y ruido en el historial si tu equipo busca linealidad.

## Antes de comenzar un bloque de trabajo importante

```bash
git fetch origin
git rebase origin/main
```

Docs:
- https://git-scm.com/docs/git-fetch
- https://git-scm.com/docs/git-rebase

Riesgos:
- Si tienes cambios locales sin commit, el rebase puede interrumpirse o generar conflictos mas complejos.

## Ciclo de trabajo (durante el dia)

```bash
git status
# trabaja

git add -A
git commit -m "Mensaje claro"
```

Docs:
- https://git-scm.com/docs/git-status
- https://git-scm.com/docs/git-add
- https://git-scm.com/docs/git-commit

Riesgos:
- `git add -A` incluye borrados y movimientos; revisa con `git status` para evitar subir cambios no deseados.

## Antes de hacer push

```bash
git fetch origin
git rebase origin/main
git push
```

Docs:
- https://git-scm.com/docs/git-fetch
- https://git-scm.com/docs/git-rebase
- https://git-scm.com/docs/git-push

Riesgos:
- Si el rebase cambia el historial, puede requerir `git push --force-with-lease` y afectar a otros; valida politicas del equipo.

## Antes de abrir un PR

```bash
git fetch origin
git rebase origin/main
# corre tests y luego git push
```

Docs:
- https://git-scm.com/docs/git-fetch
- https://git-scm.com/docs/git-rebase
- https://git-scm.com/docs/git-push

Riesgos:
- Rebase cerca de abrir el PR puede cambiar los commits revisados; informa al equipo si el historial cambia.

## Cuando tu PR ya fue mergeado a main

```bash
git checkout main
git pull
```

Docs:
- https://git-scm.com/docs/git-checkout
- https://git-scm.com/docs/git-pull

Si vas a seguir usando tu rama:

```bash
git checkout tu-rama
git rebase origin/main
git push
```

Docs:
- https://git-scm.com/docs/git-checkout
- https://git-scm.com/docs/git-rebase
- https://git-scm.com/docs/git-push

Si ya no la necesitas:

```bash
git branch -d tu-rama
```

Docs:
- https://git-scm.com/docs/git-branch

Riesgos:
- `git branch -d` falla si hay commits no mergeados; con `-D` los pierdes si no hay backup.

## Nota importante

- `pull.rebase=true` solo aplica cuando usas `git pull`. Si haces `git fetch` + `git merge`, no estas rebaseando.

## Referencias oficiales (Git)

- https://git-scm.com/docs
