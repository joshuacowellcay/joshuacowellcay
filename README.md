# Joshua Cowell

Second-year engineering student building end-to-end machine learning systems. Looking for internships and roles in software, ML, and AI engineering.

I like building things that run end to end, with real data behind them and an interface someone can actually use. A lot of my time goes into the less visible parts: cleaning the data, checking that the metrics mean what I think they mean, and making sure things hold together when the project gets bigger.

---

## Projects at a glance

| Project | What it does | Headline result |
|---|---|---|
| [**TerraTrace**](https://github.com/joshuacowellcay/terratrace) &nbsp;—&nbsp; *featured* | Satellite forest-loss detection behind an interactive map | **0.965 IoU** on a geographically held-out test split; **+8.2 IoU** over a calibrated NDVI baseline |
| [**The AI Physicist**](https://github.com/joshuacowellcay/ai-physicist) | Discovers symbolic governing equations from observed motion | **6 / 6** physical systems recovered exactly; **0.18%** mean error on recovered constants |
| [**Neural Compressor**](https://github.com/joshuacowellcay/neural-compressor) | Lossless byte-level compressor built on a small causal Transformer | **1.995 bits/byte** on held-out text — **42% smaller** than `gzip -9` |

---

## Featured project

### TerraTrace — satellite forest-change detection
[github.com/joshuacowellcay/terratrace](https://github.com/joshuacowellcay/terratrace)

A full-stack geospatial ML platform. Draw an area on the map, pick a before and an after date, and the service returns a per-pixel forest-loss heatmap together with an estimated area lost. A Siamese U-Net trained in PyTorch sits behind a FastAPI inference service; a Next.js + MapLibre frontend handles area-of-interest drawing and overlay rendering.

**Results on the held-out test split**

| Forest-loss class | Siamese U-Net | NDVI baseline (calibrated) |
|---|---:|---:|
| IoU | **0.965** | 0.883 |
| F1 | **0.982** | 0.938 |
| Precision | 0.965 | 0.897 |
| Recall | 1.000 | 0.983 |

The deep model beats the calibrated NDVI baseline by **8.2 IoU points** overall; on cloud-affected tiles the gap widens to roughly **20 IoU points**.

**What the project demonstrates**
- Geospatial data engineering: STAC fetch from the Microsoft Planetary Computer, Hansen Global Forest Change label alignment, cloud masking, and a tile-grid macro-block split that prevents train/test leakage (asserted in tests).
- Honest evaluation: a calibrated NDVI baseline, geographically separated splits, and IoU on the minority class as the headline rather than pixel accuracy.
- Full lifecycle: MLflow-tracked training, deterministic builds, a typed FastAPI service, a typed Next.js frontend, and GitHub Actions CI on every push.

**Stack:** Python · PyTorch · FastAPI · Next.js · MapLibre GL · MLflow · rasterio · Docker Compose

---

## Other flagship projects

### The AI Physicist — discovering symbolic laws from data
[github.com/joshuacowellcay/ai-physicist](https://github.com/joshuacowellcay/ai-physicist)

Watches a simulated physical system move and recovers its **governing equation as readable mathematics**, rather than fitting an opaque black box to its trajectory. A SINDy-family sparse-regression pipeline (sequentially-thresholded least squares over a polynomial-plus-trig candidate library) returns a clean LaTeX equation with fitted physical constants. A small MLP is trained alongside as a black-box baseline for contrast.

**Results across six headline systems** (harmonic oscillator, simple pendulum, damped pendulum, two-body Kepler orbit, Lotka–Volterra, Van der Pol)
- **6 / 6** systems recovered exactly on clean data.
- **0.18%** mean absolute relative error on recovered physical constants.
- The discovered law forecasts a held-out initial condition for **9.1× longer** than the neural baseline before exceeding 10% L2 error — **39×** on the Kepler orbit.
- Structurally exact recovery survives **5%** observation noise on every headline system.

**What the project demonstrates**
- Interpretable, scientific ML: sparse regression, candidate libraries, and a symbolic equation as a first-class artefact.
- Applied mathematics: smoothing, derivative estimation, ODE integration, and conservation-law diagnostics.
- Rigour: recovery is verified against the *ground-truth* equation, not only against a forecast metric.

**Stack:** Python · NumPy · SciPy · SymPy · scikit-learn · PyTorch · FastAPI · Next.js · KaTeX

---

### Neural Compressor — compression is intelligence
[github.com/joshuacowellcay/neural-compressor](https://github.com/joshuacowellcay/neural-compressor)

A **lossless** data compressor built from a small causal Transformer and an integer-arithmetic entropy coder. The model predicts the next byte; the coder turns those probabilities into the shortest possible bitstream; decompression runs the identical model in lockstep to reconstruct the original bytes exactly. The project is the shortest demonstration I could fit in one repository of the Shannon equivalence between prediction and compression.

**Results on 16 KiB of held-out *Pride and Prejudice***

| Tool | Bits per byte | Compressed bytes | vs. gzip |
|---|---:|---:|---:|
| **Neural Compressor** | **1.995** | **4,086** | **−42%** |
| gzip -9 | 3.459 | 7,084 | reference |
| bzip2 -9 | 3.085 | 6,317 | −35% |
| xz -9 | 3.363 | 6,888 | −41% |

**What the project demonstrates**
- Information theory in working code: a 32-bit integer arithmetic coder written from scratch, so encoder and decoder narrow the interval identically and the round-trip is bit-exact.
- Sequence modelling: a byte-level Transformer trained on CPU in roughly three minutes, with a deterministic probability quantiser bridging float predictions and the integer coder.
- Correctness-critical engineering: every committed checkpoint round-trips its corpus end-to-end under CI.

**Stack:** Python · PyTorch · FastAPI · Next.js · Recharts · Docker Compose

---

## In development

**Grokking Observatory** — an interactive lab for watching small transformers learn algorithmic tasks (modular arithmetic, permutation composition) and reverse-engineering the trigonometric circuits they discover. Fourier analysis of the embeddings, progress measures that flag generalisation before test accuracy moves, and ablation studies that pin down which frequencies the model relies on. In active build; will be published as a separate repo.

---

## Engineering practices applied to every project

- Repo-level README with a live demo, headline metrics on held-out data, and an architecture diagram.
- Copy-pasteable setup that runs end to end in under five minutes on CPU.
- Seeded training and evaluation; metrics regenerate from a clean checkout.
- A real baseline to beat (calibrated NDVI for TerraTrace, an MLP forecaster for The AI Physicist, gzip / bzip2 / xz for Neural Compressor).
- Incremental commit history per build phase, not a single "initial commit".
- GitHub Actions CI on every push: lint, tests, and a frontend build for the apps.

---

## Tech I use

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=flat&logo=react&logoColor=black)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat&logo=next.js&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=flat&logo=scipy&logoColor=white)
![MLflow](https://img.shields.io/badge/MLflow-0194E2?style=flat&logo=mlflow&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat&logo=githubactions&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)

---

## Contact

- Email — [joshuacowellcay@gmail.com](mailto:joshuacowellcay@gmail.com)
- LinkedIn — [linkedin.com/in/joshua-cowell](https://www.linkedin.com/in/joshua-cowell-84b64b248/)
