# mlx-video (`helios` branch — fork)

> **Fork / reference repo.** This is the upstream
> [`Blaizzy/mlx-video`](https://github.com/Blaizzy/mlx-video) package with a
> third-party **Helios** text-to-video port layered on the `helios` branch. The
> Helios code lives only on this branch — upstream `main` ships LTX-2 + Wan2.1/2.2
> only.

MLX-Video is a package for inference (and finetuning) of image/video/audio
generation models on Apple Silicon using MLX.

## Installation

To get the **Helios** pipeline, install this fork's `helios` branch:

```bash
pip install "git+https://github.com/dmunch/mlx-video.git@helios"
# or
uv pip install "git+https://github.com/dmunch/mlx-video.git@helios"
```

## Supported models

- **Helios** — autoregressive chunk-based text-to-video with multi-scale history
  memory. Transformer architecture is Wan-14B–shaped (dim 5120, 40 layers,
  40 heads) plus Helios-specific multi-scale history memory; the **Helios-Distilled**
  variant uses x0-prediction, no CFG, 2–3 steps/chunk. See
  `mlx_video/models/helios/` and `examples/poodles_helios.gif`.
- **LTX-2** — 19B video generation model (Lightricks). Distilled variant only.
- **Wan2.1** — 1.3B / 14B T2V (single-model pipeline).
- **Wan2.2** — T2V-14B, TI2V-5B, I2V-14B (dual-model pipeline). LoRA via
  `--lora-high` / `--lora-low` (e.g. Wan2.2-Lightning 4-step).

## CLI entry points

`pyproject.toml` registers three console scripts:

| Script | Module | Purpose |
|---|---|---|
| `mlx_video.generate` | `generate.py` | LTX-2 text-to-video |
| `mlx_video.generate_wan` | `generate_wan.py` | Wan2.1 / 2.2 text/image-to-video |
| `mlx_video.generate_helios` | `generate_helios.py` | Helios autoregressive T2V |

An **audio-video** pipeline, `mlx_video/generate_av.py` (Audio-Video generation for
LTX-2), has no console-script entry — run it via `python -m mlx_video.generate_av`.

### Helios generation (example)

```bash
python -m mlx_video.generate_helios --prompt "..."   # see --help for flags
```

## Weight conversion

- `convert.py` — LTX-2
- `convert_wan.py` — Wan2.1 / 2.2 (PyTorch → MLX)
- `convert_helios.py` — Helios

## Project structure

```
mlx_video/
├── generate.py / generate_wan.py / generate_helios.py / generate_av.py
├── convert.py / convert_wan.py / convert_helios.py
├── postprocess.py · utils.py · text_projection.py · version.py
└── models/
    ├── ltx/        # LTX-2
    ├── wan/        # Wan2.1 / 2.2
    └── helios/     # Helios (config, transformer, attention, rope, loading,
                    #   + scripts/ for reference comparison)
docs/PORTING-GUIDE.md
examples/  poodles.gif · poodles-wan.gif · poodles_helios.gif
```

## Requirements

- macOS with Apple Silicon
- Python >= 3.11
- MLX >= 0.22.0 (per `pyproject.toml`)
- For weight conversion: PyTorch

## License

MIT (upstream mlx-video, Prince Canuma / Blaizzy). Helios port carries the fork's
own attribution — verify before any redistribution.
