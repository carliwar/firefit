# Spec: Plan in-Flaco

Estado: borrador para aprobación

## 1. Objetivo

Página estática `/flaco` con una rutina de gimnasio en casa para un principiante total. El objetivo único es crear el hábito de entrenar. Mismo formato visual que la página principal (raíz), sin nutrición.

URL publicada: `https://carliwar.github.io/firefit/flaco/`

## 2. Decisiones tomadas

| Tema | Decisión |
|---|---|
| Título visible | **Plan in-Flaco** |
| Formato | Mismo que la raíz (`Plan_Entrenamiento_UL_6_Semanas.html`): tema claro, portada, pills semanales, bloques `<details>`, tablas por día, modal con YouTube, tracker, FAB |
| Split | Full Body, 3 días/semana (L-X-V), sesiones A y B alternadas |
| Duración | 6 semanas: Adaptación (2) → Construcción (3) → Descarga (1) |
| Nutrición | Fuera del todo (sin pestaña, sin menciones, sin objetivo corporal) |
| Lenguaje | Simple. Sin jerga: no RIR, aproximación, superserie, hipertrofia, tempo |
| Implementación | Copiar la raíz y recortar. Sin extraer CSS/JS compartido |
| Ubicación | `flaco/index.html` (precedente: `firegym/index.html`) |
| PWA / iconos | Sin manifest propio. Sin cambios en `firegym/assets` |

## 3. Equipamiento disponible

- Barra de ~7.5 kg con stand para sentadilla, con seguros
- Banco
- Discos de 2.5, 5 y 10 kg
- Mancuernas ajustables (se cargan con los discos)
- Kettlebell
- No hay barra de dominadas, poleas ni máquinas

Fuera de alcance de ejercicios: dominadas, jalón, polea, prensa, máquinas, fondos en paralelas.

## 4. Estructura del plan

### Semana tipo

| Día | Sesión |
|---|---|
| Lunes | Sesión 1 |
| Miércoles | Sesión 2 |
| Viernes | Sesión 3 |

Semanas impares: A - B - A. Semanas pares: B - A - B.

### Fases

| Fase | Semanas | Series | Reps | Descanso | Reglas |
|---|---|---|---|---|---|
| Adaptación | 1-2 | 2 | 10-12 | 60-90 s | Peso ligero. Aprender el movimiento. Sin kettlebell swing |
| Construcción | 3-5 | 3 | 8-12 | 90 s - 2 min | Sube el peso cuando completes todas las reps con buena técnica. Entra la sentadilla con barra y el kettlebell swing |
| Descarga | 6 | 2 | 8-10 | 60-90 s | 60-65% del peso de la semana 5. Recuperar |

### Sesiones (propuesta de contenido; se afina en implementación)

Cada sesión: calentamiento (5 min) → 5-6 ejercicios (máx. 45 min) → enfriamiento y estiramiento (5 min).

**Sesión A** (sentadilla + empuje horizontal + tirón horizontal)

1. Sentadilla goblet con kettlebell o mancuerna (Construcción: sentadilla con barra en stand)
2. Press de pecho con mancuernas en banco
3. Remo con mancuerna a una mano apoyado en banco
4. Puente de glúteos en el suelo
5. Curl de bíceps con mancuernas
6. Plancha

**Sesión B** (bisagra de cadera + empuje vertical + tirón)

1. Peso muerto rumano con mancuernas (Construcción: con barra)
2. Press de hombros con mancuernas sentado en banco
3. Remo inclinado con mancuernas, dos brazos
4. Estocada estática (split squat) con mancuernas
5. Flexiones con manos en el banco
6. Bicho muerto (dead bug)
7. Construcción: kettlebell swing como accesorio de cadera, 2 x 10

Cada ejercicio tiene alternativa. Las notas son cortas y en lenguaje simple (por ejemplo: "baja despacio", "espalda recta", "deja 2-3 repeticiones en reserva" se escribe como "termina sintiendo que podrías hacer 2-3 más").

### Calentamiento y enfriamiento

- Calentamiento (mismo todos los días, sin máquinas): marcha o jumping jacks suaves, círculos de hombros, movilidad de cadera, puente de glúteos.
- Enfriamiento: estiramiento de pecho, espalda, cuádriceps, isquios y cadera, 20-30 s por postura.
- Ambos se abren con un botón en la cabecera de cada día, igual que el calentamiento actual.

### Notas de seguridad (solo indicaciones, sin avisos médicos)

- Técnica primero, peso después.
- Si algo duele (no confundir con cansancio), para y cambia al ejercicio alternativo.
- Usa los seguros del stand en toda sentadilla con barra.

## 5. Requisitos funcionales

- RF1. `flaco/index.html` es una página autocontenida (CSS y JS inline) con el mismo look que la raíz.
- RF2. Sin tabs. Una sola vista de entrenamiento.
- RF3. Portada con título "Plan in-Flaco" y resumen de fases.
- RF4. Pills de distribución semanal con 3 días.
- RF5. Tres bloques `<details>` (Adaptación, Construcción, Descarga), cada uno con tablas de Sesión A y B. El bloque actual se abre por defecto.
- RF6. Columnas de tabla: `#`, `Ejercicio`, `Alternativa`, `Músculos`, `Series × Reps`, `Descanso`, `Notas`.
- RF7. Clic en una fila abre el modal con links de YouTube (principal y alternativa), con navegación anterior/siguiente.
- RF8. Botones de calentamiento y enfriamiento en cada día.
- RF9. Tracker con 3 puntos por semana, etiqueta de fase, "Sem x/y", saltar semana y mensaje al terminar las 6 semanas.
- RF10. Día colapsable en móvil (<= 720 px).
- RF11. Todo el estado en `localStorage` con prefijo `flaco:`.
- RF12. El workflow de deploy publica `flaco/` en `dist/flaco`.

## 6. Requisitos no funcionales

- RNF1. Español, lenguaje simple.
- RNF2. Accesible: roles ARIA de la raíz conservados, foco visible, navegación por teclado en el modal.
- RNF3. Mobile-first: sin scroll horizontal a 375 px (las tablas conservan `.table-wrap`).
- RNF4. Los enlaces externos usan `rel="noopener noreferrer"`. Los textos dinámicos pasan por `escapeHtml`.
- RNF5. Cero cambios en la raíz y en `/firegym`.

## 7. Criterios de aceptación (Given / When / Then)

Se verifican en navegador antes de dar por terminado el trabajo.

- CA1. Given el sitio servido, When abro `/flaco/`, Then carga con título "Plan in-Flaco" y sin errores en consola.
- CA2. Given la página, When busco el texto, Then no aparecen: nutri, kcal, proteína, comida, receta, fútbol, partido, RIR, aproximación, superserie, hipertrofia.
- CA3. Given la página, When la reviso, Then hay 3 bloques de fase y cada uno tiene tablas de Sesión A y B.
- CA4. Given las tablas, When leo los ejercicios, Then ninguno requiere dominadas, polea, máquina ni prensa.
- CA5. Given una fila, When hago clic, Then se abre el modal con 2 links de YouTube y puedo navegar con flechas y cerrar con Esc.
- CA6. Given un día, When abro calentamiento y enfriamiento, Then se muestran ejercicios distintos para cada uno.
- CA7. Given el tracker, When marco 3 días y avanzo semanas, Then pasa por Adaptación (2), Construcción (3) y Descarga (1) y muestra el mensaje final.
- CA8. Given la página con el tracker usado, When listo las claves de `localStorage`, Then todas empiezan con `flaco:`.
- CA9. Given un viewport de 375 px, When la abro, Then no hay scroll horizontal y los días están colapsados.
- CA10. Given el diff final, When lo reviso, Then solo hay cambios en `.github/workflows/deploy.yml`, `flaco/index.html` y `specs/flaco/*`.

## 8. Fuera de alcance

Nutrición, modo partido, tabs, PWA propia, extraer CSS/JS compartido, tests automatizados persistentes, enlaces desde la raíz o `/firegym` hacia `/flaco`.

## 9. Riesgos y supuestos

- R1. Confirmado: el stand tiene seguros. La sentadilla con barra entra en Construcción.
- R2. Confirmado: la barra pesa ~7.5 kg. Es muy ligera, así que sirve para aprender la técnica en Construcción. Si la carga total (barra + discos) se queda corta, la progresión pasa a más reps o a variantes más difíciles (por ejemplo, pausa abajo).
- R3. Sin conocer el peso inicial del usuario, las cargas se expresan como "peso ligero que permita todas las reps con buena técnica", no en kg.
- R4. Duplicar ~2200 líneas de HTML aumenta el mantenimiento. Aceptado por simplicidad.
