# Deforming 3D Anime Face Models by 2D Anime Face Landmarks for Anime-Like Representation


## 2D （CV, Anime...）

| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 


## 3D （Deformation, Reconstruction, Differentiable Rendering...）

### Deformation
| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| [DeformNet: Free-Form Deformation Network for 3D Shape Reconstruction from a Single Image](https://arxiv.org/abs/1708.04672) | WACV 2018 | [Youtube](https://www.youtube.com/watch?v=cKzXVL6W--8&ab_channel=ComputerVisionFoundationVideos) | 基于图像的形变。（基于二维的信息，和本研究较符合）|
| [3DN: 3D Deformation Network](https://arxiv.org/abs/1903.03322) | CVPR 2019 | | 能够基于目标图像或目标点云来形变模型 |
| [Neural Cages for Detail-Preserving 3D Deformations](https://arxiv.org/abs/1912.06395) | CVPR 2020 | | 基于Cage的形变方法，能够让源模型形变为类似目标模型结构的同时，保持原有的细节。输入源模型和目标模型，使用神经网络得到两个模型的Cage，让源模型的Cage接近目标模型的Cage从而达到形变。（基于3D模型进行形变，因此对本研究帮助不大） |
| [DeepMetaHandles: Learning Deformation Meta-Handles of 3D Meshes with Biharmonic Coordinates](https://arxiv.org/abs/2102.09105) | CVPR 2021 | [Github](https://github.com/Colin97/DeepMetaHandles) | 使用神经网络学习到元句柄（网格控制点的组合），利用其进行形变，使模型接近目标网格。其中还应用了可微渲染器和2D判别器来作为Loss。（虽然是基于目标网格进行的形变，不是基于二维的信息，但利用了控制点，整个形变过程值得参考）|


### Reconstruction
| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| [Doodle Your 3D: From Abstract Freehand Sketches to Precise 3D Shapes](https://arxiv.org/abs/2312.04043) | CVPR | | 使用素描/线稿就能生成模型，或者修改模型。（很好的工作，但本研究需要的是“形变”） |


### Differentialble Rendering
| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| [Soft Rasterizer: A Differentiable Renderer for Image-based 3D Reasoning](https://arxiv.org/abs/1904.01786) | 2019 | |  |


### FACE
| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| [Beyond Face Rotation: Global and Local Perception GAN for Photorealistic and Identity Preserving Frontal View Synthesis](https://arxiv.org/abs/1704.04086) |
| [An Improved Face Synthesis Model for Two-Pathway Generative Adversarial Network](https://dl.acm.org/doi/abs/10.1145/3318299.3318346) | 
| [Disentangled Representation Learning GAN for Pose-Invariant Face Recognition](https://openaccess.thecvf.com/content_cvpr_2017/papers/Tran_Disentangled_Representation_Learning_CVPR_2017_paper.pdf) |
| [Learning to Regress 3D Face Shape and Expression from an Image without 3D Supervision](https://arxiv.org/abs/1905.06817) | CVPR 2019 | | 能够从一张图片捕获出FLAME model的面部参数 |
| [SOFA: Style-based One-shot 3D Facial Animation Driven by 2D landmarks](https://dl.acm.org/doi/10.1145/3591106.3592291) | 2023 | 基于2D Landmark的3D面部动画 |
