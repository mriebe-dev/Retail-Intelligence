# Global Luxury Retail Intelligence — Photo Archive

This repository contains the searchable photo archive for the Global Luxury Retail Intelligence Platform.

## Contents

- `media/objects/` — content-addressed photo objects, tracked with Git LFS. Identical image bytes are stored once.
- `catalog/photo-catalog.csv` — spreadsheet-friendly searchable index with source table, record ID, brand, store/street, city, country, original URL, object path, SHA-256, size and content type.
- `catalog/photo-catalog.json` — the same index as structured JSON.
- `catalog/photo-search-index.txt` — compact human- and GitHub-searchable text index.
- `metadata/media-inventory.json` — database reference counts and CDN host inventory.
- `metadata/media-download-manifest.json` — per-URL download status, object path, byte count and SHA-256 checksum.

## How to find a photo

Search `catalog/photo-catalog.csv`, `catalog/photo-catalog.json` or `catalog/photo-search-index.txt` for a brand, store name, street, city, country, source table or numeric record ID. The matching `objectPath` identifies the image under `media/objects/`.

Examples:

```bash
grep -i "Bahnhofstrasse\|Zurich\|Louis Vuitton" catalog/photo-search-index.txt
grep -i 'streetPhotos,42' catalog/photo-catalog.csv
```

The `originalUrl` field preserves the source reference, while `sha256` provides an integrity and deduplication key. The archive is a snapshot; database records and image files should not be renamed independently without updating the catalog.

## Snapshot scope

The snapshot contains 648 successfully downloaded image references from the project CDN hosts. All downloaded objects are tracked with Git LFS to avoid GitHub's regular-file size limits.
