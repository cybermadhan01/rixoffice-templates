# Rixoffice Templates

The public template catalog for [Rixoffice](https://github.com/cybermadhan01/rixoffice).

Rixoffice does **not** bundle this library in the desktop installer. The app reads
`catalog/catalog.json`, downloads the individual file you pick, verifies its
SHA-256, caches it locally, and opens it in the matching editor.

## Layout

```
catalog/
  catalog.json        lightweight index - ids, names, tags, URLs, sizes, checksums
  categories.json     category -> template count, for the filter rail
  versions.json       catalog schema version + build stamp
templates/
  docs|sheets|slides|pdf|markdown/
    <template-id>/
      template.<ext>  the file Rixoffice downloads
      preview.webp    640px preview, ~20-100 KB, generated at build time
      metadata.json   full provenance for one template
      LICENSE.txt     licence + attribution, shipped beside the file
licenses/
  attribution.json    every source's licence, in one place
import-report.md      what was imported on the last build
```

## Catalog contract

`catalog.json` is intentionally small. It carries metadata only - never binary
content - so the app can list thousands of templates after one HTTP request.

```json
{
  "version": 1,
  "updatedAt": "2026-10-04T00:00:00Z",
  "templates": [
    {
      "id": "rix-doc-service-agreement",
      "name": "Service Agreement",
      "format": "docx",
      "category": "Legal",
      "subcategory": "Contracts",
      "tags": ["legal", "contract"],
      "thumbnail": "https://raw.githubusercontent.com/.../preview.webp",
      "preview": "https://raw.githubusercontent.com/.../preview.webp",
      "download": "https://raw.githubusercontent.com/.../template.docx",
      "size": 18342,
      "sha256": "…",
      "license": "CC0-1.0",
      "commercialUse": true,
      "redistributionAllowed": true
    }
  ]
}
```

`size` and `sha256` are mandatory. The app refuses any download whose checksum
does not match, and rejects a file whose real type disagrees with `format`.

## Licensing

Only templates classified `APPROVED` by the importer are published here. An
unknown, ambiguous, or non-redistributable licence is never published - it is
reported in `import-report.md` instead.

Current catalogue licence: **CC0-1.0**. No third-party template bodies are
redistributed. See `licenses/attribution.json` for the machine-readable record.

## Rebuilding

The catalog is generated, not hand-edited:

```bash
cd tools/template-importer
python build.py --out ../../rixoffice-templates
```

Add or edit a template in `defs.py`, rebuild, and commit the result. The app
picks up a new catalog version on its next start without an app update.

## API

These URLs are stable and public:

| Purpose | URL |
| --- | --- |
| Catalog | `https://raw.githubusercontent.com/cybermadhan01/rixoffice-templates/main/catalog/catalog.json` |
| Categories | `https://raw.githubusercontent.com/cybermadhan01/rixoffice-templates/main/catalog/categories.json` |
| Template file | `https://raw.githubusercontent.com/cybermadhan01/rixoffice-templates/main/templates/<format>/<id>/template.<ext>` |
| Thumbnail | `https://raw.githubusercontent.com/cybermadhan01/rixoffice-templates/main/templates/<format>/<id>/preview.webp` |
| Licence record | `https://raw.githubusercontent.com/cybermadhan01/rixoffice-templates/main/licenses/attribution.json` |

Third-party mirrors (Cloudflare R2, S3, a Rixoffice CDN) can replace the raw
GitHub host without any change to the app: it resolves URLs from the catalog and
verifies every file by checksum.
