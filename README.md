# Rapid automatic fat quantification in thigh muscle dystrophy for muscular dystrophies [Huashan Neuromuscular Group]

> The toolkit was developed for rapid and automatic fat quantification for thigh muscle degeneration in muscular dystrophy patients. 

This project takes advantage of [nnU-Net](https://github.com/MIC-DKFZ/nnUNet) (a self-adapting framework for U-Net-based medical image segmentation) and [MONAI](https://monai.io/) (a PyTorch-based framework for deep learning in healthcare imaging) to build the model. The training dataset included quantitative MRIs (IDEAL) from 33 muscular dystrophy patients and 17 healthy controls. 

## 1. Download the whole repository to local

```
git clone https://github.com/Hirriririir/Rapid-automatic-fat-quantification-for-thigh-muscle-degeneration
```

**OR** directly download the whole repository from this [link](https://1drv.ms/u/s!AlEvpu4I75DnkKYZsUkdpzNUuq12Ww?e=D0WoyL).

## 2. Install required dependencies

```python
# for nnU-Net pipeline
pip install nnunet

# for calculating fat fraction
pip install monai
```

## 3. Place your MRI file in the right place (Input_data)

- **Input_data**: Two kinds of IDEAL (3-point Dixon) sequences, namely fat and water, are required. You can rename your files according to a format like [Thigh_001_0000.nii.gz], where '001' represents subject number, and '0000' and '0001' represent 'Fat IDEAL sequence' and 'Water IDEAL sequence' respectively.
- **Output_segmentation**: The segmentation of the mask will be generated after running a inference process by a pretrained nnU-Net model.
- **Output_fat_fraction**: A csv. file recording the fat fractions of all thigh muscles in  each subject will be generated after running responding code. 
- **nnUNet_Results_Folder**: Download pretrained model "[model_final_checkpoint.model](https://1drv.ms/u/s!AlEvpu4I75DnkKZmOZFEznbo7OkPxw?e=DTB2Sp)" to the right place "./nnUNet_Results_Folder/nnUNet/3d_fullres/Task501_ThighMuscles/nnUNetTrainerV2__nnUNetPlansv2/fold_0"

```
├── Input_data
│   ├── Thigh_001_0000.nii.gz
│   ├── Thigh_001_0001.nii.gz
│   ├── Thigh_002_0000.nii.gz
│   ├── Thigh_002_0001.nii.gz
│   ├── Thigh_003_0000.nii.gz
│   └── Thigh_003_0001.nii.gz
├── Output_fat_fraction
├── Output_segmentation
│   ├── Thigh_001.nii.gz
│   ├── Thigh_002.nii.gz
│   ├── Thigh_003.nii.gz
│   └── plans.pkl
├── Segmentation and fat fraction .ipynb
├── nnUNet_Results_Folder
│   └── nnUNet
│       └── 3d_fullres
│           └── Task501_ThighMuscles
│               └── nnUNetTrainerV2__nnUNetPlansv2.1
│                   ├── fold_0
│                   │   ├── debug.json
│                   │   ├── model_final_checkpoint.model
│                   │   ├── model_final_checkpoint.model.pkl
│                   │   ├── postprocessing.json
│                   └── plans.pkl
├── nnUNet_preprocessed
└── nnUNet_raw_data_base
    ├── nnUNet_cropped_data
    └── nnUNet_raw_data
```

## 4. Run the code in the Jupyter notebook

```python
# Inference using Our pre-trained model in the 'nnUNetTrainerV2__nnUNetPlansv2.1' dictionary

!nnUNet_predict -i './Input_data' -o './Output_segmentation' -t 501 -tr nnUNetTrainerV2 -m 3d_fullres
```

## 5. Results

### Segmentation results

These MRIs are from the Test dataset that the model had not seen during the training process. Our model shows a good segmenting ability compared to a human radiologist. 

![](https://raw.githubusercontent.com/Cpresident/fopi/main/Model%20demo%20fig-01.png)

We recommend using [ITK-SNAP](http://www.itksnap.org/) to visualize the MRI files, and the label descriptions for this project can be found as 'Label descriptions for ITK-SNAP.label' in this repository.

![](https://raw.githubusercontent.com/Cpresident/fopi/main/20220505004543.png)

### Fat fraction visualization

![](https://raw.githubusercontent.com/Cpresident/fopi/main/20220504230640.png)

### Fat fraction results

|   Subject |       SA |       RF |       VL |       VI |       VM |       AM |       GR |       BL |       ST |       SM |       BB |
| --------: | -------: | -------: | -------: | -------: | -------: | -------: | -------: | -------: | -------: | -------: | -------: |
| Thigh_001 | 0.182836 | 0.537026 | 0.309223 | 0.343359 | 0.300494 | 0.648822 | 0.235308 | 0.639768 | 0.649394 | 0.617633 | 0.418268 |
| Thigh_002 | 0.163060 | 0.501802 | 0.536133 | 0.529328 | 0.503416 | 0.405981 | 0.118544 | 0.618817 | 0.542495 | 0.672286 | 0.156817 |
| Thigh_003 | 0.205246 | 0.215608 | 0.330503 | 0.369018 | 0.349797 | 0.224268 | 0.127358 | 0.325287 | 0.201906 | 0.266562 | 0.198464 |

BL, Biceps femoris long head; BB, Biceps femoris short head; ST, Semitendinosus; SM, Semimembranosus; AM, adductor magnus; VI, Vastus intermedius; VL, Vastus lateralis; VM, Vastus medialis; RF, Rectus femoris; GR, Gracilis; SA, Sartorius.
