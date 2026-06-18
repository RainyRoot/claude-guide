# Contributing

## fix a typo or outdated fact

1. Edit the file in `docs/`.
2. Update the `Sources:` line at the bottom with the URL you used and today's date.
3. Open a pull request with a short description.

## add something to an existing chapter

Same as above. Make sure new headings are plain text without special characters so anchor links stay stable.

## add a new chapter

1. Create a new folder under `docs/` using the `NN-slug/` naming pattern.
2. Add an `index.md` that starts with a 2 sentence what/why, has at least one diagram or screenshot placeholder, defines new terms and links them to `glossary.md`, has a gotchas section, and ends with `Sources:` and a next/see also block.
3. Add it to the `nav:` in `mkdocs.yml`.
4. Add any new terms to `docs/glossary.md`.

## style

- English only
- Short sentences, one idea per paragraph
- Commands in fenced code blocks with a language tag
- Mermaid diagrams in fenced `mermaid` blocks (render on GitHub and in MkDocs Material)
- Never claim a feature without verifying it in the live Anthropic docs first
- Mark anything uncertain with `<!-- VERIFY: ... -->`

## screenshot placeholders

If your page needs a screenshot:

```markdown
![description of what to capture](../assets/images/NN-short-name.png)
```
