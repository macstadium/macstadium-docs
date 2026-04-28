# MacStadium Docs

The source for [MacStadium's documentation site](https://docs.macstadium.com), built on [Mintlify](https://mintlify.com).

## How it works

Docs are written in MDX (Markdown + JSX) and organized by product category. `mint.json` controls navigation, redirects, and site configuration. Changes merged to `main` deploy automatically.

## Contributing

**Small fixes** (typos, broken links, outdated steps): use the **Suggest Edit** button on any page. It opens a pre-filled GitHub PR against the source file.

**Larger changes**: branch off `main`, make your edits, and open a PR. A preview deployment will be posted automatically on the PR for review before merge.
## Git setup

To avoid accidentally pushing directly to `main`, configure git to always create a new branch:

```bash
git config --global push.default current
git config --global branch.autosetuprebase always
```

Or use this alias to make branching a one-step habit:

```bash
git config --global alias.new '!git checkout -b'
```

Then `git new my-branch-name` creates and switches to a new branch.

**Branch naming:** Avoid the `docs/` prefix — Mintlify skips preview deployments for branches matching that pattern. Use `fix/`, `add/`, `update/`, etc.

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

**MacStadium customers:** use the **Suggest Edit** button on any page to flag errors or suggest improvements. For product support, email [support@macstadium.com](mailto:support@macstadium.com).

**Internal team:** open a PR or reach out on Slack.
