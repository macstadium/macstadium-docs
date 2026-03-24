# MacStadium Docs

The source for [MacStadium's documentation site](https://docs.macstadium.com), built on [Mintlify](https://mintlify.com).

## How it works

Docs are written in MDX (Markdown + JSX) and organized by product category. `mint.json` controls navigation, redirects, and site configuration. Changes merged to `main` deploy automatically.

## Contributing

**Small fixes** (typos, broken links, outdated steps): use the **Suggest Edit** button on any page — it opens a pre-filled GitHub PR against the source file.

**Larger changes**: branch off `main`, make your edits, and open a PR. A preview deployment will be posted automatically on the PR for review before merge.

## Local preview

Install the Mintlify CLI:

```bash
npm i -g mintlify
```

Then from the repo root:

```bash
mintlify dev
```

The site will be available at `http://localhost:3000`.

## Structure

```
/macstadium          # Account, billing, legal
/orka                # Orka platform docs
/remote-desktop-vdi  # VDI and Cloud Access
/iaas                # IaaS and networking
```

## Questions

Reach out in Slack or open an issue.
