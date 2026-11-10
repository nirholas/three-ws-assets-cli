---
name: find-3d-assets
description: Search the free three.ws catalog for a ready-made 3D prop, rigged character or motion clip and add it to the project with paste-ready code. Use when a scene, site or game needs a GLB asset before generating one from scratch.
---

# Find and add 3D assets with @three-ws/assets

The catalog is public: no account, no API key, no payment. It covers CC0 objects (`object`), rigged humanoids (`character`) and motion clips (`animation`). Requires Node 18+. Run with `npx`.

## Commands

```bash
npx @three-ws/assets search wooden chair --kind object     # --kind object|character|animation
npx @three-ws/assets show object:painted_wooden_chair_01   # facts plus snippet, downloads nothing
npx @three-ws/assets add object:painted_wooden_chair_01    # downloads the GLB, prints the snippet
```

`search` flags: `--kind`, `--category <c>`, `--tag <t>`, `--limit <n>` (1 to 50, default 12), `--offset <n>`, `--json`. It exits 1 when nothing matches.

`show` and `add` flags: `--framework model-viewer|agent-3d|three|react`. `add` also takes `--dir <path>` (default `public/three-ws/` if the project has `public/`, else `three-ws-assets/`), `--thumb`, `--force` and `--json`. Re-running `add` is idempotent, and a file you edited is never overwritten without `--force`.

Ids look like `object:<name>`, `character:<name>`, `animation:<id>`, copied from `search` output.

## Choosing a framework

- `model-viewer`: default for props, a `<model-viewer>` tag.
- `agent-3d`: default for characters, the `<agent-3d>` web component pinned with an integrity hash.
- `three`: default for clips (`GLTFLoader`, `THREE.AnimationClip.parse`).
- `react`: the same wrapped as a component or hook.

## Licensing

Objects are CC0. Characters and clips carry a `license` field shown by `show` and `add` and present in `--json` output. Check it before shipping.

## Other access

- `THREE_WS_API` (or `--api <origin>`) overrides the catalog origin, default `https://three.ws`.
- Direct HTTP: `GET https://three.ws/api/catalog?q=desk+lamp&kind=object`.
- Library: `import { resolveApi, searchCatalog, fetchItem, downloadAsset } from '@three-ws/assets/api'`.
- This plugin also mounts the keyless three.ws studio MCP server, which exposes `search_catalog`, `get_catalog_item` and `get_item_source`.

## Worked example

User asks for a desk lamp in a Vite project:

1. `npx @three-ws/assets search desk lamp --kind object --limit 5`
2. Pick an id, then `npx @three-ws/assets add object:<name> --framework three`
3. Paste the printed snippet. The `src` already points at the local copy in `public/three-ws/`.
