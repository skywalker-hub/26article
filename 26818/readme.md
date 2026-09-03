# 高光谱遥感传递函数增强文献清单

面向高光谱遥感图像的调制传递函数（MTF）补偿、点扩散函数（PSF）去卷积、光谱保持增强，以及去卷积与光谱解混联合建模。清单优先收录国际高水平期刊与会议，同时补充国内重要期刊文献。

> 最后检索：2026-09-03。本文仅收集论文元数据与合法公开入口，不在仓库中分发受版权保护的全文。

## Content

- [Scope and Selection](#scope-and-selection)
- [Keywords Convention](#keywords-convention)
- [Papers](#papers)
  - [Reviews and Entry Points](#reviews-and-entry-points)
  - [Task-Oriented Literature: Weak and Small Targets](#task-oriented-literature-weak-and-small-targets)
    - [Recommended Research Wording](#recommended-research-wording)
    - [Direct Bridge Papers](#direct-bridge-papers)
    - [Weak and Subpixel Target Constraints](#weak-and-subpixel-target-constraints)
    - [Scene-Adaptive Spectral Unmixing](#scene-adaptive-spectral-unmixing)
    - [Chinese Directly Related Studies](#chinese-directly-related-studies)
  - [Hyperspectral Deconvolution and Spectral-Preserving Enhancement](#hyperspectral-deconvolution-and-spectral-preserving-enhancement)
    - [Model-Driven Methods](#model-driven-methods)
    - [AI and Model-Data-Driven Methods](#ai-and-model-data-driven-methods)
  - [Deconvolution with Spectral Unmixing](#deconvolution-with-spectral-unmixing)
  - [Remote-Sensing MTF Compensation Foundations](#remote-sensing-mtf-compensation-foundations)
  - [Chinese-Language Literature](#chinese-language-literature)
- [Resources](#resources)
- [Search Notes](#search-notes)

## Scope and Selection

- 本清单将“传函增强”解释为成像系统的 **MTF/OTF/PSF 退化补偿**，在不改变原始像元网格和波段数的前提下恢复空间频率响应。
- **核心文献**：直接研究高光谱去卷积、散焦/运动模糊校正、空谱联合正则、光谱保持或“去卷积 + 解混”。
- **方法基础**：研究光学遥感影像的在轨 MTF 测量、MTF 补偿滤波或工程化复原，但并非专门针对高光谱数据。
- 超分辨率、全色锐化和 HSI-MSI 融合会改变空间采样或引入外部高分辨率观测，故不列入核心清单。
- 星标 **★** 表示与当前课题最直接、建议优先阅读；它不是期刊等级或论文质量的绝对排序。

## Keywords Convention

| Abbreviation | Full name | 本清单中的含义 |
|---|---|---|
| HSI | Hyperspectral Image | 高光谱图像 |
| MTF | Modulation Transfer Function | 调制传递函数，描述系统对不同空间频率的传递能力 |
| OTF | Optical Transfer Function | 光学传递函数，MTF 为其幅值 |
| PSF | Point Spread Function | 点扩散函数；其傅里叶变换对应 OTF |
| Deconvolution | Deconvolution | 在已知或估计 PSF 下恢复清晰图像 |
| Blind | Blind Deconvolution | 图像与模糊核同时未知 |
| SSTV | Spectral-Spatial Total Variation | 空间—光谱总变分正则 |
| PnP | Plug-and-Play Prior | 将去噪器作为迭代优化中的隐式先验 |
| LMM | Linear Mixing Model | 线性光谱混合模型 |
| PLMM / ELMM | Perturbed / Extended Linear Mixing Model | 用加性扰动或逐像元尺度变化表示端元光谱变异 |
| SAD / SAM | Spectral Angle Distance / Mapper | 光谱角距离/制图指标，越小通常表示光谱保持越好 |
| FCLS | Fully Constrained Least Squares | 满足非负与和为一约束的丰度估计 |
| ADMM | Alternating Direction Method of Multipliers | 交替方向乘子法 |
| CEM / ACE | Constrained Energy Minimization / Adaptive Cosine Estimator | 常用高光谱目标检测器 |
| GLRT | Generalized Likelihood Ratio Test | 广义似然比检验 |
| SCR | Signal-to-Clutter Ratio | 信杂比，弱目标的重要度量 |
| $P_d$ / $P_{fa}$ | Probability of Detection / False Alarm | 检出概率与虚警概率 |
| DN | Digital Number | 传感器输出的数字量化值 |
| MTF@Nyquist | MTF at Nyquist frequency | 奈奎斯特频率处的 MTF，常用于评价补偿效果与过锐风险 |

## Papers

### Reviews and Entry Points

- **★ A Survey on Hyperspectral Image Restoration: From the View of Low-Rank Tensor Approximation**  
  *Na Liu, Wei Li, Yinjian Wang, Ran Tao, Qian Du, Jocelyn Chanussot*. *Science China Information Sciences*, 2023. [[paper](https://arxiv.org/abs/2205.08839)] [[doi](https://doi.org/10.1007/s11432-022-3609-4)] [[resources](https://github.com/NaLiu613/LRTA-HSI-Restoration-Survey)]
  - 关联：从低秩张量角度梳理高光谱恢复，可用于建立空谱先验、数据集和评价指标基线。

- **Interpretable Hyperspectral Artificial Intelligence: When Nonconvex Modeling Meets Hyperspectral Remote Sensing**  
  *Danfeng Hong, Wei He, Naoto Yokoya, Jing Yao, Lianru Gao, Liangpei Zhang, Jocelyn Chanussot, Xiao Xiang Zhu*. *IEEE Geoscience and Remote Sensing Magazine*, 2021. [[paper](https://arxiv.org/abs/2103.01449)] [[doi](https://doi.org/10.1109/MGRS.2021.3064051)]
  - 关联：系统讨论模型驱动与深度学习结合，为构造可解释的 MTF/PSF 物理约束网络提供方法论。

### Task-Oriented Literature: Weak and Small Targets

本节是针对“**弱小目标 + 光谱保真传函补偿 + 场景自适应光谱解混**”重新开展的专项检索。分级含义如下：

- **A—直接桥接**：一篇论文同时覆盖目标探测、去卷积/PSF 或解混中的至少两项，能直接进入总体方案。
- **B—强相关约束**：能够定义目标成像、光谱失配、背景自适应或评价方法。
- **C—局部可用**：场景或任务并不完全相同，但其中的模块、损失或实验设计可迁移。

#### Recommended Research Wording

建议课题名称：

> **面向复杂场景弱小/亚像元目标的光谱保真传函补偿与场景自适应解混探测**

更完整的研究描述：

> 针对高光谱遥感弱小目标受系统 MTF 衰减、波段相关或空间变化 PSF、低信杂比、亚像元混合及背景端元变化共同影响的问题，研究受控传函补偿方法，在增强目标空间响应的同时保持像元光谱形状、波段间相对辐射关系与目标丰度；进一步从目标邻域或当前场景中自适应估计背景端元及其光谱变异，构建传函补偿、光谱解混与目标探测协同优化模型，提高复杂场景下的目标检出率、丰度估计精度和跨场景稳健性。

建议区分两个容易混淆的概念：

- **小/亚像元目标**：目标面积小于一个像元或 PSF 有效支撑域，目标能量会扩散至邻近像元。
- **弱目标**：目标丰度低、目标—背景光谱差异小或信杂比低；空间尺寸小不一定光谱弱，反之亦然。

可将观测过程抽象为：

$$
\mathbf{Y}_{\lambda}
=
\mathbf{H}_{\lambda,\mathbf{s}}
\left[
\mathbf{M}_{\lambda,\mathbf{s}}\mathbf{A}
\right]
+\mathbf{N}_{\lambda},
\qquad a_t \ll 1 ,
$$

其中，$\mathbf{H}_{\lambda,\mathbf{s}}$ 表示随波长和空间位置变化的 PSF/MTF 算子；$\mathbf{M}_{\lambda,\mathbf{s}}$ 表示随场景、照明和位置变化的端元；$\mathbf{A}$ 为丰度，$a_t$ 为弱小目标丰度。一个可落地的联合目标应同时包含：

$$
\mathcal{L}
=
\mathcal{L}_{data}
+\alpha\mathcal{L}_{MTF}
+\beta\mathcal{L}_{spectral}
+\gamma\mathcal{L}_{unmix}
+\delta\mathcal{L}_{target}.
$$

- $\mathcal{L}_{data}$：补偿结果经 PSF 再退化后应与原始观测一致。
- $\mathcal{L}_{MTF}$：约束目标频率响应，同时抑制过补偿、噪声放大和振铃。
- $\mathcal{L}_{spectral}$：用 SAM/SAD、SID、逐波段相对误差或 DN 比例关系约束光谱保真。
- $\mathcal{L}_{unmix}$：端元—丰度重建、非负与和为一约束，并允许局部端元或端元变异。
- $\mathcal{L}_{target}$：直接约束目标丰度保持、检测分数、AUC 或固定虚警率下的检出概率。

#### Direct Bridge Papers

- **A—直接桥接｜★ Deblurring and Sparse Unmixing of Hyperspectral Images Using Multiple Point Spread Functions**  
  *Sebastian Berisha, James G. Nagy, Robert J. Plemmons*. *SIAM Journal on Scientific Computing*, 37(5): S389–S406, 2015. [[paper](https://epubs.siam.org/doi/10.1137/140980478)] [[doi](https://doi.org/10.1137/140980478)]
  - 可用点：联合去模糊与稀疏解混，并显式处理随波长变化的多个 PSF；虽为地基空间目标成像，但其“逐波段 PSF + 稀疏丰度”模型与本课题高度吻合。

- **A—直接桥接｜★ Hyperspectral Detection and Unmixing of Subpixel Target Using Iterative Constrained Sparse Representation**  
  *Qiang Ling, Kun Li, Zhaoxu Li, Zaiping Lin, Jiawen Wang*. *IEEE Journal of Selected Topics in Applied Earth Observations and Remote Sensing*, 15: 1049–1063, 2022. [[paper](https://www.researchgate.net/publication/357609962_Hyperspectral_Detection_and_Unmixing_of_Subpixel_Target_Using_Iterative_Constrained_Sparse_Representation)] [[doi](https://doi.org/10.1109/JSTARS.2022.3140389)]
  - 可用点：从目标邻域迭代提取背景端元，以目标总丰度和重建残差联合判决；即使局部背景被目标污染，也能同时完成亚像元目标检测与解混。

- **B—强相关约束｜Target Detection from Noise-Reduced Hyperspectral Imagery Using a Spectral Unmixing Approach**  
  *Shen-En Qian, Josée Lévesque*. *Optical Engineering*, 48(2): 026401, 2009. [[paper](https://doi.org/10.1117/1.3077179)] [[doi](https://doi.org/10.1117/1.3077179)]
  - 可用点：用解混式目标探测评价预处理前后目标可探测性；可将其思路迁移为“传函补偿是否真正改善弱目标检测”的任务级评价。

还应与本清单已有的 **A Generalized Non-Convex Surrogated Framework for Anomaly Detection on Blurred Hyperspectral Images**、**Joint Hyperspectral Image Deconvolution and Unmixing via Plug-and-Play Priors** 和 **Does Deblurring Improve Geometrical Hyperspectral Unmixing?** 联读，三者分别补足“模糊条件下任务驱动探测”“现代联合去卷积—解混”和“模糊收缩光谱单纯形的理论解释”。

#### Weak and Subpixel Target Constraints

- **B—强相关约束｜Subpixel Hyperspectral Target Detection Using Local Spectral and Spatial Information**  
  *Yuval Cohen, Dan G. Blumberg, Stanley R. Rotman*. *Journal of Applied Remote Sensing*, 6(1): 063508, 2012. [[paper](https://www.spiedigitallibrary.org/journals/journal-of-applied-remote-sensing/volume-6/issue-1/063508/Subpixel-hyperspectral-target-detection-using-local-spectral-and-spatial-information/10.1117/1.JRS.6.063508.full)] [[doi](https://doi.org/10.1117/1.JRS.6.063508)]
  - 可用点：指出点目标能量会被 PSF 分散到邻近像元，并用局部均值、局部协方差和空间滤波改善 CEM、GLRT、ACE；可指导 PSF 支撑域和自适应邻域设计。

- **B—强相关约束｜Hierarchical Sub-Pixel Anomaly Detection Framework for Hyperspectral Imagery**  
  *Wenzheng Wang, Baojun Zhao, Fan Feng, Jinghong Nan, Cheng Li*. *Sensors*, 18(11): 3662, 2018. [[paper](https://www.mdpi.com/1424-8220/18/11/3662)] [[doi](https://doi.org/10.3390/s18113662)]
  - 可用点：以点扩散特性构造目标保护正则，避免空间滤波把像素级/亚像元异常目标一并平滑掉。

- **B—强相关约束｜Assessment of Target Detection Limits in Hyperspectral Data**  
  *Wolfgang Gross, Jonas Boehler, Hendrik Schilling, Wolfgang Middelmann, Joerg Weyermann, Peter Wellig, Roland Oechslin, Mathias Kneubuehler*. *SPIE Target and Background Signatures*, 2015. [[metadata](https://publica.fraunhofer.de/entities/publication/ed31f571-d88d-4654-9e2e-909e475da846)] [[doi](https://doi.org/10.1117/12.2192197)]
  - 可用点：用已知混合比例、目标/背景材料和固定虚警率估计检测下限；适合构造不同目标占比与 MTF 水平的可控实验。

- **B—强相关约束｜Multiple Sub-Pixel Target Detection for Hyperspectral Imaging Systems**  
  *Pia Addabbo, Nicomino Fiscante, Gaetano Giunta, Danilo Orlando, Giuseppe Ricci, Silvia Liberata Ullo*. *IEEE Transactions on Signal Processing*, 71: 1599–1611, 2023. [[paper](https://arxiv.org/abs/2301.06314)] [[doi](https://doi.org/10.1109/TSP.2023.3265890)]
  - 可用点：以广义替代模型描述一个像元中的多个亚像元目标及背景丰度，并通过 GLRT 检测；可作为增强后多目标混合判决基线。

- **B—强相关约束｜Hyperspectral Subpixel Target Detection Based on Interaction Subspace Model**  
  *Shengyin Sun, Jun Liu, Siyu Sun*. *Pattern Recognition*, 139: 109464, 2023. [[paper](https://www.sciencedirect.com/science/article/pii/S0031320323001644)] [[doi](https://doi.org/10.1016/j.patcog.2023.109464)]
  - 可用点：用交互子空间描述目标谱变异和目标先验失配；可验证传函补偿后因光谱轻微偏移导致的检测鲁棒性。

- **A—直接桥接｜★ Adaptive Background Endmember Extraction for Hyperspectral Subpixel Object Detection**  
  *Lifeng Yang, Xiaorui Song, Bin Bai, Zhuo Chen*. *Remote Sensing*, 16(12): 2245, 2024. [[paper](https://www.mdpi.com/2072-4292/16/12/2245)] [[doi](https://doi.org/10.3390/rs16122245)]
  - 可用点：从当前 HSI 自适应学习背景端元及其数量，无需为不同数据集手工指定字典规模；与“场景自适应背景解混”表述直接对应。

#### Scene-Adaptive Spectral Unmixing

- **A—直接桥接｜★ Spatially Adaptive Hyperspectral Unmixing**  
  *Kelly Canham, Ariel Schlamm, Amanda Ziemann, Bill Basener, David W. Messinger*. *IEEE Transactions on Geoscience and Remote Sensing*, 49(11): 4248–4262, 2011. [[paper](https://ieeexplore.ieee.org/document/6046125/)] [[doi](https://doi.org/10.1109/TGRS.2011.2169680)]
  - 可用点：在局部尺度估计端元并逐像元解混，再聚类形成全局丰度图；是“场景自适应光谱解混”最直接的经典文献。

- **A—直接桥接｜Hyperspectral Unmixing with Spectral Variability Using a Perturbed Linear Mixing Model**  
  *Pierre-Antoine Thouvenin, Nicolas Dobigeon, Jean-Yves Tourneret*. *IEEE Transactions on Signal Processing*, 64(2): 525–538, 2016. [[paper](https://arxiv.org/abs/1502.01260)] [[code](https://github.com/pthouvenin/unmixing-plmm)] [[doi](https://doi.org/10.1109/TSP.2015.2486746)]
  - 可用点：PLMM 通过逐像元扰动项描述空间与光谱端元变化；适合吸收传函补偿残差、照明和大气变化造成的谱偏移。

- **A—直接桥接｜★ Blind Hyperspectral Unmixing Using an Extended Linear Mixing Model to Address Spectral Variability**  
  *Lucas Drumetz, Miguel Angel Veganzones, Simon Henrot, Ronald Phlypo, Jocelyn Chanussot, Christian Jutten*. *IEEE Transactions on Image Processing*, 25(8): 3890–3905, 2016. [[paper](https://openremotesensing.net/knowledgebase/spectral-variability-and-extended-linear-mixing-model/)] [[pdf](https://openremotesensing.net/wp-content/uploads/2016/12/IEEE_TIP_2016_ELMM_spectral_variability.pdf)] [[doi](https://doi.org/10.1109/TIP.2016.2579259)]
  - 可用点：ELMM 用逐像元缩放描述照明导致的光谱变异，并联合空间正则；适合作为场景自适应解混的可解释模型基线。

- **B—强相关约束｜Deep Generative Endmember Modeling: An Application to Unsupervised Spectral Unmixing**  
  *Ricardo Augusto Borsoi, Tales Imbiriba, José Carlos Moreira Bermudez*. *IEEE Transactions on Computational Imaging*, 6: 374–384, 2020. [[paper](https://arxiv.org/abs/1902.05528)] [[code](https://github.com/ricardoborsoi/Unmixing_with_Deep_Generative_Models)] [[doi](https://doi.org/10.1109/TCI.2019.2948726)]
  - 可用点：直接从当前场景纯像元学习端元变异的低维流形，再与丰度联合优化；可替代固定参数化的谱变异模型。

- **B—强相关约束｜Spectral Variability Aware Blind Hyperspectral Image Unmixing Based on Convex Geometry**  
  *Lucas Drumetz, Jocelyn Chanussot, Christian Jutten, Wing-Kin Ma, Akira Iwasaki*. *IEEE Transactions on Image Processing*, 29: 4568–4582, 2020. [[paper](https://arxiv.org/abs/1904.03888)] [[doi](https://doi.org/10.1109/TIP.2020.2974062)]
  - 可用点：分析端元变化如何破坏内在维数、纯像元和单纯形几何假设，并给出变异感知的盲解混链路。

- **B—强相关约束｜Spectral Variability in Hyperspectral Data Unmixing: A Comprehensive Review**  
  *Ricardo Augusto Borsoi, Tales Imbiriba, José Carlos Moreira Bermudez, Cédric Richard, Jocelyn Chanussot, Lucas Drumetz, Jean-Yves Tourneret, Alina Zare, Christian Jutten*. *IEEE Geoscience and Remote Sensing Magazine*, 9(4): 223–270, 2021. [[paper](https://arxiv.org/abs/2001.07307)] [[pdf](https://imt-atlantique.hal.science/hal-03265014/file/2001.07307.pdf)] [[code](https://github.com/ricardoborsoi/unmixing_spectral_variability)] [[doi](https://doi.org/10.1109/MGRS.2021.3071158)]
  - 可用点：给出端元变异来源、监督程度、计算代价和算法分类；配套代码可统一比较 PLMM、ELMM 等基线。

- **B—强相关约束｜Hyperspectral Blind Unmixing Using a Double Deep Image Prior**  
  *Chao Zhou, Miguel R. D. Rodrigues*. *IEEE Transactions on Neural Networks and Learning Systems*, 35(11): 16478–16492, 2024. [[paper](https://discovery.ucl.ac.uk/id/eprint/10174936/)] [[doi](https://doi.org/10.1109/TNNLS.2023.3294714)]
  - 可用点：只利用待处理 HSI 的内部统计，同时生成端元和丰度；适合缺少跨场景标注和配对真值的场景自适应实现。

#### Chinese Directly Related Studies

- **A—直接桥接｜一种自适应匹配子空间亚像元目标探测方法**  
  *杜博, 钟燕飞, 张良培, 李平湘*. *遥感学报*, 13(4): 597–603, 2009. [[paper](https://www.ygxb.ac.cn/zh/article/doi/10.11834/jrs.20090404/)] [[doi](https://doi.org/10.11834/jrs.20090404)]
  - 可用点：根据像元端元类别和全限制分解结果动态选择端元，降低端元数目估计偏差对亚像元探测的影响。

- **B—强相关约束｜结合光谱解混的高光谱图像异常目标检测 SVDD 算法**  
  *成宝芝, 赵春晖, 王玉磊*. *应用科学学报*, 30(1): 82–88, 2012. [[paper](https://www.jas.shu.edu.cn/CN/abstract/abstract784.shtml)] [[doi](https://doi.org/10.3969/j.issn.0255-8297.2012.01.013)]
  - 可用点：使用解混残差分离复杂背景和异常目标，再通过非线性 SVDD 检测；可作为无目标先验条件下的异常探测支路。

- **B—强相关约束｜高光谱目标探测中的空间和光谱尺度效应**  
  *石婷婷, 张立福, 岑奕, 孙雪剑, 高英倩, 童庆禧*. *遥感学报*, 19(6): 954–963, 2015. [[paper](https://www.ygxb.ac.cn/zh/article/doi/10.11834/jrs.20155012/)] [[doi](https://doi.org/10.11834/jrs.20155012)]
  - 可用点：定量分析小目标探测对空间、光谱尺度的敏感性；可用于设置不同 MTF 衰减和波段聚合程度的实验梯度。

- **B—强相关约束｜基于线性解混的高光谱图像目标检测研究**  
  *杨桄, 田张男, 李豪, 关世豪*. *激光技术*, 44(2): 143–147, 2020. [[paper](https://www.jgjs.net.cn/cn/article/id/5d62310a-6084-4fae-a21b-ff90992f0cb7)] [[pdf](https://www.jgjs.net.cn/cn/article/pdf/preview/10.7510/jgjs.issn.1001-3806.2020.02.001.pdf)] [[doi](https://doi.org/10.7510/jgjs.issn.1001-3806.2020.02.001)]
  - 可用点：在复杂背景混合模型中多次去除端元以简化背景，可转化为局部背景端元迭代筛选模块。

- **A—直接桥接｜★ 利用光谱解混合的目标检测**  
  *张蕾, 乔凯, 吴银花, 等*. *光学精密工程*, 31(21): 3156–3166, 2023. [[paper](https://ope.lightpublishing.cn/zh/article/doi/10.37188/OPE.20233121.3156/)] [[pdf](https://ope.lightpublishing.cn/rc-pub/front/front-article/download/43807360/lowqualitypdf/%E5%88%A9%E7%94%A8%E5%85%89%E8%B0%B1%E8%A7%A3%E6%B7%B7%E5%90%88%E7%9A%84%E7%9B%AE%E6%A0%87%E6%A3%80%E6%B5%8B.pdf)] [[doi](https://doi.org/10.37188/OPE.20233121.3156)]
  - 可用点：融合目标端元丰度、光谱夹角和加权 CEM，降低目标像元对背景统计的污染；适合直接接在传函补偿之后形成任务损失。

### Hyperspectral Deconvolution and Spectral-Preserving Enhancement

#### Model-Driven Methods

- **★ Fast Positive Deconvolution of Hyperspectral Images**  
  *Simon Henrot, Charles Soussen, David Brie*. *IEEE Transactions on Image Processing*, 22(2): 828–833, 2013. [[paper](https://pubmed.ncbi.nlm.nih.gov/22955906/)] [[doi](https://doi.org/10.1109/TIP.2012.2216280)]
  - 关联：用非负约束及空间、光谱平滑先验进行快速去卷积，是原分辨率空谱联合复原的代表性基础工作。

- **Regularization Parameter Estimation for Non-Negative Hyperspectral Image Deconvolution**  
  *Yingying Song, David Brie, El-Hadi Djermoune, Simon Henrot*. *IEEE Transactions on Image Processing*, 25(11): 5316–5330, 2016. [[paper](https://hal.science/hal-01358458)] [[doi](https://doi.org/10.1109/TIP.2016.2601489)]
  - 关联：针对非负高光谱去卷积自动选择空间与光谱正则参数，减少经验调参。

- **★ Hyperspectral Image Deconvolution with a Spectral-Spatial Total Variation Regularization**  
  *Houzhang Fang, Chunan Luo, Gang Zhou, Xiaoping Wang*. *Canadian Journal of Remote Sensing*, 43(4): 384–395, 2017. [[paper](https://doaj.org/article/544281285eae49e29ff7775ef8cdb43c)] [[doi](https://doi.org/10.1080/07038992.2017.1356221)]
  - 关联：SSTV 同时利用相邻波段相关性并保持光谱方向的不连续变化，与“增强但不扭曲谱形”的目标高度一致。

- **Online Deconvolution for Industrial Hyperspectral Imaging Systems**  
  *Yingying Song, El-Hadi Djermoune, Jie Chen, Cédric Richard, David Brie*. *SIAM Journal on Imaging Sciences*, 12(1): 54–86, 2019. [[paper](https://epubs.siam.org/doi/10.1137/18M1177640)] [[doi](https://doi.org/10.1137/18M1177640)]
  - 关联：面向推扫式高光谱系统进行在线去卷积，适合作为星载/机载逐行成像实时处理的算法参考。

- **★ Defocus Hyperspectral Image Deblurring with Adaptive Reference Image and Scale Map**  
  *De-Wang Li, Lin-Jing Lai, Hua Huang*. *Journal of Computer Science and Technology*, 34(3): 569–580, 2019. [[paper](https://jcst.ict.ac.cn/article/cstr/32374.14.s11390-019-1927-7)] [[pdf](https://jcst.ict.ac.cn/en/article/pdf/preview/10.1007/s11390-019-1927-7.pdf)] [[doi](https://doi.org/10.1007/s11390-019-1927-7)]
  - 关联：显式分析波段间相关性与差异，由较清晰波段构造参考图引导模糊波段恢复。

- **Local Extremum Constrained Total Variation Model for Natural and Hyperspectral Image Non-Blind Deblurring**  
  *Lan Li, Meiping Song, Qiang Zhang, Yushuai Dong, Yulei Wang, Qiangqiang Yuan*. *IEEE Transactions on Circuits and Systems for Video Technology*, 34(9): 8547–8561, 2024. [[pdf](https://qzhang95.github.io/Files/TCSVT_2024_LECTV.pdf)] [[doi](https://doi.org/10.1109/TCSVT.2024.3385468)]
  - 关联：以局部极值约束抑制 TV 过平滑，在非盲高光谱去模糊中兼顾边缘与空谱结构。

- **A Generalized Non-Convex Surrogated Framework for Anomaly Detection on Blurred Hyperspectral Images**  
  *Yinjian Wang, Wei Li, Yuanyuan Gui, Haijun Xie, Lianbo Zhang*. *IEEE Transactions on Image Processing*, 34: 3108–3122, 2025. [[metadata](https://dblp.dagstuhl.de/rec/journals/tip/WangLGXZ25.html)] [[doi](https://doi.org/10.1109/TIP.2025.3568745)]
  - 关联：把模糊建模、空谱低秩表示和下游异常检测耦合，说明传函补偿可围绕任务性能而不只围绕视觉锐度设计。

#### AI and Model-Data-Driven Methods

- **Learning Spectral-Spatial Prior Via 3DDnCNN for Hyperspectral Image Deconvolution**  
  *Xiuheng Wang, Jie Chen, Cédric Richard, David Brie*. *IEEE ICASSP*, 2403–2407, 2020. [[pdf](https://www.cedric-richard.fr/Articles/wang2019learning.pdf)] [[doi](https://doi.org/10.1109/ICASSP40776.2020.9054539)]
  - 关联：使用 3D CNN 学习空谱联合先验，并嵌入高光谱去卷积框架。

- **★ Tuning-Free Plug-and-Play Hyperspectral Image Deconvolution with Deep Priors**  
  *Xiuheng Wang, Jie Chen, Cédric Richard*. *IEEE Transactions on Geoscience and Remote Sensing*, 61: 1–13, 2023. [[paper](https://arxiv.org/abs/2211.15307)] [[pdf](https://www.cedric-richard.fr/Articles/wang2022tuning.pdf)] [[code](https://github.com/xiuheng-wang/Tuning_free_PnP_HSI_deconvolution)] [[doi](https://doi.org/10.1109/TGRS.2023.3253549)]
  - 关联：物理数据保真项与深度先验结合，并自动处理关键参数；是可解释 AI 去卷积的重要基线。

- **A Highly Interpretable Deep Equilibrium Network for Hyperspectral Image Deconvolution**  
  *Alexandros Gkillas, Dimitris Ampeliotis, Kostas Berberidis*. *IEEE ICASSP*, 1–5, 2023. [[pdf](https://dimitris-ampeliotis.github.io/pdfs/C31.pdf)] [[doi](https://doi.org/10.1109/ICASSP49357.2023.10095286)]
  - 关联：将迭代优化写成深度平衡网络，兼顾可解释性、内存效率与高光谱去卷积性能。

- **★ Wavelength- and Depth-Aware Deep Image Prior for Blind Hyperspectral Imagery Deblurring with Coarse Depth Guidance**  
  *Jiahuan Li, Xiaoyu Dong, Wei He, Naoto Yokoya*. *IEEE/CVF Winter Conference on Applications of Computer Vision (WACV)*, 3162–3171, 2025. [[paper](https://openaccess.thecvf.com/content/WACV2025/html/Li_Wavelength-_and_Depth-Aware_Deep_Image_Prior_for_Blind_Hyperspectral_Imagery_WACV_2025_paper.html)] [[pdf](https://openaccess.thecvf.com/content/WACV2025/papers/Li_Wavelength-_and_Depth-Aware_Deep_Image_Prior_for_Blind_Hyperspectral_Imagery_WACV_2025_paper.pdf)] [[doi](https://doi.org/10.1109/WACV61041.2025.00313)]
  - 关联：同时考虑波长相关与深度相关的空间变模糊，适合真实光学系统中的盲 PSF/散焦恢复。

- **★ Hyperspectral Image Reconstruction Based on Blur–Kernel–Prior and Spatial–Spectral Attention**  
  *Hongyu Xie, Mingyu Yang, Huansong Huang, Mingle Zhang, Wei Zhang, Qingbin Jiao, Liang Xu, Xin Tan*. *Remote Sensing*, 17(8): 1401, 2025. [[paper](https://www.mdpi.com/2072-4292/17/8/1401)] [[doi](https://doi.org/10.3390/rs17081401)]
  - 关联：以模糊核先验和空谱注意力恢复细节；其退化模型保持原空间、光谱采样尺寸，直接契合同分辨率增强。

### Deconvolution with Spectral Unmixing

- **★ Deblurring and Sparse Unmixing for Hyperspectral Images**  
  *Xi-Le Zhao, Fan Wang, Ting-Zhu Huang, Michael K. Ng, Robert J. Plemmons*. *IEEE Transactions on Geoscience and Remote Sensing*, 51(7): 4045–4058, 2013. [[metadata](https://hub.hku.hk/handle/10722/276956)] [[doi](https://doi.org/10.1109/TGRS.2012.2227764)]
  - 关联：将图像去模糊与稀疏解混结合，是“传函增强 + 光谱解混”方向最直接的奠基文献之一。

- **★ Does Deblurring Improve Geometrical Hyperspectral Unmixing?**  
  *Simon Henrot, Charles Soussen, Manuel Dossot, David Brie*. *IEEE Transactions on Image Processing*, 23(3): 1169–1180, 2014. [[paper](https://pubmed.ncbi.nlm.nih.gov/24723521/)] [[doi](https://doi.org/10.1109/TIP.2014.2300822)]
  - 关联：从几何角度说明模糊会收缩光谱单纯形，并分析去卷积对端元与丰度估计的改善。

- **Sequential Deconvolution–Unmixing of Blurred Hyperspectral Data**  
  *Simon Henrot, Charles Soussen, David Brie*. *IEEE International Conference on Image Processing (ICIP)*, 5152–5156, 2014. [[pdf](https://projet.liris.cnrs.fr/imagine/pub/proceedings/ICIP-2014/Papers/1569910491.pdf)] [[doi](https://doi.org/10.1109/ICIP.2014.7026043)]
  - 关联：比较处理次序，并支持“先去卷积、再解混”优于相反顺序这一实践结论。

- **Joint Blind Deconvolution and Spectral Unmixing of Hyperspectral Images**  
  *Qiang Zhang*. *Advanced Maui Optical and Space Surveillance Technologies Conference (AMOS)*, 2013. [[pdf](https://amostech.com/TechnicalPapers/2013/Adaptive_Optics_Imaging/ZHANG.pdf)]
  - 关联：同时估计未知模糊与光谱混合参数；会议层级低于上述 IEEE 期刊，作为直接相关补充阅读。

- **Joint Unmixing-Deconvolution Algorithms for Hyperspectral Images**  
  *Yingying Song, El-Hadi Djermoune, David Brie, Cédric Richard*. *European Signal Processing Conference (EUSIPCO)*, 1–5, 2019. [[pdf](https://www.eurasip.org/Proceedings/Eusipco/eusipco2019/Proceedings/papers/1570533710.pdf)] [[doi](https://doi.org/10.23919/EUSIPCO.2019.8902950)]
  - 关联：在统一优化中联合求解丰度与去卷积结果，可作为联合模型和求解器设计的直接基线。

- **★ Joint Hyperspectral Image Deconvolution and Unmixing via Plug-and-Play Priors**  
  *Sina Layazali, Chrysanthe Preza*. *Remote Sensing*, 18(13): 2066, 2026. [[paper](https://www.mdpi.com/2072-4292/18/13/2066)] [[doi](https://doi.org/10.3390/rs18132066)]
  - 关联：在 ADMM 中结合卷积成像模型、DnCNN 隐式先验和丰度单纯形约束，同时改善重建与丰度估计，是该方向较新的 AI 化方案。

### Remote-Sensing MTF Compensation Foundations

以下论文主要针对全色、多光谱或通用光学遥感影像，不应直接当作高光谱结果；它们适合支撑 MTF 获取、补偿滤波器设计、噪声放大控制与工程化实现。

- **IKONOS Spatial Resolution and Image Interpretability Characterization**  
  *Robert Ryan, Braxton Baldridge, Robert A. Schowengerdt, Taeyoung Choi, Dennis L. Helder, Slawomir Blonski*. *Remote Sensing of Environment*, 88(1–2), 2003. [[paper](https://www.sciencedirect.com/science/article/pii/S003442570300230X)] [[doi](https://doi.org/10.1016/j.rse.2003.07.006)]
  - 关联：经典在轨空间分辨率、MTF 与图像可判读性研究，为“增强多少才合理”提供评价背景。

- **Removing Atmospheric MTF and Establishing an MTF Compensation Filter for the HJ-1A CCD Camera**  
  *Xiaoying Li, Xingfa Gu, Qiaoyan Fu, Tao Yu, Hailiang Gao, Jiaguo Li, Li Liu*. *International Journal of Remote Sensing*, 34(4): 1413–1427, 2013. [[paper](https://www.tandfonline.com/doi/full/10.1080/01431161.2012.721020)] [[doi](https://doi.org/10.1080/01431161.2012.721020)]
  - 关联：分离大气 MTF 并设计卫星相机补偿滤波器，适合构建从在轨 MTF 到增强核的工程链路。

- **CPU/GPU Near Real-Time Preprocessing for ZY-3 Satellite Images: Relative Radiometric Correction, MTF Compensation, and Geocorrection**  
  *Liuyang Fang, Mi Wang, Deren Li, Jun Pan*. *ISPRS Journal of Photogrammetry and Remote Sensing*, 87: 229–240, 2014. [[paper](https://www.sciencedirect.com/science/article/pii/S0924271613002724)] [[doi](https://doi.org/10.1016/j.isprsjprs.2013.11.010)]
  - 关联：把 MTF 补偿纳入近实时卫星预处理流水线，适合参考 GPU 实现与工程吞吐设计。

- **An Efficient Remote Sensing Image Compensation Method Based on Assessed Modulated Transfer Function**  
  *J. Li, M. Wei*. *Optik*, 139: 407–414, 2017. [[paper](https://www.sciencedirect.com/science/article/abs/pii/S0030402617303893)] [[doi](https://doi.org/10.1016/j.ijleo.2017.03.118)]
  - 关联：依据估计 MTF 进行补偿，重点可关注锐化收益与噪声/振铃放大的平衡。

- **Optical Satellite Image MTF Compensation for Remote-Sensing Data Production**  
  *Chao Wang, Ruifei Zhu*. *International Journal of Computer Applications in Technology*, 68(2): 132–142, 2022. [[paper](https://www.inderscience.com/info/inarticle.php?artid=123467)] [[doi](https://doi.org/10.1504/IJCAT.2022.123467)]
  - 关联：从遥感数据生产角度讨论 MTF 补偿，适合补充业务化处理流程。

### Chinese-Language Literature

#### 直接高光谱去模糊与光谱保持

- **★ 基于波段选择估计 PSF 的高光谱图像运动模糊盲校正方法**  
  *南一冰, 高昆, 倪国强*. *红外与毫米波学报*, 35(6): 715–722, 2016. [[paper](https://journal.sitp.ac.cn/hwyhmb/hwyhmbcn/article/abstract/160017)]
  - 关联：由选择波段估计 PSF，再校正全部波段；论文明确关注空间质量提升与光谱失真降低。

- **★ 用于物料混合均匀性检测的高光谱图像散焦模糊去除**  
  *钱斐, 胡凡, 苟晓东, 等*. *光学精密工程*, 34(7): 1156–1169, 2026. [[paper](https://ope.lightpublishing.cn/zh/article/doi/10.37188/OPE.20263407.1156/)] [[doi](https://doi.org/10.37188/OPE.20263407.1156)]
  - 关联：提出物理约束、自监督、非配对高光谱去模糊算法；虽属工业检测场景，但对遥感无清晰配对样本时的训练方式有参考价值。

#### 通用光学遥感 MTF 测量与补偿

- **DMC 卫星图像 MTF 分析及其复原方法研究**  
  *李盛阳, 朱重光*. *遥感学报*, 9(4): 475–479, 2005. [[paper](https://www.ygxb.ac.cn/zh/article/doi/10.11834/jrs.20050468/)] [[doi](https://doi.org/10.11834/jrs.20050468)]
  - 关联：国内较早的卫星图像 MTF 分析与复原工作，可用于追溯方法演进。

- **CBERS-02B 卫星 WFI 成像在轨 MTF 估算与图像 MTF 补偿**  
  *李小英, 顾行发, 余涛, 程天海, 高海亮, 李家国, 杨晓峰*. *遥感学报*, 13(3): 377–384, 2009. [[pdf](https://www.ygxb.ac.cn/rc-pub/front/front-article/download/10652797/lowqualitypdf/CBERS-02B%E5%8D%AB%E6%98%9FWFI%E6%88%90%E5%83%8F%E5%9C%A8%E8%BD%A8MTF%E4%BC%B0%E7%AE%97%E4%B8%8E%E5%9B%BE%E5%83%8FMTF%E8%A1%A5%E5%81%BF.pdf)]
  - 关联：完整覆盖在轨 MTF 估算与补偿，是国内工程链路的典型案例。

- **光学遥感成像系统调制传递函数补偿技术**  
  *曾湧, 陈世平, 于晋*. *中国空间科学技术*, 2010(4): 38–43, 2010. [[paper](https://journal26.magtechjournal.com/kjkxjs/CN/lexeme/showArticleByLexeme.do?articleID=8354)]
  - 关联：面向光学遥感系统讨论 MTF 补偿技术，可作为滤波器设计与系统分析的中文基础材料。

- **基于 MTFC 的遥感图像复原方法**  
  *陈龙, 徐彭梅, 周虎*. *航天返回与遥感*, 2014. [[paper](https://htfhyyg.spacejournal.cn/article/doi/10.3969/j.issn.1009-8518.2014.04.011)] [[doi](https://doi.org/10.3969/j.issn.1009-8518.2014.04.011)]
  - 关联：以 MTF 补偿滤波进行遥感图像复原，适合与高光谱逐波段补偿方案对照。

- **基于空域的自适应 MTFC 遥感图像复原算法**  
  *周楠, 齐文雯, 曹世翔, 何红艳, 邢坤, 岳春宇*. *航天返回与遥感*, 36(4): 54–62, 2015. [[pdf](https://www.spacejournal.cn/en/article/pdf/preview/10.3969/j.issn.1009-8518.2015.04.008.pdf)] [[doi](https://doi.org/10.3969/j.issn.1009-8518.2015.04.008)]
  - 关联：研究空域自适应补偿，可用于波段差异化核、局部 MTF 或空间变 PSF 的实现参考。

- **GF-2 星全色相机在轨 MTF 测量和图像复原研究**  
  *王治中, 张庆君*. *自然资源遥感*, 28(4): 93–99, 2016. [[paper](https://www.gtzyyg.com/cn/article/id/3ceb8c77-5860-4527-838e-4ce56cf6a0fb)] [[doi](https://doi.org/10.6046/gtzyyg.2016.04.15)]
  - 关联：针对国产高分卫星开展在轨 MTF 测量与复原，便于理解真实遥感系统标定条件。

- **基于在轨 MTF 测试的定量图像质量提升方法**  
  *周雨荷, 伏瑞敏, 齐文雯*. *航天返回与遥感*, 45(2): 125–133, 2024. [[paper](https://htfhyyg.spacejournal.cn/article/doi/10.3969/j.issn.1009-8518.2024.02.012?viewType=HTML)] [[pdf](https://htfhyyg.spacejournal.cn/cn/article/pdf/preview/10.3969/j.issn.1009-8518.2024.02.012.pdf)] [[doi](https://doi.org/10.3969/j.issn.1009-8518.2024.02.012)]
  - 关联：把在轨 MTF 测试与定量质量提升相连接，适合作为当前国产卫星业务方法参考。

## Resources

- [Hyperspectral Image Restoration Survey Resources](https://github.com/NaLiu613/LRTA-HSI-Restoration-Survey)：综述配套的论文、数据集与方法索引。
- [Tuning-Free PnP HSI Deconvolution](https://github.com/xiuheng-wang/Tuning_free_PnP_HSI_deconvolution)：本清单中 2023 年 TGRS 论文的公开实现。
- [PLMM Unmixing](https://github.com/pthouvenin/unmixing-plmm)：逐像元端元扰动模型的 MATLAB 实现。
- [Spectral Variability Unmixing Toolbox](https://github.com/ricardoborsoi/unmixing_spectral_variability)：PLMM、ELMM 等谱变异解混方法的统一对比工具箱。
- [Deep Generative Endmember Modeling](https://github.com/ricardoborsoi/Unmixing_with_Deep_Generative_Models)：从当前场景学习端元变异流形的 DeepGUn 实现。
- [UnDIP](https://github.com/BehnoodRasti/UnDIP)：基于单幅 HSI 内部先验估计丰度的无监督实现，可作为场景自适应网络参考。
- [Hyperspectral Remote Sensing Scenes](https://www.ehu.eus/ccwintco/index.php/Hyperspectral_Remote_Sensing_Scenes)：Indian Pines、Pavia 等常用高光谱数据入口。
- [USGS Spectral Library](https://www.usgs.gov/labs/spec-lab/capabilities/spectral-library)：构造端元、光谱保持和解混实验时可用的标准光谱库。

## Search Notes

### 推荐检索式

- 英文核心式：**("hyperspectral" AND ("MTF compensation" OR deconvolution OR deblurring) AND ("spectral preservation" OR "spectral-spatial"))**
- 解混式：**("hyperspectral" AND (deconvolution OR deblurring) AND unmixing)**
- 盲恢复式：**("hyperspectral imagery" AND ("blind deconvolution" OR "defocus deblurring" OR "spatially varying blur"))**
- 弱小目标式：**("hyperspectral" AND ("weak target" OR "small target" OR "subpixel target") AND (PSF OR blur OR "spatial resolution"))**
- 探测—解混式：**("hyperspectral" AND ("target detection" OR "anomaly detection") AND (unmixing OR abundance OR "background endmember"))**
- 场景自适应式：**("hyperspectral unmixing" AND ("spatially adaptive" OR "spectral variability" OR "local endmember" OR "deep image prior"))**
- 中文式：**(高光谱 AND (调制传递函数 OR MTF补偿 OR 去卷积 OR 去模糊) AND (光谱保持 OR 空谱联合 OR 光谱解混))**
- 中文弱目标式：**(高光谱 AND (弱小目标 OR 小目标 OR 亚像元目标) AND (点扩散函数 OR 光谱解混 OR 背景端元 OR 场景自适应))**
- 工程式：**(卫星 OR 遥感) AND (在轨MTF测量 OR MTF补偿 OR PSF估计)**

### 建议阅读顺序

1. 从 Zhao et al. (TGRS 2013) 与 Henrot et al. (TIP 2014) 建立“模糊—单纯形—解混”的理论认识。
2. 用 Fang et al. (CJRS 2017) 和 Song et al. (TIP 2016) 理解空谱正则及参数选择。
3. 用 Wang et al. (TGRS 2023) 与 Gkillas et al. (ICASSP 2023)搭建可解释 AI/PnP 基线。
4. 用 Li et al. (WACV 2025) 处理波长相关、空间变散焦或盲核问题。
5. 用 Layazali and Preza (Remote Sensing 2026) 扩展到“物理退化 + 深度先验 + 丰度约束”的联合模型。
6. 针对弱小目标，优先联读 Berisha et al. (SISC 2015)、Ling et al. (JSTARS 2022)、Cohen et al. (JARS 2012) 和 Yang et al. (Remote Sensing 2024)。

### 实验与筛选建议

- 空间质量至少报告 PSNR、SSIM、边缘保持、MTF 曲线或 MTF@Nyquist，并检查振铃与噪声放大。
- 光谱保持至少报告 SAM/SAD、ERGAS、SID 或逐像元光谱曲线；仅报告 RGB 合成图或 PSNR 不足以证明光谱保持。
- 若结合解混，同时报告端元 SAD、丰度 RMSE、非负/和为一约束误差及下游分类或检测指标。
- 优先使用真实或经标定的逐波段 PSF/MTF；若只使用统一高斯核，应明确其与真实光学系统之间的差距。
- 对学习方法分别验证已知核、未知核、空间不变核、空间变核和跨传感器泛化，避免只在合成固定核上得出工程结论。
- 弱小目标实验应按目标填充率、目标—背景光谱夹角、SCR/SNR 和 PSF/MTF 水平分层；主要报告 ROC、AUC、固定 $P_{fa}$ 下的 $P_d$，不要只报告复原图像质量。
- 传函补偿前后应同时比较目标中心丰度、PSF 支撑域内目标总丰度和检测分数，防止“中心像元变锐但总目标能量或谱形失真”。
- 局部背景端元估计要设置目标保护或污染抑制机制，否则弱目标可能被吸收到背景字典中。

### 当前文献空白

已有研究分别建立了“多 PSF 去模糊—稀疏解混”“亚像元目标检测—局部背景端元解混”和“模糊高光谱—任务探测”链路，但直接把 **逐波段在轨 MTF 标定、原分辨率空谱一致性、场景自适应端元变化、弱目标保护与任务级探测损失** 统一到一个框架中的公开研究仍较少。现有成果通常只覆盖其中两至三项，这一交叉点可作为方案创新重点。
