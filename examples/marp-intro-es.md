---
marp: true
theme: gaia
paginate: true
_paginate: skip
lang: es
title: "Marp: Presentaciones con solo Markdown"
author: Creado con Marp
transition: fade
style: |
  section {
    font-size: 28px;
  }
  section.lead {
    text-align: center;
  }
  .small {
    font-size: 0.65em;
    color: #999;
  }
---

<!-- _class: lead -->
<!-- _backgroundImage: "linear-gradient(135deg, #0288d1 0%, #01579b 100%)" -->
<!-- _color: white -->

# <!-- fit --> Marp

### Presentaciones profesionales con **solo Markdown**

:rocket: Sin PowerPoint · Sin diseño manual · Solo texto

---

<!-- _transition: slide -->

# ¿Qué es Marp?

**Marp** = **Ma**rkdown **Pr**esentation Ecosystem

Un ecosistema open-source que convierte archivos `.md` en:

- :globe_with_meridians: **HTML** — Presentaciones web interactivas
- :page_facing_up: **PDF** — Para imprimir o compartir
- :bar_chart: **PPTX** — PowerPoint, Keynote, Google Slides
- :camera: **PNG / JPEG** — Imágenes por slide

---

<!-- _transition: push -->

# ¿Por qué Marp y no PowerPoint?

| PowerPoint | Marp |
|:---:|:---:|
| Interfaz gráfica pesada | Solo un editor de texto |
| Archivos binarios | Archivos `.md` legibles |
| Difícil de versionar (no puedes ver qué cambió entre versiones) | `git diff` funciona (ves exactamente qué líneas cambiaron) |
| Diseño manual por slide | Temas CSS reutilizables (diseñas una vez, aplicas a todo el deck) |

---

<!-- _class: lead -->
<!-- _transition: cube -->

# <!-- fit --> ¿Cómo funciona?

Veamos paso a paso :sparkles:

---

<!-- _transition: fade -->

# Paso 1: Escribe Markdown

```markdown
---
marp: true
theme: default
paginate: true
---

# Mi primera presentación

Esto es un slide.

---

# Segundo slide

- Punto uno
- Punto dos
```

---

# Paso 1: La clave

Lo único especial:

- `marp: true` activa Marp en VS Code
- `---` separa cada slide

¡Eso es todo! El resto es Markdown normal.

---

<!-- _transition: slide -->

# Paso 2: Elige un tema

Marp incluye **3 temas** listos:

| Tema | Estilo | Directiva |
|------|--------|-----------|
| **Default** | Limpio, GitHub | `theme: default` |
| **Gaia** | Colorido, elegante | `theme: gaia` |
| **Uncover** | Minimal, moderno | `theme: uncover` |

<span class="small">Esta presentación usa Gaia. También puedes crear temas custom.</span>

---

<!-- _transition: wipe -->

# Paso 3: Directivas globales

Controlan **todo** el deck:

```markdown
---
theme: gaia
paginate: true
lang: es
math: katex
---
```

Se ponen en el front-matter al inicio del archivo.

---

# Paso 3: Directivas locales

Controlan **desde este slide en adelante**:

```markdown
<!-- paginate: true -->
<!-- header: 'Mi empresa' -->
<!-- footer: '© 2025' -->
<!-- backgroundColor: "#123" -->
<!-- transition: fade -->
```

---

# Paso 3: Directivas spot

Con `_` aplican **solo a este slide**:

```markdown
<!-- _backgroundColor: black -->
<!-- _color: white -->
<!-- _class: lead -->
<!-- _paginate: skip -->
```

:bulb: `_` = solo este slide. Sin `_` = este y los siguientes.

---

<!-- _transition: iris-in -->

![bg right:45% contain](https://raw.githubusercontent.com/marp-team/marp/main/marp.png)

# Paso 4: Imágenes

Marp extiende la sintaxis de imagen:

```markdown
![w:300](logo.png)
```

Imagen inline redimensionada.

```markdown
![bg](foto.jpg)
```

Imagen como **fondo** del slide.

---

![bg left:40%](https://picsum.photos/720/480?random=5)

# Fondos: Split layout

```markdown
![bg left:40%](foto.jpg)
```

La imagen ocupa el 40% izquierdo.

El contenido se ajusta al 60% derecho.

También funciona con `right`:

```markdown
![bg right:50%](foto.jpg)
```

---

<!-- _transition: zoom -->

# Fondos: Filtros

Aplica filtros CSS desde el alt text:

```markdown
![bg blur:5px](foto.jpg)
![bg opacity:.5](foto.jpg)
![bg sepia:.8](foto.jpg)
![bg brightness:.4](foto.jpg)
```

Se pueden combinar:

```markdown
![bg blur:3px opacity:.6](foto.jpg)
```

---

# Fondos: Múltiples

Varias imágenes se apilan horizontalmente:

```markdown
![bg](imagen1.jpg)
![bg](imagen2.jpg)
![bg](imagen3.jpg)
```

Para apilar en vertical:

```markdown
![bg vertical](imagen1.jpg)
![bg](imagen2.jpg)
```

---

# Fondos: Gradientes

Usa directivas CSS para colores y gradientes:

```markdown
<!-- _backgroundImage: "linear-gradient(
    to right, #fc5c7d, #6a82fb)" -->
<!-- _color: white -->
```

Perfecto para slides de título o sección.

---

<!-- _class: lead -->
<!-- _transition: diamond -->
<!-- _backgroundImage: "linear-gradient(to bottom right, #0f172a, #1e293b)" -->
<!-- _color: white -->

# <!-- fit --> Funciones extra :fire:

---

<!-- _transition: fade -->

# Encabezado auto-escalable

El comentario `<!-- fit -->` escala el heading:

```markdown
# <!-- fit --> Este texto llena todo el ancho
```

Perfecto para slides de impacto.

<span class="small">Requiere @auto-scaling en el tema (los 3 built-in lo soportan).</span>

---

# Matemáticas con LaTeX

Inline: `$E = mc^2$` → $E = mc^2$

Bloque:

$$
\int_{-\infty}^{\infty} e^{-x^2} \, dx = \sqrt{\pi}
$$

Declara la librería en front-matter: `math: katex`

---

# Listas fragmentadas

Aparecen **una por una** en la presentación HTML.

Usa `*` en vez de `-`:

* Primer punto
* Segundo punto
* Tercer punto

```markdown
* Primer punto
* Segundo punto
* Tercer punto
```

<span class="small">Solo funciona en HTML bespoke. En PDF se ven todas a la vez.</span>

---

# Notas del presentador

Comentarios HTML que NO son directivas:

```markdown
# Mi slide

Contenido visible.

<!-- Esto es una nota del presentador.
     Solo visible con la tecla P. -->
```

<!-- Esta es una nota real. Presiona P en el HTML para verla. -->

Se exportan con `--pdf-notes` en CLI.

---

# CSS personalizado

**Global** (todos los slides):

```html
<style>
h1 { color: crimson; }
</style>
```

**Scoped** (solo este slide):

```html
<style scoped>
h1 { color: blue; }
</style>
```

---

<!-- _class: lead -->
<!-- _transition: swoosh -->

# <!-- fit --> ¿Cómo lo uso?

3 formas de trabajar con Marp

---

<!-- _transition: fade -->

# Opción 1: VS Code

1. Instala la extensión **"Marp for VS Code"**
2. Crea un `.md` con `marp: true`
3. Preview: `Ctrl+Shift+V`
4. Exportar: ícono Marp → "Export slide deck..."

:white_check_mark: IntelliSense para directivas
:white_check_mark: Preview en vivo
:white_check_mark: Exporta a HTML, PDF, PPTX

---

<!-- _transition: slide -->

# Opción 2: CLI

```bash
# Instalar
npm install -g @marp-team/marp-cli

# Convertir
marp slides.md                  # → HTML
marp slides.md --pdf            # → PDF
marp slides.md --pptx           # → PPTX
marp slides.md -o salida.png    # → Imagen
```

:warning: PDF/PPTX necesita Chrome, Edge o Firefox.

---

<!-- _transition: cover -->

# Opción 3: Docker

```bash
docker pull marpteam/marp-cli

docker run --rm \
  -v $(pwd):/home/marp \
  marpteam/marp-cli slides.md --pdf
```

Ideal para **CI/CD** y **GitHub Actions**.

---

<!-- _transition: star -->

# 33 transiciones incluidas

```markdown
<!-- transition: cube -->
```

`fade` · `slide` · `cover` · `push` · `cube`
`flip` · `zoom` · `wipe` · `drop` · `reveal`
`diamond` · `star` · `swoosh` · `iris-in`
`iris-out` · `explode` · `implode` · `melt`
`glow` · `rotate` · `swap` · `cylinder`
`coverflow` · `pivot` · `pull` · `fall`
y más...

Con duración: `transition: fade 1s`

---

<!-- _transition: fade -->

# Temas de la comunidad

| Tema | Estilo |
|------|--------|
| **Beam** | LaTeX Beamer |
| **Dracula** | Paleta oscura |
| **Nord** | Minimalista nórdico |
| **Neobeam** | Beamer moderno |
| **Wave** | Ondas modernas |
| **Academic** | Estilo académico |

```bash
marp --theme ./mi-tema.css slides.md
```

---

<!-- _class: lead -->
<!-- _transition: explode -->
<!-- _backgroundImage: "linear-gradient(135deg, #667eea 0%, #764ba2 100%)" -->
<!-- _color: white -->

# <!-- fit --> Resumen

---

<!-- _transition: fade -->

# Todo lo que necesitas

:one: **Escribe Markdown** — Texto plano, nada más
:two: **Agrega directivas** — Tema, paginación, transiciones
:three: **Usa imágenes** — `![bg right](foto.jpg)`
:four: **Exporta** — HTML, PDF, PPTX con un comando

### El flujo:

```
Escribe .md → Preview en VS Code → Exporta → Presenta
```

---

<!-- _class: lead -->
<!-- _backgroundImage: "linear-gradient(135deg, #0288d1 0%, #01579b 100%)" -->
<!-- _color: white -->
<!-- _paginate: skip -->
<!-- _transition: glow -->

# <!-- fit --> ¡Ahora te toca a ti! :rocket:

```bash
npx @marp-team/marp-cli@latest mi-deck.md --pdf
```

:link: [marp.app](https://marp.app) · [GitHub](https://github.com/marp-team/marp)
