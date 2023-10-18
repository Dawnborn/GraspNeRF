# My Installation
<!-- (previous environment: `grasp37cu111` not working with blender) -->

existing environment on `ar-lx0294`: `grasp39cu111`

(assume cuda 11.1 is already installed)
here we activate the cuda 11.1 by
```
source prep_cuda.sh
```

## Blender

download and extract `blender-2.93.3-ubuntu`

only `python 3.9` is compatible with this blender version, so we create a corresponding environment

```
conda create -n grasp39cu111 python=3.9
```

create a soft link to replace the default python of blender
```
ln -s /home/junpeng.hu/anaconda3/envs/grasp39cu111 /home/junpeng.hu/Softwares/blender-2.93.3-linux-x64/2.93/python
```

install pytorch
```
pip install torch==1.10.1+cu111 torchvision==0.11.2+cu111 -f https://download.pytorch.org/whl/cu111/torch_stable.html
```

```
pip install open3d 
pip install inplace-abn
pip install scikit-image
pip install easydict
pip install opencv-python
pip install plyfile
pip install transforms3d
pip install torchmetrics
pip install tensorboard
```
modify line 8 of [`src/gd/utils/btsim.py:L8`](src/gd/utils/btsim.py#L8):
```
from .transform import Rotation, Transform
```

change all `ILSVRC2012_val` in `/data/assets/imagenet/test_paths.txt` to `ILSVRC2010_val`

now we can test the grasp simulation
```bash
conda activate grasp39cu111
source prep_cuda.sh
bash run_simgrasp.sh
```
and the training script of nerf
```
bash train.sh GPU_ID
```

## Posbile Issues

If any raised error about `np.bool` or `np.long`, in `src/nr/dataset/database.py`:
  - change all `np.bool` to `np.bool_`
  - change all `np.long` to `np.longlong`
