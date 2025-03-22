# ai-video-work
My local AI environment setup.

## Setup 

1. Install mise
```
curl -sSL https://raw.githubusercontent.com/mise-app/mise/main/install.sh | sh
``` 
2. Create a virtual environment using mise
```
mise install
mise run install-dep
```

3. Clone the ComfyUI repository in the current directory
```
git clone https://github.com/Comfy-AI/ComfyUI.git
```

##  Launch ComfyUI
```
cd ComfyUI
python main.py 
```


