+++
date = '2026-09-06T13:54:14+08:00'
draft = true
title = 'Autopilot'
+++

## 学习飞控三步走

1. 学习并实现px4 autopilot飞控。
2. 自制自己的飞控。
3. 实现商业化。

## 为何称为三步走

现在，最能耳熟能详的三步走就是嫦娥工程的探月计划。2004年，我国制定了“绕、落、回”三步走的发展路径。从计划指定，到2007年嫦娥一号发送到200公里环月轨道开始，直到2020年嫦娥五号带回2kg的月球样本。整整17年其中艰辛不言而喻。也正是如此奋发拼搏的精神才能鼓舞人心。所以选择这样的方式来激励自己。

## 1. 学习并实现px4 autopilot飞控

- [ ] 了解学习px4 autopilot。
- [ ] 下载px4 autopilot源码，并尝试运行起来。
- [ ] 尝试总结px4 autopilot的学习经验，并自制一个简单的飞控。

### [APM与PX4源码架构的区别，如何选择自驾仪？？？](https://www.bilibili.com/video/BV1oA9cYMEfL/)

- APM和PX4都是开源飞控。
- APM基于多线程架构（C++），使用HAL屏蔽底层硬件差异。代码耦合度高。
- PX4基于多线程/多进程架构，任务通过UORB通讯，模块化设计，中间件实现硬件驱动和算法分离。
- APM支持诸多硬件。
- PX4仅支持pixhawk系列。
- APM算法成熟，任务模块丰富。
- PX4 VTol算法优化更好，这模块算法独立。仿真支持更好。
- APM社区文档丰富。
- PX4代码结构更现代，比较复杂。
- 想做算法选APM。
- 多旋翼、固定翼等选PX4。

### [【2021 ROS暑期学校】基于PX4与ROS的全自主无人机开源平台 & EGO-Planner复现](https://www.bilibili.com/video/BV1RP4y1W7jY/)

- 开箱即用的仿真平台
- 链接：<https://github.com/amov-lab/Prometheus>
- 有空可以了解一下。

### [PX4飞控硬件设计理念，如何设计自己的飞控？ 调试遇到的问题及注意事项。](https://www.bilibili.com/video/BV1bvh8zfEgH/)

- 定制自己的PX4兼容飞控

### AI

- PX4是软件，Pixhawk是硬件。PX4可以运行在Pixhawk的硬件上，但两者不是绑定的，它们都可以独立的搭配其他组件。