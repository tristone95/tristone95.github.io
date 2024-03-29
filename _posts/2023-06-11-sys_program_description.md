---
layout: post
title:  "石英砂检测分析项目说明"
date:   2023-06-11 12:00:00 +0800
categories: 项目说明
tags: SysProgram ProgramDescription 
excerpt_separator: <!--more-->
---
一个C++ Qt项目的项目介绍...
<!--more-->
- [项目背景](#项目背景)
- [需求分析](#需求分析)
- [项目介绍](#项目介绍)
- [项目展示](#项目展示)
- [部分功能介绍](#部分功能介绍)
  - [任务信息](#任务信息)
  - [结果查看](#结果查看)
- [项目源码及release](#项目源码及release)

#### 项目背景
&emsp;&emsp;原项目为某油气研究院项目，旨在通过图像识别与处理技术替代现有的繁琐且漫长的石英砂颗粒填充性能检测。鄙人在此项目中担任产品助理一职，因一些原因，全程参与到此项目，从立项到开发到测试到交付，故此对此项目较为熟悉。因此作为学习C++开发后的首个实战项目以及实现对原项目的部分改进。
#### 需求分析
&emsp;&emsp;原项目分软、硬件两部分，大致需求为本软件控制测试仪(主要受控硬件有工业相机、震动送料器、天平及主机)，通过高帧率工业相机对下落石英砂颗粒拍照，通过图像识与处理对颗粒特性参数进行计算，最后统计分析形成报告。因为现开发环境缺少硬件测试，故未实现与工业相机、震动送料器及天平的对接控制，通过GetImages类从硬盘读取图片模拟拍摄过程，然后进行图像处理与计算，统计分析形成报告。
#### 项目介绍
&emsp;&emsp;本项目设计为C/S架构，更多的是出于开发实战，实现基于tcp的通信与协议设计，通过Tcp通信的自定义协议实现客户端对服务器端的控制消息以及处理结果的传递。此外用到了多线程技术、基于OpenCV库的图像处理，实现了基于pdfium的PDF查看器、实现了分页查看。
#### 项目展示
<iframe src="//player.bilibili.com/player.html?bvid=BV1Qc411u7gB&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
(PS：无UI设计故未处理UI,界面较丑勿怪)
#### 部分功能介绍
##### 任务信息
![任务信息页](https://cdn.jsdelivr.net/gh/tristone95/imgs/2023/sys_1.png)
- 测量信息为必填项
- 密度分体密度与视密度，根据对应体积、质量测量仪器测量填写，软件按公式计算得出体密度视密度
- 测量停止方式分为手动停止和照片数量两种，手动停止则在测量过程中有停止按钮供用户主动停止；指定照片数量则在采集指定数量后方才停止，在此过程中将按   ```已处理图片数÷指定照片数量```显示进度。
  ![手动停止](https://cdn.jsdelivr.net/gh/tristone95/imgs/2023/sys_2.png)![指定照片数量](https://cdn.jsdelivr.net/gh/tristone95/imgs/2023/sys_3.png)
##### 结果查看
![结果查看](https://cdn.jsdelivr.net/gh/tristone95/imgs/2023/sys_4.png)
- 包括任务基本信息、任务关键指标结果以及颗粒查看区
- 时间选择实现按秒的分页查看
  ![时间选择](https://cdn.jsdelivr.net/gh/tristone95/imgs/2023/sys_5_1.png)
- 颗粒查看区显示颗粒图片及关键指标圆度粒度球度信息。点击对应颗粒可查看该颗粒所有检测指标详细信息。
  ![颗粒查看区](https://cdn.jsdelivr.net/gh/tristone95/imgs/2023/sys_4_1.png) ![颗粒信息](https://cdn.jsdelivr.net/gh/tristone95/imgs/2023/sys_6.png)
- 点击报告查看则弹出PDF报告预览界面，可滚轮上下滚动查看，Ctrl+滚轮放大缩小，鼠标点击可拖动。点击报仇可保存PDF至客户端目录下。
  ![PDF查看](https://cdn.jsdelivr.net/gh/tristone95/imgs/2023/sys_7.png)
- 点击重新检测将初始化数据，跳转开始任务界面准备开始新任务。
#### 项目源码及release
- [GitHub仓库：https://github.com/tristone95/sysproject](https://github.com/tristone95/sysproject?_blank "Github仓库")
- [GiTee&ensp;仓库：https://gitee.com/XRay1995/sysproject](https://gitee.com/XRay1995/sysproject?_blank "Github仓库")
