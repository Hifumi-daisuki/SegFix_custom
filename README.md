# SegFix_custom
This is unofficial code for SegFix that has been modified to be used with custom datasets.

##  **1. Requirements**
### **H/W**
* CPU: Intel i5 10400F
* GPU: NVDIA GeForce RTX 1080ti
* RAM 32GB

### **S/W**
* OS: Ubuntu 16.04 LTS
* CUDA: 10.0
* Torch: 1.0.1. Recommened to use lower version

### **Configuration**
Before executing any scripts, your should first fill up the config file `config.profile` at project root directory. There are two items you should specify:
 + `PYTHON`, identifying your python executable.
 + `DATA_ROOT`, the root directory of your data. It should be the parent directory of `cityscapes`.

## **2. SegFix**

### **2.1. Data Preparation**  
The dataset directory should look like:  
```
$DATA_ROOT
├── cityscapes
│   ├── train
│   │   ├── image
│   │   └── label
│   ├── val
│   │   ├── image
│   │   └── label
│   ├── test
│   │   └── image
```
Place images and labels at appropriate location.

### **2.2. Generate ground truth offsets**  
Before generate offsets, revise [label_list](https://github.com/openseg-group/openseg.pytorch/blob/aefc75517b09068d7131a69420bc5f66cb41f0ee/lib/datasets/preprocess/cityscapes/dt_offset_generator.py#L47) first to fit custom datasets.  
```python
python ./lib/datasets/preprocess/cityscapes/dt_offset_generator.py
```

### **2.3. Download ImageNet pretrained mode**  
Before starting training, you should download the corresponding ImageNet pretrained models to `./pretrained_model`.
[hrnet18](https://github.com/hsfzxjy/models.storage/releases/download/openseg-pytorch-pretrained/hrnetv2_w18_imagenet_pretrained.pth), [hrnet32](https://github.com/hsfzxjy/models.storage/releases/download/openseg-pytorch-pretrained/hrnetv2_w32_imagenet_pretrained.pth), [hrnet48](https://github.com/hsfzxjy/models.storage/releases/download/openseg-pytorch-pretrained/hrnetv2_w48_imagenet_pretrained.pth), [hrnet2x20](https://github.com/hsfzxjy/models.storage/releases/download/openseg-pytorch-pretrained/hr_rnet_bt_w20_imagenet_pretrained.pth)

### **2.4. Train Config**  
You should revise `./config/cityscapes/H_SEGFIX.json` such as [num_classes](https://github.com/openseg-group/openseg.pytorch/blob/aefc75517b09068d7131a69420bc5f66cb41f0ee/configs/cityscapes/H_SEGFIX.json#L7), [label_list](https://github.com/openseg-group/openseg.pytorch/blob/aefc75517b09068d7131a69420bc5f66cb41f0ee/configs/cityscapes/H_SEGFIX.json#L8), [colorlist](https://github.com/openseg-group/openseg.pytorch/blob/aefc75517b09068d7131a69420bc5f66cb41f0ee/configs/cityscapes/H_SEGFIX.json#L80), [loss_weight](https://github.com/openseg-group/openseg.pytorch/blob/aefc75517b09068d7131a69420bc5f66cb41f0ee/configs/cityscapes/H_SEGFIX.json#L139) to fit custom datasets.  
Also you need to properly adjust [batch sizes](https://github.com/openseg-group/openseg.pytorch/blob/aefc75517b09068d7131a69420bc5f66cb41f0ee/configs/cityscapes/H_SEGFIX.json#L14) to utilize your resources effectively.  
Lastly, check [export dt_num_classes](https://github.com/openseg-group/openseg.pytorch/blob/aefc75517b09068d7131a69420bc5f66cb41f0ee/scripts/cityscapes/segfix/run_h_48_d_4_segfix.sh#L21) of `./scripts/cityscapes/segfix/run_h_48_d_4_segfix`

### **2.5. SegFix Training**  
```bash
bash scripts/cityscapes/segfix/<script>.sh train 1
```
