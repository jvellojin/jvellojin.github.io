# Migración a al-folio v1.0

> Rama de trabajo: `migracion-v1`. **No trabajar sobre `master`.**
> Los pasos con `bundle`/build se corren en **tu terminal local** (el sandbox no valida builds).

## 1. Diagnóstico de tu sitio

- **Versión actual:** al-folio v0.x (no tiene `api_version: 1`).
- **Tu fork es una mezcla**, no una base limpia: el núcleo es antiguo
  (contra `v0.9.0` solo difieren 6 archivos), pero tiene parches sueltos
  copiados de versiones posteriores (`_includes/social.liquid`,
  `_plugins/cache-bust.rb`). Ningún tag stock v0.x calza limpio.
- **Runtime local arrastrado:** 98 archivos — `_includes` (54),
  `_layouts` (12), `_sass` (25), `_plugins` (7). En v1 este runtime deja de
  vivir en el repo y pasa a los gems `al_folio_*`.

### Qué cambia en v1 (breaking)
1. **El tema es un gem.** `theme: al_folio_core` + familia de gems `al_*`.
   `_includes`, `_layouts`, `_sass`, `_plugins` ya no son tuyos salvo overrides
   explícitos.
2. **Motor de CSS = Tailwind 4.** `style_engine: tailwind`. Tus 25 `_sass`
   quedan mayormente obsoletos. Existe puente `al_folio_bootstrap_compat`,
   pero **se depreca en v1.3 y se elimina en v2.0** — no es solución permanente.
3. **Gemfile nuevo.** Bloque `al_folio_plugins` (~16 gems con versión fija) y
   renombres: `jekyll-archives` → `jekyll-archives-v2`; nuevos `jekyll-socials`,
   `jekyll-tabs`, `jekyll-cache-bust`, `jekyll-3rd-party-libraries`,
   `jekyll-terser` (fork git).
4. **_config.yml** necesita el bloque `al_folio:` (api_version, tailwind,
   distill, features, compat) + `theme:` + lista de plugins nueva.

## 2. Contenido REAL vs. demo (hallazgo clave)

La mayoría de lo que parece "contenido" es **demo del tema**, no tuyo. El
payload real a migrar es pequeño y **compatible de formato con v1** (verificado:
`_projects` y `papers.bib` usan el mismo esquema en v1).

### Tuyo real → migrar (copia limpia)
| Origen                         | Qué es                                             |
|--------------------------------|----------------------------------------------------|
| `_bibliography/papers.bib`     | **17 publicaciones reales** (Vellojin et al.)      |
| `assets/img/` (reales)         | prof_pic, prof_pic_color, ANID_postdoc_*, Dsalt_project, flow-transport_* |
| `_projects/1_project.md`       | Dsalt Anillo ACT210087 (2022-2023)                 |
| `_projects/2_project.md`       | ANID postdoc 3230302 (2023-2026)                   |
| `_pages/about.md` (`more_info`)| Contacto: GIMNAP, Depto. Matemática, UBB, Concepción |
| `_config.yml` (valores)        | Nombre Jesus Vellojin, `scholar_userid: yxYU88oAAAAJ`, keywords |

### Demo / placeholder → NO migrar (dejar el de v1 o rehacer)
| Origen                   | Por qué se descarta                          |
|--------------------------|----------------------------------------------|
| `_data/cv.yml`           | Es Albert Einstein. Tu CV **nunca se llenó** — se rellena el esquema nuevo de v1 desde cero |
| `_data/coauthors.yml`    | Adams, Podolsky, Rosen, Bach (demo)          |
| `_data/venues.yml`       | AJP, PhysRev (demo)                          |
| `_data/repositories.yml` | torvalds, bootstrap, jquery (demo)           |
| `_posts/` (26)           | Posts de demostración del tema               |
| `_news/` (3)             | announcement_1/2/3 (demo)                    |

> **Consecuencia:** no hay conversión laboriosa de CV que hacer — la tabla que
> preocupaba está en blanco (Einstein). Se llena el `cv:` de v1 cuando quieras,
> apoyándote en tu `papers.bib` real.

## 3. Overrides: por qué la auditoría se corre con el gem

Clasificar los 98 archivos de runtime en "override intencional" vs "copia vieja
del tema" **no se puede hacer de forma confiable a mano** porque tu base es una
mezcla de versiones. La herramienta oficial lo hace por checksum contra los
archivos reales de los plugins:

```bash
bundle exec al-folio upgrade audit --no-fail
bundle exec al-folio upgrade overrides audit
```

Por cada archivo marcado como stale o sin reconocer:

```bash
bundle exec al-folio upgrade overrides diff  RUTA_LOCAL   # ver la diferencia
bundle exec al-folio upgrade overrides accept RUTA_LOCAL  # conservar como override
```

Regla práctica: si `diff` muestra solo diferencias cosméticas contra el tema →
borra el archivo local (deja que lo aporte el gem). Si muestra lógica/estilo
que tú añadiste a propósito → `accept` para registrarlo como override en
`.al-folio-overrides.yml` (commitea ese archivo).

Candidatos a override real (tus adiciones, no stock v0.16.3): revisar con
prioridad `_includes/social.liquid`, `_plugins/cache-bust.rb`, y cualquier
edición en `_sass/_variables.scss` / `_sass/_themes.scss` (colores/marca).
Bajo Tailwind, los overrides de `_sass` probablemente haya que **rehacerlos**,
no portarlos.

## 4. Runbook (en tu terminal local)

```bash
# 0. Limpia locks si git se queja (los dejaste atascados antes)
cd /ruta/a/jvellojin.github.io
rm -f .git/index.lock .git/objects/maintenance.lock

# 1. Trabaja en la rama de migración
git checkout migracion-v1

# 2. Trae el starter v1 a un lado para copiar su contrato
git clone --depth 1 https://github.com/alshedivat/al-folio.git /tmp/afv1

# 3. Reemplaza el esqueleto por el de v1
cp /tmp/afv1/Gemfile ./Gemfile
#   Fusiona a mano en _config.yml: el bloque `al_folio:` (api_version, tailwind,
#   distill, features, compat), `theme: al_folio_core` y la lista `plugins:`
#   nueva. Conserva TUS valores (nombre, scholar, textos, social).

# 4. Instala y audita
bundle install
bundle exec al-folio upgrade audit --no-fail
bundle exec al-folio upgrade overrides audit

# 5. Resuelve overrides uno por uno (ver sección 3)

# 6. Build y revisión visual
bundle exec jekyll serve
#   Revisa: home, CV, publications, repositories, projects, posts y rutas custom.

# 7. Commit
git add -A && git commit -m "Migrar a al-folio v1.0"
#   Cuando estés conforme: merge a master.
```

## 5. Riesgos / decisiones abiertas

- **Tailwind vs tus estilos:** el mayor riesgo visual. Presupuesta tiempo para
  rehacer personalizaciones de color/tipografía como config de Tailwind, no como
  SCSS portado.
- **`bootstrap_compat` es temporal:** si lo activas para migrar rápido, deja
  registrado que hay deuda técnica (se elimina en v2.0).
- **Validación:** el sandbox no corre el build. Los pasos 4–6 son tuyos.
