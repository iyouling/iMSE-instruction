# ThinkPHP6 精通框架
## 1. 框架
### 1.1 框架简介
* Laravel - 巨匠级
* Phalcon7 - C语言编写
* Symfony
* YII
* ZendFramework
* ThinkPHP [手册](https://www.kancloud.cn/manual/thinkphp6_0/1037479)
    > 用composer安装

### 1.2 MVC模式
Model业务逻辑 - Controller控制器 - View交互界面
> ThinkPHP6需安装 think-view 模板引擎驱动
> 
`composer require topthink/think-view`

### 1.3 安装
* `composer create-project topthink/think tp`
* `composer update topthink/framework`

### 1.4 目录结构
```
www  WEB部署目录（或者子目录）
├─app           应用目录
│  ├─controller      控制器目录
│  ├─model           模型目录
│  ├─ ...            更多类库目录
│  │
│  ├─common.php         公共函数文件
│  └─event.php          事件定义文件
│
├─config                配置目录
│  ├─app.php            应用配置
│  ├─cache.php          缓存配置
│  ├─console.php        控制台配置
│  ├─cookie.php         Cookie配置
│  ├─database.php       数据库配置
│  ├─filesystem.php     文件磁盘配置
│  ├─lang.php           多语言配置
│  ├─log.php            日志配置
│  ├─middleware.php     中间件配置
│  ├─route.php          URL和路由配置
│  ├─session.php        Session配置
│  ├─trace.php          Trace配置
│  └─view.php           视图配置
│
├─view            视图目录
├─route                 路由定义目录
│  ├─route.php          路由定义文件
│  └─ ...   
│
├─public                WEB目录（对外访问目录）
│  ├─index.php          入口文件
│  ├─router.php         快速测试文件
│  └─.htaccess          用于apache的重写
│
├─extend                扩展类库目录
├─runtime               应用的运行时目录（可写，可定制）
├─vendor                Composer类库目录
├─.example.env          环境变量示例文件
├─composer.json         composer 定义文件
├─LICENSE.txt           授权说明文件
├─README.md             README 文件
├─think                 命令行入口文件
...
```

### 1.5 规范
```
命名规范
ThinkPHP6.0遵循PSR-2命名规范和PSR-4自动加载规范，并且注意如下规范：

目录和文件
目录使用小写+下划线；
类库、函数文件统一以.php为后缀；
类的文件名均以命名空间定义，并且命名空间的路径和类库文件所在路径一致；
类（包含接口和Trait）文件采用驼峰法命名（首字母大写），其它文件采用小写+下划线命名；
类名（包括接口和Trait）和文件名保持一致，统一采用驼峰法命名（首字母大写）；
函数和类、属性命名
类的命名采用驼峰法（首字母大写），例如 User、UserType；
函数的命名使用小写字母和下划线（小写字母开头）的方式，例如 get_client_ip；
方法的命名使用驼峰法（首字母小写），例如 getUserName；
属性的命名使用驼峰法（首字母小写），例如 tableName、instance；
特例：以双下划线__打头的函数或方法作为魔术方法，例如 __call 和 __autoload；
常量和配置
常量以大写字母和下划线命名，例如 APP_PATH；
配置参数以小写字母和下划线命名，例如 url_route_on 和url_convert；
环境变量定义使用大写字母和下划线命名，例如APP_DEBUG；
数据表和字段
数据表和字段采用小写加下划线方式命名，并注意字段名不要以下划线开头，例如 think_user 表和 user_name字段，不建议使用驼峰和中文作为数据表及字段命名。
请理解并尽量遵循以上命名规范，可以减少在开发过程中出现不必要的错误。
```

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

## 2. 控制器
### 2.1 定义
* 注意路由配置定义

### 2.2 空控制器
* 创建Error.php控制器，定义魔术方法__call()，访问空控制器
* response的code方法可以指定404错误码 `return response("404")->code(404)`

### 2.3 控制器中间件
```
php think make:middleware Auth
//依赖注入
protected $middleware = ["app\middleware\Auth"];
public function test(Req $request)
{
	dump($request);
}
```

### 2.4 请求对象
* 通过构造方法注入（不常用）
* 通过方法中注入 `think\Request $request`
* 通过门面静态方式 `$request = Request::instance();`

### 2.5 输入变量
```
Request::Param()
$request->has()
$request->isGet()
```

### 2.6 响应输出
```
return json(array);
return "<hr>";
return view("template");
return xml(array);
return download(源文件路径，保存文件名);
```

### 2.7 响应参数
* 链式操作
`return response()->data($html)->json([])->code()->header();`
* 重定向
`return redirect(url)`

## 3. 数据库
### 3.1 查询

```
use think\facade\Db;
Db::name('users')->select()->toArray();
Db::table('tp_users');
Db::connect('mysql2')->table('')->find();
$user = Db::name("user")->where("id",1)->find();
Db::name("user")->where("id",1)->value('username');
Db::name("user")->column('username', 'id');
$rows = Db::name("user")->insert(array);
$rows = Db::name("user")->insertAll(array); //二维数组
Db::name("user")->where("id", 5)->data(["username"=>"abc"])->update();
->inc("age")->update()
->dec("age")->update()
->delete([1,3,5]);
软删除
Db:name('users')->where('id',1)->useSoftDelte("delete_time", time())->delete();
分页查询
paginate
时间查询
whereYear
Json查询
```

### 3.2 事件
### 3.3 获取器
### 3.4 事务
最简单的方式是使用 transaction 方法操作数据库事务，当闭包中的代码发生异常会自动回滚，例如：
```
Db::transaction(function () {
    Db::table('think_user')->find(1);
    Db::table('think_user')->delete(1);
});
```
### 3.5 监听sql，调试

## 4. 模型
### 4.1 创建模型
`php think make:model index@Users`

### 4.2 基本操作
* 数据库操作完全使用模型操作
```
$arr['find'] = self::find(2)->toArray(); //toArray将collection转为简单array
$arr['find1'] = self::where("id",1)->find(); //结果集
$arr['select'] = self::select()->toArray(); //toArray将collection转为简单array
$arr['select12'] = self::select(array(1,2)); //结果集
$arr['column'] = self::column("username", "id");
$arr['value'] = self::where("id",">", 1)->value("username");
```
* 更多操作
```
delete, destory, json
```

### 4.3 获取器和修改器
### 4.4 自动时间戳
### 4.5 软删除
* select查询不出来
* withTrash删除数据一起查询出来
* restore恢复
### 4.6 类型转换
### 4.7 关联模型
* 自动实现事务
* 关联查询
* 关联更新
* 关联查询

## 5. 模板渲染

## 6. 验证

## 7. 路由

## 8. 容器、门面、中间件
