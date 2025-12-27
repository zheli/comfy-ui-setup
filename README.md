# ai-video-work
Local playground for ComfyUI and related AI video experiments.

## Manjaro quick start
- Install `mise` tooling (optional but recommended for this repo).
- Install `pipx` and `comfy-cli` so ComfyUI is managed in an isolated environment.
- Set up the project Python venv with `mise` for scripts that live in this repository.

## Install pipx and comfy-cli (Manjaro)
```bash
# pipx is packaged for Manjaro / Arch Linux
sudo pacman -S python-pipx

# ensure ~/.local/bin is on PATH (add to your shell profile if prompted)
pipx ensurepath

# install comfy-cli into its own pipx-managed environment
pipx install comfy-cli
```

### Provision ComfyUI with comfy-cli
```bash
# from the project root
cd path/to/ai-video-work

# install ComfyUI into a repo-local workspace managed by comfy-cli
comfy install --workspace "$(pwd)/comfy-managed"

# optional: make that workspace the default target for future comfy commands
comfy set-default --workspace "$(pwd)/comfy-managed"

# launch the comfy-ui instance (adds --background to detach)
comfy launch

# update later when new versions drop
comfy update
```
`comfy-cli` creates and maintains a dedicated virtualenv inside `comfy-managed/.venv`, so it stays isolated from the system Python and from the manual checkout below.

## Project tooling with mise
This repository still maintains its own Python virtual environment for custom scripts and experiments.

```bash
# install mise if you do not have it yet
curl -sSL https://raw.githubusercontent.com/mise-app/mise/main/install.sh | sh

# create or update the .venv defined in .mise.toml (Python 3.13.1)
mise install
source .venv/bin/activate

# install repo dependencies (torch, torchvision, huggingface tools)
mise run install-dep
```

## Manual ComfyUI checkout (optional)
If you need a separate copy for source-level tweaks, keep it in this repo so it does not interfere with the comfy-cli install location:

```bash
git clone https://github.com/Comfy-AI/ComfyUI.git
cd ComfyUI
pip install -r requirements.txt
python main.py
```
