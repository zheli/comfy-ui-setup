# ai-video-work
My local setup for ComfyUI and related AI video experiments.

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
comfy --workspace "$(pwd)/comfy-managed" install 

# optional: set that workspace as the default target for future comfy commands
comfy set-default "$(pwd)/comfy-managed"

# launch the comfy-ui instance (adds --background to detach)
comfy launch

# update later when new versions drop
comfy update
```
`comfy-cli` creates and maintains a dedicated virtualenv inside `comfy-managed/.venv`, so it stays isolated from the system Python and from the manual checkout below.

### Download Wan2.2 Models
Use `comfy model download` to fetch the required models. These commands assume you have set the default workspace as shown above.

```bash
# Diffusion Model (Wan2.2 5B FP16) -> models/diffusion_models
comfy model download \
  --url https://huggingface.co/Comfy-Org/Wan_2.2_ComfyUI_Repackaged/resolve/main/split_files/diffusion_models/wan2.2_ti2v_5B_fp16.safetensors \
  --relative-path models/diffusion_models

# Text Encoder (UMT5 XXL FP8 Scaled) -> models/text_encoders
comfy model download \
  --url https://huggingface.co/Comfy-Org/Wan_2.2_ComfyUI_Repackaged/resolve/main/split_files/text_encoders/umt5_xxl_fp8_e4m3fn_scaled.safetensors \
  --relative-path models/text_encoders

# VAE (Wan2.2) -> models/vae
comfy model download \
  --url https://huggingface.co/Comfy-Org/Wan_2.2_ComfyUI_Repackaged/resolve/main/split_files/vae/wan2.2_vae.safetensors \
  --relative-path models/vae
```