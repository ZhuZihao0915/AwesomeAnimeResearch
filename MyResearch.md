# Deforming 3D Anime Face Models by 2D Anime Face Landmarks for Anime-Like Representation

研究目标：让角色模型能够在各个视角下，面部发生特殊的形变，呈现2D动画的效果

<br>

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

<br>
<br>

---

<br>

## 2D （CV, Anime...）

<br>

### Anime

| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| [Quasi 3D Rotation for Hand-Drawn Characters](https://history.siggraph.org/wp-content/uploads/2022/12/2014-Poster-82-Furusawa_Quasi-3D-Rotation-for-Hand-Drawn-Characters.pdf) | SIGGRAPH 2014 | | 面向中割，提出了线稿的角色面部的旋转方案（好像是纯数学） |
| [Facial Landmark Detection for Manga Images](https://arxiv.org/abs/1811.03214) | 2018 | | 提出对漫画角色面部的关键点检测方案 |

<br>

### Face Reenactment

| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| [Triple consistency loss for pairing distributions in GAN-based face synthesis](https://arxiv.org/abs/1811.03492) | 2018 | [Github](https://github.com/ESanchezLozano/GANnotation) | 提出了新的一致性损失 |
| [LandmarkGAN: Synthesizing Faces from Landmarks](https://arxiv.org/abs/2011.00269) | 2020 | [Github](https://github.com/stephenivy07/Landmarkgan) | 基于landmark的人脸合成，能将一个人的landmark转为另一个人的。但研究只能作用于训练过的人（每个identity都有专门的解码器和生成网格） |
| [Neural Head Reenactment with Latent Pose Descriptors](https://arxiv.org/abs/2004.12000) | 2020 CVPR | [Github](https://github.com/shrubb/latent-pose-reenactment) |  |
| [FReeNet: Multi-Identity Face Reenactment](https://arxiv.org/abs/1905.11805) | 2020 CVPR | [Github](https://github.com/zhangzjn/FReeNet) | 同样有一个Landmark Converter，但貌似只变换表情，不变化姿态，且需要成对数据 |
| [A survey on deep learning based reenactment methods for deepfake applications](https://www.researchgate.net/publication/383229641_A_survey_on_deep_learning_based_reenactment_methods_for_deepfake_applications) | 2024 | |  |

<br>

### Face Rotation

| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| TP-GAN [Beyond Face Rotation: Global and Local Perception GAN for Photorealistic and Identity Preserving Frontal View Synthesis](https://arxiv.org/abs/1704.04086) | 2017 ICCV | [Github](https://github.com/HRLTY/TP-GAN) | 将正面化人脸分为两条路，一个是生成大致的正面脸，另一个是生成脸上的口鼻眼眉等部件。最后依据这两个生成最终正面图像。 |
| DR-GAN [Disentangled Representation Learning GAN for Pose-Invariant Face Recognition](https://openaccess.thecvf.com/content_cvpr_2017/papers/Tran_Disentangled_Representation_Learning_CVPR_2017_paper.pdf) | 2017 CVPR | [Github](https://github.com/tranluan/DR-GAN?tab=readme-ov-file) | 研究人脸姿态（角度）的一篇高引文章  |
| [CR-GAN: Learning Complete Representations for Multi-view Generation](https://arxiv.org/abs/1806.11191) | 2018 | [Github](https://github.com/bluer555/CR-GAN) | 对DR-GAN的改进 |
| [An Improved Face Synthesis Model for Two-Pathway Generative Adversarial Network](https://dl.acm.org/doi/abs/10.1145/3318299.3318346) | 2019 | | 对TP-GAN的改进 |
| [HoloGAN: Unsupervised learning of 3D representations from natural images](https://arxiv.org/abs/1904.01326) | 2019 ICCV | [Github](https://github.com/thunguyenphuoc/HoloGAN) | 生成物体不同视角下的图像 |
| CAPG-GAN [Pose-Guided Photorealistic Face Rotation](https://openaccess.thecvf.com/content_cvpr_2018/papers/Hu_Pose-Guided_Photorealistic_Face_CVPR_2018_paper.pdf) | 2018 CVPR | [Github(unofficial)](https://github.com/art591/CAPG-GAN) | 依赖输入面部图像和landmark heatmap，指定目标图像的（即不同视角下的）landmark heatmap，去生成目标图像（该研究没有局限于生成正面人脸，各个视角间的转换均有） |
| [High Fidelity Face Manipulation with Extreme Poses and Expressions](https://arxiv.org/abs/1903.12003) | 2019 | | 引入了boundary image，即线稿来生成旋转的人脸 |
| [Multi-view frontal face image generation: A survey](https://onlinelibrary.wiley.com/doi/abs/10.1002/cpe.6147) | 2020 | | 针对从多视角生成正面人脸图像的Survey |
| [Edge-Gan: Edge Conditioned Multi-View Face Image Generation](https://oar.a-star.edu.sg/storage/7/7j52gdvk17/edge-gan-edge-conditioned-multi-view-face-image-generation.pdf) | 2020 | | 输入两张图片，一张希望被旋转的正面脸，一张目标视角下另一个人的侧面脸。我们提取侧面脸的轮廓（edge），让其和正面脸一起作为输入。输出为edge视角的侧面脸。（有利用价值） |
| [A GAN-Based Face Rotation for Artistic Portraits](https://www.mdpi.com/2227-7390/10/20/3860) | 2022 | | 对艺术画作的人脸旋转 |
| [2D facial landmark localization method for multi-view face synthesis image using a two-pathway generative adversarial network approach](https://peerj.com/articles/cs-897/) | 2022 | [Github](https://github.com/MahmoodHB/LFMTP-GAN) | 利用2D facial landmark，对TP-GAN进行改进 |
| [Multi-view face generation via unpaired images](https://dl.acm.org/doi/abs/10.1007/s00371-021-02129-y) | 2022 | | 无需配队数据的多视角面部图像生成（无需配队数据的话，或许可以不用动画资源了） |



<br>
<br>
  
---

<br>

## 3D （Deformation, Reconstruction, Differentiable Rendering...）

<br>

### Deformation
| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| [Practice and Theory of Blendshape Facial Models](https://graphics.cs.uh.edu/wp-content/papers/2014/2014-EG-blendshape_STAR.pdf) | 2014 | | 针对面部Blendshape的讲解（最后获得形变后的模型，需要生成平滑的过渡时，值得参考） |
| [DeformNet: Free-Form Deformation Network for 3D Shape Reconstruction from a Single Image](https://arxiv.org/abs/1708.04672) | 2018 | [Youtube](https://www.youtube.com/watch?v=cKzXVL6W--8&ab_channel=ComputerVisionFoundationVideos) | 基于图像的形变。（基于二维的信息，和本研究较符合）|
| [3DN: 3D Deformation Network](https://arxiv.org/abs/1903.03322) | 2019 CVPR | [Github](https://github.com/laughtervv/3DN) | 专注于形变，能够基于目标图像或目标点云来形变模型，比较符合 |
| [3Deformer: A Common Framework for Image-Guided Mesh Deformation](https://arxiv.org/abs/2307.09892) | 2023 | | 专注于形变，能够依赖目标的semantic image去完成形变，非常符合本研究 |
| [Neural Cages for Detail-Preserving 3D Deformations](https://arxiv.org/abs/1912.06395) | 2020 CVPR | | 基于Cage的形变方法，能够让源模型形变为类似目标模型结构的同时，保持原有的细节。输入源模型和目标模型，使用神经网络得到两个模型的Cage，让源模型的Cage接近目标模型的Cage从而达到形变。（基于3D模型进行形变，因此对本研究帮助不大） |
| [DeepMetaHandles: Learning Deformation Meta-Handles of 3D Meshes with Biharmonic Coordinates](https://arxiv.org/abs/2102.09105) | 2021 CVPR | [Github](https://github.com/Colin97/DeepMetaHandles) | Deformation的研究。使用神经网络学习到元句柄（网格控制点的组合），利用其进行形变，使模型接近目标网格。其中还应用了可微渲染器和2D判别器来作为Loss。（是基于目标网格进行的形变，虽然没有sketch相关，但利用了控制点，整个形变过程值得参考）|
| [Unsupervised Shape and Pose Disentanglement for 3D Meshes](https://arxiv.org/abs/2007.11341) | 2020 | [Github](https://github.com/kzhou23/shape_pose_disent) | 实现了3D网格形状和姿态的解耦。能够实现：姿态传递（可以将一个网格的姿态传递给另一个网格），形状和姿态插值，姿态与形状的独立控制等功能。但是解耦的对象必须具有相同的mesh结构（作为VAE的输入） |
| [LeGO: Leveraging a Surface Deformation Network for Animatable Stylized Face Generation with One Example](https://kwanyun.github.io/lego/) | 2024 CVPR | [Github](https://github.com/thoyeony/LeGO_3D_Face_Stylization) | |



<br>

### Sketch-based Reconstruction
| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| [DeepSketch2Face: a deep learning based sketching system for 3D face and caricature modeling](https://dl.acm.org/doi/10.1145/3072959.3073629) | 2017 | [Github（无源码）](https://github.com/changgyhub/deepsketch2face) | 使用素描生成人脸。使用神经网络得到一个基础的面部，通过控制点对该面部进行形变优化。形变的过程并没有使用神经网络 |
| [Doodle Your 3D: From Abstract Freehand Sketches to Precise 3D Shapes](https://arxiv.org/abs/2312.04043) | 2024 CVPR | | 使用素描/线稿就能生成模型，或者修改模型。（很好的工作，但本研究需要的是“形变”） |
| [Sketch2Mesh: Reconstructing and Editing 3D Shapes from Sketches](https://openaccess.thecvf.com/content/ICCV2021/papers/Guillard_Sketch2Mesh_Reconstructing_and_Editing_3D_Shapes_From_Sketches_ICCV_2021_paper.pdf) | 2021 CVPR | [Github](https://github.com/cvlab-epfl/sketch2mesh) | CVLab, EPFL  针对一个模型，通过几笔sketch就能修改这个模型。很好的工作，但是修改了mesh |
| [PAniC-3D: Stylized Single-View 3D Reconstruction From Portraits of Anime Characters](https://openaccess.thecvf.com/content/CVPR2023/papers/Chen_PAniC-3D_Stylized_Single-View_3D_Reconstruction_From_Portraits_of_Anime_Characters_CVPR_2023_paper.pdf) | 2023 CVPR | [Github](https://github.com/ShuhongChen/panic3d-anime-reconstruction) | 虽然和研究关系不大，但很有意思的一篇文章。利用动漫角色肖像重建出角色模型。其中它代码开源，数据库开源，值得参考。 |
| [S2TD-Face: Reconstruct a Detailed 3D Face with Controllable Texture from a Single Sketch](https://arxiv.org/abs/2408.01218) | 2024 | [Github](https://github.com/wang-zidu/S2TD-Face) | |
| [SENS: Part-Aware Sketch-based Implicit Neural Shape Modeling](https://arxiv.org/abs/2306.06088) | 2024 EG | [Github](https://github.com/AlexandreBinninger/SENS) | |
| [SketchMetaFace: A Learning-based Sketching Interface for High-fidelity 3D Character Face Modeling](https://arxiv.org/abs/2307.00804) | 2023 TVCG | [Github](https://github.com/zhongjinluo/SketchMetaFace) | DeepSketch2Face只能根据参数模型生成人脸面部，有局限性。此研究能根据sketch生成更加多样的面部 |

<br>

### Differentialble Rendering
| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- | 
| [Soft Rasterizer: A Differentiable Renderer for Image-based 3D Reasoning](https://arxiv.org/abs/1904.01786) | 2019 | |  |

<br>

### FACE
| Paper | Conference | Links | 注释 |
| ---- | ---- | ---- | ---- |  
| [Joint Face Alignment and 3D Face Reconstruction with Application to Face Recognition](https://cvlab.cse.msu.edu/pdfs/Liu_Zhao_Liu_Zeng_PAMI2018.pdf) | 2018 | | |
| [From 2D to 3D real-time expression transfer for facial animation](https://link.springer.com/article/10.1007/s11042-018-6785-8) | 2019 | | 基于视频中检测道德landmark，进行的实时表情变化。（基于2D信息，而且是landmark，对某一面部模型的形变）|
| [Learning to Regress 3D Face Shape and Expression from an Image without 3D Supervision](https://arxiv.org/abs/1905.06817) | 2019 CVPR | [Github](https://github.com/soubhiksanyal/RingNet) | 能够从一张图片捕获出FLAME model的面部参数 |
| [SOFA: Style-based One-shot 3D Facial Animation Driven by 2D landmarks](https://dl.acm.org/doi/10.1145/3591106.3592291) | 2023 | | 基于2D Landmark的3D面部动画 |
| [Alive Caricature from 2D to 3D](https://arxiv.org/abs/1803.06802) | 2018 CVPR | | 卡通面部重建 |
| [Landmark Detection and 3D Face Reconstruction for Caricature using a Nonlinear Parametric Model](https://arxiv.org/abs/2004.09190) | 2021 | [Github](https://github.com/Juyong/CaricatureFace) | 基于关键点的卡通面部重建。虽然是重建，但文章涉及到了卡通面部的关键点检测，包括脸颊轮廓的关键点。甚至提出了面部模型上的Optional Landmark Set，和本研究的思路高度一致。但是神经网络依赖于一个固定的mean face，且训练时依赖几何损失，需要真实的模型数据，泛用性较低 |
| [Generating Animatable 3D Cartoon Faces from Single Portraits](https://arxiv.org/abs/2307.01468) | 2023 | | 先基于图像粗略重建面部，再基于landmark监督，使用Laplacian deformation修正模型。（后半部分值得参考） |
| [Synthesis, Style Editing, and Animation of 3D Cartoon Face](https://www.sciopen.com/article/10.26599/TST.2023.9010028) | 2024 | | 卡通面部重建 |

