# Global Luxury Retail Intelligence — Photo & News Archive

This repository contains the searchable photo archive and the authorized Shop Opening News research archive for the Global Luxury Retail Intelligence Platform.

## Photo archive

- `media/objects/` — content-addressed project photo objects, tracked with Git LFS. Identical image bytes are stored once.
- `catalog/photo-catalog.csv` — searchable index with source table, record ID, brand, store/street, city, country, original URL, object path, SHA-256, size and content type.
- `catalog/photo-catalog.json` — the same index as structured JSON.
- `catalog/photo-search-index.txt` — compact human- and GitHub-searchable text index.
- `metadata/media-inventory.json` — database reference counts and CDN host inventory.
- `metadata/media-download-manifest.json` — per-URL download status, object path, byte count and SHA-256 checksum.

## Shop Opening News archive

- `news/artifacts/` — News CSV, JSON, XLSX, HTML, Markdown, audit reports, photo manifests and export archives from the authorized `Shop Opening News` task.
- `news/photos/after-2026-08-29/` — current opening-photo objects extracted from the dated task export, tracked with Git LFS.
- `news/index/news-artifact-index.csv` — searchable file-level index with type, path, size, SHA-256 and source.
- `news/index/news-artifact-index.json` — structured version of the same index.
- `news/index/news-search-index.txt` — plain-text search index for Claude and GitHub search.
- The two large historical photo snapshots are attached to the GitHub Release `shop-opening-news-archive-2026-10-26` with SHA-256 values in the News index, rather than duplicated in the Git tree.

## How to find a photo or news item

Search `catalog/photo-search-index.txt` for a brand, store, street, city or country. Search `news/index/news-search-index.txt` or `news/index/news-artifact-index.csv` for a news source, opening, closing, city, export filename or report. The matching `repoPath` identifies the file or image; `sha256` provides the integrity key.

```bash
grep -i "Bahnhofstrasse\|Zurich\|Louis Vuitton" catalog/photo-search-index.txt
grep -i "opening\|closing\|Milan\|Paris" news/index/news-search-index.txt
```

The archives are snapshots. Do not rename image objects or source exports independently without updating the corresponding index.
