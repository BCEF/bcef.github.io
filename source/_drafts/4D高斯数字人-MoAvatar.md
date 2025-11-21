---
title: 4D高斯数字人-MoAvatar
date: 2025-11-20 16:14:32
categories: [Gaussian Splatting] 
tags: [ Python, Gaussian Splatting]
layout: post
description: 高斯泼溅
# 图片放在 source/images/ 目录下
cover: /images/moavatar.png
---

## 🌟 项目概述与目标

MoAvatar使用多视角序列帧图像和变形图训练可驱动高斯泼溅模型。

<!-- more -->
![4D高斯序列](/images/moavatar.png)


- 输入：序列帧图像，相机参数（colmap），t0到t时刻的变形图
- 过程：
- 输出：onnx模型

