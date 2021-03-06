# 2020掌酷微服务架构组建计划
---
## 0 背景
* 腾讯微服务平台TSF：公测时间为2019年3月13日至2019年7月30日
* 阿里云微服务引擎MSE：于2020年1月1日开始正式商业化，根据实际使用资源量进行扣费，费用从50.16元/月/节点起不等。

## 1 公司全年业务分析+算一笔账
![表格](https://imse-1255691551.cos.ap-shanghai.myqcloud.com/server.jpg)
>
> 某年1月，二部启动电商业务，三部启动转发APP业务，五部启动公众号业务。
>
> 2月，项目上线初期，每个部门各自拥有1台业务服务器，二部运行电商分销系统（微擎），三部运行自研后端管理系统（TP框架），五部运行公众号管理系统（微擎），服务器利用率20%。
>
> 3月，由于微信流量红利，各部门流量激增，二部和五部都将服务器增加到3台，三部增加到5台，由于二部用户可分开运营、五部公众号可分开运营，通过增加服务器勉强扛住压力。但三部用户运营是统一管理，且单体架构达到瓶颈，无法扛住压力，造成数据库崩溃一周，30%用户流失，之后流量趋于稳定。
>
> 4月，五部开始大量投放广点通广告，并启动基于公众号流量的小程序业务，短期内100个公众号共新增1000万粉，同时导流到50个小程序。服务器增加到5台，每台管理20个公众号和对应小程序。但是由于公众号的用户量参差不齐，平均利用率60%。
>
> 5月，二部电商分销模式取得裂变，服务器增加到6台，平均利用率60%。
>
> 6月，三部和五部的流量下降30%，但服务器维持不变，平均利用率下降到40%。
>
> 7月，二部流量下降50%，但服务器维持不变，平均利用率下降到30%。
>
> 8月，三部开发创新小程序产品，获得用户增长，取得500万公众号粉，并同时导流到20个小程序。并为此项目开发自研内容中心+公众号+小程序管理系统，新增服务器3台。
>
> 9月，五部和二部也开始运营三部开发的产品，并复制三部的管理系统，分别取得500万公众号粉。各新增3台服务器。
>
> 10月，二部电商分销产品流量缩减，5台服务器停止续费，保留1台
>
> 11月，五部停止运营前期的40个小程序
>
> 12月，五部关闭前期的4台服务器
>
> 假设服务器加配套SLB和数据库单台费用800/月，全年服务器成本为（53+67+58）×800=142400，全年服务器峰值平均利用率47%。
>
> 项目过程中重复造轮子，造成人工成本增加，项目结束后，无技术沉淀。产品和技术带走项目，抱怨成长太慢。
>
> 使用过的主体，空置账号等造成资源浪费，且难以统计。

## 2 关于微服务
### 2.1 微服务架构是什么？
> [详解微服务](https://www.zhihu.com/question/65502802)
#### 2.1.1 微+服务
* 小：微服务体积小，2 pizza团队。
* 独：能够独立的部署和运行。
* 轻：使用轻量级的通信机制和架构。
* 松：为服务之间是松耦合的。

![consul](https://imse-1255691551.cos.ap-shanghai.myqcloud.com/consul.png)

#### 2.1.2 掌酷微服务架构iMSE
1. 针对掌酷的业务模式而设计的分布式、跨平台的微服务架构，提供应用全生命周期管理、数据化运营、立体化监控和服务治理等功能。

![TSF](https://imse-1255691551.cos.ap-shanghai.myqcloud.com/TSF.png)
---
![TSF](https://imse-1255691551.cos.ap-shanghai.myqcloud.com/TSF-1.png)

2. 提供掌酷技术开发团队使用的，可扩展、高可用、可持续交付的技术标准化流程
> [DevOps最佳实践](iMSE-DevOps.md)

### 2.2 为什么要组建微服务架构？
#### 2.2.1 掌酷技术/运维/业务现状
* 评分
  1. 技术：20分
  2. 架构：0分

* 特点
  1. 没有统一的技术选型、架构陈旧、安全性低
  2. 部门间业务独立、分散，缺乏协同管理
  3. 部门内不同业务相互独立，资源利用率低
  4. 业务管理混乱：应用、主体、公众号、小程序、微信号缺乏统一管理
  5. 缺乏高可用、可快速部署、维护和扩展的架构
  6. 无法应对突如其来的高并发场景
  
* 现象
  1. 目前大多后端业务都使用PHP语言开发，但是框架都很陈旧或庞杂，php停留在5.x版本，thinkphp还是3.X版本，微擎框架重且耦合性高，前端技术普通偏弱，没有统一标准，h5和小程序几乎都采用原生开发，复用性低且不利于维护，跨平台技术间无法合作或效率低下。市场上竞争对手已经使用golang等异步协程框架，我们仍停留在php的单线程编程。composer依赖管理、多级分布式缓存、连接池、AOP切面编程、RPC通信、容器编排、服务治理、数据中台等等高性能技术我们接触甚少。
  2. 由于业务耦合，架构单一，应对高并发场景时极易崩溃，且难以排除故障，导致用户、流量流失，造成重大经济损失
  3. 缺乏统一解决方案，面对各部门不同业务时运维一筹莫展，导致各部门都倾向技术兼运维
  4. 公司资源管理混乱，注册账号时找不到合适主体，注册好的账号空缺没有使用，可重复利用资源浪费
  5. 可复用的服务例如推送（通知、短信、提现、域名监控）服务，几乎每做一个项目都要重做一次或复制一次，且无法重用异步队列服务，造成资源浪费
  
* 总结：业务可以很牛逼，但支撑一直很脆弱。我们还是小米加步枪，同行已经拿起了二向箔
  
#### 2.2.2 行业发展
* 技术演变
	+ 单体架构 → RPC架构 → SOA架构 → 微服务架构

* 单体架构问题
  1. 复杂性逐渐变高：比如有的项目有几十万行代码，各个模块之间区别比较模糊，逻辑比较混乱，代码越多复杂性越高，越难解决遇到的问题。
  2. 技术债务逐渐上升：公司的人员流动是再正常不过的事情，有的员工在离职之前，疏于代码质量的自我管束，导致留下来很多坑，由于单体项目代码量庞大的惊人，留下的坑很难被发觉，这就给新来的员工带来很大的烦恼，人员流动越大所留下的坑越多，也就是所谓的技术债务越来越多。
  3. 阻碍技术创新：比如以前的某个项目使用tp3.2写的，由于各个模块之间有着千丝万缕的联系，代码量大，逻辑不够清楚，如果现在想用tp5来重构这个项目将是非常困难的，付出的成本将非常大，所以更多的时候公司不得不硬着头皮继续使用老的单体架构，这就阻碍了技术的创新。
  4. 无法按需伸缩：比如说电影模块是CPU密集型的模块，而订单模块是IO密集型的模块，假如我们要提升订单模块的性能，比如加大内存、增加硬盘，但是由于所有的模块都在一个架构下，因此我们在扩展订单模块的性能时不得不考虑其它模块的因素，因为我们不能因为扩展某个模块的性能而损害其它模块的性能，从而无法按需进行伸缩。
  5. 系统高可用性差：因为所有的功能开发最后都部署到同一个框架里，运行在同一个进程之中，一旦某一功能涉及的代码或者资源有问题，那就会影响整个框架中部署的功能。
  
* 微服务与单体架构的区别
  1. 单体架构所有的模块全都耦合在一块，代码量大，维护困难，微服务每个模块就相当于一个单独的项目，代码量明显减少，遇到问题也相对来说比较好解决。
  2. 单体架构所有的模块都共用一个数据库，存储方式比较单一，微服务每个模块都可以使用不同的存储方式（比如有的用redis，有的用mysql等），数据库也是单个模块对应自己的数据库。
  3. 单体架构所有的模块开发所使用的技术一样，微服务每个模块都可以使用不同的开发技术，开发模式更灵活。

#### 2.2.3 利与弊
#### 2.2.3.1 特性
1. 每个微服务可独立运行在自己的进程里；
2. 一系列独立运行的微服务共同构建起了整个系统；
3. 每个服务为独立的业务开发，一个微服务一般完成某个特定的功能，比如：订单管理，用户管理等；
4. 微服务之间通过一些轻量级的通信机制进行通信，例如通过REST API或者RPC的方式进行调用。

#### 2.2.3.2 利
1. 易于开发和维护
> 由于微服务单个模块就相当于一个项目，开发这个模块我们就只需关心这个模块的逻辑即可，代码量和逻辑复杂度都会降低，从而易于开发和维护。

2. 启动较快
> 这是相对单个微服务来讲的，相比于启动单体架构的整个项目，启动某个模块的服务速度明显是要快很多的。

3. 局部修改容易部署
> 在开发中发现了一个问题，如果是单体架构的话，我们就需要重新发布并启动整个项目，非常耗时间，但是微服务则不同，哪个模块出现了bug我们只需要解决那个模块的bug就可以了，解决完bug之后，我们只需要重启这个模块的服务即可，部署相对简单，不必重启整个项目从而大大节约时间。

4. 技术栈不受限
> 比如订单微服务和电影微服务原来都是用java写的，现在我们想把电影微服务改成php技术，这是完全可以的，而且由于所关注的只是电影的逻辑而已，因此技术更换的成本也就会少很多。

5. 按需伸缩
> 我们上面说了单体架构在想扩展某个模块的性能时不得不考虑到其它模块的性能会不会受影响，对于我们微服务来讲，完全不是问题，电影模块通过什么方式来提升性能不必考虑其它模块的情况。

#### 2.2.3.3 弊
1. 运维要求较高
> 对于单体架构来讲，我们只需要维护好这一个项目就可以了，但是对于微服务架构来讲，由于项目是由多个微服务构成的，每个模块出现问题都会造成整个项目运行出现异常，想要知道是哪个模块造成的问题往往是不容易的，因为我们无法一步一步通过debug的方式来跟踪，这就对运维人员提出了很高的要求。

2. 分布式的复杂性
> 对于单体架构来讲，我们可以不使用分布式，但是对于微服务架构来说，分布式几乎是必会用的技术，由于分布式本身的复杂性，导致微服务架构也变得复杂起来。比如分布式事务的解决

3. 接口调整成本高
> 比如，用户微服务是要被订单微服务和积分微服务所调用的，一旦用户微服务的接口发生大的变动，那么所有依赖它的微服务都要做相应的调整，由于微服务可能非常多，那么调整接口所造成的成本将会明显提高。

4. 重复劳动
> 对于单体架构来讲，如果某段业务被多个模块所共同使用，我们便可以抽象成一个工具类，被所有模块直接调用，但是微服务却无法这样做，因为这个微服务的工具类是不能被其它微服务所直接调用的，从而我们便不得不在每个微服务上都建这么一个工具类，从而导致代码的重复。

## 3 组建方案
### 3.1 核心架构
![iMSE](https://imse-1255691551.cos.ap-shanghai.myqcloud.com/2020-iMSE.jpg)

### 3.2 服务组建
1. 业务分析
2. 服务拆分与设计
> 从单体式结构转向微服务架构中会持续碰到服务边界划分的问题：比如，我们有user服务来提供用户的基础信息，那么用户的头像和图片等是应该单独划分为一个新的service更好还是应该合并到user服务里呢？如果服务的粒度划分的过粗，那就回到了单体式的老路；如果过细，那服务间调用的开销就变得不可忽视了，管理难度也会指数级增加。目前为止还没有一个可以称之为服务边界划分的标准，只能根据不同的业务系统加以调节。拆分的大原则是当一块业务不依赖或极少依赖其它服务，有独立的业务语义，为超过2个的其他服务或客户端提供数据，那么它就应该被拆分成一个独立的服务模块。
3. 服务开发
4. 服务治理

### 3.3 技术标准化
1. 为前后端人员的技术选型提供统一的标准，减少冲突，节省沟通成本，新人快速上手
2. 整合部门间重复性高，频繁调用的业务，提供一套可复用、高可用、高性能、高维护性、易扩展、弹性伸缩的解决方案，可以承载未来的亿级用户量
3. 接入系统的资源（应用、主体、公众号、小程序、微信号）将统一管理，方便查询，提升资源利用率，部门间、部门内易于协调
4. 统一api接口文档，可读性强，易调试
5. 业务低耦合，降低单点故障影响，快速排错
6. 统一的数据中台，数据加工和沉淀，利于业务可持续发展，数据化运营

## 4 年度计划
### 4.1 总体架构
* 计划用一年时间组建完成，到2021年春节前可承载十亿级PV/日
* [全年任务拆解](https://shimo.im/sheets/T3wh9QDVxrKjHV8R/MODOC/)

### 4.2 人才计划
> 为企业持续培养技术人才，提升企业核心竞争力，提升个人价值实现
* 定期开展技术培训，促进员工交流提高
* 为技术一对一制定技术路线，让技术有清晰的发展方向
* 阶段性检验，定期评估技术人员的技能水平

## 5 分阶段目标
1. 第一阶段
* 时间：2019.12.1 - 2019.12.22
* 目标：技术储备，架构设计，实验试错，骨架搭建

2. 第二阶段（当前阶段）
* 时间：2019.12.23 - 2020.2.16
* 目标：核心架构开发

3. 第三阶段
* 时间：2020.2 - 2020.4
* 目标：小范围业务接入，灰度测试，ab压测，高并发业务接入

4. 第四阶段
* 时间：2020.5 - 2020.8
* 目标：服务拆分，完善，联调测试，标准化接入

5. 第五阶段
* 时间：2020.9 - 2021.1
* 目标：全面接入，大数据分析，全面管控，持续交付

## 6 人员配置
1. 第一阶段
3人：架构师1人 + 运维1人 + 后端1人

2. 第二阶段
3人：架构师1人 + 运维1人 + 后端1人

3. 第三阶段
5人：架构师1人 + 运维1人 + 产品1人 + 前端1人 + 后端1人

4. 第四阶段
8-10人：架构师1人 + 运维1-2人 + 产品1-2人 + 前端≥2人 + 后端≥3人

5. 第五阶段
10-15人：架构师1人 + 运维2-3人 + 产品2-3人 + 前端≥3人 + 后端≥3人 + 数据分析2人

## 7 成本预算
1. 人工成本
* 6w-30w/月
* 通过培训，降低人工成本

2. 硬件成本
* 300-500元/月/台
* 通过微服务架构，显著降低硬件成本

3. 是否增加成本
* 短期：增加
* 长期：减少
