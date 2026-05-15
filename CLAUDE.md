# Contexto del Proyecto — Web (toolcritic.co + iaprobada.com)
> Se carga automáticamente al abrir Claude Code en este directorio.
> Actualizar al final de cada sesión con: "actualiza E:\Webpages\toolcritic\CLAUDE.md con lo que hicimos hoy"
> Última actualización: 2026-05-15 (sesión 4)

---

## Descripción General

Dos sitios web de reseñas de herramientas IA, construidos con **Hugo + tema PaperMod**.
**Modelo de monetización actual:** contenido patrocinado directo + display ads (Ezoic) — no afiliados.
PartnerStack rechazó la cuenta por ser sitio nuevo. Se retomará cuando haya tráfico.

| Dominio | Idioma | Carpeta | Dominio comprado en |
|---|---|---|---|
| toolcritic.co | Inglés | `E:\Webpages\toolcritic` | Porkbun |
| iaprobada.com | Español | `E:\Webpages\iaprobada` | Namecheap |

**Objetivo de ingresos:** $3,000/mes por sitio → $6,000/mes en total.
**Plazo:** 3-4 meses.

---

## Repositorios GitHub

- toolcritic → `github.com/Zergatuto/toolcritic`
- iaprobada → `github.com/Zergatuto/iaprobada`

Ambos usan **GitHub Pages** + **GitHub Actions** para deploy automático al hacer push a `main`.
El archivo `.github/workflows/hugo.yml` ya está configurado en los dos repos.

---

## Nichos y Arquitectura de Contenido

**3 nichos + 1 formato transversal (alternativas):**

| Nicho | toolcritic.co | iaprobada.com | Por qué |
|---|---|---|---|
| Creators | `/creators/` | `/creadores/` | Sinergia con canales YouTube propios, arranque semana 1 |
| Real estate | `/real-estate/` | `/inmobiliarias/` | Intención comercial alta, muy poco en español |
| Ecommerce | `/ecommerce/` | `/tiendas-online/` | Shopify masivo en LatAm, sin cubrir en ES |
| Alternativas *(formato)* | dentro de cada nicho | dentro de cada nicho | SEO long-tail, transversal |

**Estrategia:** Inglés ataca keywords globales de nicho. Español ataca sub-nichos sin competencia.
**Alternativas:** no es un nicho separado — es un formato que refuerza todos los clusters.

---

## Estado Actual de los Sitios

### Configuración técnica ✅
- `languageCode = "es"` en iaprobada (HTML `lang="es"` correcto)
- hreflang entre dominios configurado en `layouts/partials/extend_head.html`
- Footer sin "Powered by Hugo & PaperMod" (override en `layouts/partials/footer.html`)
- og:image default 1200×630 en `static/images/og-default.png` (ambos sitios)
- RSS button oculto (`ShowRssButtonInSectionTermList = false`)
- `generate_og_images.py` en `E:\Webpages\agent\` para regenerar imágenes OG
- Enforce HTTPS: ✅ activado en GitHub Pages (ambos repos)

### Cloudflare ✅ (configurado 2026-05-15)
Ambos dominios en Cloudflare (plan Free), nameservers activos.

| Configuración | toolcritic.co | iaprobada.com |
|---|---|---|
| Zone ID | `4b0cee7a7af4e56e4391a62199c044bd` | `5fee626f5aae08bb4ad871d663bf7dde` |
| Always Use HTTPS | ✅ ON | ✅ ON |
| HSTS | ✅ 6 meses + includeSubDomains | ✅ 6 meses + includeSubDomains |
| X-Content-Type-Options | ✅ nosniff | ✅ nosniff |
| X-Frame-Options | ✅ SAMEORIGIN | ✅ SAMEORIGIN |
| Referrer-Policy | ✅ strict-origin-when-cross-origin | ✅ strict-origin-when-cross-origin |
| DNS propagado | ⏳ Porkbun propagando | ✅ Activo |

**Credenciales API (no commitear):**
- Email: `toolcritic.io@gmail.com`
- Global API Key: en `$env:CF_KEY` (pedir al usuario si se necesita de nuevo)
- Auth: `X-Auth-Email` + `X-Auth-Key` headers

### Carpetas de contenido activas
```
toolcritic.co/         iaprobada.com/
├── /real-estate/      ├── /inmobiliarias/
├── /ecommerce/        ├── /tiendas-online/
├── /creators/         ├── /creadores/
└── /reviews/          └── /resenas/
```

### Artículos publicados
| Slug | toolcritic.co | iaprobada.com | rating |
|---|---|---|---|
| descript | ✅ | ✅ | 8.0 / 7.4 |
| castmagic | ✅ | ✅ | 7.5 / 7.2 |
| opusclip | ✅ | ✅ | 7.5 / 7.5 |
| jasper-ai | ✅ (reviews/) | ✅ (resenas/) | 7.2 / 7.2 |
| writesonic | ✅ (reviews/) | ✅ (resenas/) | 7.8 / 7.4 |
| otter-ai | draft | draft | — |

Todos los artículos publicados tienen: `tool_name`, `rating`, schema JSON-LD, alt text descriptivo, títulos <60 chars.

---

## Agente Web — Pipeline Completo

**Ubicación:** `E:\Webpages\agent\`
**Python:** usar siempre `py -3.11` (no `python`)

| Archivo | Responsabilidad |
|---|---|
| `agent_web.py` | Orquestador — keyword research + generate + verify + fix + publish |
| `generator.py` | Artículos EN+ES con Claude API (niche-aware + keyword context) |
| `keyword_researcher.py` | Keywords EN+ES via Google Autocomplete + pytrends (gratis, sin auth) |
| `verifier.py` | Verifica placeholders, precios, superlativos, disclosure |
| `corrector.py` | Auto-corrige: regex para blocks, Claude Haiku para warns |
| `publisher.py` | Guarda archivos por nicho + git push |
| `scraper.py` | Obtiene info del sitio de la herramienta |
| `review_state.py` | Estado editorial por artículo (JSON en `review_state/`) |
| `editor_dashboard.py` | Dashboard Flask — puerta editorial antes de publicar |

### Pipeline automático
```
keyword_research (Autocomplete + Trends EN+ES)
  → scrape tool site
  → generate EN+ES (con keyword context diferenciado por idioma)
  → save draft → verify → fix (regex + Haiku) → re-verify
    ↓ bloqueado/warnings        ↓ todo limpio
    → estado en dashboard       → publish directo (con --publish)
    → revisar manualmente
    → agente aplica datos verificados → re-verify → publicar desde dashboard
```

**Flujo recomendado:** sin `--publish` para artículos nuevos. El dashboard es la puerta editorial.
Solo usar `--publish` directo para artículos de bajo riesgo sin precios ni afiliados.

### Nichos disponibles
`real-estate` | `ecommerce` | `creators` | `general`

### Tipos de artículo
`review` (por defecto) | `alternatives`

### Mejoras aplicadas al pipeline (2026-05-14)

**keyword_researcher.py (nuevo):**
- Consulta Google Autocomplete para el tool en EN (US) y ES (MX): búsquedas base, review, vs, alternativas
- Consulta pytrends (Google Trends) para keywords relacionadas y emergentes por idioma
- `format_keyword_context()` convierte los datos en un bloque de texto para los prompts
- Tolerante a fallos: si pytrends falla (rate limit de Google), el autocomplete sigue funcionando
- Dependencia: `py -3.11 -m pip install pytrends`

**generator.py:**
- `generate_articles()` acepta `keyword_en` y `keyword_es`
- Cada función `_generate_review_*` y `_generate_alternatives_*` recibe `keyword_context`
- El contexto de keywords se inserta en el prompt después del NICHE FOCUS — el artículo EN ataca lo que buscan en inglés, el ES lo que buscan en español (ángulos distintos, no traducción)

**agent_web.py:**
- Llama a `research_keywords()` entre el scraping y la generación
- Pasa `keyword_en` y `keyword_es` a `generate_articles()`

**hugo.toml (ambos sitios, 2026-05-14):**
- `disableKinds = ["taxonomy", "term"]` — elimina páginas /tags/, /categories/, /series/ que Google reportaba como "página alternativa con canonical adecuado" en Search Console

### Mejoras aplicadas al pipeline (2026-05-12)

**generator.py:**
- `_clean_response()` — elimina code fences ` ```toml ``` ` que Claude a veces envuelve alrededor del frontmatter TOML (Hugo no los parsea)
- `tool_url` propagado a todas las funciones — CTA usa URL real de la herramienta cuando no hay afiliado (no slugify.com inventado)
- `prompt_focus` por nicho: ángulo específico por nicho/idioma
- Creators EN: foco en podcast/video repurposing, NO "meeting tool"
- Creators ES: prueba de acentos latinoamericanos + mini-metodología
- Divulgación condicional: distinta cuando hay o no hay link de afiliado

**scraper.py:**
- Ahora raspa homepage + pricing page (`/pricing`, `/plans`, `/pricing/`)
- Devuelve sección `[PRICING PAGE]` separada para que el generador use precios reales
- Limitación: sitios con precios JS-rendered (ej. Castmagic) no exponen precios en HTML → el verifier los bloquea correctamente

**verifier.py:**
- Nuevas reglas BLOCK: `[verificar en ...]` y `[verify at ...]` — placeholders que el corrector inserta cuando no puede verificar precios; ahora bloquean publicación automática

**corrector.py:**
- `apply_structured_edits(filepath, structured_data, notes)` — Claude Haiku aplica precios/links verificados manualmente al .md antes de re-verificar
- Sanity check: si la respuesta tiene <70% de la longitud original, no sobreescribe

**agent_web.py:**
- Pasa `tool_url` al generador
- Al terminar sin `--publish`, crea estado en `review_state/*.json` vía `create_state()`
- `en_issues` y `es_issues` guardados por separado en el estado

---

## Comandos del Agente Web

```powershell
cd "E:\Webpages\agent"

# Flujo recomendado: generar sin --publish → revisar en dashboard → publicar desde dashboard
py -3.11 agent_web.py generate --tool "Castmagic" --url "https://castmagic.io" --niche creators
py -3.11 agent_web.py generate --tool "OpusClip" --url "https://opus.pro" --niche creators
py -3.11 agent_web.py generate --tool "Lofty" --url "https://lofty.com" --niche real-estate
py -3.11 agent_web.py generate --tool "Shopify Magic" --url "https://shopify.com" --niche ecommerce

# Artículo de alternativas
py -3.11 agent_web.py generate --type alternatives --tool "Otter.ai" --url "https://otter.ai" --niche creators

# Con links de afiliado (cuando los tengamos)
py -3.11 agent_web.py generate --tool "Surfer SEO" --url "https://surferseo.com" --niche creators --affiliate-en "https://link-en" --affiliate-es "https://link-es"

# Publish directo (solo si no hay precios ni datos críticos que verificar)
py -3.11 agent_web.py generate --tool "Tool" --url "https://tool.com" --niche creators --publish
```

## Editor Dashboard

**Iniciar:**
```powershell
cd "E:\Webpages\agent"
py -3.11 editor_dashboard.py
# → http://localhost:5050  (esta PC)
# → http://{ip-local}:5050 (cualquier PC en la misma WiFi)
```

**Instalar dependencias (una vez):**
```powershell
py -3.11 -m pip install flask markdown python-dotenv
```

### Archivos del dashboard
```
E:\Webpages\agent\
├── editor_dashboard.py       Flask app (puerto 5050, host 0.0.0.0)
├── review_state.py           Estado editorial — JSON por artículo
├── review_state/             Directorio con *.json (uno por slug)
└── editor_templates/
    ├── base.html             Layout, CSS, badges, botones
    ├── index.html            Lista de artículos con filtros y sync
    └── review.html           Vista de revisión: Preview / Markdown / Checks
```

### Estados del artículo
| Estado | Significado |
|---|---|
| `needs_manual_input` | Generado con warnings — verificar datos manualmente |
| `blocked` | Bloqueado por issues críticos |
| `published_with_issues` | Publicado pero con warnings activos |
| `agent_reviewing` | El agente está aplicando datos verificados + re-verificando |
| `agent_failed` | El agente falló (ver error en dashboard) |
| `ready_for_review` | Todos los checks manuales hechos — listo para enviar al agente |
| `ready_to_publish` | El agente confirmó que está limpio — botón Publicar activo |
| `published` | Publicado en ambos sitios |

### Flujo editorial completo
1. Generar artículo sin `--publish` → aparece en dashboard
2. Dashboard → tab **Checks**: marcar checkboxes, rellenar precios/planes/afiliados, notas
3. Clic **"Listo para revisión del agente"** → agente aplica datos + re-verifica en background
4. Cuando el estado cambia a `ready_to_publish` → clic **"Publicar"**
5. **Sync Articles** en el índice: sincroniza todos los artículos de ambos repos (preserva checks/notas existentes)

### Capturas de pantalla
Se guardan en:
- `E:\Webpages\toolcritic\static\images\{section}\{slug}\` (EN)
- `E:\Webpages\iaprobada\static\images\{section}\{slug}\` (ES)

Subir desde el dashboard (tab Checks → formularios de upload al final).
Cuando el agente corre, detecta automáticamente las imágenes en esa carpeta y las inserta en el artículo antes de la sección de Conclusión/Precios.

### Afiliado
Checkbox "Tiene link de afiliado" en el dashboard:
- **Desmarcado** → el agente elimina todos los placeholders de afiliado y texto de divulgación del artículo
- **Marcado** → muestra campos de URL para EN y ES, el agente los aplica al artículo

### Artículos publicados con issues
El estado `published_with_issues` (en lugar de `blocked`) se usa para artículos ya en vivo que tienen issues. Muestra un banner amarillo de advertencia en lugar del rojo de bloqueo.

---

## Plan de Contenido — Primeras 3 Semanas

### Semana 1 — Creators ✅ en progreso
| # | Herramienta | Estado |
|---|---|---|
| 1 | Otter.ai | ✅ Publicado |
| 2 | Descript | ✅ Publicado |
| 3 | Castmagic | 🔶 Generado — revisar precios en dashboard |
| 4 | OpusClip | ⏳ Pendiente |
| 5 | Alternatives to Otter.ai | ⏳ Pendiente |

### Semana 2 — Real Estate
| # | Herramienta | Comando |
|---|---|---|
| 6 | Lofty | `--tool "Lofty" --url "https://lofty.com" --niche real-estate` |
| 7 | Structurely | `--tool "Structurely" --url "https://structurely.com" --niche real-estate` |
| 8 | Canva | `--tool "Canva" --url "https://canva.com" --niche real-estate` |
| 9 | ChatGPT | `--tool "ChatGPT" --url "https://openai.com/chatgpt" --niche real-estate` |
| 10 | Best AI tools for real estate | listicle *(pendiente añadir al agente)* |

### Semana 3 — Ecommerce
| # | Herramienta | Comando |
|---|---|---|
| 11 | Shopify Magic | `--tool "Shopify Magic" --url "https://shopify.com" --niche ecommerce` |
| 12 | Describely | `--tool "Describely" --url "https://describely.ai" --niche ecommerce` |
| 13 | Tidio | `--tool "Tidio" --url "https://tidio.com" --niche ecommerce` |
| 14 | Klaviyo | `--tool "Klaviyo" --url "https://klaviyo.com" --niche ecommerce` |
| 15 | Alternatives to Jasper | `--type alternatives --tool "Jasper AI" --url "https://jasper.ai" --niche ecommerce` |

---

## Programas de Afiliados

**PartnerStack:** rechazado por sitio nuevo. Retomar cuando haya tráfico.

| Herramienta | Comisión | Estado |
|---|---|---|
| Writesonic | 30% recurrente | ⏳ Aplicado PartnerStack |
| Frase | 30% recurrente | ⏳ Aplicado PartnerStack |
| Surfer SEO | 25% recurrente | ⏳ Aplicado PartnerStack |
| GetResponse | 40-60% recurrente | ⏳ Aplicado PartnerStack |
| Castmagic | 30% recurrente 18 meses | ⏳ Aplicado PartnerStack |
| ElevenLabs | 22% recurrente 12 meses | ⏳ Aplicado PartnerStack |
| Gamma | 30% primer año | ⏳ Aplicado PartnerStack |
| MeetGeek | 30% recurrente | ⏳ Aplicado PartnerStack |
| Reclaim.ai | 40% recurrente 12 meses | ⏳ Aplicado PartnerStack |
| Descript | $25 por conversión | ⏳ Aplicado PartnerStack |
| Pictory | 20% recurrente | ⏳ Aplicado pictory.ai |

---

## Protección Anti-Ban Google (E-E-A-T)

- Páginas de autor, About, Contacto, Privacidad/Términos en ambos sitios ✅
- Google Search Console configurado + sitemaps enviados ✅
- Artículos en primera persona plural ("During our testing...", "We found...")
- Ratings no perfectos (evitar 9+/10 salvo casos excepcionales)
- Contras reales en cada reseña
- Prompt del agente con ángulo de nicho específico por idioma (no traducción literal)

---

## Roadmap SEO — Estado

### Sprint 1 ✅ (2026-05-14/15)
- [x] Schema JSON-LD (Review, BreadcrumbList, Organization) en extend_head.html
- [x] `tool_name` + `rating` en frontmatter de todos los artículos publicados
- [x] Generador: títulos <60 chars, disclosure estandarizado, rating/tool_name automático
- [x] Alt text descriptivo en corrector.py (artículos nuevos)
- [x] Alt text actualizado en artículos existentes
- [x] Fix code fence bug opusclip iaprobada

### Sprint 2 ✅ (2026-05-15)
- [x] Hreflang para homepage (.IsHome) y secciones (.IsSection)
- [x] Author `Person` en JSON-LD (Alex Turner / Alejandro Torres → /author/)
- [x] About pages expandidas con metodología, criterios de rating, independencia editorial

### Sprint 3 ✅ (2026-05-15)
- [x] 6 imágenes renombradas con nombres SEO (`{tool}-review-screenshot.png`)
- [x] Títulos acortados <60 chars en todos los artículos existentes
- [x] Section _index.md con cuerpo de contenido (real-estate, ecommerce, inmobiliarias, tiendas-online)

### Sprint 4 ✅ (2026-05-15)
- [x] `noindex = true` en privacy.md y contact.md (ambos sitios)
- [x] `meta referrer` en extend_head.html
- [x] `static/robots.txt` con Allow + Sitemap
- [x] `extend_post_content.html` — Related Posts automático por tags

### Cloudflare ✅ (2026-05-15)
- [x] Ambos dominios en Cloudflare (Free plan)
- [x] Always HTTPS ON en ambas zonas
- [x] HSTS 6 meses + includeSubDomains
- [x] Transform Rules: X-Content-Type-Options, X-Frame-Options, Referrer-Policy
- [ ] Verificar headers toolcritic.co cuando Porkbun propague (~horas)

## Próximos Pasos

### Contenido
- [ ] Publicar otter-ai (draft en ambos repos) — revisar y cambiar `draft = false`
- [ ] Semana 2: Real Estate (Lofty, Structurely, Canva, ChatGPT)
- [ ] Semana 3: Ecommerce (Shopify Magic, Describely, Tidio, Klaviyo)
- [ ] Estrategia de clusters SEO + backlinks
- [ ] Instalar pytrends: `py -3.11 -m pip install pytrends`
- [ ] Añadir comando `listicle` al agente para artículos de tipo lista

### Monetización
- [ ] Crear página "Advertise with us" en ambos sitios
- [ ] Retomar PartnerStack cuando haya tráfico
- [ ] Activar Ezoic en ambos sitios (display ads de respaldo)

### Técnico
- [ ] Configurar reenvío de correo en Porkbun (`contact@toolcritic.co`) y Namecheap (`contacto@iaprobada.com`)
- [ ] Verificar headers de toolcritic.co cuando propague DNS de Porkbun

## Notas técnicas

**Code fence bug (resuelto 2026-05-12):** Claude a veces devuelve el frontmatter TOML envuelto en ` ```toml ``` `. Hugo renderiza eso como bloque de código en lugar de parsearlo. `_clean_response()` en generator.py lo elimina automáticamente. El artículo de Castmagic en iaprobada ya fue corregido manualmente.

**Precios JS-rendered:** sitios como Castmagic renderizan precios con JavaScript. El scraper obtiene texto genérico ("per month, billed annually") pero no cifras. El verifier bloquea publicación directa → el artículo queda en dashboard para que el editor rellene precios manualmente.

**Metodología honesta (resuelto 2026-05-12):** El generador ya no instruye a Claude a afirmar testing de semanas. Usa voz editorial basada en investigación ("Based on our research...", "Según nuestra investigación..."). Artículos ya publicados corregidos: jasper-ai.md, writesonic.md, otter-ai.md (EN).

**Email de contacto:** Las páginas de contacto apuntan a `contact@toolcritic.co` y `contacto@iaprobada.com`. Pendiente configurar reenvío de correo en Porkbun (toolcritic) y Namecheap (iaprobada) para que lleguen al correo personal.
