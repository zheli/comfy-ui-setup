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

#### Use GGUF models for 5080
```
# GGUF (Wan2.2 5B FP16) -> models/unet
comfy model download \
  --url https://huggingface.co/QuantStack/Wan2.2-T2V-A14B-GGUF/resolve/main/HighNoise/Wan2.2-T2V-A14B-HighNoise-Q6_K.gguf\?download\=true \
  --relative-path models/unet

comfy model download \
  --url https://huggingface.co/QuantStack/Wan2.2-T2V-A14B-GGUF/resolve/main/LowNoise/Wan2.2-T2V-A14B-LowNoise-Q6_K.gguf\?download\=true \
  --relative-path models/unet

comfy model download \
  --url https://huggingface.co/Comfy-Org/Wan_2.2_ComfyUI_Repackaged/resolve/main/split_files/loras/wan2.2_t2v_lightx2v_4steps_lora_v1.1_high_noise.safetensors \
  --relative-path models/loras

comfy model download \
  --url https://huggingface.co/Comfy-Org/Wan_2.2_ComfyUI_Repackaged/resolve/main/split_files/loras/wan2.2_t2v_lightx2v_4steps_lora_v1.1_low_noise.safetensors \
  --relative-path models/loras

comfy model download \
  --url https://huggingface.co/QuantStack/Wan2.2-I2V-A14B-GGUF/resolve/main/HighNoise/Wan2.2-I2V-A14B-HighNoise-Q6_K.gguf?download=true \
  --relative-path models/unet



comfy model download \
  --url https://huggingface.co/QuantStack/Wan2.2-I2V-A14B-GGUF/resolve/main/LowNoise/Wan2.2-I2V-A14B-LowNoise-Q6_K.gguf\?download\=true \
  --relative-path models/unet

comfy model download \
  --url https://huggingface.co/Comfy-Org/Wan_2.2_ComfyUI_Repackaged/resolve/main/split_files/loras/wan2.2_i2v_lightx2v_4steps_lora_v1_high_noise.safetensors \
  --relative-path models/loras

comfy model download \
  --url https://huggingface.co/Comfy-Org/Wan_2.2_ComfyUI_Repackaged/resolve/main/split_files/loras/wan2.2_i2v_lightx2v_4steps_lora_v1_low_noise.safetensors \
  --relative-path models/loras
```

#### Install Kijai ComfyUI-WanVideoWrapper
```bash
comfy node install https://github.com/kijai/ComfyUI-WanVideoWrapper.git

source .venv/bin/activate
pip install -r comfy-managed/custom_nodes/ComfyUI-WanVideoWrapper/requirements.txt
```

### Download Z-Image Turbo Models
Use `comfy model download` to fetch the required models for Z-Image Turbo.

```bash
# Diffusion Model (Z-Image Turbo BF16) -> models/diffusion_models
comfy model download \
  --url https://huggingface.co/Comfy-Org/z_image_turbo/resolve/main/split_files/diffusion_models/z_image_turbo_bf16.safetensors \
  --relative-path models/diffusion_models

# Text Encoder (Qwen 3.4B) -> models/text_encoders
comfy model download \
  --url https://huggingface.co/Comfy-Org/z_image_turbo/resolve/main/split_files/text_encoders/qwen_3_4b.safetensors \
  --relative-path models/text_encoders

# VAE (Flux AE) -> models/vae
comfy model download \
  --url https://huggingface.co/Comfy-Org/z_image_turbo/resolve/main/split_files/vae/ae.safetensors \
  --relative-path models/vae

# LoRA (Pixel Art Style) -> models/loras
comfy model download \
  --url https://huggingface.co/tarn59/pixel_art_style_lora_z_image_turbo/resolve/main/pixel_art_style_z_image_turbo.safetensors \
  --relative-path models/loras
```

### Network Access (LAN / Tailscale)
To access ComfyUI from other devices on your local network (e.g. `192.168.1.x`) or via VPNs like Tailscale (e.g. `100.x.y.z`), pass the `--listen` argument to the underlying ComfyUI process.

```bash
# Listen on all network interfaces (accessible from LAN and Tailscale IPs)
comfy launch -- --listen 0.0.0.0
```