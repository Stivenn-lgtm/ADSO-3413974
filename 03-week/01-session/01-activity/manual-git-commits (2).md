# Manual de Git: Realizar commits desde la terminal

## Introducción

Git guarda los cambios de un proyecto mediante commits. Cada commit es una instantánea del código en un momento determinado, con un mensaje que describe qué se modificó y por qué. Este manual explica cómo crear commits correctamente desde la terminal.

---

## Configuración inicial

La primera vez que se usa Git en un equipo, hay que registrar el nombre y el correo del autor. Estos datos aparecen en cada commit que se cree.

```bash
git config --global user.name "Nombre Apellido"
git config --global user.email "correo@ejemplo.com"
```

Para confirmar que quedó guardado:

```bash
git config --list
```

---

## Inicializar un repositorio

Si el proyecto aún no tiene Git configurado, se inicializa con:

```bash
git init
```

Si se trabaja con un repositorio existente, se clona así:

```bash
git clone https://github.com/usuario/repositorio.git
```

---

## El área de staging

Antes de hacer un commit, Git necesita saber qué cambios incluir. Eso se gestiona con el área de staging. Los archivos no se guardan solos: hay que agregarlos explícitamente.

Ver el estado actual del repositorio:

```bash
git status
```

La salida muestra tres grupos posibles:

- **Changes to be committed** — archivos listos para el commit
- **Changes not staged for commit** — archivos modificados pero no agregados
- **Untracked files** — archivos nuevos que Git aún no rastrea

---

## Agregar archivos al staging

```bash
# Un archivo específico
git add archivo.txt

# Varios archivos a la vez
git add archivo1.txt archivo2.txt

# Todo el directorio actual
git add .

# Solo archivos de un tipo
git add *.js

# Revisión interactiva de cambios antes de agregar
git add -p
```

La opción `-p` es útil cuando se quiere elegir qué partes de un archivo incluir, sin agregar todo de golpe.

---

## Crear el commit

Una vez que los archivos están en staging, se confirman con:

```bash
git commit -m "descripción del cambio"
```

Si el mensaje necesita más detalle, se puede escribir un cuerpo:

```bash
git commit -m "título del cambio" -m "Explicación más larga de qué se hizo y por qué."
```

También se puede abrir el editor de texto del sistema para escribir el mensaje:

```bash
git commit
```

Git abre el editor configurado (por defecto, Vim o Nano). Se escribe el mensaje, se guarda y se cierra.

---

## Ver el historial de commits

```bash
# Historial completo
git log

# Formato compacto, una línea por commit
git log --oneline

# Con gráfico de ramas
git log --oneline --graph
```

Cada commit tiene un hash único, por ejemplo `a3f9d21`. Ese identificador se usa para referenciar commits en otros comandos.

---

## Revisar cambios antes de commitear

Antes de agregar archivos al staging, conviene revisar qué cambió:

```bash
# Cambios en archivos no agregados al staging
git diff

# Cambios ya en staging (los que se incluirán en el commit)
git diff --staged
```

---

## Mensajes de commit

El mensaje es parte del commit y queda en el historial para siempre. Un mensaje claro ahorra tiempo a cualquiera que tenga que entender qué pasó en ese punto del proyecto.

**Formato recomendado:**

```
tipo: descripción breve en imperativo

Cuerpo opcional con más contexto. Explicar qué cambió
y por qué, no cómo (eso está en el código).
```

**Tipos comunes:**

| Prefijo | Cuándo usarlo |
|---------|---------------|
| `feat` | Se agrega una funcionalidad nueva |
| `fix` | Se corrige un error |
| `docs` | Cambios solo en documentación |
| `style` | Formato, espaciado, sin cambios de lógica |
| `refactor` | Reestructuración sin cambiar comportamiento |
| `test` | Se agregan o modifican pruebas |
| `chore` | Tareas de mantenimiento, dependencias |

**Ejemplos:**

```bash
git commit -m "feat: agregar filtro de búsqueda por fecha"
git commit -m "fix: corregir error al guardar formulario vacío"
git commit -m "docs: agregar sección de instalación al README"
git commit -m "refactor: separar lógica de validación en módulo propio"
```

**Lo que hay que evitar:**

```bash
git commit -m "cambios"
git commit -m "arreglé cosas"
git commit -m "fix2"
git commit -m "no sé"
```

---

## Modificar el último commit

Si se olvidó agregar un archivo o el mensaje tiene un error, se puede corregir antes de hacer push:

```bash
# Cambiar el mensaje del último commit
git commit --amend -m "mensaje corregido"

# Agregar un archivo olvidado sin cambiar el mensaje
git add archivo-olvidado.txt
git commit --amend --no-edit
```

`--amend` reescribe el último commit. No usarlo si ya se hizo push, ya que modifica el historial.

---

## Deshacer commits

### Quitar archivos del staging sin perder cambios

```bash
git restore --staged archivo.txt
```

### Deshacer el último commit pero conservar los cambios en staging

```bash
git reset --soft HEAD~1
```

### Deshacer el último commit y dejar los cambios sin staging

```bash
git reset HEAD~1
```

### Deshacer el último commit y borrar todos los cambios

```bash
git reset --hard HEAD~1
```

Este último es destructivo. Los cambios no se pueden recuperar fácilmente.

### Revertir un commit sin modificar el historial

```bash
git revert <hash-del-commit>
```

Crea un commit nuevo que deshace los cambios del commit indicado. Es la opción segura cuando ya se hizo push.

---

## Atajo: commit sin `git add`

Para archivos que Git ya rastrea, se puede omitir el `git add`:

```bash
git commit -am "fix: corregir validación de email"
```

La flag `-a` agrega automáticamente todos los archivos modificados que ya están siendo rastreados. Los archivos nuevos (untracked) no se incluyen.

---

## Referencia de comandos

```bash
git status                        # Ver estado del repositorio
git add .                         # Agregar todos los cambios al staging
git add archivo.txt               # Agregar un archivo específico
git add -p                        # Agregar cambios de forma interactiva
git diff --staged                 # Ver qué se va a commitear
git commit -m "mensaje"           # Crear un commit
git commit --amend -m "mensaje"   # Modificar el último commit
git log --oneline                 # Ver historial resumido
git reset --soft HEAD~1           # Deshacer último commit (conserva cambios)
git reset --hard HEAD~1           # Deshacer último commit (borra cambios)
git revert <hash>                 # Revertir un commit sin borrar historial
```
