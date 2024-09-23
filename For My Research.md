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
  - 理论上不同的动画，其绘画风格也不同。因此，用不同的动画数据学习得到的模型也会有风格上的差异。
  - 我们需要在2D去定义3D的视角参数，比如某些Landmark的举例比例，位置等

方案二：

- Landmark的检测不需要改动
- 关于基于正面Landmark分布去生成不同视角的Landmark分布
  - 没有必要利用动画去学习各个视角（连续值）的分布规律，只需要学习几个离散的视角即可
  - 即，针对已有的线稿图，分析得到其所属视角的分类（离散值），利用这个数据去学习这些个视角的整体分布规律
  - 注：下策，因为我们并没有针对某个角色，而是大量不同的角色。因此我们能学到的只能是笼统的分布特征，不会是变换的规律。
 
方案三：

- Landmark的检测模型仍然不能少
- 不单单依赖Landmark来进行形变，而是完整的图像。使用生成模型，获得该角色在各个视角下的图像
  - 自己训练一个，能够根据正面图像，生成各个角度侧面图像的模型（这一点和方案一的学习关键点分布的模型比较像，我们依赖的数据需要是一个角色的多个视角，不能是杂乱的数据）
  - 使用现有的模型（不知道有没有）
  - 优点：在形变阶段能够不单单根据Landmark的分布来指导形变，还可以依赖图像。企业中利用该研究时也可以直接让画师绘制需要的视角的线稿。

## TIPS

[怎么画动漫角色](https://www.youtube.com/watch?v=B_iKnKoQ0qs&ab_channel=ArtSenpai)



## 2D （CV, Anime...）

### Anime

| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| [Quasi 3D Rotation for Hand-Drawn Characters](https://history.siggraph.org/wp-content/uploads/2022/12/2014-Poster-82-Furusawa_Quasi-3D-Rotation-for-Hand-Drawn-Characters.pdf) | SIGGRAPH 2014 | | 面向中割，提出了线稿的角色面部的旋转方案（好像是纯数学） |
| [Facial Landmark Detection for Manga Images](https://arxiv.org/abs/1811.03214) | 2018 | | 提出对漫画角色面部的关键点检测方案 |

### Face Rotation

| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| TP-GAN [Beyond Face Rotation: Global and Local Perception GAN for Photorealistic and Identity Preserving Frontal View Synthesis](https://arxiv.org/abs/1704.04086) | 2017 ICCV | [Github](https://github.com/HRLTY/TP-GAN) | 将正面化人脸分为两条路，一个是生成大致的正面脸，另一个是生成脸上的口鼻眼眉等部件。最后依据这两个生成最终正面图像。 |
| DR-GAN [Disentangled Representation Learning GAN for Pose-Invariant Face Recognition](https://openaccess.thecvf.com/content_cvpr_2017/papers/Tran_Disentangled_Representation_Learning_CVPR_2017_paper.pdf) | 2017 CVPR | [Github](https://github.com/tranluan/DR-GAN?tab=readme-ov-file) | 研究人脸姿态（角度）的一篇高引文章  |
| [An Improved Face Synthesis Model for Two-Pathway Generative Adversarial Network](https://dl.acm.org/doi/abs/10.1145/3318299.3318346) | 2019 | | 人脸的旋转 |
| [HoloGAN: Unsupervised learning of 3D representations from natural images](https://arxiv.org/abs/1904.01326) | 2019 ICCV | | 生成物体不同视角下的图像 |
| [Pose-Guided Photorealistic Face Rotation]() | 2018 CVPR | | 依赖输入面部图像和landmark heatmap，指定目标图像的（即不同视角下的）landmark heatmap，去生成目标图像 |
| [A GAN-Based Face Rotation for Artistic Portraits](https://www.mdpi.com/2227-7390/10/20/3860) | 2022 | | 对艺术画作的人脸旋转 |


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
| [Learning to Regress 3D Face Shape and Expression from an Image without 3D Supervision](https://arxiv.org/abs/1905.06817) | 2019 CVPR | [Github](https://github.com/soubhiksanyal/RingNet) | 能够从一张图片捕获出FLAME model的面部参数 |
| [SOFA: Style-based One-shot 3D Facial Animation Driven by 2D landmarks](https://dl.acm.org/doi/10.1145/3591106.3592291) | 2023 | | 基于2D Landmark的3D面部动画 |
| [Alive Caricature from 2D to 3D](https://arxiv.org/abs/1803.06802) | 2018 CVPR | | 卡通面部重建 |
| [Landmark Detection and 3D Face Reconstruction for Caricature using a Nonlinear Parametric Model](https://arxiv.org/abs/2004.09190) | 2021 | | 基于关键点的卡通面部重建。（虽然是重建，但文章涉及到了卡通面部的关键点检测，包括脸颊轮廓的关键点。甚至提出了面部模型上的Optional Landmark Set，和本研究的思路高度一致） |
| [Generating Animatable 3D Cartoon Faces from Single Portraits](https://arxiv.org/abs/2307.01468) | 2023 | | |
| [Synthesis, Style Editing, and Animation of 3D Cartoon Face](https://www.sciopen.com/article/10.26599/TST.2023.9010028) | 2024 | | 卡通面部重建 |

