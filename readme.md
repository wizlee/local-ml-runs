## Overview

Exploring the world of stable difussion

## History

- Trying stable-diffusion-webui
    - the `code-workspace` file in this repo contains some bootscraps that assumes you have 
        - a conda environment called `pytorch`. This can be easily modified to any other name.
        - that you are using bash (Git Bash or GNU Bash should both work)
- [The steps in the repo's readme](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-NVidia-GPUs#alternative-installation-on-windows-using-conda) matches what I did closely in creating the conda environment. Briefly as below:
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


## References
- https://github.com/Stability-AI/StableDiffusion
- https://github.com/AUTOMATIC1111/stable-diffusion-webui
