# Contexto del Proyecto — Web (toolcritic.co + iaprobada.com)
> Se carga automáticamente al abrir Claude Code en este directorio.
> Actualizar al final de cada sesión con: "actualiza E:\Webpages\toolcritic\CLAUDE.md con lo que hicimos hoy"
> Última actualización: 2026-05-12

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
- Enforce HTTPS: confirmar en GitHub Pages settings de cada repo

### Carpetas de contenido activas
```
toolcritic.co/         iaprobada.com/
├── /real-estate/      ├── /inmobiliarias/
├── /ecommerce/        ├── /tiendas-online/
├── /creators/         ├── /creadores/
└── /reviews/          └── /resenas/
```

### Artículos publicados
- `creators/otter-ai` + `creadores/otter-ai`
- `creators/descript` + `creadores/descript`

---

## Agente Web — Pipeline Completo

**Ubicación:** `E:\Webpages\agent\`
**Python:** usar siempre `py -3.11` (no `python`)

| Archivo | Responsabilidad |
|---|---|
| `agent_web.py` | Orquestador — generate + verify + fix + publish |
| `generator.py` | Artículos EN+ES con Claude API (niche-aware) |
| `verifier.py` | Verifica placeholders, precios, superlativos, disclosure |
| `corrector.py` | Auto-corrige: regex para blocks, Claude Haiku para warns |
| `publisher.py` | Guarda archivos por nicho + git push |
| `scraper.py` | Obtiene info del sitio de la herramienta |

### Pipeline automático
```
generate → save draft → verify → fix (regex + Haiku) → re-verify → publish si pasa | draft si bloqueado
```
Cero intervención humana en el flujo estándar. Solo interviene el usuario si hay bloqueos irresolubles.

### Nichos disponibles
`real-estate` | `ecommerce` | `creators` | `general`

### Tipos de artículo
`review` (por defecto) | `alternatives`

### Prompt del agente — mejoras aplicadas (2026-05-12)
- **`prompt_focus` por nicho:** instrucción específica de ángulo para cada nicho/idioma
- **Creators EN:** foco en podcast/video repurposing, NO "meeting tool"
- **Creators ES:** prueba de acentos latinoamericanos + mini-metodología
- **Divulgación condicional:** distinta cuando hay o no hay link de afiliado
- **CTA limpio:** "Visit [Tool]" con URL real cuando no hay afiliado

---

## Comandos del Agente Web

```powershell
cd "E:\Webpages\agent"

# Reseña con nicho + publish automático (pipeline completo)
py -3.11 agent_web.py generate --tool "Castmagic" --url "https://castmagic.io" --niche creators --publish
py -3.11 agent_web.py generate --tool "Lofty" --url "https://lofty.com" --niche real-estate --publish
py -3.11 agent_web.py generate --tool "Shopify Magic" --url "https://shopify.com" --niche ecommerce --publish

# Artículo de alternativas
py -3.11 agent_web.py generate --type alternatives --tool "Otter.ai" --url "https://otter.ai" --niche creators --publish

# Modo borrador (revisar antes de publicar)
py -3.11 agent_web.py generate --tool "Copy.ai" --url "https://copy.ai" --niche ecommerce
py -3.11 agent_web.py approve copy-ai

# Con links de afiliado
py -3.11 agent_web.py generate --tool "Surfer SEO" --url "https://surferseo.com" --niche creators --affiliate-en "https://link-en" --affiliate-es "https://link-es" --publish
```

---

## Plan de Contenido — Primeras 3 Semanas

### Semana 1 — Creators ✅ en progreso
| # | Herramienta | Estado |
|---|---|---|
| 1 | Otter.ai | ✅ Publicado |
| 2 | Descript | ✅ Publicado |
| 3 | Castmagic | ⏳ Pendiente |
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

## Próximos Pasos

- [ ] Terminar semana 1: Castmagic, OpusClip, Alternatives to Otter.ai
- [ ] Confirmar Enforce HTTPS en GitHub Pages settings de ambos repos
- [ ] Añadir comando `listicle` al agente para el artículo #10
- [ ] Crear página "Advertise with us" en ambos sitios (monetización directa)
- [ ] Activar Ezoic en ambos sitios (display ads de respaldo)
- [ ] Reemplazar MPT con pipeline Flux + Ken Burns + FFmpeg para videos
