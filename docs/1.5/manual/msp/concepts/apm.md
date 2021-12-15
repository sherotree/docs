# 应用监控

## 技术架构

![](https://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2021/08/13/425dde44-b6d3-42b0-a528-89aba67ef565.png)

### 产品架构
系统针对基础设施、后端的业务应用、前端页面和 App 应用等多方面进行监控，全栈覆盖，并提供多种监控功能。

### 数据探针
* TA.js，负责采集前端的监控数据，可通过平台提供的嵌入代码集成至页面。
* Java Agent、Node.js 等后端数据探针，将在应用打包时自动集成至容器，实现代码无入侵式集成。
* Monitor SDK，以 SDK 的方式，提供业务应用自定义性能指标的能力。
* Telegraf、Filebeat 等基础监控的 Agent，作为核心组件部分集成至 Erda 平台。

### 底层框架
Erda 提供通用的底层能力，包括通用的监控查询语言、数据分析表达式等，以支撑产品层的多种功能。

## 核心概念

### APM

Application Performance Management，即应用性能管理，用于实时监控企业的应用系统。Erda 集成了 APM 功能，以代码零入侵、自动接入的方式，提供针对企业应用实时监控的解决方案。

您可进入 **应用 > 部署中心** ，点击 **应用监控** 插件。

![](https://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2021/08/23/b1386723-088b-4097-9a3e-359d780f1b9c.png)

即可进入 APM 相关页面。

![](https://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2021/08/23/4a836763-cfd8-4891-b129-a37d491437f1.png)

### Trace

分布式链路追踪，用于追踪并记录整个系统中请求所经过的逻辑节点以及相关的性能指标，此处的逻辑节点称为 Span。

一条 Trace 代表针对一次请求的记录。一条 Trace 包含多个逻辑节点 Span，每个 Trace 和 Span 均有相关性能指标记录。

出于对业务系统影响的考虑，平台并不会追踪每一次请求，而是通过一定的采样率，对部分请求进行追踪记录。

### Metric

通过一组数据反应系统性能，该数据称为 Metric，即性能指标。

* **Counter**：单调递增的指标，例如网络流量，随着时间推进，网络流量将累计增加。

* **Gauge**：数值上下波动的指标，例如 CPU 使用率，将根据业务应用的运行情况而变化。

通过对一组数据的聚合计算，可发现系统的异常情况，从而发出告警，也可通过图标展示反应系统的性能趋势。

### Log

日志，指应用程序为反应运行情况而输出的文本信息。

日志可与 Trace、Exception 关联，表示一次请求所打印的全部日志。

### Exception

异常，指程序运行过程中发生的、可打断程序正常执行的事件。业务逻辑未处理的异常将由 APM 系统记录。

异常也可与请求关联，表示该异常由某个请求导致。

### 应用拓扑

将系统中的每个服务、中间件作为一个节点，从而绘制出服务之间调用关系的拓扑结构图，可反应整个系统的结构、流量走向以及各节点的性能情况。

### 主动监控

根据用户配置信息，主动发起对某个目标的定时巡检，从而记录该目标的异常及性能情况。

### 浏览监控

即页面监控、前端监控，指针对业务系统 Web 页面的性能分析，可反应页面的加载性能、PV 等情况。

### App 性能

指针对移动端 App 的性能监控，可反应 App 中各界面的打开性能、次数等情况。