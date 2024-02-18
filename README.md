### Requirement
Linux (Ubuntu 16.04)  
Python 3.6+  
PyTorch 1.7 or higher  
CUDA 10.2 or higher

### create environment
```
git clone https://github.com/jcjlin/shape-guided-refined.git
cd Shape-Guided
conda create --name myenv python=3.6
conda activate myenv
pip install -r requirement.txt
```

### MvTec3D-AD Dataset
[Here](https://www.mvtec.com/company/research/datasets/mvtec-3d-ad) to download dataset

## Implementation

### Preprocessing
It will take few minutes to remove the backgoround of the point cloud.
```
python tools/preprocessing.py DATASET_PATH
```
Divided the point cloud into multiple local patches for each instance.<br/>
```
python cut_patches.py --datasets_path DATASET_PATH --save_grid_path data/
```
*Make sure the order of execution of preprocessing.py is before cut_patches.py.* <br/>

### Train 3D Expert Model
There is the best checkpoint of the 3D expert model in ```checkpoint/best_ckpt/ckpt_000601.pth```, and you can skip this step. Alternatively, you can train the 3D expert model on your own. So, you need to execute the following commands to get the required training patches which are contained the noise points.<br/>
*Recommend setting the ```save_grid_path``` in the same directory as above.*
```
python cut_patches.py --datasets_path DATASET_PATH --save_grid_path data/ --pretrain
```
then,
```
python train_3Dmodel.py --grid_path data/ --ckpt_path "./checkpoint"
```

### Buid Memory and Inference
The result will be stored in the output directory.
You can use "--vis" to visualize our result of the heat map. 
```
python main.py --datasets_path DATASET_PATH --grid_path data/ --ckpt_path "checkpoint/best_ckpt/ckpt_000601.pth"
```

