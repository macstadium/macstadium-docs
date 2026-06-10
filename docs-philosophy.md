# MacStadium docs: philosophy, structure, and workflow

This document is for PMs who own or contribute to MacStadium product documentation. It covers why we write docs the way we do, how the site is structured, the standards we hold ourselves to, and the tooling we use to write and ship.

---

## Philosophy

### Docs are part of the product

A customer who can't figure out how to deploy our product hasn't successfully bought our product. Documentation is the last mile of product delivery. It's not a support asset or a marketing asset — it's part of what the customer gets.

This means docs are held to the same standard as the product itself: accurate, maintained, and written for the person doing the work.

### Write for the person doing the task

Our docs have two readers: someone evaluating whether to buy, and someone deploying after they've bought. Most pages are for the second person — the IT admin who needs to get something working. Write for them. Use "you." Assume they're competent but unfamiliar with our specific system. Don't explain what a VM is; do explain exactly what our playbook expects.

### One job per page

Every doc page has exactly one type:

| Type | Purpose | What it never does |
|---|---|---|
| **Conceptual** | Explains what something is and how it works | Gives steps |
| **How-to** | Numbered steps to accomplish a specific task | Explains theory |
| **Reference** | Tables, specs, quick-lookup content | Tells a story |

Mixing types is the most common doc quality problem. If you're writing a how-to and find yourself explaining architecture, that explanation belongs in a separate conceptual page — linked from the how-to, not embedded in it.

### Accuracy over completeness

A page that's wrong is worse than a page that doesn't exist. If you don't know something, say so with a Note block or leave a TODO comment. Don't fill gaps with plausible-sounding content. Engineering is the source of truth for anything about how the system actually works — ask before writing.

### Structure enables discovery

Customers find docs through search and through nav. Both depend on structure. A page in the wrong section, with the wrong title, or with a path that doesn't match its content, is effectively invisible. Getting structure right is as important as getting prose right.

---

## Site structure

docs.macstadium.com is organized into four top-level product areas:

| Area | What's in it |
|---|---|
| **Orka** | macOS virtualization: CLI, VM management, OCI registries, Kubernetes integration |
| **IaaS** | Bare metal Mac infrastructure: provisioning, networking, private cloud |
| **Remote Desktop & VDI** | macOS virtual desktops via Citrix or HP Anyware, built on Orka Engine |
| **Account Management** | Portal, SSO/SAML, billing, user admin |

Each PM owns the doc section that maps to their product area.

### Section structure within a product area

Every product area follows the same section pattern. Use this as the map when deciding where a new page lives.

| Section | Purpose | Audience |
|---|---|---|
| **Overview** | What the product is, how it works, is it right for me. Conceptual only — no steps. | Decision-maker + Admin |
| **Prerequisites** | Hardware, accounts, licenses, capacity. One dedicated page. | Admin |
| **Getting Started** | Step-by-step path to a working setup. One guide per deployment variant where steps diverge. | Admin |
| **Configuration** | Task-oriented how-to guides for features and integrations. | Admin |
| **Operations** | How to run in production: management, monitoring, operational workflows. | Admin |
| **Reference** | Lookup destination: troubleshooting, CLI quick-ref, specs. Short, scannable. | Admin |
| **Release Notes** | Versioned changelog. Always at the bottom of the nav. | Decision-maker + Admin |
| **Legacy** | Archived content with deprecation banners. Never silently deleted. | Existing customers |

---

## Writing standards

All MacStadium docs follow these rules. They apply to every page in every product section.

### Voice and style

- **Second person.** Address the reader as "you." ("You need a license" not "The administrator needs a license.")
- **Active voice.** ("MacStadium provisions the VM" not "The VM is provisioned by MacStadium.")
- **Plain language.** Short sentences. If a sentence exceeds ~25 words, break it up.
- **Contractions are fine.** ("You don't need to" not "You do not need to.")
- **Oxford comma.** Always.
- **Sentence case for all headings.** "Choose your deployment model" not "Choose Your Deployment Model."
- **Present tense for facts.** ("When you click Save, the VM restarts" not "will restart.")
- **"for example" not "e.g."** in prose.
- **`macOS` not `MacOS`.** Always lowercase the 'mac'.

### Things to avoid

- **Em dashes in prose.** Use a comma, colon, or parentheses instead.
- **Backslash line continuation in code blocks.** Write commands on one line. Users copy-paste from docs and split lines break that.
- **AI-sounding language.** Avoid: "delve into," "seamlessly," "it's worth noting," "leverage," "robust," "streamline," "furthermore," "moreover."
- **Marketing language.** Docs describe, not sell. Avoid "powerful," "best-in-class."
- **`{{ }}` in code blocks.** Mintlify parses MDX strictly — double curly braces in any code block (even yaml) can 404 the page. Use `[VARIABLE]` bracket notation instead.
- **`<placeholder>` in plain code blocks.** Use `[PLACEHOLDER]` bracket notation.
- **`<30` or `<100` in prose.** Write "under 30" or "under 100" — angle brackets parse as JSX and break the page.

### Frontmatter

Every page needs at minimum:

```
---
title: "Page title in sentence case"
description: "One sentence used in search and link previews."
---
```

### Product naming

Use exact casing and full names on first use per page:

- MacStadium VDI (not "the VDI product" or "MSVDI" in prose)
- Orka Engine (not "Orka" when referring specifically to the engine)
- Citrix DaaS (not "Citrix" alone when the product name matters)
- macOS (not MacOS)
- Apple silicon (lowercase 's')

---

## Workflow

### The loop

Every doc task follows this sequence:

1. **Jira story** — Document the scope, pages affected, and acceptance criteria before writing anything. Use the DI board.
2. **Outline first** — For new pages or major rewrites, get the structure right before writing prose. Share the outline for quick review.
3. **Write MDX** — Follow the standards above. Use the Claude skill (see below) to draft.
4. **PR** — One PR per Jira story. Small and focused is better than large and sweeping.
5. **Merge to main** — Mintlify deploys automatically from main.

### Git conventions

Branch naming: `docs/<short-description>` (for example, `docs/mdm-enrollment-intune-tab`)

Commit style: `docs: <what changed>` (for example, `docs: add Intune tab to MDM enrollment page`)

PR body: what changed, why, and which files are affected. No padding.

### When to loop in engineering

Before writing any page that describes how the system works — especially orchestration, deployment steps, networking, or security behavior — confirm the facts with engineering. Don't rely on existing doc content, which may be stale. Existing docs are a starting point for structure, not a source of truth for behavior.

---

## Tooling: Claude + the macstadium-docs skill

We use a Claude skill to accelerate doc writing. The skill encodes all of the standards above and knows the repo structure, Mintlify component patterns, and git/PR workflow.

### What the skill does

- Reads existing pages before making changes, so it doesn't overwrite good content
- Follows the one-job-per-page rule and flags when a draft mixes types
- Applies voice, style, and MDX parsing safety rules automatically
- Produces an outline for review before writing prose on new pages
- Generates the full git/PR checklist after the MDX is approved

### How to use it

The skill is available in Cowork. Start a session, describe what you're working on (a Jira story, a brain dump, or a specific page to edit), and the skill takes it from there. You'll iterate on structure before it writes anything — that's intentional.

For edits to existing pages: share the page path and what needs to change. The skill reads the current file and produces a targeted diff.

For new pages: share context on what the page covers, who reads it, and which product section it belongs to. The skill produces an outline for your review before writing MDX.

### What the skill doesn't replace

- Engineering review for technical accuracy
- Judgment on whether a page should exist at all
- The Jira story (document scope before you write)

---

## Ownership and review

Each PM is responsible for the doc section that maps to their product. That means:

- Keeping pages accurate when the product changes
- Opening PRs for any customer-visible updates, not just net-new content
- Flagging pages that are stale, wrong, or missing
- Including documentation ACs in every product story that changes user-visible behavior

Doc PRs don't require engineering review unless the change touches technical accuracy. A second PM review is sufficient for structure and style.
