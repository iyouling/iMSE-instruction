# iMSE-分布式微服务系统
## 微服务架构
+ [组建计划](iMSE-plan2020.md)
+ [架构草图](2020-iMSE.jpg)（[ProcessOn](https://www.processon.com/view/link/5e00663fe4b0125e29159020)）
+ [组建历程](iMSE-memo.md)

## 标准化
### 最佳实践
+ 王教授: [最佳实践](iMSE-DevOps.md)
+ 陈佳源: [如何使用 Composer + Github 进行版本管理](iMSE-std-composer.md)

### 技术路线
+ [学习大纲](iMSE-TechStack.md)

### 选用工具/框架
#### 项目管理
+ √ [Tapd](https://www.tapd.cn)
+ [Jira](https://zhuanlan.zhihu.com/p/107055009)
+ [worktile](https://worktile.com/)
+ [禅道](https://www.zentao.net/)

#### 代码托管
+ √ SVN
+ √ Git

#### 前端
+ √ [vue.js](https://cn.vuejs.org/)
+ √ [uniapp](https://uniapp.dcloud.io/)

#### 后端框架
+ √ [Thinkphp及衍生框架](https://www.kancloud.cn/manual/thinkphp6_0/1037479)
+ [YII](https://www.yiiframework.com)
+ [laravel](https://laravel.com)
+ [symfony](http://www.symfonychina.com)

#### 微服务
1. 容器
+ √ [Docker](https://hub.docker.com/)

2. 容器编排
+ √ [K8s](https://www.kubernetes.org.cn/k8s)
+ √ [docker-compose](https://docs.docker.com/compose/reference/overview/)

3. 框架
+ √ [Swoole](https://wiki.swoole.com/)
+ √ [Swoft](https://www.swoft.org/documents/v2/)
+ √ [Hprose](https://github.com/hprose)([wiki](https://github.com/hprose/hprose-php/wiki))
+ √ [Thrift](http://thrift.apache.org/)
+ √ [gRPC](http://doc.oschina.net/grpc?t=57966)
+ [dubbo](http://dubbo.apache.org/zh-cn/docs/user/preface/background.html)
+ Halibut
+ SCS
+ Shuttler.net

4. 消息中间件：解耦、异步、削峰
+ √ [RabbitMQ](https://www.rabbitmq.com/getstarted.html)
+ [ActiveMQ](http://activemq.apache.org/)
+ [RocketMQ](http://rocketmq.apache.org/)
+ [Kafka](http://kafka.apache.org/documentation/)

5. 服务管理：服务注册与发现、动态扩容、熔断、服务降级、限流
+ √ [Consul](https://www.consul.io)
+ [Zookeeper](http://zookeeper.apache.org/)
+ [etcd](https://etcd.io/)

6. 进程间通信
+ √ [RPC](https://www.swoft.org/documents/v2/core-components/rpc-server/)
+ √ [Restful Api](https://segmentfault.com/a/1190000019818569)

7. 日志监控分析
+ √ [Elasticsearch](https://www.elastic.co/cn/)
+ √ [Logstash](https://www.elastic.co/cn/logstash)
+ √ [Kibana](https://www.elastic.co/cn/kibana)

8. 数据存储
+ √ [Mariadb/Mysql](https://mariadb.org)
+ √ Sqlserver
+ √ [Mongodb](www.mongodb.org)
+ CouchDB
+ √ [Redis](https://redis.io)
+ Flare
+ Cassandra
+ Hbase
+ [Memcache](https://www.php.net/manual/en/book.memcache.php)

9. 数据中台
+ Python
+ [Pandas](https://www.pypandas.cn)

10. 服务网格Service Mesh
+ [Linkerd](https://github.com/linkerd/linkerd)
+ [Envoy](https://github.com/envoyproxy/envoy)
+ [Istio](https://github.com/istio/istio)
+ [Conduit](https://github.com/runconduit/conduit)
+ [nginMesh](https://github.com/nginmesh/nginmesh)
+ [Kong](https://github.com/Kong/kong)

### 性能优化

### 工程化
1. [Composer](https://www.phpcomposer.com/)

2. 单元测试

3. 自动化部署

4. 资源编排
> 遵循 ROS 定义的模板规范，编写模板文件，在模板中定义所需云计算资源的集合及资源间的依赖关系、资源配置细节等，ROS 通过编排引擎自动完成所有资源的创建和配置，以达到自动化部署、运维的目的。

5. 运维编排
> 自动化管理和执行任务。只要通过模板定义执行任务、执行顺序、执行输入和输出，然后通过执行模板来完成任务的自动化运行。

6. 通过弹性伸缩创建 ECS
> 弹性伸缩根据设置的伸缩规则，在业务需求增长时自动增加 ECS 实例以保证计算能力，在业务需求下降时自动减少 ECS 实例以节约成本。

### 大型网站


