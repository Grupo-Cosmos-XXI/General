# Manual de Comandos Git / Git Command Reference

Referencia rápida de los comandos Git más usados en el día a día.

---

## Configuración / Configuration

```bash
# Establecer nombre y email (obligatorio la primera vez)
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"

# Ver toda la configuración
git config --list

# Ver configuración de un repositorio concreto (sin --global)
git config user.name
```

---

## Inicializar y clonar / Init & Clone

```bash
# Crear un repositorio nuevo en el directorio actual
git init

# Clonar un repositorio remoto (SSH recomendado)
git clone git@github.com:GrupoCosmosXXI/nombre-repo.git

# Clonar en una carpeta con nombre diferente
git clone git@github.com:GrupoCosmosXXI/nombre-repo.git mi-carpeta
```

---

## Estado e historial / Status & History

```bash
# Ver el estado del repositorio (archivos modificados, staging, etc.)
git status

# Ver los cambios aún no añadidos al staging
git diff

# Ver los cambios que ya están en staging (listos para commit)
git diff --staged

# Ver el historial de commits
git log

# Historial compacto en una línea por commit
git log --oneline

# Historial con gráfico de ramas
git log --oneline --graph --all

# Ver los cambios introducidos por un commit concreto
git show <hash-del-commit>
```

---

## Staging y commits / Staging & Commits

```bash
# Añadir un archivo al staging
git add nombre-archivo.txt

# Añadir todos los cambios del directorio actual
git add .

# Añadir cambios de forma interactiva (selección por partes)
git add -p

# Quitar un archivo del staging (sin descartar los cambios)
git restore --staged nombre-archivo.txt

# Hacer un commit con mensaje
git commit -m "tipo: descripción del cambio"

# Añadir al staging y hacer commit en un paso (solo archivos ya rastreados)
git commit -am "tipo: descripción"

# Corregir el mensaje del último commit (solo si no está publicado)
git commit --amend -m "mensaje corregido"
```

---

## Ramas / Branches

```bash
# Ver todas las ramas locales
git branch

# Ver ramas locales y remotas
git branch -a

# Crear una nueva rama
git branch nombre-rama

# Cambiar a una rama existente
git checkout nombre-rama
# o con la sintaxis moderna:
git switch nombre-rama

# Crear y cambiar a una nueva rama en un paso
git checkout -b nombre-rama
# o:
git switch -c nombre-rama

# Renombrar la rama actual
git branch -m nuevo-nombre

# Eliminar una rama (solo si ya fue fusionada)
git branch -d nombre-rama

# Eliminar una rama forzosamente
git branch -D nombre-rama
```

---

## Sincronizar con el remoto / Remote Sync

```bash
# Ver los remotos configurados
git remote -v

# Descargar cambios del remoto (sin fusionarlos)
git fetch

# Descargar y fusionar cambios de la rama actual
git pull

# Subir una rama al remoto por primera vez
git push -u origin nombre-rama

# Subir cambios al remoto (rama ya configurada)
git push

# Eliminar una rama en el remoto
git push origin --delete nombre-rama
```

---

## Fusionar ramas / Merging

```bash
# Fusionar una rama en la rama actual
git merge nombre-rama

# Fusionar sin fast-forward (preserva el historial de la rama)
git merge --no-ff nombre-rama

# Abortar un merge con conflictos
git merge --abort
```

---

## Rebase

> Usa rebase para limpiar el historial antes de hacer merge. **Nunca hagas rebase de ramas públicas/compartidas.**

```bash
# Rebasar la rama actual sobre otra
git rebase main

# Rebase interactivo (reorganizar/editar/combinar commits)
git rebase -i HEAD~3     # últimos 3 commits

# Abortar un rebase
git rebase --abort

# Continuar un rebase tras resolver conflictos
git rebase --continue
```

---

## Deshacer cambios / Undoing Changes

```bash
# Descartar cambios en un archivo (no está en staging)
git restore nombre-archivo.txt

# Descartar TODOS los cambios no guardados (cuidado: irreversible)
git restore .

# Deshacer el último commit, conservando los cambios en staging
git reset --soft HEAD~1

# Deshacer el último commit, conservando los cambios sin staging
git reset HEAD~1

# Deshacer el último commit y descartar los cambios (peligroso)
git reset --hard HEAD~1

# Revertir un commit creando un nuevo commit que lo deshace (seguro en ramas públicas)
git revert <hash-del-commit>
```

---

## Stash — Guardar cambios temporalmente

Útil cuando necesitas cambiar de rama pero no quieres hacer commit de cambios incompletos.

```bash
# Guardar cambios actuales en el stash
git stash

# Guardar con un mensaje descriptivo
git stash push -m "trabajo en progreso: formulario login"

# Ver la lista de stashes guardados
git stash list

# Recuperar el último stash (y eliminarlo del stash)
git stash pop

# Recuperar un stash concreto
git stash pop stash@{2}

# Aplicar un stash sin eliminarlo
git stash apply

# Eliminar un stash
git stash drop stash@{0}

# Eliminar todos los stashes
git stash clear
```

---

## Tags — Versiones / Tagging

```bash
# Ver todos los tags
git tag

# Crear un tag ligero
git tag v1.0.0

# Crear un tag anotado (recomendado para versiones)
git tag -a v1.0.0 -m "Versión 1.0.0 — release inicial"

# Subir un tag al remoto
git push origin v1.0.0

# Subir todos los tags al remoto
git push origin --tags

# Eliminar un tag local
git tag -d v1.0.0

# Eliminar un tag en el remoto
git push origin --delete v1.0.0
```

---

## Búsqueda / Search

```bash
# Buscar un texto en los archivos del repositorio
git grep "texto a buscar"

# Buscar en qué commit se introdujo una línea
git log -S "texto a buscar"

# Ver quién modificó cada línea de un archivo
git blame nombre-archivo.txt
```

---

## Comandos de emergencia / Emergency Commands

```bash
# "¿Qué hice mal?" — ver últimas acciones (incluye resets y rebases)
git reflog

# Recuperar una rama o commit eliminado accidentalmente
git checkout -b rama-recuperada <hash-del-reflog>

# Ver diferencia entre tu rama local y el remoto
git diff origin/main..HEAD
```

---

## Resumen del flujo diario / Daily Workflow Summary

```bash
# Al empezar el día
git pull                          # actualizar rama actual

# Empezar una tarea nueva
git switch develop
git pull
git switch -c feature/mi-tarea

# Trabajar...
git status                        # ver qué ha cambiado
git diff                          # ver los cambios en detalle
git add .                         # añadir al staging
git commit -m "feat: descripción" # hacer commit

# Subir y abrir PR
git push -u origin feature/mi-tarea
# → Abrir Pull Request en GitHub
```
