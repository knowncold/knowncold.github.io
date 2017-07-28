---
title: SLAM-papers
layout: page
category: wiki
---

## Past, Present, and Future of Simultaneous Localization And Mapping Towards the Robust-Perception Age

SLAM到目前主要有两个阶段
- 1986-2004，经典时代
主要在于EKF、RaoBlackwellised Particle Filters还有最大似然
- 2004-2015，算法分析时代
这个阶段主要关注SLAM的基础属性，大部分的开源库也是在这个时候建立的

越接近当下，每年的热点越是SLAM的子领域，而不是整体的SLAM研究。

这文章关注两个问题：
- 自主机器人需要SLAM吗
- SLAM是以一个学术研究问题而被解决的吗

### SLAM独特在哪？
通过自主运动行为的测量和回环检测，对环境建图；没有了回环检测的话，就只能被称为里程计，通过一些标记和图像识别出初始位置来消除累积的差错。
