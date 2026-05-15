# Primeros pasos con Git / Getting Started with Git

## ¿Qué es Git?

**Git** es un sistema de control de versiones distribuido. Permite registrar el historial completo de cambios de un proyecto, trabajar en paralelo con otras personas y recuperar cualquier versión anterior del código.

**GitHub** es una plataforma web que aloja repositorios Git y añade herramientas de colaboración (Pull Requests, Issues, Actions, etc.).

---

## Conceptos fundamentales

### Repositorio (repository / repo)
Carpeta de proyecto que Git vigila. Contiene todo el código y el historial completo de cambios.

- **Local**: copia en tu máquina.
- **Remoto (remote)**: copia en GitHub, compartida con el equipo.

### Commit
Una "foto" del estado del proyecto en un momento dado. Cada commit tiene un identificador único (hash), un autor, una fecha y un mensaje.

### Rama (branch)
Una línea de desarrollo independiente. Permite trabajar en una funcionalidad sin afectar el código principal hasta que esté listo.

### Merge
Integración de los cambios de una rama en otra.

### Pull Request (PR)
Propuesta formal de integrar una rama en otra, con revisión de código por parte del equipo.

### Staging area (índice)
Zona intermedia donde preparas los cambios antes de hacer un commit. Solo lo que está en staging se incluye en el siguiente commit.

---

## Configuración inicial obligatoria

Antes de usar Git por primera vez, configura tu identidad. Esta información aparece en todos tus commits:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu_email@ejemplo.com"
```

Configura también el editor por defecto (opcional):

```bash
# VS Code
git config --global core.editor "code --wait"

# Vim
git config --global core.editor "vim"
```

Verifica tu configuración:

```bash
git config --list
```

---

## El flujo básico de trabajo

```
1. Clonar o inicializar el repositorio
2. Crear una rama para tu tarea
3. Hacer cambios en los archivos
4. Añadir cambios al staging (git add)
5. Hacer commit con mensaje descriptivo (git commit)
6. Subir la rama al remoto (git push)
7. Abrir un Pull Request en GitHub
8. Revisión y merge
```

### Diagrama de estados de un archivo

```
Sin seguimiento    →    git add    →    Staging    →    git commit    →    Historial
 (untracked)                          (staged)                            (committed)
                         ↑
               (modificado / modified)
```

---

## Buenas prácticas

### Commits pequeños y frecuentes
Un commit debe representar **un cambio lógico**. Mejor muchos commits pequeños que uno grande con todo mezclado. Es más fácil revisarlos, revertirlos y entenderlos.

### Mensajes de commit descriptivos
El mensaje debe explicar el **por qué**, no solo el qué. El código ya muestra el qué.

```bash
# Malo
git commit -m "cambios"
git commit -m "fix"
git commit -m "wip"

# Bueno
git commit -m "fix: handle null response from payment API"
git commit -m "feat(auth): add password reset via email"
```

### Nunca trabajar directamente en `main`
Siempre crea una rama para tus cambios. La rama `main` debe estar siempre estable.

### Hacer `git pull` antes de empezar a trabajar
Antes de crear una rama o continuar el trabajo del día, actualiza tu copia local para evitar conflictos innecesarios.

### Revisar qué vas a hacer commit antes de hacerlo
Usa `git status` y `git diff` para revisar los cambios antes de añadirlos al staging.

### No subir archivos sensibles
Nunca hagas commit de:
- Contraseñas, tokens o API keys
- Archivos `.env` con configuración local
- Credenciales de bases de datos
- Certificados privados

Usa un archivo `.gitignore` para excluirlos.

---

## El archivo `.gitignore`

Lista los archivos y carpetas que Git debe ignorar. Se coloca en la raíz del repositorio.

Ejemplo típico:

```gitignore
# Dependencias
node_modules/
vendor/

# Variables de entorno
.env
.env.local
*.env

# Compilados
dist/
build/
*.o
*.pyc
__pycache__/

# Editores
.vscode/
.idea/
*.swp

# Sistema operativo
.DS_Store
Thumbs.db
```

> Puedes generar un `.gitignore` para tu stack en [gitignore.io](https://www.toptal.com/developers/gitignore).

---

## Resolver conflictos / Merge Conflicts

Un conflicto ocurre cuando dos personas han modificado la misma parte del mismo archivo en ramas diferentes. Git no puede decidir automáticamente cuál versión es la correcta.

Cuando ocurre un conflicto, el archivo afectado muestra:

```
<<<<<<< HEAD
Código de tu rama actual
=======
Código de la otra rama
>>>>>>> feature/otra-rama
```

Para resolverlo:

1. Abre el archivo y decide qué código mantener (puede ser uno, el otro, o una combinación).
2. Elimina los marcadores `<<<<<<<`, `=======` y `>>>>>>>`.
3. Haz `git add` del archivo resuelto.
4. Completa el merge con `git commit`.

Los editores como VS Code tienen herramientas visuales para resolver conflictos de forma más cómoda.

---

## Recursos adicionales

- [Pro Git Book](https://git-scm.com/book/es/v2) — libro oficial gratuito, disponible en español
- [GitHub Docs](https://docs.github.com/es) — documentación oficial de GitHub en español
- [Learn Git Branching](https://learngitbranching.js.org/?locale=es_ES) — tutorial interactivo y visual
