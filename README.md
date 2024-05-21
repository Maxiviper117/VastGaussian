# VastGaussian

This is an unofficial implementation of `VastGaussian: Vast 3D Gaussians for Large Scene Reconstruction`. Since this is my first attempt at recreating the entire code from scratch, there might be some errors, and my coding style may seem somewhat naive compared to experts, lacking some engineering techniques. However, I have taken my first step because I couldn't find any implementation of VastGaussian online, so I gave it a try.

If you have any experience or feedback regarding code modifications during usage, please feel free to contact me or simply raise an issue:
> Email: 374774222@qq.com
> 
> QQ: 374774222
> 
> WeChat: k374774222

## ToDo List
1. [x] Implement Camera-position-based region division
2. [x] Implement Position-based data selection
3. [x] Implement Visibility-based camera selection
4. [x] Implement Coverage-based point selection
5. [x] Implement Decoupled Appearance Modeling
6. [x] Implement Seamless Merging
6. [ ] Implement parallel training of m*n regions on a single GPU after point cloud division
7. [ ] Conduct experiments on the UrbanScene3D and Mill-19 datasets

## Description

1. I made modifications to the original 3DGS by extracting its hyperparameters from `arguments/__init__.py` and placing them in the `arguments/parameters.py` file, making it easier to read and understand the hyperparameters.

2. To preserve the original directory structure of 3DGS, I added a new folder `VastGaussian_scene` to store the modules of VastGaussian. Some parts of the code call existing functions from the `scene` folder. To resolve import errors, I moved the Scene class to the `datasets.py` file.
![img.png](image/img2.png)
![img_1.png](image/img_1.png)
3. The file names match the methods mentioned in the paper for easier reading.

> `datasets.py`: I rewrote the Scene class from 3DGS, splitting it into BigScene and PartitionScene. The former represents the original scene, BigScene, and the latter represents the partitioned smaller scenes, PartitionScene.
>
> `data_partition.py`: Data partitioning, corresponding to `Progressive Data Partitioning` in the paper.
> ![img_3.png](image/img_3.png)
> 
> `decouple_appearance_model.py`: Decoupled Appearance Modeling module, corresponding to `Decoupled Appearance Modeling` in the paper.
> ![img.png](image/img.png)!
> ![img_2.png](image/img_2.png)
> 
> `graham_scan.py`: Convex hull computation, used in Visibility-based camera selection to project the partitioned cubes onto the camera plane and calculate the intersection of the projection area with the image area.
> 
> `seamless_merging.py`: Seamless merging, corresponding to `Seamless Merging` in the paper, merging each PartitionScene into BigScene.

4. I added a new file `train_vast.py` to modify the training process of VastGaussian. To train the original 3DGS, use `train.py`.

5. The paper mentions performing `Manhattan-world alignment to make the y-axis of the world coordinate vertical to the ground plane`. After consulting an expert, I learned that this can be manually adjusted using CloudCompare software. The general process involves adjusting the bounding box boundaries of the region where the point cloud is located to align with the overall orientation of the point cloud region.
> For example, the point cloud in the figure below was originally tilted, but after adjustment, it becomes horizontal and vertical. The expert mentioned that Manhattan-world alignment is a basic operation for large-scale 3D reconstruction (to facilitate partitioning), haha.
> ![img_4.png](image/img_4.png)![img_5.png](image/img_5.png)
6. In my implementation, I used the small-scale data provided by 3DGS for testing. My machine cannot handle larger datasets, which, according to the paper, require at least 32G of VRAM.

7. During the implementation, the authors did not clearly specify some details of the operations in the paper, so some implementations are based on my guesses and understanding. Therefore, my implementation may have some bugs, and some implementations might seem silly to experts. If you encounter any issues during use, please contact me in time so we can improve together.

## Usage

1. The data format and training commands are similar to those of 3DGS. I did not make many personalized modifications. You can refer to the following command (for more parameters, refer to `arguments/parameters.py`):
```python
python train_vast.py -s output/dataset --exp_name test
```

## Datasets
1. `Urbanscene3D`: [UrbanScene3D](https://github.com/Linxius/UrbanScene3D)

2. `Mill-19`: [Mill-19](https://opendatalab.com/OpenDataLab/Mill_19/tree/main/raw)

3. Test data: [Test Data](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/datasets/input/tandt_db.zip)