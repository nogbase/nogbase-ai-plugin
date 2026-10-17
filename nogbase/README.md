# Nogbase

Nogbase helps real-estate brokers and property developers work with their
Nogbase inventory, including off-plan projects, from their AI assistant. This
plugin pairs the Nogbase connector with four skills that turn common sales
tasks into repeatable steps:

| Skill | What it does |
| --- | --- |
| Sales offer | A branded offer for one unit at the quoted or list price, with every payment-plan installment computed by Nogbase, the offer terms and fees, and your company letterhead. |
| Client shortlist | Turns a buyer's brief (budget and currency, bedrooms, area, handover window, booking and post-handover limits) into filtered searches and compares three to five options in one table. |
| Project brief | A project summary with the developer's published handover, unit mix and starting prices, payment plans, amenities, brochures, and sales contacts, in English, Arabic, or both. |
| Deal check | Nogbase's investment estimate for one off-plan unit: entry cost, the payment schedule, price against DLD sales, value scenarios with the sale at handover, break-even and, from a rent you give, the yield. Every figure shows its source and as-of date, with Nogbase's disclaimer; it is an estimate, not financial advice. |

<!-- prompts:start -->
Try:

- **Shortlist for a buyer:** "Find off-plan 2-bedroom apartments in Jumeirah Village Circle under AED 1,500,000 with a 60/40 payment plan."
- **Compare units for a buyer:** "Compare units B1201 and B1204 in Hesperine Circuit for my client: price, size, floor, payment plan and handover."
- **Sales offer for a unit:** "Prepare a one-page sales offer for unit 518 in Hesperine Arena at AED 1,000,000 on my company letterhead."
- **Offer as a WhatsApp reply:** "Write a WhatsApp reply to my client with the offer for unit 518 in Hesperine Arena at AED 1,000,000."
- **Deal-check a unit:** "Deal-check unit 518 in Hesperine Arena at a rent of AED 90,000 a year: what would the buyer make selling at handover?"
- **Project brief in Arabic:** "Write a project brief for Hesperine Arena in Arabic."
- **Project facts:** "Tell me about Hesperine Circuit: developer, handover, amenities and payment plans."
- **Available units in a project:** "Which 1-bedroom units are available in Hesperine Circuit? List them cheapest first with size, floor and price."
- **Explain a payment plan:** "Explain the payment plan for unit B1201 in Hesperine Circuit to my client: each installment, its amount and when it is due."
- **Price against DLD sales:** "How does the price of unit 518 in Hesperine Arena compare with recent DLD sales in the area?"

جرّب:

- **قائمة مختصرة لمشترٍ:** «ابحث عن شقق على الخارطة من غرفتي نوم في قرية جميرا الدائرية بسعر لا يتجاوز 1,500,000 AED بخطة سداد 60/40.»
- **مقارنة وحدات لمشترٍ:** «قارن بين الوحدتين B1201 وB1204 في Hesperine Circuit لعميلي: السعر والمساحة والطابق وخطة السداد وموعد التسليم.»
- **عرض سعر لوحدة:** «جهّز عرض سعر من صفحة واحدة للوحدة 518 في Hesperine Arena بسعر 1,000,000 AED على الورق الرسمي لشركتي.»
- **العرض كرسالة واتساب:** «اكتب ردًا على واتساب لعميلي يتضمن عرض الوحدة 518 في Hesperine Arena بسعر 1,000,000 AED.»
- **فحص صفقة وحدة:** «افحص صفقة الوحدة 518 في Hesperine Arena بإيجار سنوي 90,000 AED: ماذا يحقق المشتري لو باع عند التسليم؟»
- **ملخص المشروع بالعربية:** «اكتب ملخصًا لمشروع Hesperine Arena باللغة العربية.»
- **معلومات المشروع:** «أخبرني عن مشروع Hesperine Circuit: المطوّر وموعد التسليم والمرافق وخطط السداد.»
- **الوحدات المتاحة في مشروع:** «ما الوحدات المتاحة من غرفة نوم واحدة في Hesperine Circuit؟ اعرضها من الأقل سعرًا مع المساحة والطابق والسعر.»
- **شرح خطة السداد:** «اشرح لعميلي خطة السداد للوحدة B1201 في Hesperine Circuit: كل قسط وقيمته وموعد استحقاقه.»
- **السعر مقارنة بمبيعات دائرة الأراضي والأملاك:** «قارن سعر الوحدة 518 في Hesperine Arena بمبيعات دائرة الأراضي والأملاك الأخيرة في المنطقة.»
<!-- prompts:end -->

## Requirements

A Nogbase broker or developer account. When you first use the plugin, your AI
application asks you to connect Nogbase: you sign in on the Nogbase page and
choose the workspace (company) the connection may read.

## What the plugin connects to and what it does

- It connects to one service, the Nogbase connector at
  `https://api.nogbase.ae/mcp`, over OAuth. It contains no scripts, hooks, or
  executables, and it sends nothing to any other destination.
- The connector's tools only read. They cannot reserve, mark sold, change
  prices, save, or send anything in Nogbase. Offers and briefs are written in
  your conversation; Nogbase does not store them.
- Results include only the projects and units your workspace may see. Locked
  projects show a short card until access is granted.
- Opening a file stored in Nogbase creates a five-minute link and an audit
  record, so the skills ask you before opening one.
- Nogbase sends your AI application only what the tools return: inventory and
  project data your workspace may read, file names and short-lived links, and
  your company's public branding. It never sends personal email, bank, or tax
  details.
- Coding agents with a terminal, such as Claude Code and Codex, can read and
  change workspace data with the separate `nogbase-api` plugin in the same
  marketplace.

## Links

- Documentation: https://nogbase.ae/ai-connectors
- Privacy policy: https://nogbase.ae/privacy
- Terms of service: https://nogbase.ae/terms
- Support: support@nogbase.ae or https://nogbase.ae/support

See [CHANGELOG.md](CHANGELOG.md) for release notes.
