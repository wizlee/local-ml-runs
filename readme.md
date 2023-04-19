## Overview

Exploring the world of stable difussion and LLMs

## History

### Exploring stable-diffusion-webui
- the `code-workspace` file in this repo contains some bootscraps that assumes you have 
    - a conda environment called `pytorch`. This can be easily modified to any other name.
    - that you are using bash (Git Bash or GNU Bash should both work)
- [The steps in the repo's readme](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-NVidia-GPUs#alternative-installation-on-windows-using-conda) matches what I did closely in creating the conda environment. Briefly as below:
- Run the commands below
    ```bash
    # Create environment
    conda create -n pytorch python=3.10.6
    # Activate environment
    conda active pytorch
    # Start local webserver
    ./webui-user.bat
    # Wait for "Running on local URL:  http://127.0.0.1:7860" and open that URI.
    ```
- note that `webui-user.bat` will create a python virtual environment despite dedicated conda environment is created. If you don't prefer this to happen perform what the script does manually. 
    - or don't use conda and directly use your system python.
- Download the stable diffusion models from [here for v2.1](https://huggingface.co/stabilityai/stable-diffusion-2-1) and [here for v1.4](https://drive.yerf.org/wl/?id=EBfTrmcCCUAGaQBXVIj5lJmEhjoP1tgl).
- Need use additional arg of `--xformers` and `--medvram` if GPU VRAM is less than 12GB. After using these arg web server is successfully launched even using GPU of only 2GB VRAM.
- HOWEVER, there might still be error when trying to generate image using GPU with less VRAM (i.e. 2GB)
- References
    - https://github.com/Stability-AI/StableDiffusion
    - https://github.com/AUTOMATIC1111/stable-diffusion-webui

### Exploring VideoCrafter
- After following the official setup guide, tried to run VideoControl model
- Got a warning of symlink not enabled and will result in more disk space use. Two solutions
    - Run as admin
    - Enable Developer mode in windows (went for this)
- Got another error when trying to run model, `AssertionError: Torch not compiled with CUDA enabled`
    - Tried install torch with cuda `conda install pytorch==1.13.1 torchvision==0.14.1 torchaudio==0.13.1 pytorch-cuda=11.6 -c pytorch -c nvidia`
    - Found that the solver is too slow and upgraded conda to use [libmamba solver](https://conda.github.io/conda-libmamba-solver/faq/)
    - Update conda version forcefully using conda install conda={latest conda version} because of this [exact error](https://github.com/conda/conda/issues/8269)
- Need to install chardet after getting `ModuleNotFoundError: No module named 'chardet'` error
    - `pip install chardet`
- However, despite the python command suceed in this stage, ran into the issue of insufficient GPU RAM as stated in the readme of the project
    - Minimum of 7GB VRAM is needed, my machine had 6GB 😰
