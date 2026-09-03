# Implementation plan: moving Bug Control to a non-US operating stack

Prepared 3 September 2026. Companion to `01-non-us-stack.md`, which holds the tool-by-tool register.

## Principles

1. **One system of record changes at a time.** Never migrate two things the same person relies on in the same month.
2. **Export first, then build, then run both, then cut over.** Every migration keeps the old tool read-only for 60 days after cutover.
3. **Consolidate before you migrate.** Two file stores into one, two CRMs into one, then move the one.
4. **Identity is the hinge.** Zitadel goes in early so every new tool is set up with single sign-on from day one and nothing is migrated twice.
5. **Managed hosting unless someone owns operations.** Prefer Infomaniak, Zitadel Cloud, Baserow Cloud or a managed Nextcloud provider over raw servers unless a named person is on call.
6. **Every phase ends with the attack-surface register updated** with owner country and hosting location for each changed system.

## Anchor decisions (Phase 0 must settle these)

| Decision | Options | Recommendation |
|---|---|---|
| What are we protecting against? | Ownership (US legal reach), residency (where data sits), or both | Both. Ownership is the stated goal; residency in AU or NZ for anything holding resident or client data. |
| Hosting home | Catalyst Cloud (NZ), Binary Lane (AU), Hetzner (DE), OVHcloud (FR), Infomaniak (CH) | Catalyst Cloud or Binary Lane for anything with resident data; Infomaniak for managed Nextcloud and mail if you want less to operate. |
| Identity provider | Zitadel (CH) self-hosted or Zitadel Cloud (EU) | Zitadel Cloud to start; self-host later if cost or control demands it. |
| CRM | Zoho CRM (IN, AU data centres), Odoo (BE), Twenty (FR) | Zoho CRM. It also removes Intercom, Buffer and part of Make with Desk, Social and Flow. |
| Files and office | Nextcloud with OnlyOffice | Nextcloud, managed. Recreate the 01_ to 08_ folder standard exactly. |
| AI layer | Stay on Claude behind an accepted exception, or move to Mistral, Matilda or Cohere | Benchmark in Phase 5. Do not switch before the operational tools are stable. |

## Phases

### Phase 0: Decide and document (weeks 1 to 2)

Actions
- Write the one-page sovereignty policy: goal, accepted US exceptions and why, review date.
- Update the attack-surface register with owner country and hosting country per system.
- Take the six anchor decisions above and record them.
- Create the operations repository (Codeberg or self-hosted Forgejo) holding the register, this plan, the skills library and a CLAUDE.md.
- Name an owner for each phase and a fortnightly 30-minute review slot.

Exit criteria: policy signed, anchor decisions recorded, register complete, owners named.

### Phase 1: Quick wins with no team disruption (weeks 3 to 6)

Actions
- Self-host Poppins and DM Sans in the assessment tool; remove the Google Fonts call.
- Install Matomo on the Knowledge Hub alongside GA4. Remove GA4 after 30 days of matching numbers.
- Replace Fireflies with Tactiq. Export Fireflies transcripts into the file store.
- Replace Buffer with Metricool or Canva Content Planner.
- Move every Zapier zap into Make and close Zapier.
- Move Google Drive contents into SharePoint under the existing folder standard. This is consolidation, not the final home.
- Stop Apollo and Vibe Prospecting. Build the facility universe from My Aged Care and the NZ Ministry of Health certified provider list into a single spreadsheet, ready for CRM import.

Exit criteria: five US tools closed, one file store, one automation tool, facility list built.

### Phase 2: Data layer (weeks 7 to 12)

Actions
- Stand up Zitadel and PostgreSQL on the chosen host. Enable multi-factor authentication for all staff.
- Rebuild the assessment engine on the backend: login via Zitadel, one tenancy per facility, audit log of every assessment edit, encrypted at rest, AU or NZ hosting. This is the product work in this repository.
- Replace Airtable with Baserow. Export Snapshots and Alerts to CSV, rebuild tables and views, repoint the ai-cfo skill and any Make scenarios.
- Stand up Zoho CRM. Import Pipedrive deals and contacts, then the facility universe. Connect MailerLite and Xero through Make. Keep Pipedrive read-only for 60 days.
- Update every Make scenario that touched Airtable, Pipedrive or Apollo.

Exit criteria: no resident data in browser storage; Airtable and Pipedrive read-only; CRM feeding MailerLite and Xero.

### Phase 3: Files and collaboration (months 4 to 6)

Actions
- Provision managed Nextcloud with OnlyOffice, single sign-on through Zitadel.
- Recreate the folder standard for each business (IPS and PBC as separate spaces), then migrate SharePoint site by site. Verify counts and spot-check 5 percent of files.
- Update the onedrive-file-management skill to point at Nextcloud paths.
- Replace Intercom with Crisp or Zoho Desk. Move help articles into the Knowledge Hub. Export conversation history to the file store.
- Move internal chat to Nextcloud Talk or Element. Keep Teams read-only for 60 days.

Exit criteria: all documents in Nextcloud; SharePoint read-only; Intercom closed.

### Phase 4: Identity and mail (months 6 to 9)

Actions
- Connect every remaining tool to Zitadel single sign-on: Nextcloud, Zoho, Baserow, Matomo, the assessment engine, LearnWorlds where the plan supports SAML.
- Migrate mailboxes and calendars to Fastmail by IMAP, one domain at a time (bugcontrol, infectioncontrol.care, ipservices.care). Set SPF, DKIM and DMARC before each MX change.
- Recreate shared mailboxes and distribution lists.
- Reduce Microsoft 365 to the minimum licences needed for Windows, then decide on desktop operating systems separately.

Exit criteria: one login for every tool; mail served outside Microsoft; Entra ID decommissioned.

### Phase 5: AI layer and code (months 9 to 12)

Actions
- Benchmark Mistral (Le Chat, Vibe and La Plateforme), Matilda and Cohere against the skills library on twenty real tasks: a Knowledge Hub article, a CFO review, a campaign plan, a file-management task, a coding change in the assessment engine.
- If a non-US model meets the bar, port the skills (they are markdown) and reconnect MCP servers. If not, record Claude as an accepted exception with a review date.
- Move repositories to Codeberg or self-hosted Forgejo. Set up continuous integration with Forgejo Actions or Woodpecker.
- Final register update. Start the quarterly review cadence.

Exit criteria: AI decision recorded with evidence; code off GitHub; register current.

## Timeline summary

| Phase | Window | Headline outcome |
|---|---|---|
| 0 Decide | Weeks 1 to 2 | Policy, decisions, owners |
| 1 Quick wins | Weeks 3 to 6 | Five US tools closed, no disruption |
| 2 Data layer | Weeks 7 to 12 | Backend, CRM, Baserow live |
| 3 Files | Months 4 to 6 | Nextcloud replaces SharePoint |
| 4 Identity and mail | Months 6 to 9 | Single sign-on and Fastmail |
| 5 AI and code | Months 9 to 12 | AI decision, code hosting moved |

## Risks and mitigations

| Risk | Mitigation |
|---|---|
| Change fatigue in a small team | One migration per person per month. Fortnightly review can pause a phase. |
| Data loss at cutover | Export, verify counts, run both for 60 days, keep the old tool read-only. |
| Dual running costs | Budget 60-day overlaps per phase up front. |
| Capability gaps (search data, social channels) | Logged as accepted exceptions with a review date, not ignored. |
| Non-US AI models fall short on the skills | Benchmark before switching. Skills remain portable either way. |
| Operations burden of self-hosting | Managed services by default. Self-host only with a named on-call owner. |
| Vendor ownership changes mid-plan | Re-verify ownership at the start of each phase. |
| Resident data exposure in the current tool | Phase 2 backend is the fix. Until then, do not use the tool with identifiable resident data on shared machines. |

## Governance

- Sponsor: Lyndon. Phase owners named in Phase 0.
- Fortnightly 30-minute review against exit criteria.
- Monthly CFO review (existing ai-cfo skill) reports migration spend.
- Quarterly attack-surface review confirms owner country and hosting per system.
- Success measures, reported quarterly: share of systems non-US owned; share of client and resident data hosted in AU, NZ or EU; number of accepted US exceptions and their review dates.

## Cutover checklist (use for every tool)

1. Export everything, including attachments and history.
2. Build the replacement with single sign-on and the same naming standard.
3. Import and verify counts; spot-check 5 percent.
4. Run both for a fixed period and repoint automations.
5. Announce the cutover date a week ahead with a one-page how-to.
6. Set the old tool read-only for 60 days, then close it.
7. Update the attack-surface register and the relevant skill.
