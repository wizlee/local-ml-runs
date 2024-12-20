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
- Updated CUDA version from 11.6 to 12.1. Pending retrying all the steps again.
    - Steps
        - `conda create -n lvdm python=3.8.5`
        - `conda activate lvdm`
        - `pip install -r requirements_xformer.txt`
        - Overwrite pip install of pytorch with pytorch compiled with CUDA
            - `conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia`
        - `pip install chardet`
        - Lastly the steps from the readme for running `VideoControl`
            ```bash
            PROMPT="An ostrich walking in the desert, photorealistic, 4k"
            VIDEO="input/flamingo.mp4"
            OUTDIR="results/"

            NAME="video_adapter"
            CONFIG_PATH="models/adapter_t2v_depth/model_config.yaml"
            BASE_PATH="models/base_t2v/model.ckpt"
            ADAPTER_PATH="models/adapter_t2v_depth/adapter.pth"

            python scripts/sample_text2video_adapter.py \
                --seed 123 \
                --ckpt_path $BASE_PATH \
                --adapter_ckpt $ADAPTER_PATH \
                --base $CONFIG_PATH \
                --savedir $OUTDIR/$NAME \
                --bs 1 --height 256 --width 256 \
                --frame_stride -1 \
                --unconditional_guidance_scale 15.0 \
                --ddim_steps 50 \
                --ddim_eta 1.0 \
                --prompt "$PROMPT" \
                --video $VIDEO
            ```
    - Conclusions
        - Still hitting the VRAM insufficient issue. Have to put off running any models that requires more than 6GB GPU memory locally. ☠️

### Exploring Genesis
- Initially try to use the experimental conda-pypi to install this project using `conda pip install .`.
    - However faces an error that I am not able to quickly overcome.
- Thus, below is the current steps I took to install this project.
    - Create env if not exist: `conda create -n pytorch python=3.10`
        - Activate: `conda activate pytorch`
    - Install pytorch: `conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia`
    - Install project from pyproject: `pip install .`
        - This is considered not a good practise when install packages in conda other than python and pip. (we installed pytorch using conda)
        - The reason is pip will override what conda has already resolved and conda will also not know what pip did. 
