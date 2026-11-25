# Where this recipe comes from

GLM-5.3-Flash on TensorFold across four DGX Spark class machines (GB10) cabled as a switchless ring.

This tree started on 2026-11-25 as a copy of
`https://github.com/MiaAI-Lab/GLM-5.3-Flash-EXL3-2x-DGX-Sparks-TensorFold`, branch `tp4`, commit
`d2200300` (Apache-2.0). LICENSE, NOTICE and CREDITS.md are kept as they were; the README is the original kit's.
It is maintained here on its own from that point: it is not synced with that repository.

Changes made here since the copy:
- `ORIGIN.md` (this file) and `scripts/local.sh.example` added; `scripts/local.sh` is git-ignored (site settings).
- The site file sets `PULL=0`, so `scripts/prepare.sh` never fetches a prebuilt image and builds locally instead.
- The image build is pinned: TensorFold by commit (`TF_COMMIT` in `scripts/config.sh`, the commit tag `v0.6.0` pointed at),
  the NGC base image by digest, and every package pip adds by exact version (`patches/constraints.txt`).

## Built and measured here

2026-11-25, four GB10s on a switchless ring: image built by `scripts/prepare.sh` from this tree (patches hash
`b25d14b4a457`). Its TensorFold files are byte-identical to the image the kit used to pull prebuilt; four indirect
packages are newer (transformers 5.24.1, hf-xet 1.8.0, typer 0.29.2, annotated-doc 0.0.7). One stream, temperature 0,
thinking off, tokens per second (structured / code / prose), then four streams in aggregate (prose / code) and cold
prefill at 16K to 128K prompt tokens:

| image | one stream | four streams | cold prefill |
|---|---|---|---|
| prebuilt, before | 189.1 / 126.6 / 99.5 | 290.1 / 350.6 | 2,355 to 2,411 tok/s |
| built here | 188.7 / 126.9 / 99.5 | 291.1 / 366.9 | 2,283 to 2,382 tok/s |

Known wart: `av==18.1.0` is marked yanked on PyPI; it is what the kit pinned and what was measured.

What it depends on:
- the TensorFold engine and the NGC PyTorch base image that `scripts/prepare.sh` builds the image from;
- the checkpoint `Mia-AiLab/GLM-5.3-Flash-EXL3-4bpw-TensorFold` (EXL3) and the DFlash2 drafter.

Launch on a four-node ring: copy `scripts/local.sh.example` to `scripts/local.sh`, set your addresses, then
`./start-tp4-switchless.sh` (stop with `./stop.sh`).
