
Docker镜像网站: https://www.wangdu.site/course/2109.html

```bash
FROM <docker image>
RUN apt update
RUN apt-get install -y wget
RUN mkdir -p ~/miniconda3
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
RUN bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
RUN rm ~/miniconda3/miniconda.sh
RUN source ~/miniconda3/bin/activate
RUN conda init --all
```

启动脚本
```bash
docker run -it --restart=always  \
		--ipc=host  \
		--ulimit memlock=-1   \
		--ulimit stack=67108864  \
		--gpus all  \
		-p 41262:22 \
		--privileged \
		--net host  \
		-v /sbin/dmidecode:/sbin/dmidecode \
		-v /dev/mem:/dev/mem \
		-v /home/dpw:/home/dpw \
		-v /etc/locale.conf:/etc/locale.conf  \
		--shm-size=32g  \
		<docker image>
```


**CLEVER**
Notes:
	Only downloading devel version, there is bin dir in cuda
```BASH
FROM <docker image>
RUN apt -y update
RUN apt-get install -y wget
RUN echo export PATH=/usr/local/cuda-11.0/bin:$PATH >> ~/.bashrc
RUN echo export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH >> ~/.bashrc
RUN source ~/.bashrc
RUN mkdir -p ~/miniconda3
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
RUN bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
RUN rm ~/miniconda3/miniconda.sh
RUN source ~/miniconda3/bin/activate
RUN conda init --all
RUN apt-get install -y git
RUN mkdir /home/CLEVER
RUN cd /home/CLEVER
RUN conda create -y --name CLEVER python=3.7
RUN conda activate CLEVER
RUN conda install -y pytorch==1.7.1 torchvision==0.8.2 cudatoolkit=11.0 -c pytorch
RUN export INSTALL_DIR=$PWD
RUN cd $INSTALL_DIR
RUN git clone https://github.com/NVIDIA/apex.git
RUN cd apex
RUN pip install -r requirements.txt
RUN python setup.py install --cuda_ext --cpp_ext

# install oscar
RUN cd $INSTALL_DIR
RUN git clone --recursive https://github.com/thunlp/CLEVER.git
RUN cd CLEVER/src/Oscar
RUN pip install -r requirements.txt
RUN cd coco_caption
RUN ./get_stanford_models.sh
RUN cd ..
RUN python setup.py build develop

# for Prompt-FT baseline and LAMA
RUN conda create -y --name CLEVER_prompt_env python=3.7
RUN conda activate CLEVER
RUN conda install -y pytorch==1.7.1 torchvision==0.8.2 cudatoolkit=11.0 -c pytorch
RUN conda install -y openprompt

unset INSTALL_DIR
```



**Errors 1：**
```bash
subprocess.CalledProcessError: Command '['ninja', '-v']' returned non-zero exit status 1.
```

solution
```bash
$ vim /root/miniconda3/envs/CLEVER/lib/python3.7/site-packages/torch/utils/cpp_extension.py
```

in command line, search:
```vim
/ Command '['ninja', '-v']'
```

modify to:

```python
Command '['ninja', '--v']'
or
Command '['ninja', '--version']'
```

**Errors 2 :**
```bash
error: command '/usr/bin/g++' failed with exit code 1
```

solution



```
nvcc fatal: Unsupported gpu architecture 'compute_86'
```