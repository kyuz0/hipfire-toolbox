# Hipfire Toolboxes

This project provides pre-built containers (“toolboxes”) for running the [hipfire](https://github.com/Kaden-Schutt/hipfire) LLM inference engine on AMD GPUs, particularly targeting Ryzen AI Max "Strix Halo" integrated GPUs. Toolbx is the standard developer container system in Fedora (and now works on Ubuntu, openSUSE, Arch, etc).

## Included Toolboxes

| Container Tag | Backend/Stack | Purpose / Notes |
| :--- | :--- | :--- |
| `rocm-7.2.4` | ROCm 7.2.4 | Built on Fedora 43, includes full toolchains (Cargo, Bun, hipcc) for JIT compilation. |

## Quick Start

### 1. Create the Toolbox
Create and enter the toolbox. **(Ubuntu users: remember to use `distrobox` instead of `toolbox`).**

```sh
toolbox create hipfire-rocm-7.2.4 \
  --image docker.io/kyuz0/hipfire-toolbox:rocm-7.2.4 \
  -- --device /dev/dri --device /dev/kfd \
  --group-add video --group-add render --group-add sudo --security-opt seccomp=unconfined

toolbox enter hipfire-rocm-7.2.4
```

### 2. Verify Installation
Inside the toolbox, check that hipfire can see your GPU and ROCm runtime:
```sh
hipfire diag
```

### 3. Run Inference
The `hipfire` wrapper is globally available. On the first run, the JIT compiler will automatically compile kernels for your GPU architecture using the built-in toolchains.

```sh
hipfire pull qwen3.5:0.8b
hipfire run qwen3.5:0.8b "Hello, world!"
```

To run the OpenAI-compatible HTTP server:
```sh
hipfire serve -d
```

## Keeping Updated

Refresh your local toolbox container to the latest image build:
```sh
./refresh-toolboxes.sh all
```
