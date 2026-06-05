# SEO Full Audit Report — edwinbayonaitmanager.online

**Fecha:** 2026-06-02
**Sitio:** Edwin Bayona Portfolio
**Tipo:** Portfolio de Desarrollador Freelance
**Auditado por:** Claude Code SEO Suite (8 agentes paralelos)

---

## Índice

1. [SEO Health Score](#seo-health-score)
2. [Hallazgo Transversal: El sitio NO es Next.js](#hallazgo-transversal)
3. [Metodología PERCEIVE → ANALYZE → VALIDATE → ACT](#metodología)
4. [Problemas Críticos — Corregir Esta Semana](#críticos)
5. [Prioridad Alta — Corregir en 1 Semana](#alta)
6. [Prioridad Media — Corregir en 1 Mes](#media)
7. [Prioridad Baja — Backlog](#baja)
8. [Detalle por Categoría](#detalle-por-categoría)
   - [Technical SEO (41/100)](#technical-seo)
   - [Content / E-E-A-T (52/100)](#content--e-e-a-t)
   - [Schema Markup (0/100)](#schema-markup)
   - [Sitemap & Robots](#sitemap--robots)
   - [GEO / AI Search Readiness (28/100)](#geo--ai-search-readiness)
   - [SXO — Search Experience (41/100)](#sxo--search-experience)
   - [Performance / Core Web Vitals](#performance--core-web-vitals)
   - [Backlinks (Tier 0)](#backlinks)
9. [JSON-LD Schemas Listos para Implementar](#json-ld-schemas)
10. [Impacto Estimado Post-Correcciones](#impacto-estimado)
11. [Plan de Acción: Sesión de 2 Horas (Mayor ROI)](#sesión-de-2-horas)

---

## SEO Health Score

| Categoría | Peso | Puntuación | Ponderado |
|-----------|------|------------|-----------|
| Technical SEO | 22% | 41/100 | 9.0 |
| Content Quality | 23% | 52/100 | 12.0 |
| On-Page SEO | 20% | 30/100 | 6.0 |
| Schema / Structured Data | 10% | 0/100 | 0.0 |
| Performance (CWV) | 10% | 35/100 | 3.5 |
| AI Search Readiness | 10% | 28/100 | 2.8 |
| Images | 5% | 20/100 | 1.0 |
| **TOTAL** | | | **35 / 100** |

> **Diagnóstico:** El sitio tiene diseño visual sólido y proyectos reales, pero es prácticamente invisible para búsquedas comerciales en español. Cero datos estructurados, sin keywords locales, sin robots.txt ni sitemap, y el rendimiento está bloqueado por el CDN de Tailwind.

---

## Hallazgo Transversal

> **El sitio NO es Next.js como está desplegado.**

Todos los agentes confirmaron independientemente: el sitio desplegado es **un único archivo HTML estático con Tailwind CSS cargado desde CDN** y JavaScript vanilla. Next.js aparece listado como *habilidad* en el stack técnico, no como framework de construcción. El pie de página dice "Construido con Next.js" — esto es **factualmente incorrecto** y daña la credibilidad frente a clientes técnicos que inspeccionen el código fuente.

**Impacto:**
- No hay `next/head`, `next/image`, ni App Router metadata API disponibles
- No se puede usar `next-sitemap` ni optimización automática de imágenes
- El footer necesita corrección

---

## Metodología

### PERCEIVE

- El sitio está diseñado para alguien que ya conoce "Edwin Bayona" — no para ser descubierto
- La navegación 100% por anclas es correcta para HTML estático (no crea URLs duplicadas)
- El sistema i18n (ES/EN/FR/IT) genera complejidad sin beneficio SEO: el contenido multilingüe es invisible para todos los crawlers porque se renderiza vía JavaScript tras una llamada a `ipapi.co`

### ANALYZE

- **Patrón sistémico:** Cero superficie de contenido indexable para búsquedas de descubrimiento. El sitio solo captura tráfico navegacional (quien ya busca "Edwin Bayona")
- **Cadena de bloqueo de rendimiento:** CDN Tailwind → LCP >4s → mal CWV → señal negativa a Google
- **Brecha de entidad:** Sin schema `Person`, Google/AI no puede consolidar la identidad entre el dominio, GitHub y LinkedIn

### VALIDATE — ¿Cómo sabríamos si las correcciones fallan?

- Verificar indexación en Google Search Console 72h después de submittir el sitemap
- Medir LCP en PageSpeed Insights antes/después de quitar el CDN de Tailwind
- Comprobar en `rich results test` de Google que los schemas se parsean sin errores
- Verificar `https://edwinbayonaitmanager.online/robots.txt` y `sitemap.xml` devuelven 200

### ACT — Cadena de dependencias (orden importa)

```
1. Infraestructura de crawl     → robots.txt + sitemap + canonical + GSC
2. Establecer entidad           → JSON-LD Person + LocalBusiness + WebSite
3. Contenido y conversión       → keywords + hero copy + testimonios + links de proyectos
4. Rendimiento                  → Tailwind build + GIF→video + fuentes
```

---

## Críticos

> Estos 9 problemas son los de mayor impacto. Corrección estimada: **2 horas total.**

---

### C-1: Sin robots.txt ni sitemap.xml

Ambos devuelven 404. Los crawlers de IA no tienen directivas explícitas; Google no tiene señal de frescura.

**Crear `robots.txt` en la raíz del proyecto:**

```
User-agent: *
Allow: /

User-agent: GPTBot
Allow: /

User-agent: OAI-SearchBot
Allow: /

User-agent: ClaudeBot
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: Google-Extended
Allow: /

Sitemap: https://edwinbayonaitmanager.online/sitemap.xml
```

**Crear `sitemap.xml` en la raíz del proyecto:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://edwinbayonaitmanager.online/</loc>
    <lastmod>2026-06-02</lastmod>
  </url>
</urlset>
```

> Nota: No agregar `/#about`, `/#skills`, etc. como URLs separadas. Google trata los fragmentos del mismo documento como una sola página.

---

### C-2: Sin canonical, sin meta robots, OG tags incompletos

Agregar en el `<head>` de `index.html`:

```html
<!-- Canonical -->
<link rel="canonical" href="https://edwinbayonaitmanager.online/" />

<!-- Meta robots con permisos de rich snippets -->
<meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large, max-video-preview:-1">

<!-- Open Graph completo -->
<meta property="og:type" content="website"/>
<meta property="og:url" content="https://edwinbayonaitmanager.online/"/>
<meta property="og:image" content="https://edwinbayonaitmanager.online/assets/og-preview.png"/>
<meta property="og:image:width" content="1200"/>
<meta property="og:image:height" content="630"/>
<meta property="og:locale" content="es_CO"/>

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image"/>
<meta name="twitter:title" content="Edwin Bayona | Desarrollador Web Full-Stack Bucaramanga"/>
<meta name="twitter:description" content="Tiendas e-commerce, IA generativa y automatización WhatsApp para empresas en Colombia y Ecuador."/>
<meta name="twitter:image" content="https://edwinbayonaitmanager.online/assets/og-preview.png"/>
```

> Crear imagen `og-preview.png` (1200×630px) — un screenshot branding del hero es suficiente para empezar.

---

### C-3: Cero datos estructurados (Schema.org)

Ver sección completa [JSON-LD Schemas Listos para Implementar](#json-ld-schemas) más abajo.

**Impacto esperado:** Lifts AI citation readiness de 28 → ~55/100. Habilita eligibilidad para Google local pack y Knowledge Panel.

---

### C-4: Title tag sin keywords en español ni ubicación

| | Texto |
|---|---|
| **Actual** | `Edwin Bayona \| Full-Stack Web Developer` |
| **Fix** | `Edwin Bayona \| Desarrollador Web Full-Stack Bucaramanga, Colombia` |

```html
<title>Edwin Bayona | Desarrollador Web Full-Stack Bucaramanga, Colombia</title>
```

---

### C-5: Meta description sin intención comercial

| | Texto |
|---|---|
| **Actual** | *"Edwin Bayona — Full-Stack Web Developer especializado en Next.js, React, TypeScript e IA generativa. Disponible para proyectos en Bucaramanga, Colombia."* |
| **Fix** | *"Contrata a Edwin Bayona, desarrollador web full-stack en Bucaramanga. Tiendas e-commerce, IA generativa y automatización WhatsApp. Respuesta en 24h."* |

```html
<meta name="description" content="Contrata a Edwin Bayona, desarrollador web full-stack en Bucaramanga. Tiendas e-commerce, IA generativa y automatización WhatsApp. Respuesta en 24h." />
```

---

### C-6: Links de proyectos rotos (mayor killer de conversión)

**5 de 6 botones "Ver proyecto" / "Demo" resuelven a `href="#"`.**

Esta es la falla de conversión más grave del sitio. Un visitante referido que llega a verificar el trabajo no puede hacerlo.

| Proyecto | Estado actual | Fix requerido |
|---------|--------------|---------------|
| PawArtStudio — Demo | `href="#"` | Agregar URL de demo en producción |
| PawArtStudio — GitHub | `href="#"` | Agregar URL del repositorio |
| KS Promocionales | `href="#"` | URL real o eliminar botón |
| El Fogón Gourmet | `href="#"` | URL real o eliminar botón |
| Parasoles Industriales | `href="#"` | URL real o eliminar botón |
| PromoGimmicks | Funciona ✅ | — |

> Si el proyecto es privado y no hay demo pública, **eliminar el botón** es mejor que dejarlo roto.

---

### C-7: README.md tiene URL placeholder — backlink gratis de DA~100

El archivo `README.md` contiene `[Insertar tu URL aquí]` en lugar de la URL del sitio.

**Fix (5 minutos, impacto máximo):** Reemplazar `[Insertar tu URL aquí]` con `https://edwinbayonaitmanager.online`

> Esto crea un backlink dofollow desde `github.com` (DA ~100) — el sitio de mayor autoridad de dominio al que tienes acceso inmediato gratuito.

---

### C-8: Tailwind CSS cargado desde CDN — bloquea LCP

```html
<!-- PROBLEMA: render-blocking, 300KB runtime, no apto para producción -->
<script src="https://cdn.tailwindcss.com?plugins=forms,container-queries"></script>
```

Este script es **render-blocking**: el navegador no puede pintar nada hasta que descargue (~300KB), parsee y ejecute el compilador de CSS en tiempo real. Tailwind documenta explícitamente que el CDN build **no es para producción**.

**Impacto en LCP estimado:** +800ms–2000ms adicionales. LCP actual estimado: 2.8s–5.0s (Poor).

**Fix:**

```bash
# Instalar Tailwind CLI
npm install -D tailwindcss
npx tailwindcss init

# Crear input.css con:
# @tailwind base;
# @tailwind components;
# @tailwind utilities;

# Generar CSS de producción (purged + minified)
npx tailwindcss -i ./input.css -o ./assets/styles.css --minify
```

Reemplazar el `<script>` de CDN por:

```html
<link rel="stylesheet" href="assets/styles.css">
```

**Resultado:** CSS reduce de ~300KB runtime a ~10–15KB estático. Elimina el mayor recurso render-blocking del sitio.

---

### C-9: No existe llms.txt — sitio invisible para agentes de IA

**Crear `llms.txt` en la raíz del proyecto:**

```
# Edwin Bayona — Full-Stack Web Developer

Edwin Bayona is a freelance full-stack web developer based in Bucaramanga, Colombia.
Specializes in Next.js, React, TypeScript, generative AI integrations (Google Gemini),
3D web visualizations (React Three Fiber), e-commerce development, technical SEO,
and WhatsApp automation for local businesses in Colombia and Ecuador.

## Services
- E-commerce platforms with visual editors and AI-generated content
- Generative AI integrations (Gemini API, image generation, chat)
- 3D product visualization (React Three Fiber / Three.js)
- Multi-country payment gateway integration (Wompi Colombia, PayPhone Ecuador)
- WhatsApp automation and conversion flows for local businesses
- Technical SEO optimization for Colombian businesses

## Contact
- Email: info@edwinbayonaitmanager.online
- WhatsApp: +573116462342
- LinkedIn: https://www.linkedin.com/in/edwin-bayona/
- GitHub: https://github.com/edt092
- Location: Bucaramanga, Santander, Colombia

## Featured Projects
- PawArtStudio: AI-powered pet art e-commerce with 3D preview, Gemini AI image generation,
  multi-country payments (Wompi Colombia + PayPhone Ecuador)
- PromoGimmicks: Promotional products e-commerce with product catalog management
- KS Promocionales: Corporate promotional products store
```

> Perplexity, Claude y un número creciente de agentes de IA verifican activamente `/llms.txt` como descriptor de sitio legible por máquinas.

---

## Alta

> Corregir dentro de los próximos 7 días.

| # | Problema | Fix |
|---|---------|-----|
| 10 | **Sin keywords comerciales en español en el cuerpo** | Agregar "desarrollador web Bucaramanga", "contratar desarrollador full-stack Colombia" de forma natural en los párrafos del `<section id="about">` |
| 11 | **Sin testimonios de clientes** | Agregar al menos 1 cita atribuida con nombre + empresa en la sección de contacto o proyectos. Ej: *"Edwin entregó la tienda en 3 semanas. — Carlos M., KS Promocionales"* |
| 12 | **6 GIFs sin `loading="lazy"` — 18-48MB de ancho de banda** | Paso 1: `loading="lazy"` en los 5 GIFs debajo del fold. Paso 2: Convertir a `<video autoplay loop muted playsinline>` con fuentes WebM+MP4 (90–95% menos tamaño) |
| 13 | **Imágenes sin atributos `width`/`height`** — causa CLS | Agregar dimensiones a todos los `<img>` para que el navegador reserve espacio antes de cargar |
| 14 | **Copyright del footer muestra 2025** | Actualizar a `© 2026` o generar dinámicamente: `document.querySelector('.copyright').textContent = new Date().getFullYear()` |
| 15 | **Sin favicon declarado** | Exportar el logo hexagonal EB a `assets/favicon.svg` y agregar `<link rel="icon" type="image/svg+xml" href="assets/favicon.svg">` |
| 16 | **Footer dice "Construido con Next.js" pero el sitio es HTML estático** | Corregir a "HTML · Tailwind CSS · Vanilla JS" — o migrar el sitio a Next.js de verdad |
| 17 | **Campos "About" de repos de GitHub vacíos** | Agregar `https://edwinbayonaitmanager.online` en el panel About de cada repo público (10 segundos cada uno → backlinks DA~100) |
| 18 | **Campo "Website" de LinkedIn probablemente vacío** | Verificar y agregar la URL del portfolio en el perfil de LinkedIn (DA~98) |

---

## Media

> Corregir dentro de 1 mes.

| # | Problema | Fix |
|---|---------|-----|
| 19 | **Hero centrado en el dev, no en el cliente** | Reescribir tagline: *"Convierto tu idea en una tienda online, plataforma o landing que genera ventas — con tecnología moderna y entrega rápida en Bucaramanga."* |
| 20 | **Sin sección "Cómo trabajo"** | Agregar bloque de 3–4 pasos entre Proyectos y Contacto: Briefing → Prototipo → Desarrollo → Entrega |
| 21 | **Sin señal de precio en ningún lado** | Agregar "Proyectos desde $X USD" o "solicita cotización gratuita" — la ausencia de precios genera abandono |
| 22 | **Sección "Sobre mí" no es citable por IA** | Consolidar los 3 párrafos cortos en 1 párrafo en tercera persona de ~150 palabras (rango óptimo para citación IA: 134–167 palabras) |
| 23 | **`ipapi.co` fetch sin caché** | Cachear resultado en localStorage con TTL de 24h |
| 24 | **Google Fonts: 2 requests separados, 7 pesos de Inter** | Combinar en 1 request, eliminar pesos 300 y 800 (no se usan) |
| 25 | **Sin perfiles en plataformas freelance** | Crear perfil en Upwork, Contra y Workana con link al portfolio (backlinks de alta DA + relevancia local) |
| 26 | **Sin contenido indexable de descubrimiento** | Crear perfil en Dev.to + 1 artículo técnico sobre "Next.js + Gemini AI + Wompi payments" — bajo competencia, alta citabilidad |
| 27 | **`rel="me"` ausente en links sociales** | Agregar `rel="noopener me"` a los `<a>` de GitHub y LinkedIn |
| 28 | **Desajuste de identidad del dominio** | "itmanager.online" ≠ "Full-Stack Developer". Planificar migración a `edwinbayona.dev` o `edwinbayona.co` con redirect 301 permanente |

---

## Baja

> Backlog — mejoras de optimización.

- [ ] Implementar protocolo **IndexNow** para notificación casi instantánea de indexación en Bing/Yandex
- [ ] Auto-hospedar subconjunto de **Material Symbols** (~220KB → ~20KB con solo los ~15 iconos usados)
- [ ] Reemplazar animación `box-shadow` del botón WhatsApp por `outline` (composited por GPU)
- [ ] Agregar fallback `<noscript>` para scroll-reveal: `.reveal { opacity: 1 !important; transform: none !important; }`
- [ ] Simplificar o implementar correctamente el sistema i18n — actualmente invisible para todos los crawlers
- [ ] Agregar botón "Descargar CV" para persona reclutador
- [ ] Considerar Calendly/Cal.com para persona startup founder
- [ ] Reemplazar `backdrop-filter: blur()` en cards con `rgba()` para dispositivos móviles de gama media
- [ ] Mover animación de typing a `requestAnimationFrame` en lugar de `setTimeout` recursivo
- [ ] Eliminar handlers `onmouseover`/`onmouseout` inline del botón WhatsApp → usar CSS `:hover`

---

## Detalle por Categoría

### Technical SEO

**Score: 41 / 100**

#### Puntuación por sub-categoría

| Sub-categoría | Score | Estado |
|---|---|---|
| Crawlability | 30/100 | Crítico |
| Indexability | 35/100 | Crítico |
| URL Structure | 55/100 | Alto |
| Security | No verificado* | — |
| Mobile | 80/100 | Pass |
| Core Web Vitals | 35/100 | Alto |
| JS Rendering | 45/100 | Alto |
| Structured Data | 0/100 | Crítico |
| Internacional/Hreflang | 20/100 | Crítico |

*Los security headers requieren inspección de headers HTTP en vivo. Verificar en [securityheaders.com](https://securityheaders.com/?q=edwinbayonaitmanager.online)*

#### Hreflang — Problema Arquitectural

El sistema de i18n (ES/EN/FR/IT) detecta el idioma vía JavaScript llamando a `ipapi.co` después de cargar la página. Googlebot crawlea desde IPs de EE.UU. pero **no puede correlacionar las versiones de idioma como páginas alternas** porque:

1. El HTML siempre tiene `<html lang="es">` en el servidor
2. No existen tags `<link rel="alternate" hreflang="...">` en ningún lugar
3. El contenido en inglés/francés/italiano **nunca es indexado** — solo existe en el cliente tras ejecutar JS

**Opciones:**
- **Opción A (recomendada para freelancer local):** Eliminar todo el sistema i18n. Establecer `<html lang="es">` permanentemente. Reducir ~1,100 líneas de JS y eliminar la dependencia de `ipapi.co`.
- **Opción B (si el alcance internacional es real):** Migrar a rutas separadas (`/en/`, `/fr/`) con tags hreflang correctos. Requiere servidor o generador de sitios estáticos.

---

### Content / E-E-A-T

**Scores:** Content Quality: 52/100 | E-E-A-T Composite: 44/100

#### Desglose E-E-A-T

| Pilar | Score | Problemas principales |
|---|---|---|
| **Experience** (Experiencia) | 14/20 | GIFs no cargando en producción, sin métricas de resultados, sin fechas en proyectos |
| **Expertise** (Expertise) | 17/25 | Stack técnico claro, pero footer dice "Next.js" y el sitio es HTML — inconsistencia técnica crítica |
| **Authoritativeness** (Autoridad) | 8/25 | **El pilar más débil.** Cero testimonios, cero logos de clientes, cero menciones externas |
| **Trustworthiness** (Confianza) | 16/30 | Email y WhatsApp presentes, pero dominio genera fricción de confianza |

#### Keywords Comerciales Ausentes

El sitio tiene **cero instancias** de las siguientes frases de alta intención comercial:

| Keyword objetivo | Intención | Prioridad |
|---|---|---|
| desarrollador web Bucaramanga | Contratación local | Crítica |
| desarrollador full stack Colombia | Contratación nacional | Crítica |
| contratar desarrollador web | Transaccional | Crítica |
| desarrollo web Colombia | Servicio amplio | Alta |
| tienda online Colombia | Servicio e-commerce | Alta |
| automatización WhatsApp | Keyword de servicio | Media |

El H1 es `Edwin Bayona` — nombre de marca puro, sin señal de keyword.

#### Oportunidades de Contenido Faltante

| Prioridad | Elemento | Impacto |
|---|---|---|
| Crítica | Testimonios con nombre y empresa | E-E-A-T Authoritativeness |
| Crítica | Links de demo reales en proyectos | Conversión directa |
| Alta | Sección "Cómo trabajo" (3–4 pasos) | Reduce fricción de contacto |
| Alta | Señal de precio mínimo | Reduce abandono por incertidumbre |
| Media | Mini case study por proyecto (problema→solución→resultado) | Expertise + AI citation |

#### Indicadores CTA

| CTA | Estado | Problema |
|---|---|---|
| "Contrátame" en navbar | Bien | Persistente, acción clara |
| Botón WhatsApp flotante | Bien | Funcional, siempre accesible |
| "Contáctame" en hero | Redundante | Mismo destino que "Contrátame" |
| Links "Ver proyecto" | **Roto** | 5/6 apuntan a `href="#"` |
| Email en contacto | Débil | Dirección larga, dominio de baja confianza |

---

### Schema Markup

**Score: 0 / 100** — No existe ningún dato estructurado.

| Tipo | Presencia | Prioridad |
|---|---|---|
| `Person` | ❌ | Crítica |
| `LocalBusiness` / `ProfessionalService` | ❌ | Crítica |
| `WebSite` | ❌ | Alta |
| `Service` (por servicio) | ❌ | Media |
| `CreativeWork` / `SoftwareApplication` (proyectos) | ❌ | Baja |
| `FAQPage` | ❌ | No recomendado (sitio comercial, restricción Google ago 2023) |
| `HowTo` | ❌ | No recomendado (deprecado sep 2023) |

Ver [sección de JSON-LD Schemas](#json-ld-schemas) para implementación lista.

---

### Sitemap & Robots

| Check | Resultado | Severidad |
|---|---|---|
| `sitemap.xml` existe | ❌ FAIL | Crítico |
| `robots.txt` existe | ❌ FAIL | Alto |
| Sitemap referenciado en robots.txt | ❌ FAIL | Alto |
| Tag `<link rel="canonical">` | ❌ FAIL | Crítico |
| `og:url` declarado | ❌ FAIL | Alto |
| `og:image` declarado | ❌ FAIL | Alto |
| `lang="es"` en HTML | ✅ PASS | — |
| HTTPS | ✅ PASS (asumido) | — |

---

### GEO / AI Search Readiness

**Score: 28 / 100**

#### Desglose GEO

| Dimensión | Score | Peso | Ponderado |
|---|---|---|---|
| Citabilidad | 22/100 | 25% | 5.5 |
| Legibilidad Estructural | 38/100 | 20% | 7.6 |
| Contenido Multi-modal | 20/100 | 15% | 3.0 |
| Autoridad y Señales de Marca | 25/100 | 20% | 5.0 |
| Accesibilidad Técnica | 35/100 | 20% | 7.0 |
| **Total** | | | **28.1** |

#### Estado de Crawlers de IA

| Crawler | Estado |
|---|---|
| GPTBot (OpenAI) | Sin directiva explícita |
| OAI-SearchBot | Sin directiva explícita |
| ClaudeBot (Anthropic) | Sin directiva explícita |
| PerplexityBot | Sin directiva explícita |
| Google-Extended | Sin directiva explícita |
| CCBot (Common Crawl training) | Sin directiva explícita |

#### Visibilidad Estimada por Plataforma

| Plataforma | Visibilidad estimada | Bloqueador principal |
|---|---|---|
| Google AI Overviews | 5–10% | Sin JSON-LD, sin pasajes de respuesta, sin autoridad |
| ChatGPT (browse) | 5% | Sin robots.txt para GPTBot, sin entidad estructurada |
| Perplexity | 10–15% | HTML estático crawleable, pero sin llms.txt ni schema |
| Bing Copilot | 5–10% | Sin schema, sin Bing Webmaster, sin sitemap |
| Claude (con búsqueda) | 10–15% | Sin directiva ClaudeBot, sin llms.txt |

#### Problema de Citabilidad por IA

Los párrafos del "Sobre mí" tienen ~41–55 palabras cada uno — demasiado cortos para ser citados como pasaje. El rango óptimo para citación IA es 134–167 palabras en un bloque cohesivo.

**Reescritura sugerida del "Sobre mí" para citabilidad IA (148 palabras, tercera persona):**

> "Edwin Bayona es un desarrollador full-stack freelance con base en Bucaramanga, Colombia, con más de dos años de experiencia construyendo aplicaciones web de producción para empresas locales e internacionales. Se especializa en Next.js 14/15, React 18/19, TypeScript, Tailwind CSS e integraciones de IA generativa usando la API de Google Gemini. Su portfolio incluye plataformas e-commerce con IA y visualización 3D de productos (React Three Fiber), editores visuales interactivos con React-Konva, integración de pasarelas de pago en Colombia (Wompi) y Ecuador (PayPhone), y sitios web corporativos optimizados para SEO para negocios en Bucaramanga. Edwin combina arquitectura técnica con decisiones de UX orientadas al producto — cada desarrollo apunta a conversión, retención y resultados de negocio medibles. Actualmente disponible para nuevos proyectos freelance y colaboraciones."

---

### SXO — Search Experience

**Score: 41 / 100**

#### Desglose SXO

| Factor | Score | Peso |
|---|---|---|
| Coincidencia de tipo de página | 7/15 | Alta |
| Profundidad de contenido | 7/15 | Alta |
| Señales UX | 8/15 | Media |
| Schema markup | 0/15 | Alta |
| Multimedia | 8/15 | Media |
| Señales de autoridad | 4/15 | Alta |
| Frescura | 7/10 | Media |

#### Análisis de Intención de Búsqueda

| Query | Intención | Tipo esperado en SERP | Coincidencia |
|---|---|---|---|
| "desarrollador web freelance Bucaramanga" | Alta (comercial) | Landing de servicio | ❌ MISMATCH |
| "contratar desarrollador Next.js Colombia" | Alta (comercial) | Landing / agencia | ❌ MISMATCH |
| "crear tienda ecommerce Colombia" | Transaccional | Página de servicio o producto | ❌ MISMATCH |
| "Edwin Bayona desarrollador" | Navegacional | Portfolio personal | ✅ ALIGNED |
| "desarrollador Next.js React portfolio" | Navegacional | Portfolio | ✅ ALIGNED |

> La página captura tráfico navegacional pero no puede competir en queries comerciales de alta intención. Está diseñada para quien ya te conoce, no para quien te está buscando.

#### Scoring por Persona

| Persona | Score | Mayor Gap |
|---|---|---|
| Dueño de PYME Colombia/Ecuador (e-commerce, landing) | 37/100 | Sin precio, sin proceso, jerga técnica |
| Startup founder (plataforma con IA) | 50/100 | Sin discovery call link, sin caso de estudio |
| Reclutador técnico | 58/100 | Sin descarga CV, repos privados no verificables |
| Cliente referido (lead cálido) | 65/100 | Links de proyectos rotos — falla crítica |

#### Análisis Above-the-Fold (1366×768)

Lo que ve un visitante primero:

1. Nav glassmorphism con logo y "Contrátame"
2. Badge: "Disponible para proyectos · Bucaramanga, Colombia" ✅
3. H1: "Edwin / Bayona" en texto gradient
4. Typing animation (en blanco hasta que carga JS) ⚠️
5. Tagline: "Construyo productos web de alto rendimiento con Next.js..." (orientado al dev, no al cliente) ⚠️
6. Dos CTAs: "Ver proyectos" + "Contáctame"
7. Stats: 6+ proyectos / 2+ años / 4+ clientes ⚠️ (auto-reportado, sin validación externa)

**Tagline actual:**
> "Construyo productos web de alto rendimiento con Next.js, React e integraciones de IA generativa."

**Tagline recomendado (orientado al cliente):**
> "Convierto tu idea en una tienda online, plataforma o landing que genera ventas — con tecnología moderna y entrega rápida en Bucaramanga."

---

### Performance / Core Web Vitals

**Score estimado: 35 / 100**

#### Métricas CWV Estimadas (Análisis Estático — Sin Lab Data)

| Métrica | Rango Estimado | Estado |
|---------|---------------|--------|
| **LCP** (Largest Contentful Paint) | 2.8s – 5.0s | Needs Improvement / Poor |
| **INP** (Interaction to Next Paint) | 80ms – 280ms | Borderline Good/Needs Improvement |
| **CLS** (Cumulative Layout Shift) | 0.08 – 0.22 | Borderline Good/Needs Improvement |
| **FCP** (First Contentful Paint) | 1.5s – 3.5s | Needs Improvement |

> Umbrales de referencia: LCP Good <2.5s / INP Good <200ms / CLS Good <0.1

#### Problemas de Rendimiento por Severidad

**Críticos:**

| Problema | Impacto | Fix |
|---|---|---|
| Tailwind CDN `<script>` render-blocking | +800ms–2000ms LCP | Build CSS estático con CLI |
| 6 GIFs sin `loading="lazy"` | 18–48MB de ancho de banda en carga | `loading="lazy"` + convertir a `<video>` |
| GIFs sin `width`/`height` | CLS alto en sección proyectos | Agregar dimensiones explícitas |

**Altos:**

| Problema | Impacto | Fix |
|---|---|---|
| 2 requests de Google Fonts separados, 7 pesos de Inter | +200ms FCP por font | Combinar en 1 request, eliminar pesos 300/800 |
| `ipapi.co` fetch sin caché — se ejecuta en cada carga | +100–500ms, falla silenciosa | Cachear en localStorage con TTL 24h |
| `backdrop-filter: blur()` en 10+ elementos `.glass` | Contención GPU en Android gama media | `rgba()` fallback con `@media (prefers-reduced-motion)` |

**Medios:**

| Problema | Fix |
|---|---|
| Animación de typing usa `setTimeout` recursivo (sin `requestAnimationFrame`) | Migrar a `requestAnimationFrame` |
| SVG del logo inlineado dos veces (nav + footer) con `<defs>` duplicados | Usar `<use href="#symbol-id">` |
| WhatsApp pulse anima `box-shadow` (no composited) | Migrar a `outline` animation |

#### Plan de Conversión de GIFs a Video

```html
<!-- Antes -->
<img src="assets/gif-pawart.gif" alt="PawArtStudio demo" class="w-full ...">

<!-- Después -->
<video autoplay loop muted playsinline width="800" height="500"
       preload="metadata" class="w-full h-full object-cover">
  <source src="assets/pawart.webm" type="video/webm">
  <source src="assets/pawart.mp4" type="video/mp4">
</video>
```

Usar `preload="metadata"` en el primer video (featured), `preload="none"` en los demás. Reducción esperada de tamaño: **90–95% por archivo**.

---

### Backlinks

**Tier: 0 (Common Crawl basic — sin Moz/Bing/DataForSEO configurados)**

**Backlinks confirmados inbound: ~0**

> El dominio es nuevo y no aparece aún en el índice de Common Crawl. Esto es esperado para dominios registrados en 2024–2025.

#### Análisis del Perfil del Dominio

| Señal | Valor observado | Estado |
|---|---|---|
| TLD | `.online` | ⚠️ Menor confianza que `.com`/`.co` |
| Nombre de dominio | "itmanager" | ⚠️ No coincide con "Full-Stack Developer" |
| Canonical self-link | No presente | ❌ |
| Email coincide con dominio | Sí | ✅ |
| Schema `sameAs` | Ninguno | ❌ |

#### Inventario de Links Salientes (Oportunidades Recíprocas)

| Destino | Colocación |
|---|---|
| `github.com/edt092` | Nav, Proyectos, Contacto, Footer |
| `github.com/edt092/promogimmicks` | Card de proyecto |
| `linkedin.com/in/edwin-bayona/` | Contacto, Footer |
| `wa.me/573116462342` | Botón flotante |

#### Plan de Link Building (Ordenado por ROI)

| Prioridad | Acción | Plataforma | DA est. | Esfuerzo |
|---|---|---|---|---|
| **Crítica** | Completar URL en README.md | github.com | ~100 | 5 min |
| **Alta** | Agregar URL en campo "About" de todos los repos públicos | github.com | ~100 | 10 min total |
| **Alta** | Verificar campo "Website" en LinkedIn | linkedin.com | ~98 | 5 min |
| **Alta** | Crear perfil en Upwork con link al portfolio | upwork.com | ~93 | 1 hora |
| **Alta** | Submittir sitemap a Google Search Console | Google | — | 5 min |
| **Media** | Crear perfil + 1 artículo técnico en Dev.to | dev.to | ~91 | 3–4 horas |
| **Media** | Crear perfil en Contra | contra.com | ~62 | 30 min |
| **Media** | Crear perfil en Workana (Latam) | workana.com | ~65 | 30 min |
| **Media** | Artículo Hashnode: "Wompi + Next.js + PayPhone" | hashnode.com | ~88 | 3–4 horas |
| **Media** | Submittir a directorio Colombia Dev | colombia.dev | regional | 15 min |
| **Media** | Crear perfil en Platzi | platzi.com | ~71 | 15 min |
| **Baja** | Google Business Profile (servicio, Bucaramanga) | Google | — | 30 min |
| **Baja** | Perfil en CodePen con demos de efectos glassmorphism | codepen.io | ~93 | 1 hora |

---

## JSON-LD Schemas

> Listos para copiar y pegar en el `<head>` de `index.html` antes de `</head>`.

### Block 1 — Person (CRÍTICO)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Person",
  "@id": "https://edwinbayonaitmanager.online/#person",
  "name": "Edwin Bayona",
  "givenName": "Edwin",
  "familyName": "Bayona",
  "jobTitle": "Full-Stack Web Developer",
  "description": "Desarrollador full-stack freelance especializado en e-commerce, integraciones de IA, visualizaciones 3D, optimización SEO y automatización con WhatsApp. Basado en Bucaramanga, Colombia.",
  "url": "https://edwinbayonaitmanager.online/",
  "email": "info@edwinbayonaitmanager.online",
  "telephone": "+573116462342",
  "address": {
    "@type": "PostalAddress",
    "addressLocality": "Bucaramanga",
    "addressRegion": "Santander",
    "addressCountry": "CO"
  },
  "sameAs": [
    "https://github.com/edt092",
    "https://www.linkedin.com/in/edwin-bayona/"
  ],
  "knowsAbout": [
    "Desarrollo Full-Stack",
    "E-commerce",
    "Inteligencia Artificial",
    "Visualizaciones 3D",
    "SEO",
    "Automatización WhatsApp",
    "Next.js",
    "React",
    "TypeScript"
  ],
  "worksFor": {
    "@type": "Organization",
    "@id": "https://edwinbayonaitmanager.online/#organization"
  }
}
</script>
```

### Block 2 — LocalBusiness / ProfessionalService (CRÍTICO)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": ["LocalBusiness", "ProfessionalService"],
  "@id": "https://edwinbayonaitmanager.online/#organization",
  "name": "Edwin Bayona — Desarrollo Web",
  "alternateName": "Edwin Bayona Full-Stack Developer",
  "description": "Servicios freelance de desarrollo full-stack en Bucaramanga, Colombia: tiendas e-commerce, integraciones de IA, visualizaciones 3D, optimización SEO y automatización con WhatsApp.",
  "url": "https://edwinbayonaitmanager.online/",
  "telephone": "+573116462342",
  "email": "info@edwinbayonaitmanager.online",
  "address": {
    "@type": "PostalAddress",
    "addressLocality": "Bucaramanga",
    "addressRegion": "Santander",
    "addressCountry": "CO"
  },
  "areaServed": [
    { "@type": "City", "name": "Bucaramanga" },
    { "@type": "Country", "name": "Colombia" },
    { "@type": "Country", "name": "Ecuador" }
  ],
  "hasOfferCatalog": {
    "@type": "OfferCatalog",
    "name": "Servicios de Desarrollo Web",
    "itemListElement": [
      {
        "@type": "Offer",
        "itemOffered": {
          "@type": "Service",
          "name": "Desarrollo de Tiendas E-commerce",
          "description": "Tiendas en línea completas con pasarelas de pago, gestión de inventario y experiencia de usuario optimizada."
        }
      },
      {
        "@type": "Offer",
        "itemOffered": {
          "@type": "Service",
          "name": "Integraciones de Inteligencia Artificial",
          "description": "Integración de modelos de IA y automatización inteligente usando Google Gemini API."
        }
      },
      {
        "@type": "Offer",
        "itemOffered": {
          "@type": "Service",
          "name": "Visualizaciones 3D para Web",
          "description": "Experiencias visuales inmersivas en 3D con React Three Fiber para productos y presentaciones."
        }
      },
      {
        "@type": "Offer",
        "itemOffered": {
          "@type": "Service",
          "name": "Automatización con WhatsApp",
          "description": "Bots y flujos automatizados de WhatsApp Business para atención al cliente y ventas."
        }
      },
      {
        "@type": "Offer",
        "itemOffered": {
          "@type": "Service",
          "name": "Optimización SEO Técnica",
          "description": "Auditoría y optimización técnica de SEO para mejorar el posicionamiento en Google."
        }
      }
    ]
  },
  "priceRange": "$$",
  "founder": {
    "@id": "https://edwinbayonaitmanager.online/#person"
  },
  "sameAs": [
    "https://github.com/edt092",
    "https://www.linkedin.com/in/edwin-bayona/"
  ]
}
</script>
```

### Block 3 — WebSite (ALTO)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "@id": "https://edwinbayonaitmanager.online/#website",
  "name": "Edwin Bayona IT Manager",
  "alternateName": "Edwin Bayona — Desarrollador Full-Stack",
  "url": "https://edwinbayonaitmanager.online/",
  "description": "Portfolio y servicios de Edwin Bayona, desarrollador full-stack freelance en Bucaramanga, Colombia.",
  "inLanguage": "es-CO",
  "publisher": {
    "@id": "https://edwinbayonaitmanager.online/#person"
  }
}
</script>
```

> Nota: El `SearchAction` en el WebSite schema solo aplica si el sitio tiene funcionalidad de búsqueda real (`?s=query`). Para este portfolio, omitirlo es lo correcto.

---

## Impacto Estimado

| Categoría | Actual | Después de Sprint 1+2 |
|-----------|--------|----------------------|
| Technical SEO | 41/100 | ~72/100 |
| Content Quality | 52/100 | ~68/100 |
| On-Page SEO | 30/100 | ~65/100 |
| Schema | 0/100 | ~80/100 |
| Performance (CWV) | 35/100 | ~60/100 |
| AI Search Readiness | 28/100 | ~58/100 |
| **Overall Score** | **35/100** | **~65/100** |

---

## Sesión de 2 Horas

> Mayor ROI por minuto. Estas acciones mueven el score de 35 → ~55/100 sin tocar el diseño ni la estructura.

| Paso | Tarea | Tiempo est. |
|------|-------|-------------|
| 1 | Crear `robots.txt` + `sitemap.xml` + `llms.txt` en raíz | 20 min |
| 2 | Agregar `<link rel="canonical">` + JSON-LD schemas (3 bloques) + meta robots | 30 min |
| 3 | Reescribir `<title>` y `<meta name="description">` | 10 min |
| 4 | Completar URL en `README.md` + agregar URL en campos "About" de repos de GitHub | 15 min |
| 5 | Agregar `loading="lazy"` + `width`/`height` a imágenes del proyecto | 15 min |
| 6 | Agregar OG tags faltantes (`og:url`, `og:image`, `og:type`, `og:locale`) | 10 min |
| 7 | Corregir año de copyright en footer (2025 → 2026) + texto Next.js | 5 min |
| 8 | Submittir sitemap en Google Search Console | 5 min |
| **Total** | | **~1h 50min** |

---

## Notas Finales

### El problema del dominio

`edwinbayonaitmanager.online` tiene dos issues permanentes:
- **"itmanager"** es un término de IT Operations, no de desarrollo web — confunde a clientes y a buscadores de IA
- **".online"** genera menor confianza que `.dev`, `.co` o `.com` en el mercado colombiano

No es urgente cambiarlo ahora (el dominio tiene inversión en links y reconocimiento incipiente), pero es un objetivo de 6–12 meses migrar a `edwinbayona.dev` o `edwinbayona.co` con un redirect 301 permanente.

### El problema del i18n

El sistema de 4 idiomas (ES/EN/FR/IT) es una deuda técnica real. Agrega ~1,100 líneas de JS, una dependencia de terceros (`ipapi.co`), y produce contenido multilingüe que ningún crawler ve. Para un freelancer que sirve principalmente al mercado colombiano, el retorno no justifica el costo. La recomendación es eliminar el sistema completamente o aceptar la complejidad de implementar URL separadas por idioma.

### Siguiente paso recomendado

Ejecutar la [Sesión de 2 Horas](#sesión-de-2-horas) y luego:
1. Verificar en Google Search Console que la URL se indexa correctamente
2. Correr PageSpeed Insights para medir LCP antes/después del build de Tailwind
3. Usar la herramienta [Rich Results Test](https://search.google.com/test/rich-results) para validar los schemas

---

*Informe generado el 2026-06-02 con Claude Code SEO Suite — 8 agentes de análisis paralelo: Technical SEO, Content/E-E-A-T, Schema, Sitemap, GEO/AI Search, SXO, Performance, Backlinks.*
