# Where this recipe comes from

GLM-5.3-Flash on TensorFold across four DGX Spark class machines (GB10) cabled as a switchless ring.

This tree started on 2026-11-25 as a copy of
`https://github.com/MiaAI-Lab/GLM-5.3-Flash-EXL3-2x-DGX-Sparks-TensorFold`, branch `tp4`, commit
`d2200300` (Apache-2.0). LICENSE, NOTICE and CREDITS.md are kept as they were; the README is the original kit's.
It is maintained here on its own from that point: it is not synced with that repository.

Changes made here since the copy:
- `ORIGIN.md` (this file) and `scripts/local.sh.example` added; `scripts/local.sh` is git-ignored (site settings).
- The site file sets `PULL=0`, so `scripts/prepare.sh` never fetches a prebuilt image and builds locally instead.

What it depends on:
- the TensorFold engine and the NGC PyTorch base image that `scripts/prepare.sh` builds the image from;
- the checkpoint `Mia-AiLab/GLM-5.3-Flash-EXL3-4bpw-TensorFold` (EXL3) and the DFlash2 drafter.

Launch on a four-node ring: copy `scripts/local.sh.example` to `scripts/local.sh`, set your addresses, then
`./start-tp4-switchless.sh` (stop with `./stop.sh`).
