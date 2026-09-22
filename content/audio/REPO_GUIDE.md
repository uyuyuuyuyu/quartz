# DiariZen Repository Guide

## Executive Summary
1. DiariZen is a speaker diarization toolkit: given audio, it predicts who spoke when and can write RTTM outputs.
2. The packaged inference API is `diarizen.pipelines.inference.DiariZenPipeline`, which wraps a pyannote-style diarization pipeline.
3. The learned segmentation model is local to this repo, while speaker embedding and pipeline utilities rely heavily on bundled `pyannote-audio`.
4. The main research recipes live under `recipes/`, with separate tracks for single-channel training, multi-channel training, and structured pruning.
5. Training uses chunked audio plus RTTM/UEM metadata, then optimizes a segmentation model that predicts frame-level speaker activity.
6. Inference runs segmentation first, then estimates speaker count, extracts speaker embeddings, clusters them, reconstructs diarization, and writes RTTM.
7. The most important model variants are `WavLM + Conformer`, `Fbank + Conformer`, and a pyannote-style SincNet/LSTM baseline.
8. Clustering is done with either agglomerative clustering or VBx, with PLDA assets expected for VBx.
9. Installation is version-sensitive: the README pins Python 3.10, PyTorch 2.1.1, CUDA 12.1, and also requires editable installs of local `pyannote-audio`.
10. The fastest way to understand the repo is: `README.md` -> `diarizen/pipelines/inference.py` -> `recipes/diar_ssl/run_stage.sh` -> recipe config TOMLs -> dataset/model/trainer files.

## 1. Repository Overview

### What the project does
In plain English, DiariZen is a research-oriented toolkit for speaker diarization. It takes audio, breaks it into analysis windows, predicts frame-level multi-speaker activity with a neural segmentation model, extracts speaker embeddings, groups those embeddings into speakers with clustering, and returns diarization results as pyannote annotations or RTTM files.

It is built on three layers:

- Local DiariZen code in [`diarizen/`](./diarizen) for models, training utilities, clustering integration, and the packaged inference API.
- Bundled [`pyannote-audio/`](./pyannote-audio) for the upstream diarization framework, pipeline logic, embedding model support, and model base classes.
- Bundled [`dscore/`](./dscore) for DER-style scoring during recipe evaluation.

### Main workflows in this repo

- Installation:
  Follow [`README.md`](./README.md), install the root package, then install the vendored [`pyannote-audio/`](./pyannote-audio), then initialize the `dscore` submodule.
- Inference:
  Use the Python API shown in [`README.md`](./README.md), or run the CLI in [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py).
- Training:
  Use recipe drivers in [`recipes/diar_ssl/`](./recipes/diar_ssl), especially [`run_stage.sh`](./recipes/diar_ssl/run_stage.sh) plus the TOML configs in [`recipes/diar_ssl/conf/`](./recipes/diar_ssl/conf).
- Evaluation:
  Recipe inference scripts write RTTM files, then `dscore/score.py` is used from the shell stage scripts.
- Data preparation:
  The repo expects Kaldi-style metadata files such as `wav.scp`, `rttm`, and `all.uem`. Example layouts are in [`recipes/diar_ssl/data/`](./recipes/diar_ssl/data).
- Pruning:
  Structured pruning research code is in [`recipes/diar_ssl_pruning/`](./recipes/diar_ssl_pruning).
- Multi-channel diarization:
  Spatially aware WavLM recipes are in [`recipes/diar_ssl_mc/`](./recipes/diar_ssl_mc).

## 2. File/Folder Map

### Concise tree of important folders
```text
DiariZen/
├─ diarizen/
│  ├─ pipelines/
│  ├─ models/
│  │  ├─ eend/
│  │  ├─ module/
│  │  └─ pruning/
│  ├─ clustering/
│  └─ trainer_*.py
├─ recipes/
│  ├─ diar_ssl/
│  │  ├─ conf/
│  │  └─ data/
│  ├─ diar_ssl_mc/
│  │  └─ conf/
│  └─ diar_ssl_pruning/
│     └─ conf/
├─ example/
├─ pyannote-audio/
├─ dscore/
├─ README.md
├─ pyproject.toml
└─ requirements.txt
```

### What each important folder contains

- [`diarizen/`](./diarizen)
  Core library code for DiariZen itself. This is the main folder to study if you want to understand the implementation.

- [`diarizen/pipelines/`](./diarizen/pipelines)
  Packaged inference entry points and small helpers. Most important file: [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py).

- [`diarizen/models/eend/`](./diarizen/models/eend)
  Neural diarization segmentation models. These are the models trained in the recipes.

- [`diarizen/models/module/`](./diarizen/models/module)
  Reusable building blocks such as Conformer layers, Fbank extraction, WavLM config/helpers, and multi-channel utilities.

- [`diarizen/models/pruning/`](./diarizen/models/pruning)
  Structured pruning and distillation support for compact WavLM-based diarization models.

- [`diarizen/clustering/`](./diarizen/clustering)
  VBx implementation and PLDA-space support used during clustering.

- [`recipes/`](./recipes)
  End-to-end experiment workflows. If you want to know how the authors actually train/evaluate models, this matters as much as the library code.

- [`recipes/diar_ssl/`](./recipes/diar_ssl)
  Main single-channel diarization recipe. This is the best starting recipe for learning the repo.

- [`recipes/diar_ssl_mc/`](./recipes/diar_ssl_mc)
  Multi-channel recipe using spatially aware WavLM and channel-fusion logic.

- [`recipes/diar_ssl_pruning/`](./recipes/diar_ssl_pruning)
  WavLM pruning/distillation recipe.

- [`recipes/*/conf/`](./recipes)
  TOML configs controlling models, datasets, trainers, and optimization.

- [`recipes/diar_ssl/data/`](./recipes/diar_ssl/data)
  Example metadata layout for datasets using `wav.scp`, `rttm`, and `all.uem`.

- [`example/`](./example)
  Contains one included audio file: [`example/EN2002a_30s.wav`](./example/EN2002a_30s.wav), useful for smoke tests.

- [`pyannote-audio/`](./pyannote-audio)
  Vendored upstream pyannote project. DiariZen depends on it both as a library and as a conceptual reference.

- [`dscore/`](./dscore)
  Evaluation tooling for scoring RTTM outputs against reference RTTMs.

### Folder categories

- Core library code:
  [`diarizen/`](./diarizen)
- Scripts:
  [`recipes/diar_ssl/*.py`](./recipes/diar_ssl), [`recipes/diar_ssl_mc/*.py`](./recipes/diar_ssl_mc), [`recipes/diar_ssl_pruning/*.py`](./recipes/diar_ssl_pruning), [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py)
- Configs:
  [`recipes/*/conf/*.toml`](./recipes)
- Examples:
  [`example/`](./example)
- Recipes:
  [`recipes/`](./recipes)
- Pretrained-model integration:
  [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py), [`diarizen/models/module/wav2vec2/utils/import_huggingface_wavlm.py`](./diarizen/models/module/wav2vec2/utils/import_huggingface_wavlm.py), pruning conversion scripts
- Pyannote integration:
  [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py), model classes under [`diarizen/models/eend/`](./diarizen/models/eend), bundled [`pyannote-audio/`](./pyannote-audio)
- Evaluation tools:
  [`dscore/`](./dscore), plus recipe `infer_avg.py` scripts

## 3. Entry Points

### Most important runnable files

- [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py)
  Main packaged inference class and CLI.

- [`recipes/diar_ssl/run_stage.sh`](./recipes/diar_ssl/run_stage.sh)
  Main single-channel train -> infer -> score orchestration script.

- [`recipes/diar_ssl/run_dual_opt.py`](./recipes/diar_ssl/run_dual_opt.py)
  Training entry point for WavLM-based models with separate optimizers for WavLM and non-WavLM parameters.

- [`recipes/diar_ssl/run_single_opt.py`](./recipes/diar_ssl/run_single_opt.py)
  Training entry point for simpler single-optimizer models such as Fbank or pyannote baseline.

- [`recipes/diar_ssl/infer_avg.py`](./recipes/diar_ssl/infer_avg.py)
  Batch inference over `wav.scp`, with averaged checkpoints and RTTM output.

- [`recipes/diar_ssl_mc/run_stage.sh`](./recipes/diar_ssl_mc/run_stage.sh)
  Multi-channel workflow orchestration.

- [`recipes/diar_ssl_pruning/run_stage.sh`](./recipes/diar_ssl_pruning/run_stage.sh)
  Pruning workflow orchestration.

- [`recipes/diar_ssl_pruning/run_distill_prune.py`](./recipes/diar_ssl_pruning/run_distill_prune.py)
  Structured pruning/distillation training entry point.

- [`recipes/diar_ssl_pruning/convert_wavlm_from_hf.py`](./recipes/diar_ssl_pruning/convert_wavlm_from_hf.py)
  Converts Hugging Face WavLM to this repo’s expected checkpoint format.

- [`dscore/score.py`](./dscore/score.py)
  Scores RTTM output against references.

### Inference entry points

#### Python API
From [`README.md`](./README.md):

```python
from diarizen.pipelines.inference import DiariZenPipeline

diar_pipeline = DiariZenPipeline.from_pretrained("BUT-FIT/diarizen-wavlm-large-s80-md")
result = diar_pipeline("./example/EN2002a_30s.wav")
```

Inputs:
- Hugging Face repo id for a DiariZen model.
- Audio file path or a pyannote `ProtocolFile`.

Outputs:
- A pyannote `Annotation`.
- Optional RTTM written if `rttm_out_dir` is set.

Key arguments:
- `repo_id`
- `cache_dir`
- `rttm_out_dir`

#### CLI
[`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py) also exposes a CLI:

```powershell
python -m diarizen.pipelines.inference `
  --in_wav_scp recipes/diar_ssl/data/AMI_AliMeeting_AISHELL4/test/AMI/wav.scp `
  --diarizen_hub <local_model_dir> `
  --embedding_model <embedding_model.bin> `
  --rttm_out_dir out_rttm
```

Inputs:
- `--in_wav_scp`: a Kaldi-style `wav.scp`
- `--diarizen_hub`: local directory containing `config.toml`, `pytorch_model.bin`, and likely `plda/`
- `--embedding_model`: speaker embedding checkpoint

Outputs:
- One RTTM per session in `--rttm_out_dir`

Key arguments:
- Inference: `--seg_duration`, `--segmentation_step`, `--batch_size`, `--apply_median_filtering`
- Speaker constraints: `--min_speakers`, `--max_speakers`
- Clustering: `--clustering_method`, `--ahc_threshold`, `--min_cluster_size`, `--Fa`, `--Fb`, `--lda_dim`, `--max_iters`

### Data preparation entry points

There is no one obvious universal “prepare data” script in the root recipe. Instead, the repo expects prepared metadata files:

- `wav.scp`
- `rttm`
- `all.uem`

The format is consumed by [`recipes/diar_ssl/dataset.py`](./recipes/diar_ssl/dataset.py), which reads:

- `load_scp(...)`
- `load_uem(...)`
- `rttm2label(...)`

So the real “data preparation contract” is the file format, not a single preparation command.

### Training/fine-tuning entry points

#### Single optimizer
[`recipes/diar_ssl/run_single_opt.py`](./recipes/diar_ssl/run_single_opt.py)

Example:
```bash
accelerate launch run_single_opt.py -C conf/fbank_conformer.toml -M train validate
```

Inputs:
- A TOML config
- Dataset metadata referenced inside that config

Outputs:
- Experiment directory under `exp/<config_stem>/`
- Checkpoints
- TensorBoard logs
- Saved config snapshot

Key arguments:
- `-C/--configuration`
- `-M/--mode`
- `-R/--resume`

#### Dual optimizer
[`recipes/diar_ssl/run_dual_opt.py`](./recipes/diar_ssl/run_dual_opt.py)

This is the important one for WavLM fine-tuning.

Example:
```bash
accelerate launch run_dual_opt.py -C conf/wavlm_updated_conformer.toml -M train
```

Key behavior:
- Builds model from config.
- Splits parameters into WavLM and non-WavLM groups.
- Uses separate optimizers and trainer logic.

### Evaluation entry points

#### Batch inference over checkpoints
[`recipes/diar_ssl/infer_avg.py`](./recipes/diar_ssl/infer_avg.py)

Purpose:
- Select checkpoints by validation metric
- Average or choose them
- Run batch inference from `wav.scp`
- Write RTTM files

Inputs:
- `-C` config TOML
- `-i` input `wav.scp`
- `-o` output folder
- `--embedding_model`
- optional `--diarizen_hub` for VBx PLDA assets
- validation-selection parameters

Key arguments:
- `--avg_ckpt_num`
- `--val_metric`
- `--val_mode`
- `--val_metric_summary`
- the same inference/clustering knobs as the packaged CLI

#### Scoring
From [`recipes/diar_ssl/run_stage.sh`](./recipes/diar_ssl/run_stage.sh):

```bash
python dscore/score.py \
  -r <reference_rttm> \
  -s <system_rttm_glob> \
  --collar 0
```

Inputs:
- Reference RTTM
- System RTTM(s)

Outputs:
- Text summary of DER-style scoring

### Downloading/loading models

- Packaged pretrained loading:
  [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py)
  uses `snapshot_download(...)` and `hf_hub_download(...)`.

- WavLM conversion:
  [`recipes/diar_ssl_pruning/convert_wavlm_from_hf.py`](./recipes/diar_ssl_pruning/convert_wavlm_from_hf.py)

- Fine-tuned checkpoint averaging:
  `average_ckpt(...)` from [`diarizen/ckpt_utils.py`](./diarizen/ckpt_utils.py), used by recipe runners.

## 4. Dependency/Environment Understanding

### Root environment metadata

- [`pyproject.toml`](./pyproject.toml)
  - package name: `diarizen`
  - Python requirement: `>=3.10`
  - build backend: `flit_core`

- [`requirements.txt`](./requirements.txt)
  includes:
  - `numpy==1.26.4`
  - `librosa`
  - `soundfile`
  - `scipy`
  - `tensorboard`
  - `accelerate==1.6.0`
  - `onnxruntime-gpu`
  - `torchinfo`
  - `einops`

- [`README.md`](./README.md)
  explicitly recommends:
  - Python `3.10`
  - `pytorch==2.1.1`
  - `torchvision==0.16.1`
  - `torchaudio==2.1.1`
  - `pytorch-cuda=12.1`
  - `mkl<2024.1`

### Bundled dependencies

- [`pyannote-audio/`](./pyannote-audio)
  is not just a dependency; it is vendored and installed editable with:
  ```bash
  cd pyannote-audio && pip install -e .[dev,testing]
  ```

- [`dscore/`](./dscore)
  is a git submodule, initialized with:
  ```bash
  git submodule init
  git submodule update
  ```

### Expected runtime stack

- Python:
  3.10 is the clearly intended version.
- PyTorch:
  2.1.1 from README.
- CUDA:
  README expects CUDA 12.1 builds.
- GPU:
  Strongly expected for training. Inference can fall back to CPU in code, but likely much slower.

### Version-sensitive or risky points

- `pyannote-audio` versioning matters a lot because DiariZen subclasses pyannote classes directly.
- The packaged inference path downloads from Hugging Face, so offline or restricted environments will fail unless assets are already cached locally.
- VBx clustering expects PLDA artifacts under a `plda/` directory in the model hub.
- Recipe configs use placeholder paths like `/YOUR_PATH/...`; they are not runnable as-is.
- The repo includes a `.DS_Store` file under a Python module path, which is harmless but a sign the tree is research-oriented rather than polished packaging.
- Root `requirements.txt` does not install PyTorch itself; the README’s conda command is part of the real installation procedure.
- `onnxruntime-gpu` can create CUDA/toolkit compatibility issues depending on your local environment.

## 5. Pipeline Explanation

### Likely diarization pipeline from audio to RTTM

1. Load waveform
2. Run neural segmentation model over windows/chunks
3. Optionally smooth segmentation with median filtering
4. Estimate frame-level speaker count
5. Extract speaker embeddings from active regions
6. Cluster embeddings into global speaker identities
7. Reconstruct discrete diarization timeline
8. Binarize into annotation tracks
9. Save RTTM
10. Score RTTM against reference during evaluation

### Stage-by-stage with implementation pointers

#### Audio loading

- Packaged inference:
  [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py)
  uses `torchaudio.load(in_wav)`.

- Training data loading:
  [`recipes/diar_ssl/dataset.py`](./recipes/diar_ssl/dataset.py)
  uses `soundfile.read(...)` in `extract_wavforms(...)`.

Notes:
- In single-channel inference, the code explicitly keeps `waveform[0]`, so it forces the first channel.
- Training datasets can load multi-channel waveforms, then models decide how to use channels.

#### Segmentation/windowing

- Inference chunking is handled by pyannote pipeline internals, configured via:
  - `seg_duration`
  - `segmentation_step`

- Training chunking is explicit in:
  [`recipes/diar_ssl/dataset.py`](./recipes/diar_ssl/dataset.py)
  through `_gen_chunk_indices(...)`, `chunk_size`, and `chunk_shift`.

#### Neural diarization model

Main segmentation models:

- WavLM + Conformer:
  [`diarizen/models/eend/model_wavlm_conformer.py`](./diarizen/models/eend/model_wavlm_conformer.py)

- Multi-channel spatial WavLM:
  [`diarizen/models/eend/model_wavlm_conformer_mc.py`](./diarizen/models/eend/model_wavlm_conformer_mc.py)

- Fbank + Conformer:
  [`diarizen/models/eend/model_fbank_conformer.py`](./diarizen/models/eend/model_fbank_conformer.py)

- Pyannote-style SincNet baseline:
  [`diarizen/models/eend/model_pyannote.py`](./diarizen/models/eend/model_pyannote.py)

What these models output:
- Frame-level multi-speaker activity scores in pyannote’s segmentation format.

#### Overlap handling

- The segmentation models are configured as powerset-style diarization models via pyannote base classes.
- In inference, embeddings are extracted with:
  `exclude_overlap=self.embedding_exclude_overlap`
  in [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py)

That means overlap is handled by:
- powerset segmentation on the segmentation side
- overlap exclusion during embedding extraction

#### Speaker embedding extraction

- Done by the inherited pyannote pipeline methods:
  `get_embeddings(...)`
  in [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py) and recipe `infer_avg.py`.

- The packaged pretrained flow downloads:
  `pyannote/wespeaker-voxceleb-resnet34-LM`
  as the embedding model.

#### Clustering / speaker assignment

- Pipeline supports:
  - `AgglomerativeClustering`
  - `VBxClustering`

- Wiring happens in:
  [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py)
  and recipe `infer_avg.py`.

- VBx implementation details:
  [`diarizen/clustering/VBx.py`](./diarizen/clustering/VBx.py)

- VBx PLDA setup:
  `vbx_setup(...)` in [`diarizen/clustering/VBx.py`](./diarizen/clustering/VBx.py)

#### Output reconstruction

- After clustering, the pipeline calls:
  `reconstruct(segmentations, hard_clusters, count)`
  in [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py)

- Then converts to annotation via:
  `Binarize(...)`

#### Output writing

- RTTM writing in packaged inference:
  [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py)

- RTTM writing in recipe inference:
  [`recipes/diar_ssl/infer_avg.py`](./recipes/diar_ssl/infer_avg.py)

#### Evaluation

- Recipe stage script calls:
  [`dscore/score.py`](./dscore/score.py)

- References are expected in RTTM form alongside `wav.scp` and `all.uem`.

## 6. Config System

### Config formats present

- Root packaging config:
  [`pyproject.toml`](./pyproject.toml)
- Recipe experiment configs:
  [`recipes/diar_ssl/conf/*.toml`](./recipes/diar_ssl/conf)
  [`recipes/diar_ssl_mc/conf/*.toml`](./recipes/diar_ssl_mc/conf)
  [`recipes/diar_ssl_pruning/conf/*.toml`](./recipes/diar_ssl_pruning/conf)
- Pyannote upstream configs:
  numerous YAML files under [`pyannote-audio/pyannote/audio/cli/`](./pyannote-audio/pyannote/audio/cli)

### How configs are organized

Recipe TOMLs are the main configs you will touch for DiariZen workflows. They usually define:

- `[meta]`
- `[finetune]`
- `[trainer]`
- `[optimizer]` or `[optimizer_small]`/`[optimizer_big]`
- `[model]`
- `[train_dataset]`
- `[validate_dataset]`

Example:
- [`recipes/diar_ssl/conf/wavlm_updated_conformer.toml`](./recipes/diar_ssl/conf/wavlm_updated_conformer.toml)

### Which values control what

#### Model path / checkpoint

- Training model class:
  `[model].path`
- WavLM source checkpoint:
  `[model.args].wavlm_src`
- Fine-tune checkpoint options:
  `[finetune]`
- Pruning teacher/student checkpoints:
  [`recipes/diar_ssl_pruning/conf/s80_large.toml`](./recipes/diar_ssl_pruning/conf/s80_large.toml)
  - `teacher_ckpt`
  - `student_ckpt`

#### Input audio / data

- Training:
  `[train_dataset.args].scp_file`
  `[train_dataset.args].rttm_file`
  `[train_dataset.args].uem_file`
- Validation:
  `[validate_dataset.args].scp_file`
  `[validate_dataset.args].rttm_file`
  `[validate_dataset.args].uem_file`
- Inference CLI:
  `--in_wav_scp`

#### Output directory

- Training experiment root:
  `[meta].save_dir`
- Inference output:
  `-o/--out_dir` in recipe inference
  `--rttm_out_dir` in packaged inference CLI

#### Number of speakers / speaker constraints

- In packaged and recipe inference:
  - `--min_speakers`
  - `--max_speakers`

#### Segmentation / window length

- In training dataset:
  - `chunk_size`
  - `chunk_shift`
- In model configs:
  - `chunk_size`
- In inference:
  - `--seg_duration`
  - `--segmentation_step`

#### Clustering thresholds

- Agglomerative:
  - `--ahc_threshold`
  - `--min_cluster_size`
- VBx:
  - `--ahc_criterion`
  - `--ahc_threshold`
  - `--Fa`
  - `--Fb`
  - `--lda_dim`
  - `--max_iters`

#### Evaluation settings

- In stage scripts:
  - `collar`
  - `val_metric`
  - `val_mode`
  - `avg_ckpt_num`

## 7. Recommended Learning Path

### Read these first

1. [`README.md`](./README.md)
   This gives the project pitch, install steps, and the minimal inference example.

2. [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py)
   Best single file for the deployed inference mental model.

3. [`recipes/diar_ssl/run_stage.sh`](./recipes/diar_ssl/run_stage.sh)
   Best single file for understanding how the authors train, infer, and score.

4. [`recipes/diar_ssl/conf/wavlm_updated_conformer.toml`](./recipes/diar_ssl/conf/wavlm_updated_conformer.toml)
   Best config for seeing what knobs matter in the main WavLM setup.

### Second layer

5. [`recipes/diar_ssl/dataset.py`](./recipes/diar_ssl/dataset.py)
   Explains dataset expectations, chunking, and RTTM-to-frame-label conversion.

6. [`diarizen/models/eend/model_wavlm_conformer.py`](./diarizen/models/eend/model_wavlm_conformer.py)
   Core single-channel WavLM-based segmentation model.

7. [`recipes/diar_ssl/run_dual_opt.py`](./recipes/diar_ssl/run_dual_opt.py)
   Shows how configs instantiate models, optimizers, and trainers.

8. [`diarizen/trainer_dual_opt.py`](./diarizen/trainer_dual_opt.py)
   Explains experiment directory layout, checkpointing, validation, and training control.

### Third layer

9. [`diarizen/clustering/VBx.py`](./diarizen/clustering/VBx.py)
   Read this if you want to understand how speaker assignment works beyond generic clustering.

10. [`diarizen/models/eend/model_pyannote.py`](./diarizen/models/eend/model_pyannote.py)
    Useful as a simpler baseline model.

11. [`diarizen/models/eend/model_wavlm_conformer_mc.py`](./diarizen/models/eend/model_wavlm_conformer_mc.py)
    Read after you already understand the single-channel model.

12. [`recipes/diar_ssl_pruning/README.md`](./recipes/diar_ssl_pruning/README.md) and [`recipes/diar_ssl_pruning/conf/s80_large.toml`](./recipes/diar_ssl_pruning/conf/s80_large.toml)
    Read only when you want pruning-specific details.

### What not to read first

- Do not start inside the full vendored [`pyannote-audio/`](./pyannote-audio) tree.
- Do not start with the pruning code unless compact-model research is your immediate goal.
- Do not start by reading every trainer helper file. You can defer those until you know the top-level flow.

## 8. Practical First Commands

These are safe, non-destructive commands. They do not install anything.

### Verify the repo layout

```powershell
Get-ChildItem
```

### Verify key files exist

```powershell
Get-ChildItem diarizen, recipes, example, pyannote-audio, dscore
```

### Inspect the included example audio

```powershell
Get-Item .\example\EN2002a_30s.wav
```

### Check whether Python can import the package from the repo root

```powershell
python -c "import diarizen; print(diarizen.__file__)"
```

### Check whether the packaged inference module imports

```powershell
python -c "from diarizen.pipelines.inference import DiariZenPipeline; print(DiariZenPipeline)"
```

This may fail if dependencies are not installed yet. That is still useful information.

### Minimal smoke test without assuming downloads

```powershell
python -c "from pathlib import Path; p=Path('example/EN2002a_30s.wav'); print(p.exists(), p.stat().st_size)"
```

### If your environment is already installed and network/model access is available

```powershell
python -c "from diarizen.pipelines.inference import DiariZenPipeline; p=DiariZenPipeline.from_pretrained('BUT-FIT/diarizen-wavlm-large-s80-md'); r=p('example/EN2002a_30s.wav'); print(r)"
```

### If you have local model files already

```powershell
python -m diarizen.pipelines.inference `
  --in_wav_scp recipes/diar_ssl/data/AMI_AliMeeting_AISHELL4/test/AMI/wav.scp `
  --diarizen_hub <local_diarizen_model_dir> `
  --embedding_model <local_embedding_model.bin> `
  --rttm_out_dir out_rttm
```

Notes:
- The packaged CLI expects a `wav.scp`, not just a raw `.wav` path.
- The simplest raw-file inference path is the Python API shown in the README.

## 9. Questions and Risks

### Unclear or underdocumented parts

- There is no single documented “prepare your dataset” script; the repo mostly documents the expected metadata layout rather than a universal prep pipeline.
- The exact structure of a local `diarizen_hub` directory is implied by code rather than fully documented. It appears to need at least:
  - `config.toml`
  - `pytorch_model.bin`
  - `plda/` for VBx
- The README shows Hugging Face inference, but the exact local/offline model layout is not spelled out in one place.
- The training/evaluation recipes are mostly shell-script driven and assume you are comfortable editing hardcoded paths.

### Things to be careful about

- Large downloads:
  Hugging Face model downloads can be sizable.

- GPU requirements:
  Training clearly assumes GPUs and uses `accelerate launch` with multiple processes in the example scripts.

- Path placeholders:
  Many recipe scripts contain `/YOUR_PATH/...` placeholders and are not ready to run without editing.

- Hugging Face access:
  Pretrained loading depends on network access and local cache state.

- Submodule state:
  `dscore/` depends on git submodule initialization.

- Local editable install assumptions:
  The root package and vendored `pyannote-audio` are both expected to be installed in editable mode.

- Dataset availability:
  The repo includes metadata examples and one short WAV, but not the full training datasets.

- Channel assumptions:
  Several single-channel paths explicitly force the first channel, which matters if your input audio is multichannel.

- VBx asset assumptions:
  VBx needs PLDA transform files in the expected directory structure.

- Version coupling:
  DiariZen subclasses internal pyannote components, so upgrading pyannote independently may break things.

## 10. Files to Study First

If you only read seven files, read these:

1. [`README.md`](./README.md)
2. [`diarizen/pipelines/inference.py`](./diarizen/pipelines/inference.py)
3. [`recipes/diar_ssl/run_stage.sh`](./recipes/diar_ssl/run_stage.sh)
4. [`recipes/diar_ssl/conf/wavlm_updated_conformer.toml`](./recipes/diar_ssl/conf/wavlm_updated_conformer.toml)
5. [`recipes/diar_ssl/dataset.py`](./recipes/diar_ssl/dataset.py)
6. [`diarizen/models/eend/model_wavlm_conformer.py`](./diarizen/models/eend/model_wavlm_conformer.py)
7. [`diarizen/trainer_dual_opt.py`](./diarizen/trainer_dual_opt.py)

That set gives you the highest-value mental model with the least reading overhead.
