# Usage

Ask Codex to use `$web-article-capture` with one or more target URLs and an output directory.

Example:

```text
Use $web-article-capture to capture these pages into .tmp/web-article-capture/sources:
- https://example.com/article
- https://example.com/product-announcement
```

The skill uses the Codex in-app Browser to inspect rendered pages, select the article/main DOM content, collect original inline image assets, and write packages under the requested output root.

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
