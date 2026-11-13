# Guía de colaboración con Git y GitHub

> **Objetivo:** que todos los colaboradores puedan descargar el proyecto, trabajar en una rama propia y proponer cambios al repositorio principal sin sobrescribir el trabajo de otros.

## 1. Cómo trabajaremos

- **Repositorio principal:** proyecto compartido alojado en GitHub.
- **`main`:** rama estable. **No se trabaja ni se sube código directamente a `main`.**
- **Ramas de trabajo:** cada tarea se desarrolla en una rama independiente.
- **Pull Request (PR):** solicitud para revisar e incorporar una rama a `main`.
- **Responsable de revisión:** persona con permisos de revisión y fusión en el repositorio.

**Flujo habitual:** clonar → actualizar `main` → crear rama → programar → commit → push → Pull Request → revisión → merge.

## 2. Preparación inicial (solo una vez)

1. Instala Git desde <https://git-scm.com/downloads>.
2. Crea una cuenta en <https://github.com/>.
3. Solicita al administrador acceso al repositorio si es privado. Acepta la invitación de GitHub.
4. Abre Git Bash, PowerShell o una terminal y configura tu identidad:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu-correo@example.com"
```

Comprueba que Git funciona:

```bash
git --version
```

> **Autenticación:** al trabajar por HTTPS, GitHub puede pedirte iniciar sesión mediante el navegador o usar un token personal; **la contraseña normal de GitHub no sirve para operaciones Git por HTTPS**. También puedes usar SSH si ya lo tienes configurado. Nunca compartas tokens ni claves.

## 3. Clonar el repositorio principal

En GitHub, abre el repositorio → **Code** → copia la URL **HTTPS**. En la terminal:

```bash
git clone https://github.com/ORGANIZACION/REPOSITORIO.git
cd REPOSITORIO
```

Sustituye la URL y el nombre por los reales. `clone` descarga los archivos y el historial del proyecto. Solo necesitas clonar una vez en cada equipo.

Comprueba dónde estás:

```bash
git status
git branch
```

## 4. Actualizar la rama principal antes de empezar

Antes de crear una nueva rama, asegúrate de no tener cambios locales sin guardar:

```bash
git status
git switch main
git pull --ff-only origin main
```

`pull` descarga e incorpora los últimos cambios de GitHub. La opción `--ff-only` evita crear una fusión inesperada; si falla, pide ayuda antes de continuar.

## 5. Crear una rama para tu tarea

Crea la rama desde `main` actualizada:

```bash
git switch -c feature/pagina-login
```

Ejemplos de nombres:

- `feature/pagina-login`: nueva funcionalidad.
- `fix/error-formulario`: corrección de un fallo.
- `docs/actualizar-readme`: documentación.

Consulta tu rama actual:

```bash
git branch
```

La rama marcada con `*` es la activa. **Comprueba que no estás en `main` antes de modificar o subir código.**

## 6. Guardar los cambios: add y commit

Trabaja con tu editor habitual. Cuando termines un cambio coherente:

```bash
git status
git diff
git add ruta/del/archivo
git commit -m "feat: añadir formulario de inicio de sesión"
```

- `status`: muestra los archivos modificados.
- `diff`: permite revisar los cambios todavía no preparados.
- `add`: selecciona qué cambios incluir en el commit.
- `commit`: guarda una versión en tu repositorio local.

Si has revisado **todos** los archivos y quieres incluirlos:

```bash
git add .
git commit -m "fix: corregir validación del formulario"
```

> **Importante:** `git add .` puede incluir archivos no deseados. No subas contraseñas, tokens, archivos `.env`, dependencias generadas ni archivos personales. Configura correctamente `.gitignore`.

## 7. Subir tu rama a GitHub

La primera vez que publiques esa rama:

```bash
git push -u origin feature/pagina-login
```

Después, mientras trabajes en esa misma rama:

```bash
git push
```

**`push` no incorpora automáticamente tus cambios a `main`:** publica los commits en tu rama remota.

## 8. Crear un Pull Request hacia main

1. Abre el repositorio en GitHub.
2. Pulsa **Compare & pull request** (si aparece) o **Pull requests → New pull request**.
3. Selecciona **base: `main`** y **compare: tu rama**.
4. Escribe un título claro y explica qué has cambiado, cómo probarlo y si hay algo pendiente.
5. Pulsa **Create pull request** y solicita revisión.
6. Si te piden correcciones, hazlas en **la misma rama**, crea otro commit y ejecuta `git push`: el PR se actualizará automáticamente.
7. Una persona autorizada revisará y fusionará el PR cuando corresponda.

### Plantilla breve de Pull Request

```markdown
## ¿Qué cambia?
- 

## ¿Cómo se ha probado?
- 

## Capturas (si procede)

## Pendiente / observaciones
- 
```

## 9. Mantener tu rama actualizada

Si otras personas han incorporado cambios a `main` mientras trabajabas:

```bash
git status
git fetch origin
git merge origin/main
```

Ejecuta estos comandos **desde tu rama de trabajo**, no desde `main`. Si aparecen conflictos, resuélvelos siguiendo el apartado siguiente. Después prueba el proyecto y sube el resultado:

```bash
git push
```

> Si tienes cambios sin confirmar, guárdalos primero en un commit antes de fusionar para evitar complicaciones.

## 10. Resolver conflictos

Un conflicto puede aparecer cuando dos personas editan las mismas líneas. Git puede mostrar algo parecido a esto:

```text
<<<<<<< HEAD
Texto de tu rama
=======
Texto que llega de main
>>>>>>> origin/main
```

1. Abre el archivo señalado y decide qué contenido debe quedar (puedes combinar ambos).
2. Elimina los marcadores `<<<<<<<`, `=======` y `>>>>>>>`.
3. Guarda el archivo y prueba que el proyecto funciona.
4. Marca el conflicto como resuelto y completa la fusión:

```bash
git add ruta/del/archivo
git commit
git push
```

En algunas configuraciones Git abre un editor para confirmar el mensaje del commit de fusión. **No borres código ajeno sin consultarlo.** Si no tienes claro qué conservar, pide ayuda.

## 11. Después de que aprueben tu Pull Request

Cuando el PR ya esté fusionado:

```bash
git switch main
git pull --ff-only origin main
git branch -d feature/pagina-login
```

`-d` borra **solo la rama local** si Git considera que está fusionada. Puedes eliminar la rama remota desde GitHub cuando ya no haga falta. Para la siguiente tarea, crea una rama nueva desde `main` actualizada.

## 12. Problemas habituales

| Problema | Causa posible | Qué hacer |
|---|---|---|
| `Permission denied` o error 403 | Sin permisos o autenticación incorrecta | Acepta la invitación y revisa el acceso a GitHub. |
| `fatal: not a git repository` | Estás fuera de la carpeta del proyecto | Entra con `cd REPOSITORIO`. |
| `nothing to commit` | No hay cambios nuevos preparados | Ejecuta `git status` y revisa los archivos. |
| `rejected (non-fast-forward)` | La rama remota tiene cambios que no tienes | Ejecuta `git fetch`, revisa la situación y coordina la integración; no fuerces el push. |
| Conflictos al fusionar | Se modificaron líneas incompatibles | Sigue el apartado 10. |
| Has editado `main` por error | No creaste tu rama a tiempo | Si aún no has hecho commit, usa `git switch -c nombre-rama` para conservar tus cambios en una rama nueva. |

**Nunca ejecutes `git push --force` sobre ramas compartidas ni `git reset --hard` sin saber qué cambios perderás.**

## 13. Reglas de equipo recomendadas

1. Cada tarea o incidencia debe tener su propia rama.
2. Nadie sube cambios directamente a `main`.
3. Todo cambio entra mediante Pull Request revisado.
4. Los commits deben ser pequeños y tener mensajes descriptivos.
5. Antes de empezar una tarea nueva, actualiza `main`.
6. Antes de abrir un PR, ejecuta las pruebas disponibles y revisa los archivos cambiados.
7. No subas credenciales, datos privados, binarios innecesarios ni carpetas generadas.
8. Comunica los cambios que afecten a archivos compartidos o a la estructura del proyecto.
9. Si existe un conflicto que no entiendes, consulta antes de resolverlo.
10. El administrador puede proteger `main` desde **Settings → Branches / Rulesets**, exigiendo PR y revisiones según las opciones disponibles.

## 14. Ejemplo completo de principio a fin

Supongamos que quieres añadir un menú de navegación:

```bash
# Primera vez en este ordenador:
git clone https://github.com/ORGANIZACION/REPOSITORIO.git
cd REPOSITORIO

# Antes de empezar una nueva tarea:
git switch main
git pull --ff-only origin main
git switch -c feature/menu-navegacion

# Edita los archivos y comprueba tus cambios:
git status
git diff

# Guarda y publica el trabajo:
git add .
git commit -m "feat: crear menú de navegación"
git push -u origin feature/menu-navegacion
```

Ahora abre GitHub y crea un **Pull Request de `feature/menu-navegacion` hacia `main`**. Espera a que lo revisen y fusionen.

## 15. Chuleta rápida

| Acción | Comando |
|---|---|
| Clonar | `git clone URL` |
| Ver estado | `git status` |
| Ver rama actual | `git branch` |
| Cambiar a main | `git switch main` |
| Actualizar main | `git pull --ff-only origin main` |
| Crear rama | `git switch -c feature/nombre` |
| Revisar cambios | `git diff` |
| Preparar archivos | `git add archivo` |
| Crear commit | `git commit -m "mensaje"` |
| Subir rama por primera vez | `git push -u origin nombre-rama` |
| Subir siguientes commits | `git push` |
| Descargar referencias remotas | `git fetch origin` |
| Integrar main en tu rama | `git merge origin/main` |

---

**Datos del proyecto (rellenar por el administrador)**

- Repositorio principal: `https://github.com/ORGANIZACION/REPOSITORIO`
- Rama principal: `main`
- Responsable de revisión: `Por definir`
- Canal para dudas y conflictos: `Por definir`

*Esta guía describe el flujo de colaboración recomendado para el equipo. Si el repositorio usa otras ramas, comprobaciones automáticas o normas específicas, deben prevalecer las instrucciones del proyecto.*
