# mlx-video (`helios` branch — fork)

> **Fork / reference repo.** This checkout's git remote is
> `github.com/dmunch/mlx-video` on branch **`helios`** — it is **not** an
> xocialize repo. It is the upstream `Blaizzy/mlx-video` package with a third-party
> **Helios** port layered on the `helios` branch, kept here as a reference. The
> sections below correct the existing `README.md`, which still describes only the
> `main`-branch surface (LTX-2 + Wan2.1/2.2) and **omits everything the `helios`
> branch adds**.

MLX-Video is a package for inference (and finetuning) of image/video/audio
generation models on Apple Silicon using MLX.

## Installation

> The README installs from `Blaizzy/mlx-video` (upstream `main`). To get the
> **Helios** pipeline you must install **this fork/branch** instead:

```bash
pip install "git+https://github.com/dmunch/mlx-video.git@helios"
# or
uv pip install "git+https://github.com/dmunch/mlx-video.git@helios"
```

## Supported models

- **Helios** — autoregressive chunk-based text-to-video with multi-scale history
  memory. **(Undocumented in the existing README — this branch's headline
  addition.)** Transformer architecture is Wan-14B–shaped (dim 5120, 40 layers,
  40 heads) plus Helios-specific multi-scale history memory; the **Helios-Distilled**
  variant uses x0-prediction, no CFG, 2–3 steps/chunk. See
  `mlx_video/models/helios/` and `examples/poodles_helios.gif`.
- **LTX-2** — 19B video generation model (Lightricks). Distilled variant only.
- **Wan2.1** — 1.3B / 14B T2V (single-model pipeline).
- **Wan2.2** — T2V-14B, TI2V-5B, I2V-14B (dual-model pipeline). LoRA via
  `--lora-high` / `--lora-low` (e.g. Wan2.2-Lightning 4-step).

## CLI entry points

`pyproject.toml` registers **three** console scripts (the README documents only
the first two):

| Script | Module | Purpose |
|---|---|---|
| `mlx_video.generate` | `generate.py` | LTX-2 text-to-video |
| `mlx_video.generate_wan` | `generate_wan.py` | Wan2.1 / 2.2 text/image-to-video |
| `mlx_video.generate_helios` | `generate_helios.py` | **Helios** autoregressive T2V (undocumented in README) |

There is also an **audio-video** pipeline, `mlx_video/generate_av.py`
("Audio-Video generation pipeline for LTX-2"), which has **no console-script
entry and no README coverage** — run via `python -m mlx_video.generate_av`.

### Helios generation (example)

```bash
python -m mlx_video.generate_helios --prompt "..."   # see --help for flags
```

## Weight conversion

- `convert.py` — LTX-2
- `convert_wan.py` — Wan2.1 / 2.2 (PyTorch → MLX)
- `convert_helios.py` — **Helios (undocumented in README)**

## Project structure (actual)

```
mlx_video/
├── generate.py / generate_wan.py / generate_helios.py / generate_av.py
├── convert.py / convert_wan.py / convert_helios.py
├── postprocess.py · utils.py · text_projection.py · version.py
└── models/
    ├── ltx/        # LTX-2
    ├── wan/        # Wan2.1 / 2.2
    └── helios/     # Helios (config, transformer, attention, rope, loading,
                    #   + scripts/ for reference comparison)  [README omits this]
docs/PORTING-GUIDE.md         # [README omits this]
examples/  poodles.gif · poodles-wan.gif · poodles_helios.gif
```

## Requirements

- macOS with Apple Silicon
- Python >= 3.11
- MLX >= 0.22.0 (per `pyproject.toml`)
- For weight conversion: PyTorch

> Doc nit: the existing README's "Requirements" section is correct, but
> `pyproject.toml` classifiers also (inconsistently) list Python 3.10 while
> `requires-python` is `>=3.11` — the 3.11 floor governs.

## Discrepancies vs the existing README (summary)

- README documents only LTX-2 + Wan2.1/2.2; **Helios is entirely absent** despite
  being the whole point of this branch (model dir, `generate_helios`,
  `convert_helios`, `poodles_helios.gif`).
- README's "Project Structure" omits `models/helios/`, `generate_av.py`,
  `text_projection.py`, `convert_helios.py`, and `docs/PORTING-GUIDE.md`.
- README install command points at upstream `Blaizzy/mlx-video` (no Helios); the
  Helios code lives only on this fork's `helios` branch.

## License

MIT (upstream mlx-video, Prince Canuma / Blaizzy). Helios port carries the fork's
own attribution — verify before any redistribution.
