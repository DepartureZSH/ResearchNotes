# My PC

```bash
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 22.04.4 LTS
Release:	22.04
Codename:	jammy

$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2023 NVIDIA Corporation
Built on Tue_Jun_13_19:16:58_PDT_2023
Cuda compilation tools, release 12.2, V12.2.91
Build cuda_12.2.r12.2/compiler.32965470_0

```
# Installation  
  
## Requirements  
  
* Python 3.9
* Pytorch 2.4.0
* cuda 12.1
  
## Setup with Conda  

```sh  
# create a new environment  
conda create --name CLEVER python=3.9
conda activate CLEVER

# install pytorch  
conda install pytorch==2.4.0 torchvision==0.19.0 torchaudio==2.4.0 pytorch-cuda=12.1 -c pytorch -c nvidia 
  
export INSTALL_DIR=$PWD  
  
# install apex  
cd $INSTALL_DIR  
git clone https://github.com/NVIDIA/apex.git  
cd apex
pip install -r requirements.txt
```

open py file apex/setup.py add this in around line 39: 
```python
if (bare_metal_version != torch_binary_version):
	# allow minor version mismatch
	if bare_metal_version.major == torch_binary_version.major and bare_metal_version.minor != torch_binary_version.minor:
	return
	
	raise RuntimeError(
             "Cuda extensions are being compiled with a version of Cuda that does "
             "not match the version used to compile Pytorch binaries.  "
```


https://github.com/NVIDIA/apex/issues/1792

then continue
```bash
python setup.py install --cuda_ext --cpp_ext  
  
# install oscar  
cd $INSTALL_DIR  
git clone --recursive https://github.com/thunlp/CLEVER.git  
cd CLEVER/src/Oscar/coco_caption  
./get_stanford_models.sh  
cd ..  
python setup.py build develop  
  
# install requirements  
pip install -r requirements.txt  
  
  
# for Prompt-FT baseline and LAMA  
conda create --name CLEVER_prompt_env python=3.9 
conda activate CLEVER  
conda install pytorch==2.4.0 torchvision==0.19.0 torchaudio==2.4.0 pytorch-cuda=12.1 -c pytorch -c nvidia 
pip install openprompt 
  
unset INSTALL_DIR  
```

# Data Preparation

```bash
cd CLEVER/data
bash download.sh

cd ..  
mkdir output
mv CLEVER/data/download_weight.sh CLEVER/output/
cd output
bash download_weight.sh
```

# Training

open bash file and modify:
```bash
CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7,8,9 python -m torch.distributed.launch --nproc_per_node=10 --master_port 10225 oscar/run_bag.py
```
to your devices amount, e.g. 2 gpu availible
```bash
CUDA_VISIBLE_DEVICES=0,1 python -m torch.distributed.launch --nproc_per_node=2 --master_port 10225 oscar/run_bag.py
```


```bash
cd src/Oscar
bash train.sh
```

## Errors

```bash
ModuleNotFoundError: No module named 'oscar'
ModuleNotFoundError: No module named 'sklearn'
ModuleNotFoundError: No module named 'tensorboard'
```

