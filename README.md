# env-isaaclab

A uv-managed Python environment that makes it easier to run NVIDIA IsaacLab experiments on Windows. The project ships with pinned dependencies, custom package indexes, and VS Code settings to make it easy to bootstrap IsaacLab + Isaac Sim quickly on a fresh machine.

## Prerequisites
- [`uv` CLI](https://github.com/astral-sh/uv) v0.4 or newer.
- NVIDIA GPU drivers and CUDA 12.6 runtime that match your Isaac Sim install.

## Quick Start
1. Create or update the virtual environment from the lockfile:
```powershell
uv sync
```
2. Activate the virtual environment (PowerShell prompt):
```powershell
.\.venv\Scripts\Activate
```
3. Run IsaacSim!
```powershell
isaacsim
```

## Verifying CUDA availability:
```powershell
uv run python test_cuda.py
```
If this raises a `RuntimeError`, check that the correct GPU driver and CUDA toolkit are installed.

## Configuring Pylance in VS Code

By default, Intellisense in VS Code will not work for the isaaclab and issacsim packages. To fix, first edit the Isaac Sim main.py file in `.venv\Lib\site-packages\isaacsim\__main__.py`. Replace the following code (on line 118):

```python
for folder in ["exts", "extscache", "extsDeprecated", "extsUser"]:
```

With

```python
for folder in ["exts"]:
```

Then run the 'generate-vscode-settings' command in IsaacSim:

```powershell
python -m isaacsim --generate-vscode-settings
```

This will insert mock files into the Isaac Sim package extensions (.venv\Lib\site-packages\isaacsim\exts). Unfortunately, it generates an incomplete .vscode\settings.json (and one that is incompatible with Windows). To fix, overwrite the generated settings.json with a working version:
```powershell
cp .\.vscode\settings-working.json .\.vscode\settings.json
```

## Scripts

The scripts (in the scripts folder) are a snapshot from [NVIDIA's IsaacLab repo](https://github.com/isaac-sim/IsaacLab) to test everything is working correctly. These scripts are not kept in sync with latest updates, however, so you should always use the IsaacLab repo as the source of truth. 

## Running and Debugging Scripts

`.vscode/launch.json` contains examples of how to list environments, create a new scene, and train/run the CartPole example. 

## Troubleshooting
- **Package download failures:** Confirm you can reach the NVIDIA and PyTorch indexes listed in `pyproject.toml`. Corporate proxies may need additional configuration.
- **Mismatched CUDA versions:** Ensure the PyTorch index (`pytorch-cu126`) aligns with your installed CUDA runtime. Adjust the index names and versions if you move to a newer Isaac Sim release.
- **Environment drift:** Re-run `uv sync --locked` to recreate the environment exactly as captured in `uv.lock`.

## Recreating the Environment

To recreate the environment remove the virtual environment and reinstall:

```powershell
rm .\.venv
uv sync --reinstall
```

## References
- Isaac Lab documentation: <https://developer.nvidia.com/isaac-lab>
- Isaac Sim system requirements: <https://docs.nvidia.com/isaacsim/latest/transfer/install_faq.html>
- uv user guide: <https://docs.astral.sh/uv/>
