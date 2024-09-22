# Deforming 3D Anime Face Models by 2D Anime Face Landmarks for Anime-Like Representation

研究目标：让角色模型能够在各个视角下，面部发生特殊的形变，呈现2D动画的效果

#### 从2D的角度考虑

我们首先要学习到2D动画不同视角下的特征以及其各视角下的转换关系，才能去指导后续在各个视角下的面部形变

方案一（目前来看比较合理）：

- 为了学习到上述内容，我们需要先能够识别面部特征，即，我们需要一个能够精准识别面部Landmark的神经网络
  - 这里需要考虑怎样设计Landmark才能帮助到后续的形变
  - 该网络应该能识别各个视角下的Landmark
  - 数据方面：大量的线稿
- 在完成了识别特征的任务后，我们需要考虑在不同视角下，这些特征是如何变化的
  - 考虑构建一个模型：输入为正面的Landmark分布，输出为各视角下的Landmark分布
  - 为了训练该模型可能需要动画资源（同一角色多个视角下的资源）
  - 我们需要在2D去定义3D的视角参数，比如某些Landmark的举例比例，位置等

方案二：

-


## 2D （CV, Anime...）

| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 


## 3D （Deformation, Reconstruction, Differentiable Rendering...）

### Deformation
| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| [Practice and Theory of Blendshape Facial Models](https://graphics.cs.uh.edu/wp-content/papers/2014/2014-EG-blendshape_STAR.pdf) | 2014 | | 针对面部Blendshape的讲解（最后获得形变后的模型，需要生成平滑的过渡时，值得参考） |
| [DeformNet: Free-Form Deformation Network for 3D Shape Reconstruction from a Single Image](https://arxiv.org/abs/1708.04672) | 2018 | [Youtube](https://www.youtube.com/watch?v=cKzXVL6W--8&ab_channel=ComputerVisionFoundationVideos) | 基于图像的形变。（基于二维的信息，和本研究较符合）|
| [3DN: 3D Deformation Network](https://arxiv.org/abs/1903.03322) | 2019 CVPR | | 能够基于目标图像或目标点云来形变模型 |
| [Neural Cages for Detail-Preserving 3D Deformations](https://arxiv.org/abs/1912.06395) | 2020 CVPR | | 基于Cage的形变方法，能够让源模型形变为类似目标模型结构的同时，保持原有的细节。输入源模型和目标模型，使用神经网络得到两个模型的Cage，让源模型的Cage接近目标模型的Cage从而达到形变。（基于3D模型进行形变，因此对本研究帮助不大） |
| [DeepMetaHandles: Learning Deformation Meta-Handles of 3D Meshes with Biharmonic Coordinates](https://arxiv.org/abs/2102.09105) | 2021 CVPR | [Github](https://github.com/Colin97/DeepMetaHandles) | 使用神经网络学习到元句柄（网格控制点的组合），利用其进行形变，使模型接近目标网格。其中还应用了可微渲染器和2D判别器来作为Loss。（虽然是基于目标网格进行的形变，不是基于二维的信息，但利用了控制点，整个形变过程值得参考）|


### Reconstruction
| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| [Doodle Your 3D: From Abstract Freehand Sketches to Precise 3D Shapes](https://arxiv.org/abs/2312.04043) | 2024 CVPR | | 使用素描/线稿就能生成模型，或者修改模型。（很好的工作，但本研究需要的是“形变”） |


### Differentialble Rendering
| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| [Soft Rasterizer: A Differentiable Renderer for Image-based 3D Reasoning](https://arxiv.org/abs/1904.01786) | 2019 | |  |


### FACE
| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- |  
| [Joint Face Alignment and 3D Face Reconstruction with Application to Face Recognition](https://cvlab.cse.msu.edu/pdfs/Liu_Zhao_Liu_Zeng_PAMI2018.pdf) | 2018 | | |
| [From 2D to 3D real-time expression transfer for facial animation](https://link.springer.com/article/10.1007/s11042-018-6785-8) | 2019 | | 基于视频中检测道德landmark，进行的实时表情变化。（基于2D信息，而且是landmark，对某一面部模型的形变）|
| [Beyond Face Rotation: Global and Local Perception GAN for Photorealistic and Identity Preserving Frontal View Synthesis](https://arxiv.org/abs/1704.04086) |
| [An Improved Face Synthesis Model for Two-Pathway Generative Adversarial Network](https://dl.acm.org/doi/abs/10.1145/3318299.3318346) | 
| [Disentangled Representation Learning GAN for Pose-Invariant Face Recognition](https://openaccess.thecvf.com/content_cvpr_2017/papers/Tran_Disentangled_Representation_Learning_CVPR_2017_paper.pdf) |
| [Learning to Regress 3D Face Shape and Expression from an Image without 3D Supervision](https://arxiv.org/abs/1905.06817) | 2019 CVPR | [Github](https://github.com/soubhiksanyal/RingNet) | 能够从一张图片捕获出FLAME model的面部参数 |
| [SOFA: Style-based One-shot 3D Facial Animation Driven by 2D landmarks](https://dl.acm.org/doi/10.1145/3591106.3592291) | 2023 | | 基于2D Landmark的3D面部动画 |
| [Alive Caricature from 2D to 3D](https://arxiv.org/abs/1803.06802) | 2018 CVPR | | 卡通面部重建 |
| [Landmark Detection and 3D Face Reconstruction for Caricature using a Nonlinear Parametric Model](https://arxiv.org/abs/2004.09190) | 2021 | | 基于关键点的卡通面部重建。（虽然是重建，但文章涉及到了卡通面部的关键点检测，包括脸颊轮廓的关键点。甚至提出了面部模型上的Optional Landmark Set，和本研究的思路高度一致） |
| [Generating Animatable 3D Cartoon Faces from Single Portraits](https://arxiv.org/abs/2307.01468) | 2023 | | |
| [Synthesis, Style Editing, and Animation of 3D Cartoon Face](https://www.sciopen.com/article/10.26599/TST.2023.9010028) | 2024 | | 卡通面部重建 |

