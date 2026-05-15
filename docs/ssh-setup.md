# Configuración de SSH para GitHub / SSH Setup for GitHub

Las claves SSH permiten autenticarte con GitHub sin introducir usuario y contraseña en cada operación. Este método es más seguro y cómodo que HTTPS.

---

## Requisitos previos

- Tener Git instalado ([git-scm.com](https://git-scm.com/downloads))
- Tener una cuenta en GitHub
- Acceso a una terminal:
  - **Windows**: Git Bash, PowerShell o WSL
  - **macOS / Linux**: Terminal

---

## Paso 1 — Comprobar claves SSH existentes

Antes de generar una nueva clave, comprueba si ya tienes una:

```bash
ls -al ~/.ssh
```

Si ves archivos como `id_ed25519` e `id_ed25519.pub` (o `id_rsa` / `id_rsa.pub`), ya tienes un par de claves. Puedes usarlas directamente en el Paso 3.

---

## Paso 2 — Generar una nueva clave SSH

Se recomienda usar el algoritmo **Ed25519** (más moderno y seguro que RSA):

```bash
ssh-keygen -t ed25519 -C "tu_email@ejemplo.com"
```

> Sustituye `tu_email@ejemplo.com` por el email asociado a tu cuenta de GitHub.

Cuando el comando lo pida:

1. **Ubicación del archivo**: pulsa Enter para aceptar la ruta por defecto (`~/.ssh/id_ed25519`).
2. **Passphrase**: introduce una contraseña segura (opcional pero muy recomendable). Se te pedirá cada vez que uses la clave, a menos que uses el agente SSH (ver Paso 4).

---

## Paso 3 — Añadir la clave pública a GitHub

### 3.1 Copiar la clave pública

```bash
# macOS
pbcopy < ~/.ssh/id_ed25519.pub

# Windows (Git Bash o PowerShell)
cat ~/.ssh/id_ed25519.pub | clip

# Linux
xclip -selection clipboard < ~/.ssh/id_ed25519.pub
# o simplemente muestra la clave y cópiala manualmente:
cat ~/.ssh/id_ed25519.pub
```

### 3.2 Añadirla en GitHub

1. Ve a **GitHub → Settings → SSH and GPG keys** ([enlace directo](https://github.com/settings/keys)).
2. Haz clic en **New SSH key**.
3. Pon un título descriptivo (p.ej. `MacBook Pro 2024` o `PC Trabajo`).
4. En **Key type** selecciona `Authentication Key`.
5. Pega la clave copiada en el campo **Key**.
6. Haz clic en **Add SSH key**.

---

## Paso 4 — Añadir la clave al agente SSH

El agente SSH guarda tu clave en memoria para no pedirte la passphrase en cada uso.

```bash
# Iniciar el agente (si no está corriendo)
eval "$(ssh-agent -s)"

# Añadir la clave
ssh-add ~/.ssh/id_ed25519
```

### En Windows (persistencia entre sesiones)

Añade esto a tu `~/.bashrc` o `~/.bash_profile` (si usas Git Bash):

```bash
if [ -z "$SSH_AUTH_SOCK" ]; then
  eval "$(ssh-agent -s)"
  ssh-add ~/.ssh/id_ed25519
fi
```

---

## Paso 5 — Verificar la conexión

```bash
ssh -T git@github.com
```

Si todo está correcto verás:

```
Hi TuUsuario! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## Paso 6 — Clonar con SSH

A partir de ahora, al clonar repositorios usa la URL SSH (no HTTPS):

```bash
# Formato SSH
git clone git@github.com:GrupoCosmosXXI/nombre-del-repo.git

# No usar HTTPS (requiere contraseña)
# git clone https://github.com/GrupoCosmosXXI/nombre-del-repo.git
```

Para cambiar un repositorio ya clonado de HTTPS a SSH:

```bash
git remote set-url origin git@github.com:GrupoCosmosXXI/nombre-del-repo.git
```

Verifica el cambio:

```bash
git remote -v
```

---

## Solución de problemas / Troubleshooting

### "Permission denied (publickey)"

1. Comprueba que el agente SSH tiene tu clave: `ssh-add -l`
2. Si aparece `The agent has no identities`, añádela: `ssh-add ~/.ssh/id_ed25519`
3. Verifica que la clave pública está en tu cuenta de GitHub.

### "Host key verification failed"

Añade GitHub a los hosts conocidos:

```bash
ssh-keyscan github.com >> ~/.ssh/known_hosts
```

### Varias cuentas de GitHub en el mismo equipo

Crea un archivo `~/.ssh/config`:

```
# Cuenta principal (trabajo)
Host github-trabajo
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_trabajo

# Cuenta personal
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal
```

Luego clona usando el alias del host:

```bash
git clone git@github-trabajo:GrupoCosmosXXI/repo.git
git clone git@github-personal:mi-usuario/repo-personal.git
```
