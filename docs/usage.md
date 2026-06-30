# Usage

Ask Codex to use `$web-article-capture` with one or more target URLs and an output directory.

Example:

```text
Use $web-article-capture to capture these pages into .tmp/web-article-capture/sources:
- https://example.com/article
- https://example.com/product-announcement
```

The skill uses the Codex in-app Browser to inspect rendered pages, select the article/main reading flow, collect original inline image assets that belong to that flow, and write packages under the requested output root.

For each page, expect the capture to:

- choose the smallest content scope that contains the title and正文;
- stop before related content, comments, recommendations, support/contact panels, and page chrome;
- keep images near the text they support;
- record capture mode and fallback details when Browser access is partial or blocked.

## Validation

Run the validator from the skill root or repository root:

```powershell
python scripts/validate_capture_package.py <output-root> --require-images when-referenced
```

For a single package directory that directly contains `source.md`:

```powershell
python scripts/validate_capture_package.py <output-root>/<source-slug>
```

The validator checks package shape, referenced local images, image extensions, and screenshot-like file names.

The validator is structural. Human review should still inspect `source.md` or `review.html` for正文 boundary quality, image ownership, and fallback reliability.
