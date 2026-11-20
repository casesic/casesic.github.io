---
permalink: /
title: "余悦"
author_profile: true
redirect_from:
  - /about/
  - /about.html
---

我是**余悦**，天津大学 2026 级硕士研究生，在 **TJU3DV** 实验室。我的研究兴趣是**具身智能中的灵巧手操作**和 **3D 人体**：既关心手怎么稳定地抓住东西，也关心人怎么被准确地重建出来、并且在不同视角之间保持身份一致。

本科同样在天津大学读**计算机科学与技术**（2022–2026），毕设做的是**双相机鱼眼全景下的人体跨相机拼接** —— 要把 A 相机拍到的人，可信地放进 B 相机的画面里。这件事往下拆，就变成了四个仓库：先把合成数据产出来，再训跨相机语义点对应，然后做定量评价，最后落到真实鱼眼图上的合图与实时推理。我喜欢把一条链路从数据一直打通到实时性能，而不是只做其中一段。

**Research interests**：embodied dexterous manipulation · 3D human capture · synthetic data for human-centric vision · multi-camera correspondence · real-time inference

---

## 🎓 Education

| 时间 | 学校 | 专业 / 学位 |
|---|---|---|
| 2026.09 – 2029.06 | 天津大学 | 电子信息 · 硕士研究生（TJU3DV 实验室） |
| 2022.09 – 2026.06 | 天津大学 | 计算机科学与技术 · 工学学士 |

## 🧪 Research

**具身智能 · 灵巧手**
研究灵巧手在真实与仿真环境中的操作：手物接触、抓取稳定性，以及从人类演示到机器人手的动作迁移。关注点是把「人怎么做」变成「手怎么学」。

**3D 人体与跨相机对应**
研究多视角鱼眼/ERP 全景下的人体捕捉：从人体重建投影，到直接预测二维语义点，再到多相机之间的点对应与合图。核心问题是**遮挡、边缘和视角变化下的点对应可靠性**。

## 📄 Projects {: #projects}

> 四个仓库是同一条链路的四段。数字都来自各仓库的文档，口径与限制写在仓库里。

### 1 · 合成人体数据生成器

[`human_data_generator`](https://github.com/casesic/human_data_generator)

用已有的人体、动作、衣物和发型资产，在 Blender 里程序化生成走路、骑行、多人交会等场景。每个相机输出 RGB、目标人物 mask、有序 512 点坐标与可见性三态（可见 / 遮挡 / 未知）；框由 mask 派生，不额外标注。

- **规模**：35 个完整场景（原城市 20 + 遮挡 15），按 28 个训练 / 7 个验证划分
- **技术栈**：Python · Blender (bpy) · NumPy · OpenEXR · Blender 程序化渲染

### 2 · 跨相机语义点对应

[`densekp_retrain`](https://github.com/casesic/densekp_retrain)

针对指定人体框回归 **138 个身体语义点**，服务跨相机同一人体的点对应。仓库覆盖数据准备、Dataset、训练、推理与评价；多人遮挡、框中心/尺度变化、截断与鱼眼展开畸变都在研究范围内。

- **技术栈**：PyTorch · PyTorch Lightning · Hydra · Detectron2 系 backbone
- **一句话**：这条路线后来被 MAMMA 取代，现在保留为历史对照 —— 但遮挡与框变化鲁棒性的结论仍然有效

### 3 · 微调与定量评价

[`mamma_retrain`](https://github.com/casesic/mamma_retrain)

MAMMA（CVPR 2026 Oral）的 fork。我在上游推理链之上加了微调 / 重训流程、可见性阈值研究和数据适配，并用统一验证集做定量评价。

| 指标（7 场景 / 814 条观察） | 原模型（阈值 0.5） | 微调后（阈值 0.85） |
|---|---:|---:|
| ERP 点均误差 | 4.3954 px | **2.5242 px** |
| 可见召回 | 82.64% | **91.33%** |
| 遮挡召回 | 95.73% | 94.76% |

点均误差下降约 **42.6%**。**三条限制必须一起读**：0.85 这个阈值是在同一份验证集上看过之后选的（同集选择，不是独立验证）；这不是独立测试；真实图片没有人工点位真值，人工判断是整体画面意见，不是逐帧准确率。

- **技术栈**：PyTorch · CUDA · 多相机 SMPL-X / 2D 点位模型微调

### 4 · 鱼眼全景合图与实时化

[`Panorama-Stitch-Lab`](https://github.com/casesic/Panorama-Stitch-Lab)

双相机鱼眼 / ERP 全景下的人体捕捉与跨相机合图实验工程（本科毕设）。链路从原始鱼眼视频走到「把 A 相机里的人搬进 B 相机画面」，中途把最贵的 SMPL-X 拟合用 C++ / CUDA 重写成可自测、可基准的 staged 求解器。

- **实时化**：C++ staged-LM 单帧拟合 **5 FPS → 25–30 FPS**（RTX 3090；点数与命令当时未记录）
- **规模**：约 72k 行 Python + 约 10.7k 行 C++/CUDA
- **技术栈**：Python · C++ / CUDA · Eigen · TensorRT · 解析 Jacobian + Schur 补

## 🏆 Awards

| 时间 | 奖项 |
|---|---|
| 2024 – 2026 | 国家级大学生创新创业训练计划 —— **可信投影的非结构化大场景光场图像缝合** |
| 2025 | 全国大学生集成电路创新创业大赛 · 华北分赛区 **二等奖** |
| 2022 – 2023 | 天津大学 **三好学生** |

## 🌿 Beyond

头像是一只躺着玩手机的猫。

## 📫 Contact

- **Email**：[2083733118@qq.com](mailto:2083733118@qq.com)
- **GitHub**：[@casesic](https://github.com/casesic)
- 天津，中国
