# Plan técnico: Plan in-Flaco

Ver [spec.md](spec.md) para requisitos y criterios de aceptación.

## 1. Cambios en el repo

| Archivo | Cambio |
|---|---|
| `flaco/index.html` | Nuevo. Copia recortada de `Plan_Entrenamiento_UL_6_Semanas.html` |
| `.github/workflows/deploy.yml` | Añadir `cp -R flaco dist/flaco` en el paso de staging |
| `specs/flaco/*` | Spec, plan y tareas (no se publican) |

Sin cambios en la raíz, `firegym/` ni `README.md`.

## 2. Qué se quita de la copia

- Tabs y panel `#tab-nutricion` completo, más su CSS (`.nutri-*`, `.meal-*`, `.macros-*`, `.goal-table`, `.foods-*`, `.day-mode-toggle`).
- JS: `initTabs`, `initNutritionToggle`, `initMealCollapsibles`, `renderNutritionProgress`, `NUTRI_TARGET_WEEKS`, `MATCH_*`, `setMatchMode`, `buildAltRowsHtml`, `storageKey` de modo partido.
- Supersets: filas `ss-start` / `ss-end` y su badge en el modal.
- Columna `Aproximación`.
- `<meta description>` y textos de portada que mencionen nutrición, definición o fútbol.

## 3. Qué se adapta

| Pieza | Adaptación |
|---|---|
| `CYCLE` | `adaptacion` (2), `construccion` (3), `descarga` (1). Reusa clases CSS `hiper`, `fuerza`, `desc` para no tocar estilos |
| `DAYS_PER_WEEK` | 3. Puntos del tracker: 3 |
| `DAY_NAMES` | `Sesión 1`, `Sesión 2`, `Sesión 3` |
| `STORAGE_KEY` | `flaco:tracker` |
| `expandCurrentBlock` | Mapea ids `adaptacion`, `construccion`, `descarga` |
| `dayType()` | Devuelve `a` o `b` según el título del día |
| `WARMUPS` | Una entrada general, sin máquinas |
| Cooldown | Nuevo `COOLDOWNS`, mismo patrón y modal que el calentamiento. Segundo botón en la cabecera del día |
| Migración de tracker | Se elimina el código que adapta arrays de 4 días (no aplica) |
| Pills semanales | 3 pills (Lun, Mié, Vie) con "A/B según semana" |
| Cover | Título "Plan in-Flaco", meta: 3 días, 6 semanas, casa |
| FAB | Siempre apunta a `#distribucion` |
| Footer y `description` | Texto nuevo sin nutrición |

## 4. Sesiones por semana

Las tablas solo existen una vez por fase (A y B). El tracker muestra la sesión del día usando la paridad de la semana (A-B-A en impares, B-A-B en pares), sin duplicar tablas.

## 5. Deploy

```yaml
cp -R firegym dist/firegym
cp -R flaco dist/flaco
```

La URL resultante es `/firefit/flaco/`.

## 6. Verificación

No hay infraestructura de tests. Los criterios CA1-CA10 del spec se ejecutan como checks de navegador (Playwright) contra un servidor estático local (`python3 -m http.server` desde un `dist` armado igual que el workflow). No se versionan scripts de test.

Los checks se escriben antes de la implementación y deben fallar primero (404 en `/flaco/`).

## 7. Orden de trabajo (riesgo creciente)

1. Checks de aceptación definidos y en rojo.
2. Deploy + página esqueleto que cargue.
3. Recorte de nutrición, tabs y partido.
4. Contenido de entrenamiento y lenguaje simple.
5. Tracker de 3 días y fases.
6. Calentamiento y enfriamiento.
7. Verificación completa y revisión de código.
