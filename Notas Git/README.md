# Notas para Git

Conjunto de apuntes breves para introducir Git desde cero. Esta es una guía desde los conceptos básicos hasta los primeros pasos colaborativos.

## Cómo usar estos apuntes
1. Leer los capítulos en orden para construir el conocimiento gradualmente.
2. Practicar cada comando en tu propio repositorio de pruebas.
3. Regresar a esta página cuando necesites recordar workflows típicos.

### Capítulos disponibles
1. [¿Qué es Git?](01-que-es-git.md)
2. [Instalar y configurar Git](02-instalar-configurar-git.md)
3. [Tu primer repositorio local](03-primer-repositorio.md)

Próximos temas sugeridos: ramas (`git branch`/`git switch`), trabajo colaborativo (`git pull`, `git push`, `git merge`) y resolución de conflictos.

## Resumen de comandos esenciales
### Preparar el entorno
```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu_correo@example.com"
```

El siguiente comando muestra la configuración global de Git en tu dispositivo
```bash
git config --global --list
```

### Ciclo básico de trabajo
```bash
git status                    # inspecciona los cambios pendientes
git add archivo.txt            # prepara un archivo específico
git add .                      # prepara todos los cambios rastreados
git commit -m "Mensaje claro"  # guarda un snapshot con descripción
git log --oneline              # revisa el historial compacto
```

### Sincronizar con el remoto
```bash
git push origin main           # envía commits locales a la rama remota
git pull origin main           # trae cambios remotos e intenta fusionarlos
git fetch origin               # actualiza referencias sin fusionar todavía
git merge origin/main          # fusiona manualmente después de un fetch
```

### Flujo típico para Pull Requests
```bash
git switch -c mi-feature             # crea una rama para tu cambio
git add . && git commit -m "Describe tu cambio"
git push -u origin mi-feature        # publica la rama y crea el seguimiento
# abre el PR desde la plataforma (GitHub/GitLab/Bitbucket)
```
> Tras aprobarse el PR, fusiona desde la interfaz web o con `git merge`, luego actualiza tu rama principal con `git pull origin main`.

Mantén esta chuleta a mano para recordar los comandos más usados mientras avanzas por los tutoriales.
