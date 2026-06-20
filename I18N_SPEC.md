# WorldTech — Language Toggle Spec (EN ⟷ ES)

This spec is the source of truth for adding a language toggle to `index.html`.
Claude wrote the copy + UX here; Codex implements the JavaScript mechanism.

## Goal
Add an English/Spanish toggle to the existing single-page site. **English is the default.**
A visible button in the navbar switches the whole page between EN and ES.

## Behavior requirements
1. **Default language = English (`en`).** On first visit (no saved choice), render English.
2. **Persist** the choice in `localStorage` under key `wt-lang` (`"en"` or `"es"`). On load, read it; if absent, use `en`.
3. Update `document.documentElement.lang` to `en`/`es` when switching.
4. Also update the **`<title>`** and **`<meta name="description">`** when switching (keys `meta.title`, `meta.desc`).
5. The **React FAQ** must re-render in the active language when toggled (pass the language in / re-render on change).
6. No flash of the wrong language: apply the saved/default language as early as possible.
7. Keep the code **beginner-friendly and minimal** — plain JS in `script.js` (alongside the existing jQuery), well commented in simple terms. The dictionary can live in `script.js` or a new `i18n.js` loaded before it.

## Toggle button UX
- Place a button in the navbar (`<header class="nav">`), **always visible including on mobile** (do not hide it inside the hamburger).
- The button shows the language you'll switch **to**: label `"ES"` while English is active, `"EN"` while Spanish is active. Give it `id="lang-toggle"`, `type="button"`, and an `aria-label` (`"Switch to Spanish"` / `"Cambiar a inglés"`).
- Style it to match the existing nav (small, subtle pill/outline). Claude will refine styling after; a clean minimal default is fine.

## Implementation approach (suggested)
- Add a `data-i18n="key"` attribute to each text element below. For strings that contain inner HTML (a `<span>` or `<b>`), set `innerHTML` from the dictionary (these are flagged **(HTML)** below).
- Build a dictionary `T = { en: { key: "..." }, es: { key: "..." } }`.
- A `setLang(lang)` function loops over `[data-i18n]` elements, looks up the key, and sets text/HTML; updates title/meta/`html lang`/button label/`aria-label`; saves to localStorage; triggers FAQ re-render.
- Keep the existing Spanish text in the HTML as the no-JS fallback is **not required** — it's fine to populate from the dictionary on load.

---

## TRANSLATION DICTIONARY (key — English — Spanish)

### Head / meta
- `meta.title` — `WorldTech — We solve your business's digital problems` — `WorldTech — Resolvemos los problemas digitales de tu negocio`
- `meta.desc` — `Is your business invisible online or losing customers? WorldTech fixes it: more customers, more trust, and a professional 24/7 presence. We connect your business to the world.` — `¿Tu negocio no aparece en internet o pierdes clientes? En WorldTech lo resolvemos: más clientes, más confianza y presencia profesional 24/7. Conectamos tu negocio al mundo.`

### Navbar
- `nav.services` — `Services` — `Servicios`
- `nav.why` — `Why?` — `¿Por qué?`
- `nav.portfolio` — `Portfolio` — `Portafolio`
- `nav.packages` — `Packages` — `Paquetes`
- `nav.cta` — `Let's talk` — `Hablemos`
- `nav.menuAria` (hamburger aria-label) — `Open menu` — `Abrir menú`

### Hero
- `hero.eyebrow` — `DIGITAL SOLUTIONS FOR YOUR BUSINESS` — `SOLUCIONES DIGITALES PARA TU NEGOCIO`
- `hero.title` **(HTML)** — `We connect your business <span>to the world</span>` — `Conectamos tu negocio <span>al mundo</span>`
- `hero.text` — `Is your business invisible online or losing customers? We fix that. You tell us the problem and we handle everything.` — `¿Tu negocio no aparece en internet o pierdes clientes? Nosotros lo resolvemos. Tú nos cuentas el problema y nos encargamos de todo.`
- `hero.btnPrimary` — `Tell us your problem` — `Cuéntanos tu problema`
- `hero.btnGhost` — `See portfolio` — `Ver portafolio`

### Hook
- `hook.title` **(HTML)** — `How many customers are you losing because they can't find you <span>online</span>?` — `¿Cuántos clientes pierdes porque no te encuentran <span>en internet</span>?`
- `hook.text` — `Every day your business isn't online is a sale that goes to your competition.` — `Cada día que tu negocio no está online es una oportunidad de venta que se va con la competencia.`

### Services
- `services.title` — `What problem do we solve?` — `¿Qué problema resolvemos?`
- `services.sub` — `We don't sell websites: we solve what's holding your business back. Here are the most common roadblocks — and how we fix them.` — `No vendemos páginas web: resolvemos lo que no deja crecer a tu negocio. Estas son las trabas más comunes — y cómo las solucionamos.`
- `card1.title` — `"They can't find me"` — `«No me encuentran»`
- `card1.text` — `We build your online presence so customers find you when they search on Google.` — `Creamos tu presencia en internet para que tus clientes te hallen cuando te buscan en Google.`
- `card2.title` — `"My site looks old"` — `«Mi página se ve vieja»`
- `card2.text` — `Modern, custom design that looks perfect on phone, tablet, and computer.` — `Diseño moderno y a medida que se ve perfecto en celular, tablet y computadora.`
- `card3.title` — `"I lose sales at night"` — `«Pierdo ventas de noche»`
- `card3.text` — `Your business available 24/7, with info, contact, and WhatsApp always one click away.` — `Tu negocio disponible 24/7, con información, contacto y WhatsApp siempre a un clic.`
- `card4.title` — `"I need something custom"` — `«Necesito algo a medida»`
- `card4.text` — `Bookings, catalogs, or live data connected to your business with React and APIs.` — `Reservas, catálogos o datos en vivo conectados a tu negocio con React y APIs.`

### Why WorldTech
- `why.title` — `What changes for your business` — `Lo que cambia para tu negocio`
- `why.badTitle` — `❌ Without a website` — `❌ Sin página web`
- `why.bad1` — `Customers can't find you` — `Tus clientes no pueden encontrarte`
- `why.bad2` — `You lose sales after hours` — `Pierdes ventas fuera de horario`
- `why.bad3` — `Less credibility for your brand` — `Menor credibilidad para tu marca`
- `why.bad4` — `You depend only on social media` — `Dependes solo de las redes sociales`
- `why.bad5` — `Your competition wins your customers` — `Tu competencia gana tus clientes`
- `why.goodTitle` — `✅ With WorldTech` — `✅ Con WorldTech`
- `why.good1` — `Your business available 24/7` — `Tu negocio disponible 24/7`
- `why.good2` — `You get more leads and sales` — `Generas más contactos y ventas`
- `why.good3` — `More trust and professionalism` — `Mayor confianza y profesionalismo`
- `why.good4` — `More visibility on Google` — `Más visibilidad en Google`
- `why.good5` — `You drive your business's growth` — `Impulsas el crecimiento de tu negocio`

### Portfolio
- `portfolio.title` — `Our work` — `Nuestros trabajos`
- `portfolio.sub` — `Real projects, live and working.` — `Proyectos reales, en vivo y funcionando.`
- `work1.tag` — `API · Live data` — `API · Datos en vivo`
- `work1.title` — `Criollos en MLB` — `Criollos en MLB`  *(proper name — same in both)*
- `work1.text` — `An app showing active Venezuelan players in MLB with real-time data from the official API. Bilingual, with search.` — `App que muestra venezolanos activos en la MLB con datos en tiempo real desde la API oficial. Bilingüe y con buscador.`
- `work1.btn` — `View live →` — `Ver en vivo →`

### Certification
- `cert.title` — `Certified training` — `Formación certificada`
- `cert.sub` — `Behind WorldTech is real, professional training.` — `Detrás de WorldTech hay preparación profesional y real.`
- `cert.imgTitle` (button title attr) — `Enlarge certificate` — `Ampliar certificado`
- `cert.verified` — `✓ Verified certificate` — `✓ Certificado verificado`
- `cert.program` — `Software Development Career Program` — `Software Development Career Program`  *(proper name — same)*
- `cert.issuer` — `Springboard · November 2025` — `Springboard · Noviembre 2025`
- `cert.desc` — `A program with 1:1 mentorship and projects reviewed by industry experts.` — `Programa con mentorías 1:1 y proyectos evaluados por expertos de la industria.`
- `cert.btn` — `View full certificate →` — `Ver certificado completo →`

### Process
- `process.title` — `How we work` — `Cómo trabajamos`
- `step1.title` — `We listen` — `Escuchamos`
- `step1.text` — `We understand your problem and what your business needs.` — `Entendemos tu problema y lo que necesita tu negocio.`
- `step2.title` — `We design` — `Diseñamos`
- `step2.text` — `We create the perfect solution for you.` — `Creamos la solución perfecta para ti.`
- `step3.title` — `We launch` — `Impulsamos`
- `step3.text` — `We bring your business into the digital world.` — `Llevamos tu negocio al mundo digital.`
- `step4.title` — `We grow with you` — `Crecemos contigo`
- `step4.text` — `Results that make an impact.` — `Resultados que generan impacto.`

### Packages
- `packages.title` — `Choose by your problem` — `Elige según tu problema`
- `packages.sub` — `Every business starts from a different place. Find yours — and if you're unsure, tell us and we'll recommend the right one. Quotes are always free.` — `Cada negocio parte de un punto distinto. Encuentra el tuyo — y si tienes dudas, cuéntanos y te decimos cuál te conviene. La cotización siempre es gratis.`
- `plan.basic.name` — `Basic` — `Básico`
- `plan.basic.for` — `If you're not online yet` — `Si aún no existes en internet`
- `plan.basic.maint` **(HTML)** — `Optional maintenance: <b>$25/mo</b>` — `Mantenimiento opcional: <b>$25/mes</b>`
- `plan.basic.f1` — `1-section page (landing)` — `Página de 1 sección (landing)`
- `plan.basic.f2` — `Mobile-friendly` — `Adaptada a celulares`
- `plan.basic.f3` — `WhatsApp & call button` — `Botón de WhatsApp y llamada`
- `plan.basic.f4` — `Social media links` — `Enlaces a redes sociales`
- `plan.basic.f5` — `Domain + hosting (1st year included)` — `Dominio + hosting (1er año incluido)`
- `plan.basic.f6` — `3 revisions included` — `3 revisiones incluidas`
- `plan.pro.badge` — `Most popular` — `Más popular`
- `plan.pro.name` — `Professional` — `Profesional`
- `plan.pro.for` — `If you want to look professional and sell more` — `Si quieres verte profesional y vender más`
- `plan.pro.maint` **(HTML)** — `Optional maintenance: <b>$45/mo</b>` — `Mantenimiento opcional: <b>$45/mes</b>`
- `plan.pro.f1` — `Everything in Basic` — `Todo lo del plan Básico`
- `plan.pro.f2` — `3 to 5 pages` — `3 a 5 páginas`
- `plan.pro.f3` — `Contact form` — `Formulario de contacto`
- `plan.pro.f4` — `Photo gallery` — `Galería de fotos`
- `plan.pro.f5` — `Google Maps location` — `Ubicación en Google Maps`
- `plan.pro.f6` — `Basic SEO (show up on Google)` — `SEO básico (apareces en Google)`
- `plan.pro.f7` — `Custom professional design` — `Diseño profesional a medida`
- `plan.premium.name` — `Premium` — `Premium`
- `plan.premium.for` — `If you need custom features` — `Si necesitas funciones a medida`
- `plan.premium.maint` **(HTML)** — `Optional maintenance: <b>$80/mo</b>` — `Mantenimiento opcional: <b>$80/mes</b>`
- `plan.premium.f1` — `Everything in Professional` — `Todo lo del plan Profesional`
- `plan.premium.f2` — `Advanced interactive features (React/jQuery)` — `Funciones interactivas avanzadas (React/jQuery)`
- `plan.premium.f3` — `Third-party integrations (bookings, catalog)` — `Integración con herramientas de terceros (reservas, catálogo)`
- `plan.premium.f4` — `Live data / API connections` — `Datos en vivo / conexión a APIs`
- `plan.premium.f5` — `More pages` — `Más páginas`
- `plan.premium.f6` — `Priority delivery` — `Entrega prioritaria`
- `plan.cta` (all three "Cotizar" buttons) — `Get a quote` — `Cotizar`
- `pricing.note` **(HTML)** — `All plans include <b>3 revisions</b> and the <b>first year of domain + hosting</b> (after that the client pays the annual renewal). <b>Monthly maintenance is optional</b> and varies by package. Reserve with 50% · Zelle, PayPal & Binance.` — `Todos los planes incluyen <b>3 revisiones</b> y el <b>dominio + hosting del primer año</b> (luego el cliente paga la renovación anual). El <b>mantenimiento mensual es opcional</b> y varía según el paquete. Aparta con el 50% · Zelle, PayPal y Binance.`

### FAQ (React component — re-render on language change)
- `faq.title` — `Frequently asked questions` — `Preguntas frecuentes`
- `faq.sub` — `We answer your questions before we start.` — `Resolvemos tus dudas antes de empezar.`
- `faq.loading` — `Loading questions…` — `Cargando preguntas…`
- `faq.q1` — `How long does my website take?` — `¿Cuánto tarda mi página web?`
- `faq.a1` — `It depends on the package: a Basic landing page can be ready in a few days; a Professional site in 1 to 2 weeks.` — `Depende del paquete: una landing Básica puede estar lista en pocos días; un sitio Profesional en 1 a 2 semanas.`
- `faq.q2` — `Do I need to know tech?` — `¿Necesito saber de tecnología?`
- `faq.a2` — `Not at all. You tell us about your business and we handle everything.` — `Para nada. Tú nos cuentas de tu negocio y nosotros nos encargamos de todo.`
- `faq.q3` — `What does monthly maintenance include?` — `¿Qué incluye el mantenimiento mensual?`
- `faq.a3` — `Keeping your site online, making small changes and updates. It's optional and the cost varies by package: Basic $25, Professional $45, and Premium $80 per month.` — `Mantener tu página online, hacer cambios pequeños y actualizaciones. Es opcional y su costo varía según el paquete: Básico $25, Profesional $45 y Premium $80 al mes.`
- `faq.q4` — `How do payments work?` — `¿Cómo son los pagos?`
- `faq.a4` — `50% to reserve and 50% on delivery. We accept Zelle, PayPal, and Binance.` — `50% para apartar y 50% al entregar. Aceptamos Zelle, PayPal y Binance.`
- `faq.q5` — `What languages do you work in?` — `¿En qué idiomas trabajan?`
- `faq.a5` — `We build your site in Spanish or English, depending on your customers.` — `Hacemos tu sitio en español o inglés, según tus clientes.`

### Contact
- `contact.title` — `Tell us your problem` — `Cuéntanos tu problema`
- `contact.text` **(HTML)** — `Tell us what your business needs and we'll tell you how we solve it. <b>Free</b> quote, no commitment.` — `Dinos qué necesita tu negocio y te decimos cómo lo resolvemos. Cotización <b>gratis</b>, sin compromiso.`
- (Email / WhatsApp / Instagram button labels stay the same in both languages.)

### Footer
- `footer.slogan` — `Your success is our mission` — `Tu éxito es nuestra misión`
- (Footer copyright line keeps the dynamic year and email — same in both.)

---

## Notes for Codex
- Do **not** change any non-text attributes, layout, styling, links, the logo images, the SVG sprites, or the existing jQuery/React/lightbox behavior. Only add: the toggle button, `data-i18n` hooks, the dictionary, and the `setLang` logic + FAQ language wiring.
- Match the existing code style in `script.js` (jQuery, simple comments in plain language).
- Test that toggling updates every section including the React FAQ, the title/meta, and that the choice survives a page reload.
