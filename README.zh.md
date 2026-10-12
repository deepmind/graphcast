# WeatherNext

<p align="center">
  <a href="README.md">English</a> · <b>简体中文</b>
</p>

## WeatherNext 2

本仓库包含由 Google DeepMind 与 Google Research 联合研发的全球中程大气与气旋预报模型 **WeatherNext 2 (WN2)** 的开源代码。

仓库中同时收录了前代模型 [GraphCast](https://deepmind.google/blog/graphcast-ai-model-for-faster-and-more-accurate-global-weather-forecasting/) 与 [GenCast](https://deepmind.google/blog/gencast-predicts-weather-and-the-risks-of-extreme-conditions-with-sota-accuracy/) 的完整代码。

### 获取预报数据流

如果您希望直接获取 WN2 模型的每日预报数据输出，而非自行运行模型，我们在多个平台上提供了数据接入服务：

*   [Google Cloud](https://developers.google.com/weathernext/guides/access-forecast)（包括 Earth Engine、BigQuery 与 Vertex AI）。
*   [WeatherLab](https://deepmind.google.com/science/weatherlab)（包含热带气旋路径追踪）。
*   [OpenMeteo](https://open-meteo.com/en/docs/google-weathernext-api)（包含 API 与交互式构建器）。

### 了解更多

*   **模型指南与文档：** [Google Developers WeatherNext 指南](https://developers.google.com/weathernext/guides/models)
*   **WeatherNext Cyclones 论文：** [Operational tropical cyclone forecasting with AI (Nature 2026)](https://www.nature.com/articles/s41586-026-10953-2)
*   **FGN/WN2 技术报告：** [Skillful joint probabilistic weather forecasting from marginals (arXiv:2506.10772)](https://arxiv.org/abs/2506.10772)
*   **WeatherNext 2 官方博客：** [WeatherNext 2: Our most advanced weather forecasting model](https://blog.google/innovation-and-ai/models-and-research/google-deepmind/weathernext-2/)
*   **WeatherNext Cyclones 官方博客：** [WeatherNext: AI model achieves breakthrough in forecasting cyclones](https://deepmind.google/blog/weathernext-ai-model-achieves-breakthrough-in-forecasting-cyclones/)

### 早期模型

本仓库作为 WeatherNext 家族系列模型的核心主页。除 WN2 之外，还维护以下经典与专用模型的代码及文档：

*   [WeatherNext Graph](docs/weathernext1_graph/README.md)：基于图神经网络（GNN）的确定性中程天气预报模型（发表为 GraphCast）。
*   [WeatherNext Gen](docs/weathernext1_gen/README.md)：基于扩散模型的集合中程天气预报模型（发表为 GenCast）。

## 提供的预训练模型

本仓库提供了运行不同版本 WeatherNext 2 与 WeatherNext Cyclones 的代码。两者的唯一区别在于 WN2 还能预测 100 米风场（100m wind）。在气旋预测方面，WN2 采用与 WN Cyclones 完全相同的算法。由于训练批次独立，两者的权重有所不同。

### WeatherNext 2

1.  **WeatherNext2_<2025**（业务运行版本）：0.25° 分辨率（约 30 公里）。在 ECMWF HRES 数据上进行微调，专为直接从业务 HRES 初始场初始化而设计（而非 ERA5 再分析资料）。使用截至 2024 年的数据进行训练。对应权重文件为：`WeatherNext2_<2025_model{1,2,3,4}.npz`。

### WeatherNext Cyclones（复现论文结果的模型）

1.  **WeatherNextCyclones_<2025**（业务运行版本）：0.25° 分辨率。在 2025 年大西洋飓风季期间实时在线运行的模型，公开名称为 FNV3（美国国家飓风中心 NHC 的后处理版本称为 GDMI）。基于截至 2024 年的数据训练。论文附录中包含该模型权重在 NHC 海盆上的部分评估，以及 2025 年 9 月追踪器改进对预测效果的提升。对应权重文件为：`WeatherNextCyclones_<2025_model{1,2,3,4}.npz`。
2.  **WeatherNextCyclones_<2024**：0.25° 分辨率。复现论文中 2024 年评估结果。基于截至 2023 年的数据训练。对应权重文件为：`WeatherNextCyclones_<2024_model{1,2,3,4}.npz`。
3.  **WeatherNextCyclones_<2023**：0.25° 分辨率。复现论文中 2023 年评估结果。基于截至 2022 年的数据训练。对应权重文件为：`WeatherNextCyclones_<2023_model{1,2,3,4}.npz`。

### WeatherNext Cyclones Mini（轻量版）

1.  **WeatherNextCyclones_Mini_<2024**：1° 分辨率。适用于显存和算力受限环境（例如本地测试或单张 TPU/GPU）的轻量级版本。其性能预期不及全量模型，但预测要素与 WeatherNext2_<2025 一致（包含气旋）。基于截至 2023 年的数据训练。对应权重文件为：`WeatherNextCyclones_Mini_<2024.npz`。
2.  **WeatherNextCyclones_Mini_<2023**：同上，但仅基于截至 2022 年的数据训练。对应权重文件为：`WeatherNextCyclones_Mini_<2023.npz`。

WeatherNextCyclones_Mini 的评估结果请参见 [WeatherNext Cyclones 论文](https://www.nature.com/articles/s41586-026-10953-2) 的附录。

## 快速上手指南

快速上手体验 WeatherNext 2 的最简便方式是运行我们的交互式 [Colab Notebook](docs/weathernext2/wn2_demo.ipynb)，可直接在 [Colaboratory](https://colab.research.google.com/github/google-deepmind/weathernext/blob/master/docs/weathernext2/wn2_demo.ipynb) 中打开。
该 Notebook 默认配置为 WeatherNext Cyclones Mini，推荐使用 Colab 提供的免费 `v5e-1` 运行时环境。同时该 Notebook 也可用于运行上述其他全量模型（全量模型需要 `v5p` 加速器）。

总体而言，我们建议尽可能在 TPU 上运行 WeatherNext 2，因为其系统实现针对 TPU 进行了高度优化。若选择在 GPU 上运行，必须按演示 Notebook 中的示例切换注意力机制实现。非 Mini 模型需要 H100 级别的显存支持，而 Mini 模型可在 P100 级别显卡上完成推理。

预训练权重与示例数据托管于我们的 [Google Cloud Bucket 存储桶](https://console.cloud.google.com/storage/browser/dm_graphcast)。

**在 Notebook 中，你将学习如何：**

1.  从存储桶自动加载所需的模型权重。
2.  加载初始状态气象数据（例如 HRES 初始场）。
3.  初始化 WN2（FGN）网络架构。
4.  运行自回归推演（autoregressive rollout）步进以生成天气预报预测。
5.  可视化输出结果（例如气温、风速、位势高度）。
6.  对模型输出运行直接追踪器（direct tracker）以获取热带气旋路径数据。
7.  计算模型预测与真实观测之间的训练损失，并执行梯度更新步。

## 环境配置

### 安装

> [!NOTE]
> 本仓库代码为用于运行和复现已发表论文模型的科研代码，按“原样（as-is）”提供。不保证 API 的绝对向后兼容性，后续更新可能会引入破坏性变更。建议在实际使用时锁定特定 Release 版本。

例如：

```bash
pip install git+https://github.com/google-deepmind/weathernext.git@v0.3.0
```

### 模型权重

运行 WeatherNext 2 或 WeatherNext Cyclones 需要下载预训练模型权重。权重文件可从 [Google Cloud Bucket 存储桶](https://console.cloud.google.com/storage/browser/dm_graphcast) 获取。

### 共享工具库

`utils/` 目录包含了多个 WeatherNext 模型共用的通用库，提供自回归推演、输入归一化、图构建基础组件、损失计算以及与 JAX 兼容的 xarray 数据处理等通用底层基础设施。模型专用代码请参阅各模型专属子目录的 README。

### 训练数据

完整训练模型需要从 [ECMWF（欧洲中期天气预报中心）](https://www.ecmwf.int/) 下载 [ERA5](https://www.ecmwf.int/en/forecasts/datasets/reanalysis-datasets/era5) 再分析数据集，推荐通过 [WeatherBench2](https://weatherbench2.readthedocs.io/en/latest/data-guide.html#era5) 以 Zarr 格式高效访问。

业务微调数据可通过 [WeatherBench2 的 HRES 数据](https://weatherbench2.readthedocs.io/en/latest/data-guide.html#ifs-hres-t-0-analysis) 获取。

上述数据集可能受独立的条款及条件约束，使用前请确保符合适用的授权限制。

## 许可证

Copyright 2026 Google LLC.

Colab Notebook 及其相关代码遵循 Apache License, Version 2.0 (Apache 2.0) 开源；除非符合 Apache 2.0 许可证条款，否则不得使用这些材料。许可证副本见：
https://www.apache.org/licenses/LICENSE-2.0.

所有其他材料遵循 Creative Commons Attribution 4.0 International (CC BY 4.0) 许可。许可证副本见：
[https://creativecommons.org/licenses/by/4.0/](https://creativecommons.org/licenses/by/4.0/)。

除非适用法律另有要求或以书面形式明确同意，根据 Apache 2.0 或 CC-BY 4.0 分发的软件与材料均按“原样”分发，不附带任何明示或暗示的保证或条件。有关权限和限制的具体条款请参见相关许可证。

## 免责声明

本项目并非 Google 官方正式支持的产品。

WeatherNext 模型属于实验性科研项目。您须自行承担使用或分发这些模型或其生成输出的所有风险，并自负遵循 Google 相关许可授权的一切法律责任。在依赖、发布、下载或使用这些模型及其任何输出之前，请保持谨慎评估。

WeatherNext 模型未与任何官方政府气象机构或部门合作制作，亦未获得任何此类机构的背书认可，绝不能替代官方气象部门发布的预警、警报或通报。

## 论文引用

若您在研究工作中使用了 WeatherNext 系列模型，请引用对应论文：

<!-- disableFinding(SNIPPET_INVALID_LANGUAGE) -->
**WeatherNext 2 或 WeatherNext Cyclones**

```latex
@article{Alet2026,
  title={Operational Tropical Cyclone Forecasting with AI},
  author={Alet, Ferran and Andersson, Tom R. and Price, Ilan and Markou, Stratis and El-Kadi, Andrew and Masters, Dominic and Li, Amy and Merchant, Samier and Williams, Natalie and Thornton, Gregory and MacKay, Ken and Graham, Olivia and Uddin, Akib and Gaiarin, Ben and Shah, Devaja and Kruse, Elinor and Hogsett, Wallace and Zelinsky, David and Cangialosi, John and Martinez, Jonathan and Franklin, James and DeMaria, Mark and Musgrave, Kate and Bain, Caroline L. and Titley, Helen and Stott, Jacklynn and Lam, Remi and Bell, Aaron and Komarek, Paul and Willson, Matthew and Sanchez-Gonzalez, Alvaro and Battaglia, Peter},
  journal={Nature},
  year={2026},
  issn={1476-4687},
  doi={10.1038/s41586-026-10953-2},
  url={https://doi.org/10.1038/s41586-026-10953-2}
}
```

```latex
@article{alet2025skillful,
  title={Skillful joint probabilistic weather forecasting from marginals},
  author={Alet, Ferran and Price, Ilan and El-Kadi, Andrew and Masters, Dominic and Markou, Stratis and Andersson, Tom R and Stott, Jacklynn and Lam, Remi and Willson, Matthew and Sanchez-Gonzalez, Alvaro and Battaglia, Peter},
  journal={arXiv preprint arXiv:2506.10772},
  year={2025}
}
```

**WeatherNext 1 Gen (GenCast)**

```latex
@article{price2024gencast,
  title={Probabilistic weather forecasting with machine learning},
  author={Price, Ilan and Sanchez-Gonzalez, Alvaro and Alet, Ferran and Andersson, Tom R and El-Kadi, Andrew and Masters, Dominic and Ewalds, Timo and Stott, Jacklynn and Mohamed, Shakir and Battaglia, Peter and Lam, Remi and Willson, Matthew},
  journal={Nature},
  volume={637},
  number={8044},
  year={2024},
  publisher={Nature Publishing Group},
  doi={10.1038/s41586-024-08252-9}
}
```

**WeatherNext 1 Graph (GraphCast)**

```latex
@article{lam2023learning,
  title={Learning skillful medium-range global weather forecasting},
  author={Lam, Remi and Sanchez-Gonzalez, Alvaro and Willson, Matthew and Wirnsberger, Peter and Fortunato, Meire and Alet, Ferran and Ravuri, Suman and Ewalds, Timo and Eaton-Rosen, Zach and Hu, Weihua and others},
  journal={Science},
  volume={382},
  number={6677},
  pages={1416--1421},
  year={2023},
  publisher={American Association for the Advancement of Science}
}
```

## 致谢

WeatherNext 模型与以下独立外部数据源及库进行交互：

*   欧洲中期天气预报中心（ECMWF）的数据与衍生产品（经 Google 适配处理）。
*   修改后的 2023 哥白尼气候变化服务（Copernicus Climate Change Service）数据。
*   NOAA 的国际气候管理最佳路径档案（IBTrACS）数据（首次访问于 2022 年 12 月 1 日）。

此外，Colab Notebook 中包含少量来自 ECMWF 的 ERA5 与 HRES 示例数据，用于作为模型的输入示例。

欧盟委员会和 ECMWF 均不对哥白尼数据或其包含信息的任何使用负责。ECMWF HRES 数据集版权声明：Copyright "© 2023 European Centre for Medium-Range Weather Forecasts (ECMWF)"。来源：[www.ecmwf.int](http://www.ecmwf.int/)。许可证声明：ECMWF 开放数据遵循知识共享署名 4.0 国际许可（CC BY 4.0）发布：
[https://creativecommons.org/licenses/by/4.0/](https://creativecommons.org/licenses/by/4.0/)。
免责声明：ECMWF 对数据中的任何错误或遗漏、其可用性或因使用该数据而引起的任何损失或损害不承担任何责任。

上述第三方材料的使用可能受独立的条款及条件约束。使用第三方材料须遵守任何此类条款，且您应在实际使用前确认符合所有适用的限制要求。

## 联系方式

关于代码库或模型的反馈与问题，请通过 `weathernext@google.com` 与我们联系。

通过电子邮件收集的任何信息均将根据 [Google 隐私权政策](http://policies.google.com/privacy) 进行妥善处理。

---
*注：本文档由社区志愿者维护（贡献者：[@JasonYeYuhe](https://github.com/JasonYeYuhe)），内容与英文官方文档保持同步（更新时间：2026年10月12日）。如有任何疑问或更新建议，欢迎提交 PR 或 Issue。*
