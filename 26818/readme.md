# 高光谱遥感传递函数增强文献清单

面向高光谱遥感图像的调制传递函数（MTF）补偿、点扩散函数（PSF）去卷积、光谱保持增强，以及去卷积与光谱解混联合建模。清单优先收录国际高水平期刊与会议，同时补充国内重要期刊文献。

> 最后检索：2026-08-27。本文仅收集论文元数据与合法公开入口，不在仓库中分发受版权保护的全文。

## Content

- [Scope and Selection](#scope-and-selection)
- [Keywords Convention](#keywords-convention)
- [Papers](#papers)
  - [Reviews and Entry Points](#reviews-and-entry-points)
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
| SAD / SAM | Spectral Angle Distance / Mapper | 光谱角距离/制图指标，越小通常表示光谱保持越好 |
| FCLS | Fully Constrained Least Squares | 满足非负与和为一约束的丰度估计 |
| ADMM | Alternating Direction Method of Multipliers | 交替方向乘子法 |
| MTF@Nyquist | MTF at Nyquist frequency | 奈奎斯特频率处的 MTF，常用于评价补偿效果与过锐风险 |

## Papers

### Reviews and Entry Points

- **★ A Survey on Hyperspectral Image Restoration: From the View of Low-Rank Tensor Approximation**  
  *Na Liu, Wei Li, Yinjian Wang, Ran Tao, Qian Du, Jocelyn Chanussot*. *Science China Information Sciences*, 2023. [[paper](https://arxiv.org/abs/2205.08839)] [[doi](https://doi.org/10.1007/s11432-022-3609-4)] [[resources](https://github.com/NaLiu613/LRTA-HSI-Restoration-Survey)]
  - 关联：从低秩张量角度梳理高光谱恢复，可用于建立空谱先验、数据集和评价指标基线。

- **Interpretable Hyperspectral Artificial Intelligence: When Nonconvex Modeling Meets Hyperspectral Remote Sensing**  
  *Danfeng Hong, Wei He, Naoto Yokoya, Jing Yao, Lianru Gao, Liangpei Zhang, Jocelyn Chanussot, Xiao Xiang Zhu*. *IEEE Geoscience and Remote Sensing Magazine*, 2021. [[paper](https://arxiv.org/abs/2103.01449)] [[doi](https://doi.org/10.1109/MGRS.2021.3064051)]
  - 关联：系统讨论模型驱动与深度学习结合，为构造可解释的 MTF/PSF 物理约束网络提供方法论。

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
- [Hyperspectral Remote Sensing Scenes](https://www.ehu.eus/ccwintco/index.php/Hyperspectral_Remote_Sensing_Scenes)：Indian Pines、Pavia 等常用高光谱数据入口。
- [USGS Spectral Library](https://www.usgs.gov/labs/spec-lab/capabilities/spectral-library)：构造端元、光谱保持和解混实验时可用的标准光谱库。

## Search Notes

### 推荐检索式

- 英文核心式：**("hyperspectral" AND ("MTF compensation" OR deconvolution OR deblurring) AND ("spectral preservation" OR "spectral-spatial"))**
- 解混式：**("hyperspectral" AND (deconvolution OR deblurring) AND unmixing)**
- 盲恢复式：**("hyperspectral imagery" AND ("blind deconvolution" OR "defocus deblurring" OR "spatially varying blur"))**
- 中文式：**(高光谱 AND (调制传递函数 OR MTF补偿 OR 去卷积 OR 去模糊) AND (光谱保持 OR 空谱联合 OR 光谱解混))**
- 工程式：**(卫星 OR 遥感) AND (在轨MTF测量 OR MTF补偿 OR PSF估计)**

### 建议阅读顺序

1. 从 Zhao et al. (TGRS 2013) 与 Henrot et al. (TIP 2014) 建立“模糊—单纯形—解混”的理论认识。
2. 用 Fang et al. (CJRS 2017) 和 Song et al. (TIP 2016) 理解空谱正则及参数选择。
3. 用 Wang et al. (TGRS 2023) 与 Gkillas et al. (ICASSP 2023)搭建可解释 AI/PnP 基线。
4. 用 Li et al. (WACV 2025) 处理波长相关、空间变散焦或盲核问题。
5. 用 Layazali and Preza (Remote Sensing 2026) 扩展到“物理退化 + 深度先验 + 丰度约束”的联合模型。

### 实验与筛选建议

- 空间质量至少报告 PSNR、SSIM、边缘保持、MTF 曲线或 MTF@Nyquist，并检查振铃与噪声放大。
- 光谱保持至少报告 SAM/SAD、ERGAS、SID 或逐像元光谱曲线；仅报告 RGB 合成图或 PSNR 不足以证明光谱保持。
- 若结合解混，同时报告端元 SAD、丰度 RMSE、非负/和为一约束误差及下游分类或检测指标。
- 优先使用真实或经标定的逐波段 PSF/MTF；若只使用统一高斯核，应明确其与真实光学系统之间的差距。
- 对学习方法分别验证已知核、未知核、空间不变核、空间变核和跨传感器泛化，避免只在合成固定核上得出工程结论。

### 当前文献空白

直接把 **逐波段在轨 MTF 标定、原分辨率空谱一致性约束、光谱解混与可解释深度先验** 统一到一个框架中的公开研究仍较少。现有工作通常只覆盖其中两至三项，这一交叉点可作为后续方案设计和继续检索的重点。
