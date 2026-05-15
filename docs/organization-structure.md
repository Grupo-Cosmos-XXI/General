# Estructura de la Organización / Organization Structure

## Visión general

La organización **GrupoCosmosXXI** en GitHub agrupa todos los proyectos y repositorios del equipo. Esta guía explica cómo están organizados y las normas de convivencia para trabajar en ellos de forma eficiente.

---

## Tipos de repositorios / Repository Types

Los repositorios se clasifican por prefijo de nombre para facilitar su identificación:

| Prefijo | Tipo | Descripción |
|---------|------|-------------|
| `base-` | Infraestructura / Base | Repositorios de configuración y documentación compartida |
| `app-` | Aplicación | Proyectos de software completos (web, móvil, etc.) |
| `lib-` | Librería | Paquetes o módulos reutilizables entre proyectos |
| `srv-` | Servicio | Microservicios, APIs o workers independientes |
| `infra-` | Infraestructura | Configuraciones de servidores, CI/CD, Docker, etc. |
| `doc-` | Documentación | Documentación técnica o de producto |

---

## Equipos y permisos / Teams & Permissions

La organización usa **equipos de GitHub** para gestionar el acceso:

| Equipo | Acceso | Descripción |
|--------|--------|-------------|
| `@owners` | Admin | Administradores de la organización |
| `@leads` | Maintain | Responsables técnicos de proyecto |
| `@developers` | Write | Desarrolladores con acceso de escritura |
| `@reviewers` | Read | Colaboradores externos o revisores |

> **Regla:** Nunca dar permisos individuales. Siempre asignar permisos a través de equipos.

---

## Flujo de trabajo con ramas / Branching Strategy

Usamos un flujo basado en **Git Flow simplificado**:

```
main          ←── rama principal, siempre estable y desplegable
develop       ←── integración continua, base para nuevas features
feature/xxx   ←── desarrollo de nuevas funcionalidades
fix/xxx       ←── correcciones de bugs
hotfix/xxx    ←── correcciones urgentes sobre main
release/x.x   ←── preparación de versiones
```

### Reglas de ramas

- `main` está **protegida**: requiere Pull Request + aprobación de al menos 1 revisor.
- `develop` está **protegida**: requiere Pull Request (sin aprobación obligatoria para agilidad).
- Las ramas de trabajo (`feature/`, `fix/`) se crean desde `develop` y se fusionan de vuelta a `develop`.
- Los `hotfix/` se crean desde `main` y se fusionan en `main` **y** `develop`.

---

## Nomenclatura de ramas / Branch Naming

```
feature/nombre-descriptivo-en-kebab-case
fix/descripcion-del-bug
hotfix/descripcion-urgente
release/1.2.0
```

Ejemplos válidos:
- `feature/user-authentication`
- `fix/login-redirect-loop`
- `hotfix/payment-gateway-crash`

---

## Convenciones de commits

Seguimos la especificación **Conventional Commits**:

```
<tipo>(<ámbito opcional>): <descripción corta en minúsculas>

[cuerpo opcional]

[pie opcional: referencias a issues]
```

### Tipos de commit

| Tipo | Uso |
|------|-----|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de bug |
| `docs` | Solo cambios en documentación |
| `style` | Cambios de formato (sin lógica) |
| `refactor` | Refactorización sin cambio funcional |
| `test` | Añadir o corregir tests |
| `chore` | Tareas de mantenimiento (deps, config) |
| `ci` | Cambios en pipelines de CI/CD |

Ejemplos:
```
feat(auth): add OAuth2 login with Google
fix(api): handle null response from payment service
docs: update SSH setup guide
```

---

## Pull Requests

- El título del PR debe seguir el mismo formato que los commits.
- El cuerpo debe describir **qué** se hace y **por qué**.
- Enlaza siempre el issue relacionado con `Closes #123` o `Refs #123`.
- Solicita revisión antes de hacer merge.
- No hacer merge de tu propio PR salvo en casos excepcionales (repositorios personales, hotfixes urgentes).

---

## Versionado / Versioning

Usamos **Semantic Versioning** (`MAJOR.MINOR.PATCH`):

- `MAJOR`: cambios incompatibles con versiones anteriores.
- `MINOR`: nueva funcionalidad compatible hacia atrás.
- `PATCH`: correcciones de bugs compatibles hacia atrás.

Ejemplo: `2.1.4`
