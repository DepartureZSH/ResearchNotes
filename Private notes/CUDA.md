# 显卡

```bash
# 查询显卡方法一
$ lspci | grep -i vga
18:00.0 VGA compatible controller: NVIDIA Corporation GA102GL [RTX A5000] (rev a1)
3b:00.0 VGA compatible controller: NVIDIA Corporation GA102GL [RTX A5000] (rev a1)
# 查询显卡方法二
$ nvidia-smi
```

查询网站 
http://pci-ids.ucw.cz/mods/PC/10de?action=help?help=pci

显卡规格数据库
[GPU Specs Database](https://www.techpowerup.com/gpu-specs/)

查询显卡算力
https://developer.nvidia.com/zh-cn/cuda-gpus#compute

```bash
# 查询驱动版本
cat /proc/driver/nvidia/version
```

# CUDA

```bash
# 查询Cuda版本方法一
nvcc -V

# 查询Cuda版本方法二
cat /usr/local/cuda/version.txt
```

# cuDNN

```bash
# 查询cuDNN版本方法一
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2

# 查询cuDNN版本方法二
cat /usr/local/cuda/include/cudnn.h | grep cudnn
find / -name cudnn_version.h 2>&1 | grep -v "权限不够"
cat Your/path/to/cudnn_version.h
```
