---
title: Mirrortime:4D高斯训练
date: 2025-12-18 10:48:32
categories: [Gaussian Splatting] 
tags: [ Python, Gaussian Splatting]
layout: post
description: 高斯泼溅
# 图片放在 source/images/ 目录下
# cover: /images/gs_seq_3dgs.png
---
## 🌟 项目简介
Mirrortime:4D高斯训练使用多视角序列帧图像训练时序高斯泼溅模型。
<!-- more -->

- 输入：序列帧图像，相机参数（colmap）
- 过程：
    + 使用生命周期函数控制高斯透明度
    + 根据多视角一致的重要性稠密化和剪枝
- 输出：带有lifetime参数的ply文件

## 🌟 实现
---
### **lifetime**
*   **优化变量**:
    *   `_lifetime_mu`: The temporal center (when the point is most active).
    *   `_lifetime_w`: The temporal half-width (duration/2).
    *   `_lifetime_k`: The temporal sharpness (controls how "hard" the appearance/disappearance is).
*   **双Sigmoid函数**: 使用双Sigmoid函数构建盒函数
    $$ \text{Lifetime}(t) = \sigma(k \cdot (t - (\mu - w))) \times \sigma(-k \cdot (t - (\mu + w))) $$
    $$ \text{Opacity}(t) = \text{Base Opacity} \times \text{Lifetime}(t) $$

### **FastGS**
*   **多视角一致性稠密化 (Multi-view Consistent Densification, VCD)**:
    * 不只看单张图的梯度，而是根据多视角下重建误差的一致性来判断是否需要加密高斯点。如果一个区域在多个视角下都有很大误差，才进行分裂。
*   **多视角一致性剪枝 (Multi-view Consistent Pruning, VCP)**:
    * 同样基于多视角一致性来删除冗余的高斯点，而不是仅仅依靠不透明度（opacity）阈值。




## 🌟 成果演示
![训练监控](/images/mouse_dash.png)
![训练监控](/images/mouse_count.png)
- r=4
- 30帧数据
- 训练时间：37min
- 整个场景点数：57万
- 裁减有效区域后点数：33万
- 运动区域点数：15万
- 运动区域重要性buffer*5+5

参考文献：
===
> FastGS: Training 3D Gaussian Splatting in 100 Seconds
> FreeTimeGS: Free Gaussian Primitives at Anytime Anywhere for Dynamic Scene Reconstruction