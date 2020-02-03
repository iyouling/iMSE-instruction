# ThinkPHP6 精通框架
## 1. 框架
### 1.1 框架简介
* Laravel - 巨匠级
* Phalcon7 - C语言编写
* Symfony
* YII
* ZendFramework
* ThinkPHP
    > 用composer安装

### 1.2 MVC模式
Model业务逻辑 - Controller控制器 - View交互界面
> ThinkPHP6需安装 think-view 模板引擎驱动
> 
`composer require topthink/think-view`

### 1.3 安装
略

### 1.4 目录结构
略

### 1.5 语言规范
略

### 1.6 生命周期
```
HTTP请求流程
对于一个HTTP应用来说，从用户发起请求到响应输出结束，大致的标准请求流程如下：

载入Composer的自动加载autoload文件
实例化系统应用基础类think\App
获取应用目录等相关路径信息
加载全局的服务提供provider.php文件
设置容器实例及应用对象实例，确保当前容器对象唯一
从容器中获取HTTP应用类think\Http
执行HTTP应用类的run方法启动一个HTTP应用
获取当前请求对象实例（默认为 app\Request 继承think\Request）保存到容器
执行think\App类的初始化方法initialize
加载环境变量文件.env和全局初始化文件
加载全局公共文件、系统助手函数、全局配置文件、全局事件定义和全局服务定义
判断应用模式（调试或者部署模式）
监听AppInit事件
注册异常处理
服务注册
启动注册的服务
加载全局中间件定义
监听HttpRun事件
执行全局中间件
执行路由调度（Route类dispatch方法）
如果开启路由则检查路由缓存
加载路由定义
监听RouteLoaded事件
如果开启注解路由则检测注解路由
路由检测（中间流程很复杂 略）
路由调度对象think\route\Dispatch初始化
设置当前请求的控制器和操作名
注册路由中间件
绑定数据模型
设置路由额外参数
执行数据自动验证
执行路由调度子类的exec方法返回响应think\Response对象
获取当前请求的控制器对象实例
利用反射机制注册控制器中间件
执行控制器方法以及前后置中间件
执行当前响应对象的send方法输出
执行HTTP应用对象的end方法善后
监听HttpEnd事件
执行中间件的end回调
写入当前请求的日志信息
至此，当前请求流程结束。
```

### 1.7 命令行工具
* `php think xxx`
* `php think list`
* `php think make:controller index@demo` 创建资源控制器
* `php think make:command` 创建自定义命令

### 1.8 Xdebug调试
1. Windows: [xdebug.org](https://xdebug.org/wizard) 分析phpinfo文件 → 下载相应版本 / Linux: pecl install xdebug
2. php.ini 配置PHPSTORM远程调试
3. phpstorm的Settings设置xdebug端口和代理
4. phpstorm的Run执行Web Server Debug Validation，验证是否配置成功
5. 浏览器安装Xdebug插件，设置PHPSTORM，并启用
6. phpstorm监听Xdebug，断点调试
