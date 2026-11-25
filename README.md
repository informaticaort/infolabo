# Panel — Laboratorio de Informática

Panel web que muestra en tiempo real el horario de clases de todos los cursos (3° a 5°), la columna de Arte y qué asistentes están en el Laboratorio en cada bloque. Está pensado para quedar abierto en una pantalla del Labo, pero también funciona en celulares y tablets.

**Autores:** Fede y Marto

---

## Índice

1. [Cómo abrirlo](#cómo-abrirlo)
2. [Manual de uso](#manual-de-uso)
3. [Estructura del proyecto](#estructura-del-proyecto)
4. [Cómo hacer modificaciones](#cómo-hacer-modificaciones)
5. [Cosas a tener en cuenta](#cosas-a-tener-en-cuenta)
6. [Publicar los cambios](#publicar-los-cambios)

---

## Cómo abrirlo

No necesita instalación, servidor ni internet.

1. Descargar o clonar el repositorio.
2. Abrir `index.html` con cualquier navegador (Chrome, Edge, Firefox).
3. Para usarlo en la pantalla del Labo, poner el navegador en pantalla completa con **F11**.

> `infolabo.png` (el logo) tiene que estar en la misma carpeta que `index.html`.

La página se actualiza sola cada segundo: no hace falta recargarla.

---

## Manual de uso

### Barra superior

- **Logo y título** del panel.
- **Reloj y fecha**, siempre en hora de Argentina (UTC-3), sin importar la zona horaria de la computadora.

### Barra de estado

| Elemento | Qué muestra |
|---|---|
| **Insignia del día** | El día que se está viendo (ej. `MARTES`). |
| **Punto de color** | 🟢 verde = en un bloque de clase · 🟡 amarillo = recreo · ⚪ gris = fuera de horario o viendo otro día. |
| **Estado** | Bloque o recreo actual y su horario (ej. `Bloque 2 · 09:20 - 10:40`). |
| **Info** | Cuántas clases hay en el bloque actual y cuántos asistentes hay en el Labo. |
| **Próximo** | Cuál es el siguiente bloque y a qué hora empieza. |

### Filtros

- **Día:** `Hoy (Auto)` muestra el día actual y resalta el bloque en curso. Si elegís otro día (Lunes a Viernes) ves ese horario sin resaltado.
- **Vista:**
  - `Todos los cursos`: tabla del día con todos los cursos, Arte y Labo.
  - Un curso (ej. `4B`): horario **semanal** completo de ese curso, con la columna de hoy y el bloque actual resaltados.
- **Volver a Hoy:** vuelve a `Hoy (Auto)` + `Todos los cursos`. Está deshabilitado cuando ya estás en esa vista.

### Cómo leer la tabla

Cada fila es un bloque (1 a 6) y cada columna un curso. En cada celda aparece:

- **Materia:** las materias de Informática se muestran con fondo celeste.
- **Aula:** las aulas de Informática (CIDI, HMP, LUM, CEO, AMI, 4002) se muestran con borde y en negrita.
- **Profesor/es.**
- **Asistente** (en verde), si algún asistente acompaña a ese curso en ese bloque.

Además:

- **Columna Arte** (violeta): la clase de Arte de ese bloque y su asistente, si tiene.
- **Columna Labo** (verde): los asistentes que están en el Laboratorio. Debajo del nombre dice `En lab` o `Consulta`.
- **Fila Almuerzo:** quién cubre el Labo al mediodía (12:15 a 13:10).
- **Fila amarilla:** el bloque que está en curso.
- Los fines de semana aparece el mensaje "Fin de semana — No hay clases programadas".

### Horarios de los bloques

| Bloque | Horario |
|---|---|
| 1 | 07:45 – 09:05 |
| Recreo | 09:05 – 09:20 |
| 2 | 09:20 – 10:40 |
| Recreo | 10:40 – 10:55 |
| 3 | 10:55 – 12:15 |
| Almuerzo | 12:15 – 13:10 |
| 4 | 13:10 – 14:30 |
| Recreo | 14:30 – 14:40 |
| 5 | 14:40 – 16:00 |
| Recreo | 16:00 – 16:10 |
| 6 | 16:10 – 17:30 |

---

## Estructura del proyecto

```
infolabo/
├── index.html     ← todo el panel: estilos (CSS), estructura (HTML), datos y lógica (JS)
├── infolabo.png   ← logo
└── README.md
```

Todo está en `index.html`, en tres partes:

| Sección | Dónde está | Qué contiene |
|---|---|---|
| `<style>` | arriba | Colores, tamaños y la versión para celulares (`@media (max-width: 768px)`). |
| `<body>` | medio | Barra superior, barra de estado, filtros y las tablas vacías. |
| `<script>` | abajo | **Los datos** (horarios, clases, asistentes) y las funciones que arman las tablas. |

Para el día a día **solo hace falta editar los datos dentro de `<script>`**. Para encontrarlos rápido, buscá con **Ctrl+F** el nombre de la variable.

| Variable | Qué define |
|---|---|
| `CURSOS` | Lista de cursos (columnas de la tabla). |
| `BLOQUES` | Horarios de bloques, recreos y almuerzo. |
| `CLASES` | Todas las clases de la semana. |
| `ENCARGADOS_MEDIODIA` | Quién cubre el Labo al mediodía cada día. |
| `ASISTENTES_ARTE` | Asistente que acompaña la clase de Arte. |
| `ASISTENTES` | Dónde está cada asistente en cada bloque. |
| `esMateriaInfo` | Materias que se resaltan como de Informática. |
| `esAulaInfo` | Aulas que se resaltan como de Informática. |

### Números de día (`dayIdx`)

En todos los datos los días se escriben con números:

| 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|
| Lunes | Martes | Miércoles | Jueves | Viernes |

---

## Cómo hacer modificaciones

> **Antes de cambiar algo:** hacé una copia de `index.html` o asegurate de tener todo commiteado en git, así podés volver atrás si algo se rompe.
>
> **Después de cambiar algo:** guardá, recargá la página (**F5**) y revisá que se vea bien. Si la tabla aparece vacía, seguramente falta una coma o una comilla (ver [Errores comunes](#errores-comunes)).

### 1. Cambiar o agregar una clase

Buscá `var CLASES`. Cada clase es una línea así:

```js
{ dayIdx: 1, bloque: 2, curso: "3A", materia: "GEOGRAFIA", aula: "1103", profesor: "Scarcella" },
```

| Campo | Qué es | Ejemplo |
|---|---|---|
| `dayIdx` | Día (1 = Lunes … 5 = Viernes) | `1` |
| `bloque` | Número de bloque (1 a 6) | `2` |
| `curso` | Curso, igual que en `CURSOS`, o `"ARTE"` para la columna de Arte | `"3A"` |
| `materia` | Nombre de la materia | `"GEOGRAFIA"` |
| `aula` | Aula | `"1103"` |
| `profesor` | Profesor/es. Si son dos: `"Goldfeld / Peña"`. Puede quedar vacío: `""` | `"Scarcella"` |

- **Modificar:** buscá la línea por día, bloque y curso (están ordenadas y agrupadas con comentarios como `// LUNES - Bloque 2`) y cambiá el valor.
- **Agregar:** copiá una línea parecida, pegala en el grupo que corresponde y cambiá los datos.
- **Sacar:** borrá la línea entera. La celda va a mostrar `-`.

**Arte** va en el mismo array, con `curso: "ARTE"`. En `materia` se ponen las divisiones:

```js
{ dayIdx: 1, bloque: 1, curso: "ARTE", materia: "2F/2K", aula: "AULA", profesor: "Flaminio" },
```

### 2. Cambiar los asistentes

Buscá `var ASISTENTES`. Está agrupado por asistente (`// JESI`, `// JOHI`, `// FEDE`, `// DELFI`, `// MARTO`).

**Asistente en el Labo:**

```js
{ dayIdx: 1, bloque: 1, nombre: "FEDE", enLabo: true, curso: "" },
```

**Asistente en el Labo en horario de consulta:** agregá `consulta: true`:

```js
{ dayIdx: 2, bloque: 6, nombre: "JESI", enLabo: true, curso: "", consulta: true },
```

**Asistente acompañando a un curso:** `enLabo: false` y el curso:

```js
{ dayIdx: 2, bloque: 3, nombre: "JESI", enLabo: false, curso: "5C" },
```

| Campo | Qué es |
|---|---|
| `dayIdx` | Día (1 a 5). |
| `bloque` | Bloque (1 a 6). |
| `nombre` | Nombre tal como querés que se vea (en mayúsculas, por convención). |
| `enLabo` | `true` = está en el Labo (sale en la columna Labo). `false` = acompaña a un curso. |
| `curso` | Curso que acompaña. Vacío (`""`) si está en el Labo. |
| `consulta` | Opcional. `true` para que diga "Consulta" en vez de "En lab". |

**Agregar un asistente nuevo:** agregá un comentario con su nombre (ej. `// ANA`) y debajo una línea por cada bloque en que trabaja.

### 3. Asistente en la clase de Arte

Buscá `var ASISTENTES_ARTE`:

```js
var ASISTENTES_ARTE = [
  { dayIdx: 1, bloque: 3, nombre: "JOHI" },
  { dayIdx: 4, bloque: 2, nombre: "FEDE" }   // ← ejemplo de una línea nueva
];
```

Solo aparece si en ese día y bloque hay una clase de `ARTE` cargada en `CLASES`.

### 4. Quién cubre el mediodía

Buscá `var ENCARGADOS_MEDIODIA`. Hay una línea por día con el texto tal cual se muestra:

```js
var ENCARGADOS_MEDIODIA = {
  1: "(12:15-12:40): Delfi | (12:40-13:10): Marto",
  2: "(12:15-12:40): Delfi | (12:40-13:10): Jesi",
  ...
};
```

Cambiá el texto entre comillas. Si un día no tiene línea, aparece "Sin asignar".

### 5. Cambiar horarios de los bloques

Buscá `var BLOQUES`. Cambiá `start` y `end` (formato `HH:MM`, 24 horas):

```js
{ n: 1, label: 'Bloque 1', start: '07:45', end: '09:05', type: 'block' },
```

- `type` puede ser `'block'` (clase), `'break'` (recreo) o `'lunch'` (almuerzo).
- Los recreos no se muestran como fila en la tabla, pero sí en la barra de estado.
- Los horarios tienen que ir en orden y no superponerse.

### 6. Agregar o sacar un curso

Hay que tocar **tres** lugares:

1. **`var CURSOS`** (en `<script>`): agregar o sacar el curso de la lista.
   ```js
   var CURSOS = ['3A', '3B', '3C', '3D', '4A', '4B', '4C', '4D', '5A', ...];
   ```
2. **El selector "Vista"** (en el HTML, buscá `id="curso-select"`): agregar o sacar la opción.
   ```html
   <option value="4D">4D</option>
   ```
3. **La fila del almuerzo** (en `renderTable`, buscá `colspan="14"`): el número tiene que ser **cantidad de cursos + 2** (Arte y Labo). Hoy son 12 cursos → `14`. Con 13 cursos sería `15`.

Después cargá sus clases en `CLASES` usando el mismo nombre de curso.

### 7. Resaltar materias o aulas de Informática

- **Materias:** buscá `function esMateriaInfo` y agregá el nombre a la lista. Tiene que coincidir **exactamente** con lo que está en `CLASES` (mayúsculas, espacios, todo).
- **Aulas:** buscá `function esAulaInfo` y agregá el nombre. Acá alcanza con que el aula *contenga* el texto, por ejemplo `'CIDI'` también resalta `"CIDI IZQ"` y `"HMP / CIDI DER"`.

### 8. Cambiar colores y tamaños

Todo está en `<style>`. Los colores principales:

| Color | Dónde se usa | Clase CSS |
|---|---|---|
| `#365885` | Barra superior | `.top-bar` |
| `#d1dfec` | Fondo de la página | `body` |
| `#608ec9` | Encabezados de tabla, insignia del día, selectores | `.schedule-table th`, `.day-badge`, `.filter-select` |
| `#7742d4` | Encabezado de Arte | `.schedule-table th.arte` |
| `#16a34a` | Encabezado de Labo | `.schedule-table th.labo` |
| `#fefce8` | Fila del bloque actual | `.schedule-table tr.current td` |
| `#afc7eb` | Fondo de materias de Informática | `.materia-info` |

- Los tamaños de letra de la tabla usan `vh` (se adaptan al alto de la pantalla). Si en la pantalla del Labo se ve muy chico o muy grande, ajustá `font-size` en `.schedule-table`, `.materia`, `.profesor`, `.aula` y `.asistente`.
- La versión para celular está al final del `<style>`, dentro de `@media (max-width: 768px)`. Si cambiás algo de tamaño, revisá también esa parte.

### 9. Cambiar el logo

Reemplazá `infolabo.png` por otra imagen con el **mismo nombre**. Si querés usar otro nombre, cambiá `src="infolabo.png"` en el HTML.

---

## Cosas a tener en cuenta

### Errores comunes

| Problema | Causa probable |
|---|---|
| La tabla no aparece o queda vacía | Falta una coma entre líneas, sobra una coma después de la **última** línea del array, o falta cerrar una comilla `"` o una llave `}`. |
| Un asistente no aparece en un curso | No hay ninguna clase cargada en `CLASES` para ese curso, día y bloque. El asistente solo se ve si el curso tiene clase. |
| Un asistente aparece en el Labo y no en el curso | Tiene `enLabo: true`. Para que salga en el curso tiene que ser `enLabo: false` con el `curso` completado. |
| Una materia de Informática no se resalta | El nombre en `CLASES` no coincide exacto con el de `esMateriaInfo` (ojo con espacios de más, ej. `" HISTORIA "`). |
| El curso no aparece en "Vista" | Se agregó a `CURSOS` pero no al `<select id="curso-select">`. |
| La fila del almuerzo queda corta o larga | El `colspan` no coincide con cursos + 2. |
| El bloque actual no se resalta | El filtro "Día" no está en `Hoy (Auto)`, o es fin de semana / fuera de horario. |

**Tip:** si algo no anda, abrí la consola del navegador con **F12 → Console**. Si hay un error de sintaxis te dice en qué línea está.

### Reglas de formato

- Los textos van entre comillas: `"3A"`, `"FEDE"`.
- Los números y `true`/`false` van **sin** comillas: `dayIdx: 1`, `enLabo: true`.
- Cada línea de un array termina en coma, **menos la última**.
- Usá el mismo nombre de curso en todos lados (`"3A"`, no `"3 A"` ni `"3a"`).

---

## Publicar los cambios

Si el panel se usa desde el repositorio de git:

```bash
git add index.html
git commit -m "Descripción corta del cambio (ej: cambio horarios viernes)"
git push
```

Después, recargar la página en la pantalla del Labo (**F5**).

---

Hecho por **Fede** y **Marto** para el Laboratorio de Informática.
