# Contexto del Proyecto — Web (toolcritic.co + iaprobada.com)
> Se carga automáticamente al abrir Claude Code en este directorio.
> Actualizar al final de cada sesión con: "actualiza E:\Webpages\toolcritic\CLAUDE.md con lo que hicimos hoy"
> Última actualización: 2026-05-12

---

## Descripción General

Dos sitios web de reseñas de herramientas IA, construidos con **Hugo + tema PaperMod**, monetizados con **links de afiliados con comisión recurrente** (30-45%).

| Dominio | Idioma | Carpeta | Dominio comprado en |
|---|---|---|---|
| toolcritic.co | Inglés | `E:\Webpages\toolcritic` | Porkbun |
| iaprobada.com | Español | `E:\Webpages\iaprobada` | Namecheap |

**Objetivo de ingresos:** $3,000/mes por sitio → $6,000/mes en total.
**Plazo:** 3-4 meses.
**Modelo:** Afiliados recurrentes (el visitante se suscribe a una herramienta IA → se cobra comisión mensual mientras siga suscrito).

---

## Repositorios GitHub

- toolcritic → `github.com/Zergatuto/toolcritic`
- iaprobada → `github.com/Zergatuto/iaprobada`

Ambos usan **GitHub Pages** + **GitHub Actions** para deploy automático al hacer push a `main`.
El archivo `.github/workflows/hugo.yml` ya está configurado en los dos repos.

---

## Nichos y Arquitectura de Contenido (2026-05-12)

**3 nichos + 1 formato transversal:**

| Nicho | toolcritic.co | iaprobada.com | Por qué |
|---|---|---|---|
| Real estate | `/real-estate/` | `/inmobiliarias/` | Intención comercial alta, muy poco en español |
| Ecommerce | `/ecommerce/` | `/tiendas-online/` | Shopify masivo en LatAm, sin cubrir en ES |
| Creators | `/creators/` | `/creadores/` | Sinergia directa con canales YouTube propios |
| Alternativas *(formato)* | `/alternatives/` o dentro de cada nicho | `/alternativas/` | SEO long-tail, transversal a los 3 nichos |

**Estrategia:** Inglés ataca keywords globales de nicho. Español ataca sub-nichos sin competencia (ej: "herramientas IA para captación inmobiliaria").

---

## Estado Actual

### toolcritic.co ✅ ACTIVO
- Sitio Hugo + PaperMod configurado
- DNS en Porkbun apuntando a GitHub Pages
- Deploy automático funcionando
- `content/about.md` creado

### iaprobada.com ✅ ACTIVO
- Sitio Hugo + PaperMod configurado
- DNS propagado y verificado por GitHub Pages (2026-05-09)
- HTTPS activo ("DNS check successful" confirmado)
- Deploy automático funcionando (Actions en verde)

---

## Canales YouTube ✅ CREADOS

- `@ToolCritic` — inglés (cuenta Google dedicada al proyecto)
- `@IAProbada` — español (misma cuenta Google)
- Descripciones configuradas en ambos canales
- Banners creados con Canva

---

## Agente Web ✅ COMPLETO

**Ubicación:** `E:\Webpages\agent\`

| Agente | Archivo | Responsabilidad |
|---|---|---|
| Agente Web | `agent_web.py` | Artículos EN+ES → toolcritic.co + iaprobada.com |
| Agente Video | `agent_video.py` | Videos → YouTube ToolCritic + IAProbada |
| (Legacy) | `agent.py` | Hace ambas cosas, se mantiene por compatibilidad |

**Módulos compartidos:**
- `scraper.py` — obtiene info de la herramienta desde su web
- `generator.py` — artículos con Claude API (EN + ES)
- `publisher.py` — guarda archivos y hace git push a ambos repos
- `video_script_generator.py` — scripts de video con Claude API (EN + ES)
- `video_creator_mpt.py` — crea MP4 via MoneyPrinterTurbo + FFmpeg audio merge
- `video_creator.py` — solo se usa para generar thumbnails
- `youtube_uploader.py` — sube videos a YouTube Data API v3

**Archivo `.env` necesita:**
```
ANTHROPIC_API_KEY=sk-ant-tu-clave-aqui
TOOLCRITIC_PATH=E:\Webpages\toolcritic
IAPROBADA_PATH=E:\Webpages\iaprobada
VIDEOS_PATH=E:\Webpages\agent\videos
```

---

## Comandos del Agente Web

```powershell
cd "E:\Webpages\agent"

# Reseña individual con nicho
python agent_web.py generate --tool "Jasper AI" --url "https://jasper.ai" --niche real-estate --publish
python agent_web.py generate --tool "Shopify Magic" --url "https://shopify.com" --niche ecommerce --publish
python agent_web.py generate --tool "Descript" --url "https://descript.com" --niche creators --publish

# Artículo de alternativas (formato transversal)
python agent_web.py generate --type alternatives --tool "Jasper AI" --url "https://jasper.ai" --niche real-estate --publish

# Modo borrador (con checklist de verificación humana)
python agent_web.py generate --tool "Copy.ai" --url "https://copy.ai" --niche ecommerce
python agent_web.py approve copy-ai

# Con links de afiliado
python agent_web.py generate --tool "Surfer SEO" --url "https://surferseo.com" --niche creators --affiliate-en "https://link-en" --affiliate-es "https://link-es" --publish
```

**Nichos disponibles:** `real-estate` | `ecommerce` | `creators` | `general`
**Tipos de artículo:** `review` (por defecto) | `alternatives`

---

## MoneyPrinterTurbo

**Ubicación:** `E:\Webpages\MoneyPrinterTurbo\`
- Servicio REST en `http://127.0.0.1:8080`
- Config en `config.toml` — usa litellm + Claude Haiku + Pexels API
- **Arrancar antes de generar videos:**
  ```powershell
  cd E:\Webpages\MoneyPrinterTurbo
  venv\Scripts\activate
  $env:ANTHROPIC_API_KEY="sk-ant-..."
  python main.py
  ```
- El agente lo arranca automáticamente si no está corriendo

**⚠️ MPT será reemplazado:** El nuevo pipeline usará Together AI (Flux) + Ken Burns (FFmpeg) + Edge TTS. Más rápido, sin bugs, sin Pexels genérico.

---

## Programas de Afiliados

**Cuenta PartnerStack creada** — perfil de red pendiente de aprobación (toolcritic.co)

| Herramienta | Comisión | Estado | Plataforma |
|---|---|---|---|
| Jasper AI | — | ❌ Sin programa activo | jasper.ai |
| Copy.ai | 45% | ❌ Sin programa activo | copy.ai |
| Synthesia | 25% recurrente | ❌ Skip — solo PayPal | synthesia.io |
| Writesonic | 30% recurrente de por vida | ⏳ Esperando aprobación | PartnerStack |
| Frase | 30% recurrente | ⏳ Esperando aprobación | PartnerStack |
| Surfer SEO | 25% recurrente | ⏳ Esperando aprobación | PartnerStack |
| GetResponse | 40-60% recurrente 12 meses | ⏳ Esperando aprobación | PartnerStack |
| Prezi | 50% por suscripción | ⏳ Esperando aprobación | PartnerStack |
| Castmagic | 30% recurrente 18 meses | ⏳ Esperando aprobación | PartnerStack |
| ElevenLabs | 22% recurrente 12 meses | ⏳ Aplicado 2026-05-10 | PartnerStack |
| Gamma | 30% primer año | ⏳ Aplicado 2026-05-10 | PartnerStack |
| MeetGeek | 30% recurrente de por vida | ⏳ Aplicado 2026-05-10 | PartnerStack |
| Reclaim.ai | 40% recurrente 12 meses | ⏳ Aplicado 2026-05-10 | PartnerStack |
| Descript | $25 por conversión | ⏳ Aplicado 2026-05-10 | PartnerStack |
| Pictory | 20% recurrente | ⏳ Aplicado 2026-05-10 | pictory.ai |

Para reemplazar placeholder `[ADD_AFFILIATE_LINK]` / `[AGREGAR_LINK_AFILIADO]` con el link real cuando llegue aprobación.

---

## Estrategia de Contenido

- **Tipos de artículos:** Reseñas individuales, comparativas ("X vs Y"), listicles ("Top 7 herramientas para..."), alternativas ("Alternativas a X")
- **Ritmo:** 3-4 artículos/día + videos en cada sitio/canal con el agente
- **Revisión humana:** Mínima — el agente genera contenido de calidad suficiente para publicar directo

**Lista de herramientas prioritarias:**

| Herramienta | Categoría | URL |
|---|---|---|
| Jasper AI | Escritura IA | jasper.ai |
| Writesonic | Escritura IA | writesonic.com |
| Copy.ai | Escritura IA | copy.ai |
| Frase | SEO + Escritura | frase.io |
| Synthesia | Video IA | synthesia.io |
| Pictory | Video IA | pictory.ai |
| Descript | Video/Audio IA | descript.com |
| Midjourney | Imágenes IA | midjourney.com |
| Leonardo AI | Imágenes IA | leonardo.ai |
| ElevenLabs | Voz IA | elevenlabs.io |
| Murf AI | Voz IA | murf.ai |
| Notion AI | Productividad | notion.so |
| Otter.ai | Transcripción | otter.ai |
| Grammarly | Escritura | grammarly.com |
| Surfer SEO | SEO IA | surferseo.com |

**Calendario sugerido:**
- Semana 1-2: 2-3 artículos/día (período de calentamiento)
- Semana 3+: 3-4 artículos/día constante
- Mezclar tipos: no solo reseñas — intercalar comparativas y listicles

---

## Protección Anti-Ban Google (E-E-A-T)

### Páginas existentes en ambos sitios ✅
- Página de autor
- Página About
- Página de Contacto
- Página de Privacidad / Términos
- Google Search Console configurado + sitemaps enviados

### En el agente (ya aplicado)
- Artículos en primera persona plural ("During our testing...", "We found...")
- Ratings no perfectos (evitar 9+/10 salvo casos excepcionales)
- Incluir contras reales en cada reseña
- Variar estructura ligeramente entre artículos

---

## Próximos Pasos

- [ ] **Reemplazar MPT con pipeline Flux + Ken Burns + FFmpeg**
  - Crear cuenta en together.ai y obtener API key
  - Nuevo `video_creator_flux.py`: Flux → Ken Burns (FFmpeg zoompan) → Edge TTS → subtítulos FFmpeg
  - ~$0.003/imagen, 14 imágenes/video = $0.04/video
- [ ] **Actualizar agente para guardar links de afiliado automáticamente** (cuando lleguen aprobaciones)
- [ ] **Empezar publicación masiva** con `agent_web.py` (2-3/día semana 1)
- [ ] **YouTube Shorts + TikTok** (mes 2, cuando haya tracción en el canal)
