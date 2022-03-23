# Java 应用通过日志分析跟踪异常指标

通常在业务中，我们记录的日志最多的一类应该是异常日志了，你有没有想过通过一个看板来监控各个业务模块的各类异常发生的频率呢？本节通过自定义日志分析规则，结合运维大盘 + 自定义告警，我们来演示如何基于日志分析，实现对业务异常数据的持续监控和告警。

## 明确需求

首先我们再明确下需求：
- 通过自定义大盘展示近 1 小时内各类异常发生次数的时序图
- 5 分钟内异常发生次数超过 10 则发送邮件报警通知

## 前提条件
- 本示例中以 java 应用为例
- 集成了 Erda 平台提供的 Java agent（使用流水线从源码构建打包部署时，会自动注入 Java agent）

## 创建日志分析规则

第一步我们需要创建一个日志分析规则，从日志数据中提取出异常类型信息，并转换为指标数据。

我们期望转换生成的指标数据能包含如下字段：

- timestamp：异常发生时间
- type：异常类型，如 java.lang.NullPointerException

其中，timestamp 字段默认会自动从日志的 timestamp 转换过来，并不需要用户再次从日志中提取，因此，我们仅需要从日志中提取出异常类型的内容作为指标的字段即可。 参考 [日志分析规则](../../guides/log/rules.md) 的指导，我们创建如下图所示分析规则：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/02/24/0256c3a3-e9a4-4bec-a7cc-b29bcf8c04d1.png)

- 指标名称：命名为 exception，表示转换成的指标就叫做这个命名，中文英文都可以，之后在创建运维大盘和创建告警规则时我们都将需要通过这个名字找到对应的指标
- 日志筛选：我们设置了项目名称和环境两个筛选条件，表示我们只关心 `log-demo` 项目的 `prod` 环境产生的异常日志
- 指标提取表达式：我们设置为 `([^\s]+Exception):`，如下图所示对一条异常日志的示例可以看出，这个表达式可以匹配出异常类型的部分（当然，这个表达式约束条件比较少，可能会匹配到非预期的内容，但这里作为示例，我们不过分追求准确性）

![](https://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2021/08/23/f98d1d2e-d597-4270-8d09-e95a3a793f77.png)

- 提取结果：我们将提取的指标命名为 `type`，给指标取个更容易理解的别名 `异常类型`

填好后，点击页面最下面的确定按钮，完成日志分析规则的创建。

## 创建运维大盘

第二步我们前往 **诊断分析 >  自定义大盘**，创建一个大盘，这个大盘只需要包含一个能按照时间纬度分别展示各种类型异常发生的次数的时序图表。

按如下图所示操作进入大盘图表编辑页面：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/02/24/379a75cc-24b7-4b62-b354-11bc8226babc.png)

然后配置图表：

![](https://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2021/08/12/f5920f9f-eed5-4fea-b2e6-7f7e0fe3ebf9.gif)

我们对设置稍作说明：

![](https://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2021/08/12/1afb50ed-5dd1-46e1-8b2c-bdf9a34cad88.png)

- ①：我们想按照时间展示每一时刻异常发生的次数，因此选择折线图
- ②：选择我们刚才创建的日志分析规则指标 `exception` （日志分析规则转换而来的指标都会放在 `Log Metrics` 分组下）
- ③：我们按时间轴为主 X 轴
- ④：为了分别展示每种异常的数量，我们增加 `异常类型` 字段，这个是我们创建分析规则时定义的 `type` 字段的别名
- ⑤：我们想统计异常发生的次数，所以其实我们只要选择指标的任意一个字段做 count 计算，这里我们选择 `异常类型`
- ⑥：因为我们关心的是发生次数，所以需要在字段上做 count 运算，操作方法是：点击 `异常类型` 标签，在弹出菜单中点击 `计数值`
- ⑦：给图表命个名
- ⑧：点击 `确认` 保存图表

确认后回到大盘页面，我们可以继续调整图表位置和大小，最后保存大盘，如下图所示：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/02/24/57a7b8ed-d57d-425d-b275-2adde6594ba4.png)

## 配置告警

### 创建通知组
如果没有可用的通知组，需要先到 **DevOps平台 > 项目 > 项目设置 > 通知管理** 页面创建通知组，如下图所示：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/02/24/965f3e82-b902-4839-ab1e-10a881154a53.png)

需要说明的是，由于我们的需求是通知到邮箱，因此 `通知到` 这里选择了 `成员` 的方式，你也可以选择其他方式，如 `钉钉地址`。

### 创建自定义告警规则
回到 **微服务治理平台 > 项目列表 > 选择项目 > 告警管理 > 规则管理** 页面，点击页面右上角 **创建自定义规则**。

首先设置触发规则定义部分，如下图所示：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/02/24/d553243c-fe20-474a-a855-bde3dcd480c0.png)

- ①：名称，可以设置为任意能让你容易理解规则含义的标题，这里设置为 `5分钟异常发生超10次`
- ②：周期，单位是分钟，我们设置为 5
- ③：指标，选择我们刚才创建的日志分析规则指标 `exception`
- ④：过滤规则，这里我们不填写
- ⑤：分组规则，我们想针对每个应用分别统计告警，因此选择 `应用ID`
- ⑥：字段规则，相当于设置条件，如图所示的设置相当于条件 `count(type) >= 10` 同时我们给这个 `count(type)` 设置了个别名 `ex_count`，以便我们在发送告警消息时引用这个值

然后，设置下方的 `消息模版定义` 部分：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/02/24/47ea6ebf-a737-4d4d-ab0e-926ed54f23d2.png)

- ①：通知方式，根据我们的需求，这里选择邮箱
- ②：消息标题， `【{{application_name}}项目异常告警】`
- ③：消息内容，支持引用指标中的字段变量
```text
项目: {{project_name}}

应用: {{application_name}}

服务: {{runtime_name}} - {{service_name}}

事件: {{window}}分钟内异常发生次数{{ex_count}}

时间: {{timestamp}}
```

最后点击确定，完成自定义告警规则的创建。

### 创建告警策略
完成自定义告警规则后，我们还需要再创建告警策略：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/02/24/a39fa8f9-86e5-46ee-a287-e0fa648396b1.png)

- 告警名称：管理告警策略时方便识别的命名，设置为 `异常次数告警`
- 告警规则：选择我们刚才创建的告警规则 `5分钟异常发生超10次`
- 沉默周期：设置为 5 分钟内不再重复告警
- 通知对象：选择我们刚才创建的群组 `异常告警通知组`
- 通知方式：根据我们的需求，选择 `邮箱`

到这里我们就完成了整个告警的配置，尝试触发一些异常，即可收到告警通知。