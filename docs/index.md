# Web Article Capture

`web-article-capture` captures the rendered reading experience of webpages for downstream agents.

Use it when a later task needs source-grounded webpage material: article text, official documentation pages, media-rich announcements, figures, charts, diagrams, and image-caption context. The skill writes one compact source package per page, centered on a readable `source.md` plus the original webpage images referenced by that file.

## What It Produces

Each captured page is written as:

```text
<output-root>/<source-slug>/
  source.md
  images/
```

`source.md` keeps source metadata, captured date, article/main content, local image links, original image URLs, captions, and capture notes.

## Boundaries

- Capture the page's article or main reading flow, not a full-page screenshot archive.
- Preserve original webpage images where possible; do not substitute rendered screenshots for source images.
- Keep capture reliability notes close to the source package when a page is blocked, lazy-loaded, partially available, or lacks body images.
- Validate packages before handing them to another agent.

## Related Files

- Runtime instructions: `SKILL.md`
- Output contract: `references/output-contract.md`
- Validator: `scripts/validate_capture_package.py`
