# QHCube — Cómo organizamos el trabajo

Guía de referencia para planificar, desarrollar y publicar QHCube con GitHub:
issues, Project (kanban), milestones, ramas, commits, PRs y releases.

> Este documento vive en el repo `qhcube/.github` (`docs/flujo-de-trabajo.md`).

---

## 0. Vocabulario rápido

| Término | Qué es |
|---|---|
| **Issue** | Una unidad de trabajo: un fallo, una funcionalidad, una tarea. Vive en un repo y tiene número (`#12`). |
| **Tipo de issue** | Clasificación principal: `Bug`, `Feature` o `Task`. Se define a nivel de organización. |
| **Etiqueta (label)** | Marca adicional para filtrar (`security`, `blocked`…). Se define en cada repo. |
| **Sub-issue** | Issue hijo de otro. Sirve para trocear algo grande. |
| **Epic** | Issue padre con sub-issues: una funcionalidad grande o que toca varios repos. |
| **Milestone** | Una meta con nombre. Aquí: **una release** (`v0.2.0`). Agrupa los issues que entran en ella. |
| **Project** | Tablero/tabla que reúne issues de varios repos. Aquí: uno solo, "QHCube". |
| **Kanban** | Forma de ver el trabajo en columnas por estado (por hacer → en curso → hecho). |
| **Backlog** | Lista de cosas pendientes sin fecha comprometida. |
| **PR (pull request)** | Petición para fusionar una rama en otra. Ahí se revisa el cambio. |
| **Release** | Versión publicada: tag de git + notas de la versión en GitHub. |

---

## 1. Repositorios

| Repo | Contenido |
|---|---|
| `qhcube/qhcube-front` | Frontend (Vue + Vite) |
| `qhcube/qhcube-back` | Backend (Spring Boot) |
| `qhcube/qhcube-deploy` | Despliegue: compose.yml, Caddyfile, procedimientos |
| `qhcube/.github` | Plantillas y documentación comunes a toda la organización |

**Regla:** cada issue va al repo al que afecta.

Si algo toca varios repos (p. ej. "mostrar versiones en el footer" → endpoint
en el back + visualización en el front):

- se crea un **issue padre** (epic) en el repo principal;
- cada parte va en un **sub-issue** en su repo;
- el padre se completa cuando se cierran todos los hijos.

Los epics se usan solo para funcionalidades grandes o que cruzan repos, no por
sistema.

---

## 2. El Project "QHCube" (kanban)

Un único Project en la organización `qhcube` que reúne los issues de los
tres repos. Es la vista global del trabajo.

### Columnas (campo `Status`)

| Columna | Significado | Cómo llega ahí |
|---|---|---|
| **Backlog** | Pendiente, sin release asignada | Automático al crear el issue |
| **Por hacer** | Comprometido para una release (tiene milestone) | A mano, al planificar |
| **En curso** | Trabajando en ello (hay rama) | A mano, al empezar |
| **En dev** | PR mergeado en `dev`; aún no publicado | Automático al mergear el PR |
| **Hecho** | Publicado en `main` / producción; issue cerrado | Automático al cerrarse el issue |

Recomendación: **no más de 1–2 issues "En curso" a la vez.**

### Campos adicionales

- **Prioridad**: Alta / Media / Baja.
- (Más adelante, si hace falta: **Tamaño** S / M / L).

### Vistas

- **Tablero**: el kanban del día a día.
- **Por release**: tabla agrupada por milestone. Muestra qué entra en cada
  versión y qué falta.

### Automatismos (se configuran en el Project → Workflows)

- *Auto-add*: todo issue nuevo de los tres repos entra en el Project → Backlog.
- *Pull request merged*: el issue enlazado pasa a **En dev**.
- *Item closed*: pasa a **Hecho**.

---

## 3. Milestones = releases

- Un milestone por versión: `v0.2.0`, `v1.0.0`…
- Los milestones son **de cada repo**, así que se crea el mismo nombre en los
  tres. El Project los agrupa por nombre.
- Front y back llevan **la misma versión**, aunque uno de ellos no tenga cambios.
- Versionado semántico `MAYOR.MENOR.PARCHE`:
  - **PARCHE** (0.1.**2**): solo correcciones.
  - **MENOR** (0.**2**.0): funcionalidades nuevas compatibles.
  - **MAYOR** (**1**.0.0): hito importante o cambios incompatibles.
- Próximas: **`v0.2.0`** (arreglo de deshacer en reproducción + versiones en
  el footer) y **`v1.0.0`**.

Un issue sin milestone está en el backlog. Al asignarle milestone se pasa a
"Por hacer".

---

## 4. Tipos y etiquetas

### Tipo de issue (uno por issue)

| Tipo | Cuándo |
|---|---|
| **Bug** | Algo no funciona como debería |
| **Feature** | Funcionalidad nueva o mejora visible para el usuario |
| **Task** | Trabajo interno: configuración, despliegue, dependencias, documentación técnica… |

### Etiquetas (las mismas en los tres repos; opcionales, cero o varias)

| Etiqueta | Uso |
|---|---|
| `documentation` | Afecta a documentación |
| `refactor` | Reorganizar código sin cambiar comportamiento |
| `security` | Seguridad |
| `regression` | Algo que funcionaba y se ha roto |
| `blocked` | No se puede avanzar (depende de otra cosa; explicar en un comentario) |
| `needs-info` | Falta información para poder hacerlo |
| `duplicate` | Cerrado por repetido (enlazar el original) |
| `wontfix` | Cerrado: no se hará (explicar por qué) |
| `invalid` | Cerrado: no era un problema real |
| `good first issue` | Fácil para alguien nuevo (cuando los repos sean públicos) |
| `help wanted` | Se agradece ayuda externa (ídem) |

La **prioridad** va en el campo del Project, no en etiquetas.

---

## 5. Cómo escribir un issue

### Título

- En castellano. Describe **el problema** (bug) o **el resultado** (feature).
- Sin prefijos tipo `[BUG]`: eso ya lo dice el tipo.
- Ejemplos:
  - *Deshacer movimiento no funciona al reproducir una partida en producción*
  - *Mostrar las versiones de front y back en el pie de página*

### Cuerpo

**Bug:** qué pasa · pasos para reproducir · qué se esperaba · entorno y
versión (front/back) · capturas o logs si los hay.

**Feature:** qué se quiere · para qué · criterios de aceptación (lista
de cosas que deben cumplirse para darlo por terminado).

**Task:** qué hay que hacer · por qué · cómo se comprueba que está hecho.

Las plantillas de `qhcube/.github` rellenan esta estructura automáticamente
al crear un issue desde la web.

Si viene de Jira, añade al final: `Antes: QHCUBE-23`.

---

## 6. Ramas, commits y PRs (enlazar todo)

Flujo de ramas: **feature → `dev` → `main`**.

### Ramas

Nombre: `<nº de issue>-descripcion-corta`

```
14-versiones-footer
12-deshacer-reproduccion
```

Atajo: `gh issue develop 14 --checkout` crea la rama desde el issue y la
deja enlazada.

### Commits

Se mantiene el estilo habitual (en castellano, participio) y se añade la
referencia al issue en una línea aparte:

```
Corregido deshacer al reproducir una partida.

Fixes #12
```

| Palabra | Efecto |
|---|---|
| `Fixes #12` / `Closes #12` / `Resolves #12` | Enlaza **y cierra** el issue cuando el commit llega a `main` |
| `Refs #12` | Solo enlaza, no cierra |
| `qhcube/qhcube-back#5` | Referencia a un issue de otro repo |

- Las palabras clave **deben ir en inglés** (GitHub no reconoce "Corrige #12").
- **Solo cierran al llegar a `main`** (la rama por defecto). Como pasamos por
  `dev`, el issue se cierra **al publicar la release**. Hasta entonces, el
  tablero lo muestra en "En dev". Así "cerrado" significa "en producción".

### Pull requests

1. **Feature → `dev`**: un PR por issue (o por grupo pequeño de issues
   relacionados). En la descripción: qué cambia, `Fixes #12`, cómo probarlo.
2. **`dev` → `main`** (release): un PR que agrupa todo lo de la versión. Al
   mergearlo se cierran los issues y se pasa a crear la release.

---

## 7. Ciclo de vida de un issue (de principio a fin)

```
 1. Se crea el issue                         → Backlog (automático)
 2. Planificación: milestone + prioridad     → Por hacer
 3. gh issue develop N --checkout            → En curso (moverlo a mano)
 4. Commits con "Fixes #N"
 5. PR rama → dev, se mergea                 → En dev (automático)
 6. Release: PR dev → main, se mergea        → issue cerrado → Hecho (automático)
 7. Tag vX.Y.Z + GitHub Release + despliegue (ver qhcube-deploy)
```

---

## 8. Releases

Cuando todos los issues del milestone están en "En dev":

1. Subir la versión en `package.json` / `pom.xml`, en `dev`.
2. PR `dev` → `main` y merge (cierra los issues).
3. Tag `vX.Y.Z` en `main`.
4. **GitHub Release** desde el tag con *Generate release notes*. Las notas
   se generan a partir de los PRs mergeados. Con `.github/release.yml` salen
   agrupadas según las **etiquetas de los PRs** (Seguridad, Regresiones
   corregidas, Documentación, Refactorización y el resto en "Cambios"). El
   tipo de issue (Bug/Feature) no interviene, así que conviene que los
   títulos de los PRs sean descriptivos. Se pueden retocar las notas a mano
   antes de publicar.
5. Cerrar el milestone.
6. Desplegar según el README de `qhcube-deploy`.

---

## 9. Plantillas comunes (repo `qhcube/.github`)

Un repo llamado `.github` en la organización aplica sus ficheros por defecto
a todos los repos que no tengan los suyos propios:

```
.github/                          (en local: qhcube-dotgithub/)
├── ISSUE_TEMPLATE/
│   ├── bug.yml                   # formulario de bug (tipo Bug)
│   ├── feature.yml               # formulario de funcionalidad (tipo Feature)
│   ├── task.yml                  # formulario de tarea (tipo Task)
│   └── config.yml                # permite también issues en blanco
├── PULL_REQUEST_TEMPLATE.md
├── docs/
│   └── flujo-de-trabajo.md       # este documento
└── para-cada-repo/
    └── .github/release.yml       # NO se hereda: copiar a cada repo
```

Lo que **no** se hereda desde `qhcube/.github` y vive en cada repo:
etiquetas, milestones y `.github/release.yml`. Las etiquetas por defecto
para repos nuevos se definen en Settings de la organización → Repository
defaults → Repository labels.

(Cuando los repos sean públicos: `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`,
`SECURITY.md`).

---

## 10. Chuleta de comandos `gh`

```bash
# Issues
gh issue list -R qhcube/qhcube-front                  # abiertos
gh issue list -R qhcube/qhcube-front --milestone v0.2.0
gh issue view 12 -R qhcube/qhcube-front
gh issue create -R qhcube/qhcube-front                # interactivo
gh issue comment 12 -R qhcube/qhcube-front -b "texto"
gh issue edit 12 -R qhcube/qhcube-front --milestone v0.2.0 --add-label regression
gh issue close 12 -R qhcube/qhcube-front -r "not planned" -c "motivo"

# Trabajo (dentro del repo local)
gh issue develop 12 --checkout                        # rama enlazada al issue
gh pr create --base dev --fill                        # PR hacia dev
gh pr list / gh pr view / gh pr merge

# Releases
gh release create v0.2.0 --generate-notes --target main
```

Dentro del directorio de un repo, `-R ...` se puede omitir.

---

## 11. Estado del montaje

- [x] Tipos de issue activados en la organización (Settings → Issue types)
- [x] Permiso `project` para gh: `gh auth refresh -h github.com -s project`
- [x] Etiquetas en los tres repos
- [ ] Etiquetas por defecto de la organización (para repos futuros)
- [ ] Milestones `v0.2.0` y `v1.0.0` en los tres repos
- [ ] Project "QHCube": columnas, campo Prioridad, vistas, automatismos
- [x] Repo `qhcube/.github` con plantillas, `release.yml` y este documento
- [ ] Copiar `release.yml` a front, back y deploy
- [ ] Primeros issues de `v0.2.0`
