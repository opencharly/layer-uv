# layer-uv

The [uv](https://github.com/astral-sh/uv) Python package and project manager for
OpenCharly images, shipped as the standalone `uv` + `uvx` binaries.

The `uv` candy downloads the latest astral-sh/uv release tarball and extracts
the two standalone Rust binaries (`uv`, `uvx`) directly into `/usr/local/bin`.
The release nests them under an arch directory, so `strip_components: 1` lands
them flat. uv needs no Python runtime and no pixi environment — it is a
self-contained binary.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `uv` |
| Binaries | `/usr/local/bin/uv`, `/usr/local/bin/uvx` |
| Install | `download:` the astral-sh/uv release tarball, `strip_components: 1` |
| Dependencies | none (self-contained Rust binary) |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-python-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-uv:v2026.239.1631'
```

Then, inside the built image:

```bash
uv --version                 # the package/project manager
uv python install 3.13       # install a Python toolchain
uvx ruff check .             # run a tool in an ephemeral environment
```

The candy's `plan:` asserts both binaries at their fixed paths and `uv --version`
exiting cleanly. The `download:` step declares an explicit `uninstall:` list so
`charly fleet del` removes only these two binaries, not the whole shared
`/usr/local/bin` directory.

## Layout

- `charly.yml` — the `uv:` candy entity (the `download:`/extract `plan:` step
  with its `uninstall:` list, the `check:` assertions) and the embedded
  `uv-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-coder:uv`
- `/charly-languages:pixi` — the direct-download pattern this candy mirrors
- `/charly-coder:typst` — sibling direct-download binary pattern
- `/charly-languages:python` — the pixi-python meta-layer this candy does **not** depend on
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
