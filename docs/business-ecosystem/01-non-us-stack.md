# Bug Control operating stack: ownership audit and non-US replacements

Prepared 3 September 2026 for the IPS / Bug Control team. Companion to `02-implementation-plan.md`.

## Why ownership, not just data location

A US-owned vendor is subject to US legal process (for example the CLOUD Act) wherever its
servers sit. An Australian region of a US product solves data residency, not ownership.
This register records both: who owns the company, and where the data can live.

Ownership facts change with acquisitions. Re-verify before each phase of the plan.
Entries marked *to confirm* were not verified.

## Status key

- **Keep**: already non-US owned. No change.
- **Replace**: US-owned; a credible non-US replacement exists.
- **Accept**: US-owned with no viable replacement for this business. Log as an accepted exception and minimise what it sees.
- **Decide**: a choice the leadership team has to make; options listed.

## Ownership audit and replacements

| Function | Current tool | Owner (country) | Status | Non-US replacement | Replacement owner (country) | Notes |
|---|---|---|---|---|---|---|
| Email, calendar, contacts | Microsoft 365 (Outlook, Exchange) | Microsoft (US) | Replace, last | Fastmail | Fastmail (Australia, Melbourne) | Mail is the highest-disruption swap. Do it after files and identity are stable. |
| Identity and single sign-on | Microsoft Entra ID | Microsoft (US) | Replace | Zitadel | Zitadel (Switzerland) | Self-host or Zitadel Cloud (EU). Becomes the login for every other tool. |
| Documents and file storage | SharePoint, OneDrive, Google Drive | Microsoft (US), Google (US) | Replace | Nextcloud on AU or NZ hosting; Infomaniak kDrive; Tresorit | Nextcloud GmbH (Germany); Infomaniak (Switzerland); Tresorit (Switzerland, Swiss Post) | Keep the 01_Admin to 08_Archives folder standard. Retire Google Drive first, into one place. |
| Office document editing | Word, Excel, PowerPoint | Microsoft (US) | Replace | OnlyOffice or Collabora Online inside Nextcloud | Ascensio (Latvia); Collabora (UK) | Both read and write .docx, .xlsx and .pptx. |
| Team chat and meetings | Microsoft Teams | Microsoft (US) | Replace | Nextcloud Talk; Infomaniak kMeet; Element | Nextcloud (Germany); Infomaniak (Switzerland); Element (UK) | Client meetings can still join whatever the client uses. |
| Operational database, no-code | Airtable (AI CFO base) | Airtable (US) | Replace | Baserow; SeaTable | Baserow (Netherlands); SeaTable (Germany) | Airtable exports to CSV. Rebuild Snapshots and Alerts tables, repoint the ai-cfo skill. |
| Accounting | Xero | Xero (New Zealand) | Keep | | | Already non-US. MYOB is US-owned (KKR), so Xero is the right choice. |
| CRM and pipeline | Pipedrive; Apollo | Pipedrive: Estonia-founded, majority-owned by Vista Equity Partners (US); Apollo (US) | Replace | Zoho CRM; Odoo CRM; Twenty | Zoho (India, AU data centres in Sydney and Melbourne); Odoo (Belgium); Twenty (France, open source) | Run one CRM only. Zoho if you want a suite that also covers Desk, Projects, Flow and Social. |
| Prospect and contact data | Apollo; Vibe Prospecting | Apollo (US); Vibe Prospecting (to confirm) | Replace | Public registers plus Cognism or Kaspr | Australian and NZ government registers; Cognism (UK); Kaspr (France, Cognism-owned) | Your universe is a finite public list: My Aged Care provider finder (AU) and the Ministry of Health certified provider list (NZ). Load it once into the CRM. |
| Email marketing and automation | MailerLite | MailerLite (Lithuania) | Keep | Brevo as fallback | Brevo (France) | Already non-US. |
| Learning platform | LearnWorlds | LearnWorlds (Cyprus) | Keep | Moodle if sovereign hosting is wanted | Moodle Pty Ltd (Australia, Perth) | Already non-US. |
| Workflow automation | Make.com; Zapier (via LearnWorlds) | Make: Czech-founded, owned by Celonis (Germany); Zapier (US) | Keep Make, retire Zapier | n8n for self-hosted flows | n8n GmbH (Germany) | Move every Zapier zap into Make. |
| Ecommerce and payments | Shopify; Shopify Payments (Stripe) | Shopify (Canada); Stripe (US) | Keep Shopify, replace payments | Adyen; Mollie; Pin Payments; Tyro | Adyen (Netherlands); Mollie (Netherlands); Pin Payments (Australia, owned by Checkout.com, UK); Tyro (Australia) | Check Shopify's third-party gateway fee before switching. |
| Website and Knowledge Hub | WordPress with Elementor | WordPress open source (Automattic, US, steward); Elementor (Israel) | Keep, host locally | Self-hosted WordPress on an Australian host; Ghost for publishing | VentraIP (Australia); Ghost Foundation (Singapore, non-profit) | Ownership risk sits in hosting and plugins, not the open-source code. |
| Web analytics | Google Analytics 4 | Google (US) | Replace | Matomo; Plausible | InnoCraft (New Zealand, Wellington); Plausible (Estonia) | Matomo is NZ-made and self-hostable. Run both for 30 days, then remove GA4. |
| Search and keyword data | Google Search Console; Keywords Everywhere | Google (US); Axeman Tech (India, Mumbai) | Accept GSC, keep KE | SE Ranking; SISTRIX for rank tracking | SE Ranking (UK, Ukrainian-founded); SISTRIX (Germany) | Nobody else has Google's own index data. Use GSC read-only. |
| Social scheduling | Buffer | Buffer (US) | Replace | Metricool; Publer; Canva Content Planner | Metricool (Spain); Publer (Albania); Canva (Australia) | |
| Social channels | Meta Business Suite; LinkedIn | Meta (US); Microsoft (US) | Accept | None | | The audience is there. Treat them as publishing endpoints and share the minimum. |
| Support chat and help centre | Intercom | Intercom (US-headquartered, Irish-founded) | Replace | Crisp; Chatwoot; Zoho Desk | Crisp (France); Chatwoot (India, open source); Zoho (India) | Move help articles into the Knowledge Hub. |
| Meeting notes and transcripts | Fireflies | Fireflies (US) | Replace | Tactiq; tl;dv; Noota | Tactiq (Australia, Sydney); tl;dv (Germany); Noota (France) | Tactiq reads captions without a bot joining the call. |
| Tasks and projects | xTiles | xTiles (Ukraine) | Keep | OpenProject as fallback | OpenProject (Germany) | Already non-US. |
| Design, images, video | Higgsfield; Google Fonts CDN | Higgsfield (US); Google (US) | Replace | Canva with Leonardo.ai; FLUX; self-hosted Poppins | Canva (Australia); Black Forest Labs (Germany); Indian Type Foundry (India) | Self-hosting the font is a one-line change in the assessment tool. |
| Code hosting and CI | GitHub | Microsoft (US) | Replace | Codeberg; self-hosted Forgejo | Codeberg e.V. (Germany, non-profit); Forgejo (community, Codeberg-hosted) | GitLab Inc. and Atlassian are both US-domiciled. |
| Application backend and database | None yet (browser localStorage); Supabase was suggested | Supabase Inc. is Delaware-incorporated, Singapore HQ | Decide | PostgreSQL plus Zitadel on Catalyst Cloud, Binary Lane, Hetzner or OVHcloud | Catalyst Cloud (New Zealand); Binary Lane (Australia); Hetzner (Germany); OVHcloud (France) | Required before the assessment engine holds resident data for a team. |
| AI assistant, agents, skills | Claude and Claude Code with MCP connectors | Anthropic (US) | Decide | Mistral (Le Chat, Vibe, La Plateforme); Matilda by Maincode; Cohere North | Mistral (France); Maincode (Australia, Melbourne); Cohere (Canada) | The 28 skills are markdown and portable. MCP is an open protocol Mistral supports. Benchmark before switching. |
| Desktop operating system | Windows | Microsoft (US) | Accept for now | Linux later if wanted | | Out of scope for this plan. |

## Non-US AI models and where to run them

| Model or provider | Country | Open weights | Best for | Where to run it |
|---|---|---|---|---|
| Mistral Large 3, Medium 3.5, Small 4, Devstral 2, Codestral, Voxtral | France | Large 3, Small 4, Devstral, Voxtral open; Medium via API | General chat, agents, coding, transcription | Mistral La Plateforme (EU); self-host; Scaleway or OVHcloud (France) |
| Cohere Command A+ (May 2026), Embed, Rerank | Canada | Apache 2.0 | Enterprise search, citations, agents, 48 languages | Cohere API; self-host on one B200 or two H100 |
| Matilda by Maincode | Australia (Melbourne) | Proprietary, hosted in Melbourne | Chat and coding agent, public beta | matilda.maincode.com |
| Sovereign Australia AI: Ginan (open research model), Australis | Australia (Sydney, NEXTDC) | Ginan to be open | Australian-trained general model, in development | Watch list; not yet production |
| Aleph Alpha Pharia | Germany | Partly | Government-grade compliance and traceability | Aleph Alpha cloud (EU); on-premise |
| Qwen 3 family (Alibaba) | China | Open | Strong general and coding models | Self-host only, if Chinese origin is acceptable |
| DeepSeek V3 and R1; Kimi K2 (Moonshot); GLM (Zhipu) | China | Open | Reasoning and agents | Self-host only, if Chinese origin is acceptable |
| Falcon H1 (TII) | UAE | Open | General | Self-host |
| EXAONE 4 (LG AI Research) | South Korea | Open | General, bilingual | Self-host |
| FLUX (Black Forest Labs) | Germany | Open | Image generation | Self-host; EU APIs |
| Speechmatics; Gladia | UK; France | No | Speech to text for meeting notes | Vendor APIs (EU) |
| Inference hosts | | | | Mistral (France); Scaleway (France); OVHcloud AI Endpoints (France); IONOS AI Model Hub (Germany); Nebius (Netherlands); Sharon AI and ResetData (Australia, GPU); Catalyst Cloud (New Zealand) |

Model origin and data residency are separate questions. A US-origin open-weight model run on
Australian hardware keeps the data in Australia but is still a US-designed model. Decide which
of the two you care about before choosing.

## Accepted US dependencies (to be signed off)

1. Meta and LinkedIn as publishing channels.
2. Google Search Console as a read-only data source.
3. Windows on staff computers, pending a later decision.
4. Claude, until a benchmark against the skills library shows a non-US model does the work.
