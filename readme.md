# Fooocus

- https://cloudstudio.net/a/28358727150886912

/workspace/.vscode/preview.yml

```
autoOpen: false

apps:
  - port: 7865
    run: |
      # 1. 创建并切换到 fooocus 环境
      conda env create -f environment.yaml
      conda activate fooocus

      # 2. 安装 Python 依赖
      pip install -r requirements_versions.txt

      # 3. 启动你的应用
      python entry_with_update.py
    root: .                   # 项目根目录
    name: Fooocus-App
    description: 启动 fooocus 环境并运行 entry_with_update.py（包括环境创建和依赖安装）
    autoOpen: true

```





### Windows

You can directly download Fooocus with:

**[>>> Click here to download <<<](https://github.com/lllyasviel/Fooocus/releases/download/v2.5.0/Fooocus_win64_2-5-0.7z)**

After you download the file, please uncompress it and then run the "run.bat".

Below is a test on a relatively low-end laptop with **16GB System RAM** and **6GB VRAM** (Nvidia 3060 laptop). The speed on this machine is about 1.35 seconds per iteration. Pretty impressive – nowadays laptops with 3060 are usually at very acceptable price.


### Python

```
import os
import subprocess

def run_command(command, cwd=None):
    """Helper function to run shell commands and handle errors."""
    try:
        subprocess.run(command, shell=True, check=True, cwd=cwd)
    except subprocess.CalledProcessError as e:
        print(f"Error while running command: {command}\n{e}")
        exit(1)

# Step 1: 安装依赖
run_command("pip install pygit2==1.15.1")

# Step 2: 设置工作目录
workspace_dir = "/content"
fooocus_dir = os.path.join(workspace_dir, "Fooocus")

# 确保工作目录存在
if not os.path.exists(workspace_dir):
    os.makedirs(workspace_dir)

# 切换到工作目录
os.chdir(workspace_dir)

# Step 3: 克隆项目
if not os.path.exists(fooocus_dir):
    run_command("git clone https://github.com/lllyasviel/Fooocus.git")
else:
    print("Fooocus 项目已存在，跳过克隆步骤")

# Step 4: 切换到项目目录
if os.path.exists(fooocus_dir):
    os.chdir(fooocus_dir)
else:
    raise FileNotFoundError("Fooocus 项目未正确克隆，请检查网络连接或仓库URL")

# Step 5: 检查脚本是否存在并运行
entry_script = "entry_with_update.py"
if os.path.exists(entry_script):
    run_command(f"python {entry_script} --share --always-high-vram", cwd=fooocus_dir)
else:
    raise FileNotFoundError(f"{entry_script} 文件未找到，请确认 Fooocus 项目是否完整")
```

### Colab

(Last tested - 2024 Aug 12 by [mashb1t](https://github.com/mashb1t))

| Colab | Info
| --- | --- |
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/lllyasviel/Fooocus/blob/main/fooocus_colab.ipynb) | Fooocus Official

```
!pip install pygit2==1.15.1
%cd /content
!git clone https://github.com/lllyasviel/Fooocus.git
%cd /content/Fooocus
!python entry_with_update.py --share --always-high-vram
```

Thanks to [camenduru](https://github.com/camenduru) for the template!

### Linux (Using Anaconda)

If you want to use Anaconda/Miniconda, you can

    git clone https://github.com/lllyasviel/Fooocus.git
    cd Fooocus
    conda env create -f environment.yaml
    conda activate fooocus
    pip install -r requirements_versions.txt

Then download the models: download [default models](#models) to the folder "Fooocus\models\checkpoints". **Or let Fooocus automatically download the models** using the launcher:

    conda activate fooocus
    python entry_with_update.py

Or, if you want to open a remote port, use

    conda activate fooocus
    python entry_with_update.py --listen

Use `python entry_with_update.py --preset anime` or `python entry_with_update.py --preset realistic` for Fooocus Anime/Realistic Edition.

### Linux (Using Python Venv)

Your Linux needs to have **Python 3.10** installed, and let's say your Python can be called with the command **python3** with your venv system working; you can

    git clone https://github.com/lllyasviel/Fooocus.git
    cd Fooocus
    python3 -m venv fooocus_env
    source fooocus_env/bin/activate
    pip install -r requirements_versions.txt

See the above sections for model downloads. You can launch the software with:

    source fooocus_env/bin/activate
    python entry_with_update.py

Or, if you want to open a remote port, use

    source fooocus_env/bin/activate
    python entry_with_update.py --listen

Use `python entry_with_update.py --preset anime` or `python entry_with_update.py --preset realistic` for Fooocus Anime/Realistic Edition.

### Linux (Using native system Python)

If you know what you are doing, and your Linux already has **Python 3.10** installed, and your Python can be called with the command **python3** (and Pip with **pip3**), you can

    git clone https://github.com/lllyasviel/Fooocus.git
    cd Fooocus
    pip3 install -r requirements_versions.txt

See the above sections for model downloads. You can launch the software with:

    python3 entry_with_update.py

Or, if you want to open a remote port, use

    python3 entry_with_update.py --listen

Use `python entry_with_update.py --preset anime` or `python entry_with_update.py --preset realistic` for Fooocus Anime/Realistic Edition.

### Linux (AMD GPUs)

Note that the [minimal requirement](#minimal-requirement) for different platforms is different.

Same with the above instructions. You need to change torch to the AMD version

    pip uninstall torch torchvision torchaudio torchtext functorch xformers 
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/rocm5.6

AMD is not intensively tested, however. The AMD support is in beta.

Use `python entry_with_update.py --preset anime` or `python entry_with_update.py --preset realistic` for Fooocus Anime/Realistic Edition.

### Windows (AMD GPUs)

Note that the [minimal requirement](#minimal-requirement) for different platforms is different.

Same with Windows. Download the software and edit the content of `run.bat` as:

    .\python_embeded\python.exe -m pip uninstall torch torchvision torchaudio torchtext functorch xformers -y
    .\python_embeded\python.exe -m pip install torch-directml
    .\python_embeded\python.exe -s Fooocus\entry_with_update.py --directml
    pause

Then run the `run.bat`.

AMD is not intensively tested, however. The AMD support is in beta.

For AMD, use `.\python_embeded\python.exe entry_with_update.py --directml --preset anime` or `.\python_embeded\python.exe entry_with_update.py --directml --preset realistic` for Fooocus Anime/Realistic Edition.

### Mac

Note that the [minimal requirement](#minimal-requirement) for different platforms is different.

Mac is not intensively tested. Below is an unofficial guideline for using Mac. You can discuss problems [here](https://github.com/lllyasviel/Fooocus/pull/129).

You can install Fooocus on Apple Mac silicon (M1 or M2) with macOS 'Catalina' or a newer version. Fooocus runs on Apple silicon computers via [PyTorch](https://pytorch.org/get-started/locally/) MPS device acceleration. Mac Silicon computers don't come with a dedicated graphics card, resulting in significantly longer image processing times compared to computers with dedicated graphics cards.

1. Install the conda package manager and pytorch nightly. Read the [Accelerated PyTorch training on Mac](https://developer.apple.com/metal/pytorch/) Apple Developer guide for instructions. Make sure pytorch recognizes your MPS device.
1. Open the macOS Terminal app and clone this repository with `git clone https://github.com/lllyasviel/Fooocus.git`.
1. Change to the new Fooocus directory, `cd Fooocus`.
1. Create a new conda environment, `conda env create -f environment.yaml`.
1. Activate your new conda environment, `conda activate fooocus`.
1. Install the packages required by Fooocus, `pip install -r requirements_versions.txt`.
1. Launch Fooocus by running `python entry_with_update.py`. (Some Mac M2 users may need `python entry_with_update.py --disable-offload-from-vram` to speed up model loading/unloading.) The first time you run Fooocus, it will automatically download the Stable Diffusion SDXL models and will take a significant amount of time, depending on your internet connection.

Use `python entry_with_update.py --preset anime` or `python entry_with_update.py --preset realistic` for Fooocus Anime/Realistic Edition.

### Docker

See [docker.md](docker.md)

### Download Previous Version

See the guidelines [here](https://github.com/lllyasviel/Fooocus/discussions/1405).

## Minimal Requirement

Below is the minimal requirement for running Fooocus locally. If your device capability is lower than this spec, you may not be able to use Fooocus locally. (Please let us know, in any case, if your device capability is lower but Fooocus still works.)

| Operating System  | GPU                          | Minimal GPU Memory           | Minimal System Memory     | [System Swap](troubleshoot.md) | Note                                                                       |
|-------------------|------------------------------|------------------------------|---------------------------|--------------------------------|----------------------------------------------------------------------------|
| Windows/Linux     | Nvidia RTX 4XXX              | 4GB                          | 8GB                       | Required                       | fastest                                                                    |
| Windows/Linux     | Nvidia RTX 3XXX              | 4GB                          | 8GB                       | Required                       | usually faster than RTX 2XXX                                               |
| Windows/Linux     | Nvidia RTX 2XXX              | 4GB                          | 8GB                       | Required                       | usually faster than GTX 1XXX                                               |
| Windows/Linux     | Nvidia GTX 1XXX              | 8GB (&ast; 6GB uncertain)    | 8GB                       | Required                       | only marginally faster than CPU                                            |
| Windows/Linux     | Nvidia GTX 9XX               | 8GB                          | 8GB                       | Required                       | faster or slower than CPU                                                  |
| Windows/Linux     | Nvidia GTX < 9XX             | Not supported                | /                         | /                              | /                                                                          |
| Windows           | AMD GPU                      | 8GB    (updated 2023 Dec 30) | 8GB                       | Required                       | via DirectML (&ast; ROCm is on hold), about 3x slower than Nvidia RTX 3XXX |
| Linux             | AMD GPU                      | 8GB                          | 8GB                       | Required                       | via ROCm, about 1.5x slower than Nvidia RTX 3XXX                           |
| Mac               | M1/M2 MPS                    | Shared                       | Shared                    | Shared                         | about 9x slower than Nvidia RTX 3XXX                                       |
| Windows/Linux/Mac | only use CPU                 | 0GB                          | 32GB                      | Required                       | about 17x slower than Nvidia RTX 3XXX                                      |

&ast; AMD GPU ROCm (on hold): The AMD is still working on supporting ROCm on Windows.

&ast; Nvidia GTX 1XXX 6GB uncertain: Some people report 6GB success on GTX 10XX, but some other people report failure cases.

*Note that Fooocus is only for extremely high quality image generating. We will not support smaller models to reduce the requirement and sacrifice result quality.*

