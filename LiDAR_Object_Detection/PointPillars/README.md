# [PointPillars: Fast Encoders for Object Detection from Point Clouds](https://arxiv.org/abs/1812.05784) 

A Simple PointPillars PyTorch Implenmentation for 3D Lidar(KITTI) Detection. [[Zhihu](https://zhuanlan.zhihu.com/p/521277176)]

- It can be run without installing [Spconv](https://github.com/traveller59/spconv), [mmdet](https://github.com/open-mmlab/mmdetection) or [mmdet3d](https://github.com/open-mmlab/mmdetection3d). 
- Only one detection network (PointPillars) was implemented in this repo, so the code may be more easy to read. 
- Sincere thanks for the great open-souce architectures [mmcv](https://github.com/open-mmlab/mmcv), [mmdet](https://github.com/open-mmlab/mmdetection) and [mmdet3d](https://github.com/open-mmlab/mmdetection3d), which helps me to learn 3D detetion and implement this repo.

## Detection Visualization

![](./figures/pcd_train_000134.png)
![](./figures/img_train_000134.png)

## [Compile] 

```
cd ops
pip install -r requirements.txt
python setup.py develop
```

## [Datasets]

1. Download

    Download [point cloud](https://s3.eu-central-1.amazonaws.com/avg-kitti/data_object_velodyne.zip)(29GB), [images](https://s3.eu-central-1.amazonaws.com/avg-kitti/data_object_image_2.zip)(12 GB), [calibration files](https://s3.eu-central-1.amazonaws.com/avg-kitti/data_object_calib.zip)(16 MB)和[labels](https://s3.eu-central-1.amazonaws.com/avg-kitti/data_object_label_2.zip)(5 MB)。
    
    Then, make format the datasets as follows:
    ```
    dataset
        |- kitti
            |- training
                |- calib (#7481 .txt)
                |- image_2 (#7481 .png)
                |- label_2 (#7481 .txt)
                |- velodyne (#7481 .bin)
            |- testing
                |- calib (#7518 .txt)
                |- image_2 (#7518 .png)
                |- velodyne (#7418 .bin)
    ```

2. Make dataset
    ```
    cd ~/Download
    unzip data_object_velodyne.zip
    unzip data_object_image_2.zip
    unzip data_object_calib.zip
    unzip data_object_label_2.zip

    mkdir -p ~/dataset/kitti/training && mkdir -p ~/dataset/kitti/testing
    sudo mv -r ~/Download/data_object_velodyne/training/velodyne ~/dataset/kitti/training/
    sudo mv -r ~/Download/data_object_velodyne/testing/velodyne ~/dataset/kitti/testing/
    sudo mv -r ~/Download/data_object_image_2/training/image_2 ~/dataset/kitti/training/
    sudo mv -r ~/Download/data_object_image_2/testing/image_2 ~/dataset/kitti/testing/
    sudo mv -r ~/Download/data_object_calib/training/calib ~/dataset/kitti/training/
    sudo mv -r ~/Download/data_object_calib/testing/calib ~/dataset/kitti/testing/
    sudo mv -r ~/Download/data_object_label_2/training/label_2 ~/dataset/kitti/training/
    ```
    Now which in Download folder becomes dummy. So delete as follows:
    ```
    sudo rm -rf ~/Download/data_object_velodyne
    sudo rm -rf ~/Download/data_object_image_2
    sudo rm -rf ~/Download/data_object_calib
    sudo rm -rf ~/Download/data_object_label_2
    ```

3. Pre-process KITTI datasets first
    ```
    cd PointPillars/
    python pre_process_kitti.py
    ```

    Now, we have datasets as follows:
    ```
    dataset
        |- kitti
            |- training
                |- calib (#7481 .txt)
                |- image_2 (#7481 .png)
                |- label_2 (#7481 .txt)
                |- velodyne (#7481 .bin)
                |- velodyne_reduced (#7481 .bin)
            |- testing
                |- calib (#7518 .txt)
                |- image_2 (#7518 .png)
                |- velodyne (#7518 .bin)
                |- velodyne_reduced (#7518 .bin)
            |- kitti_gt_database (# 19700 .bin)
            |- kitti_infos_train.pkl
            |- kitti_infos_val.pkl
            |- kitti_infos_trainval.pkl
            |- kitti_infos_test.pkl
            |- kitti_dbinfos_train.pkl
    ```

## [Training]

```
cd PointPillars/
python train.py
```

## [Evaluation]

```
cd PointPillars/
python evaluate.py
```

## [Test]

```
cd PointPillars/

# 1. infer and visualize point cloud detection
python test.py --pc_path your_pc_path 


# 2. infer and visualize point cloud detection and gound truth
python test.py --pc_path your_pc_path --calib_path your_calib_path  --gt_path your_gt_path


# 3. infer and visualize point cloud & image detection
python test.py --pc_path your_pc_path --calib_path your_calib_path --img_path your_img_path
```

* e.g. [infer on val set 000040]
```
# 1. infer and visualize point cloud detection
python test.py --pc_path /home/research/dataset/kitti/testing/velodyne_reduced/000040.bin


# 2. infer and visualizae point cloud detection & image detection & ground truth
python test.py --pc_path /home/research/dataset/kitti/testing/velodyne_reduced/000040.bin --calib_path /home/research/dataset/kitti/testing/calib/000040.txt --img_path /home/research/dataset/kitti/testing/image_2/000040.png --gt_path /home/research/dataset/kitti/testing/label_2/000040.txt
```

## [Test Detection Visualization]

![](./figures/pcd_test_000040.png)
![](./figures/img_test_000040.png)