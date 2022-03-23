# 告警中心

进入微服务平台，选择项目后进入告警中心，在告警中心中包含告警总览、告警列表、告警配置，在告警中心用户可以
直观看到当前系统的告警统计和通知历史。

## 告警总览

告警中心的首页为告警总览页，在该页面展示用户选择的时间段内的告警和通知的统计分析数据，如图所示：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/03/01/a5c59a3d-d4d4-47a0-88c0-fba6ac0fe02a.png)

在告警总览页中用户可以看到默认一小时内的未恢复告警数、告警事件、通知成功与失败数量等。
* 告警事件<br>
  该字段表示在选定时间内触发的告警时间的数量，即达到用户创建在告警策略中所设置的告警规则阈值的次数。
* 恢复事件<br>
  该字段表示所触发的告警事件恢复的次数
* 事件降噪<br>
  该字段表示多条告警合并成一条或者将告警根据级别合并的次数
  
* 事件静默<br>
  在用户选择的时间段内告警事件的静默次数

* 通知发送成功<br>
  该字段表示触发的告警事件发送到对应的通知组中的成功次数
* 通知发送失败<br>
  该字段表示触发的告警事件发送都对应的通知组中的失败次数
 
在告警总览页中的告警事件有两张曲线图，分别是按级别与按类别
* 按级别<br>
  级别包含故障、严重、警告、提示，每一条曲线代表一种告警级别
  
* 按类别<br>
  类别表示对应的类型模版，包含浏览器性能、应用错误、应用资源、微服务自定义、主动监控、微服务、应用事务。每一条曲线代表一个类别。
  
告警持续事件分析气泡图展示了对应时间段内告警平均持续时间的触发的告警事件次数。如图所示：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/03/01/0c84c411-4786-4896-9ee3-d88201c2f3b2.png)

## 告警列表
在告警列表页中分为事件和通知两个tab页分别代表通过告警策略触发的告警事件和发送的告警通知

* 事件<br>
在事件tab页中用户可以通过左上角的筛选器和名称搜索框来筛选告警事件。如图所示：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/03/01/53e50bb9-4a9c-4290-9895-5dc15b9afb3e.png)

在事件列表中展示了告警事件的名称、所属策略、事件触发次数、告警级别、状态、事件来源和最后触发时间。用户在该页面可以清楚的看到告警策略的触发情况。如图所示：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/03/01/0853caf1-99be-4ce7-a60b-8a998ba85617.png)

用户可以通过点击事件列表中任意一条记录知道该事件告警的详情，在事件详情中用户可以直观的看到当前告警事件的名称和状态(告警/恢复/屏蔽)，该页中的事件概览详细展示了该告警事件绑定的告警规则，
告警策略，级别等。在监控数据中可以看到对应告警策略中告警规则的指标数据对应时间段的平均值曲线图。如图所示：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/03/04/52e02ac4-a3e9-4439-9a23-6d119f22b636.png)
在该页面的事件历史中用户可以直观的看到该告警事件的历史记录，触发值字段展示了用户配置的告警规则指标的阈值和该告警事件产生时该指标的值，持续事件为该告警事件触发的持续事件
当告警恢复时该持续事件会被置为0，在指标这个字段中可以看到该告警事件触发时相关tag的值。如图所示：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/03/07/6c73a166-7ce3-492c-887b-aa0ef9a6a8d3.png)

在该页面中的右上角的更多操作的下拉框中用户可以根据需要选择暂停告警和屏蔽告警

暂停告警：选择暂停告警之后会根据事件概览中的关联对象的键值对进行判断，当符合关联对象中的所有键值对后该告警将不会被触发。
在选择暂停告警后会要求用户选择暂停的截止时间，在时间到后会自动取消暂停告警，用户也可以在更多操作中选择取消暂停。

告警屏蔽：选择告警屏蔽之后会根据关联对象中的键值对进行判断，当符合关联对象所有的键值对后该事件的告警还是会触发，但是用户将
不会再收到该告警的通知，在选择屏蔽告警时不会要求用户选择暂停屏蔽的时间，当用户想要停止告警屏蔽时想要手动在更多操作中选择取消屏蔽。

* 通知<br>
在通知的tab页中用户可以通过左上角的筛选器和名称搜索框筛选通知。如图所示：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/03/01/82f4d2ec-20b1-4786-ac34-5f4bf1744cec.png)

通知列表中可以看到发送通知的名称、状态、通知渠道、关联策略、发送事件，可以根据发送时间进行排序。如图所示：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/03/01/4ec92569-49da-4665-b199-526984834bf9.png)

点击通知列表中的任意一条通知可以进到该通知的通知详情页，在详情页中包含了告警策略名称、通知方式、发送状态、发送时间、关联通知组、关联告警规则和通知的详细内容。如图所示：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/03/01/bb08ca0f-0e6d-402c-9b19-59e3b8952c52.png)

## 告警配置
在告警配置页中包含了告警策略，规则管理，通知组管理。

* 告警策略<br>
在告警策略tab页中是对告警策略的增删改查操作，在原先告警策略展示列表的基础上用户可以根据告警策略的名称进行搜索，对于告警策略的启用和停用在状态那一栏进行修改。具体操作如下：
1.用户需要在通知组管理的tab中创建通知组
2.点击创建告警策略

  2.1 用户可以根据需要添加一条或者多条过滤规则<br>
  2.2 用户可以在类型模版中选择需要的告警类型，添加该类型的所有告警规则，也可以通过点击添加规则，在规则名称的下拉框中选择需要的告警规则进行逐条添加<br>
      * 持续周期<br>
      用户所选择的的时间段中所触发的告警，单位分钟<br>
      * 告警级别<br>
      用户可以判断告警的严重程度选择告警级别，每条规则会带有默认的告警级别，用户也可以设置为故障、严重、警告、提示，级别逐级递减<br>
      * 触发恢复<br>
      用户可以根据需要选择是否在对应的告警规则恢复时发送告警恢复的通知。部分规则和自定义规则不支持触发恢复<br>
  2.3 选择沉默周期，该字段表示下一次发送该告警策略对应的通知与上一次的时间间隔<br>
  2.4 选择沉默周期策略，默认为固定，表示每次相隔的时间间隔为用户选择的沉默周期，翻倍表示下一次的触发时间间隔为前一次的时间间隔的双倍<br>
  2.5 选择通知到的对象，该群组就是用户事先创建好的通知组，根据对应的通知组通知到的不同，可以选择一种或者多种的通知方式，
  当只有通知组中的级别包含告警规则的级别时，该告警通知才能发送到该通知组中，用户也可以添加多个不同的通知组<br>
  
具体告警策略的新建如图所示：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/03/07/9e1626a6-b833-4139-af5c-437245929fca.png)

* 规则管理<br>
监控系统采集的指标多而广，有系统自带的各类监控指标，也有用户通过Monitor SDK采集的自定义指标，用户若是希望针对某些特定的指标定义告警规则或是
自定义告警通知的消息模版可在规则管理中进行配置。
* 名称：规则名称，用户可自定义
* 周期：对指标聚合产生结果的时间
* 过滤规则：对指标进行筛选，仅针对特定指标进行筛选计算
* 分组规则：对字段进行分组，各组分别按照告警规则进行计算
* 字段规则：可选择多个字段，将计算结果与定义的阈值进行判断，若符合条件则触发告警

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/03/07/538b1266-edf6-48e6-a35d-d2be177d9352.png)
* 通知方式：可选择钉钉、邮箱、站内信、短信等多种通知方式
* 消息标题：用户自定义告警发送的消息标题模版
* 消息内容：用户自定义告警发送的消息内容模版

在消息标题和消息内容中可用花括号引用对应指标的字段，例如{{project_name}},可引用的字段包括project_name、application_name、runtime_name、service_name、host_ip、cluster_name 等。
![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/03/07/d9fecbe1-a571-429d-bf27-aca0e6413011.png)

完成自定义告警配置，即定义了一个告警规则的模版，用户可以在告警策略中选择已定义的告警规则模版，关联通知组等，进一步配置具体信息，完成配置之后
若对应指标满足配置条件则可以触发告警，根据自定义的消息内容模版发出对应的告警通知。

* 通知组管理<br>
在创建告警策略时用户需要先创建接受告警通知的通知组，可以根据自身需要选择通知到不同的对象，根据通知到的对象的不同需要填写对应的url或者选择对应的角色
可选择的通知到的有成员、钉钉地址、外部API、外部用户、成员角色。如图所示：

![](http://terminus-paas.oss-cn-hangzhou.aliyuncs.com/paas-doc/2022/03/07/a7748976-ed8c-4465-8869-f58693ef7abd.png)

用户创建好通知组后可在告警策略中引用该通知组，当触发告警时，该通知组会收到对应告警策略发送的告警通知消息。