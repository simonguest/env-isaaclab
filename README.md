# env-isaaclab

A uv-managed Python environment for NVIDIA IsaacLab experiments. The project ships with pinned dependencies, custom package indexes, and a minimal CUDA test script so you can bootstrap IsaacLab + Isaac Sim quickly on a fresh machine.

## Prerequisites
- [`uv` CLI](https://github.com/astral-sh/uv) v0.4 or newer.
- NVIDIA GPU drivers and CUDA 12.6 runtime that match your Isaac Sim install.

## Quick Start
1. Clone this repository and switch into it.
2. Create or update the virtual environment from the lockfile:
   ```powershell
   uv sync
   ```
3. Activate the virtual environment (PowerShell example):
   ```powershell
   .\.venv\Scripts\Activate.ps1
   ```
4. Run IsaacSim!
   ```powershell
   isaacsim
   ```

## Verifying CUDA availability:
   ```powershell
   uv run python scripts/test_cuda.py
   ```
   If this raises a `RuntimeError`, check that the correct GPU driver and CUDA toolkit are installed.

## Managing Dependencies
- Update packages while respecting the locked versions:
  ```powershell
  uv sync --upgrade          # upgrade everything respecting indexes
  uv sync --upgrade-package isaaclab
  ```
- Edit `pyproject.toml` to add new dependencies. After changes, run `uv lock` (optional) followed by `uv sync` to refresh `.venv` and `uv.lock`.
- The `[tool.uv.index]` entries define custom indexes for CUDA-enabled wheels. Keep them in sync with the CUDA runtime you intend to use.

## Troubleshooting
- **Package download failures:** Confirm you can reach the NVIDIA and PyTorch indexes listed in `pyproject.toml`. Corporate proxies may need additional configuration.
- **Mismatched CUDA versions:** Ensure the PyTorch index (`pytorch-cu126`) aligns with your installed CUDA runtime. Adjust the index names and versions if you move to a newer Isaac Sim release.
- **Environment drift:** Re-run `uv sync --locked` to recreate the environment exactly as captured in `uv.lock`.

## References
- Isaac Lab documentation: <https://developer.nvidia.com/isaac-lab>
- Isaac Sim system requirements: <https://docs.nvidia.com/isaacsim/latest/transfer/install_faq.html>
- uv user guide: <https://docs.astral.sh/uv/>
