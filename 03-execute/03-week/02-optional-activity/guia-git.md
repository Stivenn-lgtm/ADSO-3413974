# Guía de Comandos Git

> Control de versiones distribuido para proyectos de cualquier tamaño. Esta guía cubre desde la configuración inicial hasta técnicas avanzadas de trabajo en equipo.

---

## Configuración Inicial

Lo primero antes de empezar a trabajar con Git es decirle quién eres:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
git config --global core.editor "code --wait"
git config --global init.defaultBranch main
git config --list
```

Para ver la configuración de un solo parámetro:

```bash
git config user.name
```

---

## Iniciar o Clonar un Proyecto

```bash
git init                          # Convierte la carpeta actual en un repo
git init mi-proyecto              # Crea carpeta nueva y la inicializa

git clone <url>                   # Descarga un repo remoto completo
git clone <url> nombre            # Lo descarga con otro nombre de carpeta
git clone --depth 1 <url>         # Clona solo el último commit (más rápido)
```

---

## Ciclo Básico de Trabajo

```bash
git status                        # ¿Qué está pasando en el repo?
git status -s                     # Versión resumida

git add archivo.txt               # Enviar un archivo al área de preparación
git add .                         # Enviar todos los cambios
git add src/                      # Enviar solo una carpeta
git add -p                        # Elegir qué partes de cada archivo agregar

git commit -m "descripción clara" # Guardar los cambios con un mensaje
git commit -am "descripción"      # Agregar y commitear en un solo paso
git commit --amend --no-edit      # Añadir algo al último commit sin cambiar el mensaje
```

### Buenas prácticas para mensajes de commit

Un buen mensaje explica el **qué** y el **por qué**, no el cómo:

```
feat: permitir login con Google
fix: corregir error al cargar imágenes en Safari
docs: actualizar instrucciones de instalación
refactor: simplificar lógica de validación de formularios
chore: actualizar dependencias a versiones estables
```

---

## Ver el Historial

```bash
git log                           # Historial completo con detalles
git log --oneline                 # Un commit por línea
git log --oneline --graph --all   # Árbol visual de todas las ramas
git log -10                       # Solo los últimos 10 commits
git log --author="Ana"            # Commits de una persona específica
git log --since="1 week ago"      # Commits de la última semana
git log --until="2024-12-31"      # Commits hasta cierta fecha
git log -- src/index.js           # Historial de un archivo específico

git show <hash>                   # Ver qué cambió en un commit puntual
git diff                          # Cambios actuales no preparados
git diff --staged                 # Lo que está listo para commitear
git diff main..feature/login      # Comparar dos ramas
```

---

## Trabajar con Ramas

Las ramas permiten desarrollar funcionalidades sin afectar el código principal.

```bash
git branch                        # Ver ramas locales
git branch -a                     # Ver ramas locales y remotas
git branch nueva-rama             # Crear una rama
git branch -d rama                # Borrar rama (solo si ya fue mergeada)
git branch -D rama                # Borrar rama a la fuerza
git branch -m nombre-viejo nombre-nuevo  # Renombrar rama

git switch nombre-rama            # Moverse a una rama
git switch -c nombre-rama         # Crear rama y moverse a ella
```

---

## Fusionar Ramas

### Merge

Combina dos ramas creando un commit de fusión. Ideal para trabajo en equipo.

```bash
git merge nombre-rama             # Fusionar rama al branch actual
git merge --no-ff nombre-rama     # Siempre crear commit de merge
git merge --squash nombre-rama    # Comprimir todos los commits en uno solo
git merge --abort                 # Cancelar si hay conflictos
```

### Rebase

Reescribe el historial como si los commits se hubieran hecho encima de otra base. Ideal para limpiar antes de hacer merge.

```bash
git rebase main                   # Reubicar el branch actual sobre main
git rebase -i HEAD~4              # Editar, comprimir o reordenar últimos 4 commits
git rebase --continue             # Seguir después de resolver un conflicto
git rebase --abort                # Cancelar y volver al estado anterior
```

**Cuando usar cada uno:** merge para ramas compartidas con el equipo, rebase para ordenar tu historial local antes de publicarlo.

---

## Repositorios Remotos

```bash
git remote -v                          # Ver remotos configurados
git remote add origin <url>            # Conectar con un repositorio remoto
git remote set-url origin <nueva-url>  # Cambiar la URL del remoto
git remote remove origin               # Desconectar el remoto

git fetch origin                       # Descargar cambios sin aplicarlos
git fetch --all                        # Traer cambios de todos los remotos
git pull origin main                   # Traer y aplicar cambios de main
git pull --rebase origin main          # Traer y aplicar con rebase

git push origin main                   # Subir la rama main al remoto
git push -u origin nombre-rama         # Subir rama y configurar seguimiento
git push origin --delete rama          # Borrar una rama en el remoto
git push --force-with-lease            # Forzar push sin sobreescribir trabajo ajeno
```

---

## Etiquetas (Tags)

Se usan para marcar versiones o lanzamientos importantes.

```bash
git tag                               # Ver todas las etiquetas
git tag v2.0.0                        # Crear etiqueta simple
git tag -a v2.0.0 -m "Versión 2.0"   # Crear etiqueta con descripción
git tag -a v1.5.0 <hash>              # Etiquetar un commit anterior
git push origin v2.0.0               # Publicar etiqueta
git push origin --tags               # Publicar todas las etiquetas
git tag -d v2.0.0                    # Borrar etiqueta local
git push origin --delete v2.0.0     # Borrar etiqueta del remoto
```

---

## Guardar Cambios Temporalmente (Stash)

Útil cuando necesitas cambiar de rama pero no quieres perder lo que estás haciendo.

```bash
git stash                             # Guardar cambios en progreso
git stash push -m "trabajo login"     # Guardar con un nombre descriptivo
git stash list                        # Ver todos los stashes guardados
git stash pop                         # Recuperar el último y borrarlo
git stash apply stash@{1}             # Aplicar uno específico sin borrarlo
git stash drop stash@{0}              # Borrar un stash puntual
git stash clear                       # Borrar todos los stashes
git stash branch hotfix stash@{0}     # Crear una rama a partir de un stash
```

---

## Deshacer Cambios

```bash
# Descartar cambios en un archivo (sin tocar el staging)
git restore archivo.txt

# Sacar un archivo del staging sin perder los cambios
git restore --staged archivo.txt

# Deshacer el último commit pero conservar los cambios
git reset --soft HEAD~1

# Deshacer el último commit y sacar los cambios del staging
git reset --mixed HEAD~1

# Eliminar el último commit y todos sus cambios permanentemente
git reset --hard HEAD~1

# Crear un commit que deshace otro (sin reescribir historial)
git revert <hash>

# Borrar archivos nuevos que no están rastreados
git clean -fd
```

---

## Cherry-pick

Permite copiar commits individuales de una rama a otra sin hacer un merge completo.

```bash
git cherry-pick <hash>             # Traer un commit específico
git cherry-pick <hash1> <hash2>    # Traer varios commits
git cherry-pick <hashA>..<hashB>   # Traer un rango de commits
git cherry-pick --no-commit <hash> # Aplicar cambios sin commitear todavía
git cherry-pick --abort            # Cancelar la operación
```

---

## Buscar en el Historial

```bash
git grep "texto buscado"          # Buscar texto en todos los archivos rastreados
git log -S "nombre_funcion"       # Ver cuándo apareció o desapareció algo
git blame archivo.js              # Ver quién escribió cada línea
git bisect start                  # Iniciar búsqueda binaria de un bug
git bisect bad                    # El commit actual tiene el problema
git bisect good <hash>            # Este commit funcionaba bien
git bisect reset                  # Finalizar la búsqueda
```

---

## Alias para Trabajar Más Rápido

```bash
git config --global alias.s "status -s"
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.undo "reset --soft HEAD~1"
git config --global alias.unstage "restore --staged"
git config --global alias.last "log -1 HEAD --stat"
git config --global alias.aliases "config --get-regexp alias"
```

Ejemplo de uso: en lugar de escribir `git log --oneline --graph --all --decorate` simplemente escribes `git lg`.

---

## Flujo de Trabajo en Equipo

```
main        ──●────────────────────────────●──  producción estable
              │                            │
develop     ──●──────●──────●──────────────●──  integración continua
                     │      │
feature/pago   ──────●      │
feature/perfil          ────●
```

```bash
# Paso 1 — Crear tu rama desde develop
git switch develop
git pull origin develop
git switch -c feature/nombre-funcionalidad

# Paso 2 — Desarrollar y commitear
git add .
git commit -m "feat: descripción de lo que hiciste"

# Paso 3 — Actualizar con los cambios del equipo
git fetch origin
git rebase origin/develop

# Paso 4 — Subir la rama y abrir Pull Request
git push -u origin feature/nombre-funcionalidad

# Paso 5 — Después del merge, limpiar
git switch develop
git pull origin develop
git branch -d feature/nombre-funcionalidad
```

---

## Referencia de Emergencia

| Problema | Solución |
|---|---|
| Hice commit en la rama equivocada | `git cherry-pick <hash>` en la rama correcta + `git reset --hard HEAD~1` en la incorrecta |
| Subí un archivo que no debía | `git rm --cached archivo` + commit + push |
| Necesito cambiar de rama con trabajo a medias | `git stash` → cambiar rama → `git stash pop` |
| Perdí commits con `reset --hard` | `git reflog` para encontrar el hash + `git reset --hard <hash>` |
| El merge tiene conflictos imposibles | `git merge --abort` y hablar con el equipo |
| Quiero deshacer algo ya publicado | `git revert <hash>` (nunca `reset` en ramas compartidas) |

---

## Ciclo de Vida de un Archivo

```
Sin rastrear  →  git add  →  Preparado  →  git commit  →  Confirmado
                                                               │
                         git restore --staged  ◄──────────────┘
     git restore  ◄────────────────────────────────────────────┘
```

---

> Documentación oficial: [git-scm.com/doc](https://git-scm.com/doc)
> Práctica interactiva: [learngitbranching.js.org](https://learngitbranching.js.org)
