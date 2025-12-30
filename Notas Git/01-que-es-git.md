# 01 · ¿Qué es Git?

Git es un sistema de control de versiones distribuido que permite:
- Guardar el historial de cambios de un proyecto.
- Colaborar con otras personas sin sobrescribir el trabajo ajeno.
- Recuperar versiones anteriores cuando algo falla.

## Conceptos clave
- **Repositorio (repo):** carpeta que contiene tu proyecto y su historial.
- **Commit:** captura de los archivos en un momento específico.
- **Branch (rama):** línea de trabajo paralela; `main` suele ser la rama principal.
- **Remote (remoto):** copia del repositorio alojada en otra máquina, por ejemplo GitHub.

Git es la herramienta de control de versiones; GitHub/GitLab/Bitbucket son plataformas para alojar repositorios y colaborar.

## Flujo básico
1. Modificas archivos en tu carpeta local.
2. Preparas los cambios con `git add`.
3. Creas un commit con `git commit`.
4. Sincronizas con otros repositorios (remotos) mediante `git push` o `git pull`.

> Consejo: Git funciona igual en proyectos de código, documentos o cualquier tipo de archivo de texto.

Riesgos:
- Git no es backup por si solo; necesitas un remoto o copias externas para recuperar datos.

Resumen rapido:
- Git guarda historial y facilita colaborar sin pisar cambios.
- Repositorio, commit, rama y remoto son los conceptos base.

## Referencias oficiales (Git):
- https://git-scm.com/docs
- https://git-scm.com/docs/gitglossary
