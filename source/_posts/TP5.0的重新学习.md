---
title: TP5.0的重新学习
photos: https://cdn.jsdelivr.net/gh/lh-xuyong/CDN/img/cover/(2).jpg.webp
categories: 生活
---


    0. Thinkphp官网
        `http://www.thinkphp.cn`
    
    1. 使用composer安装
        composer create-project topthink/think=5.0.* tp5  --prefer-dist
    
    2. 使用GIT安装
        ThinkPHP 远程仓库
        GitHub:
            应用项目：https://github.com/top-think/think
            核心框架：https://github.com/top-think/framework
        码云:
            应用项目： https://gitee.com/liu21st/thinkphp5
            核心框架： https://gitee.com/liu21st/framework.git

一. 初识佳人 (ThinkPHP 5.0 基本)

0. TP5 环境要求

     PHP >= 5.4.0
     PDO PHP Extension       PDO类
     MBstring PHP Extension  多字节字符串函数
     CURL PHP Extension      钩子/爬虫

1. 目录结构

    1).部署框架 目录结构
~~~
        tp5
        ├─application     应用目录
        ├─extend          扩展类库目录（可定义）
        ├─public          网站对外访问目录
        ├─runtime         运行时目录（可定义）
        ├─vendor          第三方类库目录（Composer）
        ├─thinkphp        框架核心目录
        ├─build.php       自动生成定义文件（参考）
        ├─composer.json   Composer定义文件
        ├─LICENSE.txt     授权说明文件
        ├─README.md       README 文件
        └─think           命令行工具入口
        !(如果在linux环境下面的话,需要给runtime目录755权限)
~~~
**PS. 几个关键的路径:**

~~~
        目录            说明             常量
        tp5             项目根目录       ROOT_PATH
        tp5/application 应用目录         APP_PATH
        tp5/thinkphp    框架核心目录     THINK_PATH
        tp5/exend       应用扩展目录     EXTEND_PATH
        tp5/vendor      Composer扩展目录 VENDOR_PATH
~~~

**2).核心框架 目录的结构**

~~~
        ├─thinkphp 框架系统目录
        │  ├─lang               语言包目录
        │  ├─library            框架核心类库目录
        │  │  ├─think          think 类库包目录
        │  │  └─traits         系统 traits 目录
        │  ├─tpl                系统模板目录
        │  │
        │  ├─.htaccess          用于 apache 的重写
        │  ├─.travis.yml        CI 定义文件
        │  ├─base.php           框架基础文件
        │  ├─composer.json      composer 定义文件
        │  ├─console.php        控制台入口文件
        │  ├─convention.php     惯例配置文件
        │  ├─helper.php         助手函数文件（可选）
        │  ├─LICENSE.txt        授权说明文件
        │  ├─phpunit.xml        单元测试配置文件
        │  ├─README.md          README 文件
        │  └─start.php          框架引导文件
~~~

**3).默认应用 目录结构:**

~~~
        ├─application            应用目录（可设置）
        │  ├─index              模块目录(可更改)
        │  │  ├─config.php     模块配置文件
        │  │  ├─common.php     模块公共文件
        │  │  ├─controller     控制器目录
        │  │  ├─model          模型目录
        │  │  └─view           视图目录
        │  │
        │  ├─command.php        命令行工具配置文件
        │  ├─common.php         应用公共文件
        │  ├─config.php         应用配置文件
        │  ├─tags.php           应用行为扩展定义文件
        │  ├─database.php       数据库配置文件
        │  └─route.php          路由配置文件

~~~

### 开始应用

`<http://www.tp5.0.com/admin/user/index>`

admin:模板

user:user.php  user类

index:方法 



#### 助手函数 (详见thinkphp/helper)

abort 中断执行并发送HTTP状态码
action 调用控制器类的操作
cache 缓存管理
config 获取和设置配置参数
controller 实例化控制器
cookie Cookie管理
db 实例化数据库类
debug 调试时间和内存占用
dump 浏览器友好的变量输出
exception 抛出异常处理
halt 变量调试输出并中断执行
import 导入所需的类库
input 获取输入数据 支持默认值和过滤
json JSON数据输出
jsonp JSONP数据输出
lang 获取语言变量值
load_trait
快速导入Traits PHP5.5 以上无需调
用
model 实例化Model
redirect 重定向输出
request 实例化Request对象
response 实例化Response对象
session Session管理
trace 记录日志信息
token 生成表单令牌输出
url Url生成
validate 实例化验证器

vendor 快速导入第三方框架类库
view 渲染模板输出
widget 渲染输出Widget
xml XML数据输出

## PHPstorim

#### 生成 rest模块的 User资源控制器

php think build --module rest //这样的创建出一个rest模块,包括了MVC
php think make:controller rest/User

#### 创建模块

导入tink\Controller

生成model 

php think make:model rest/User



----

`$this->assign('title','tp5后台');`

`return $this->fetch();`  //相当于smarty里的display

view //渲染模板输出

```php+HTML
use think\Controller;

class Index extends Controller
{
    public function Index()
    {
//        return '德玛西亚'

//        $this->assign('title','tp5后台');
//        return $this->fetch(); //这里其实是有参数的默认'index/index',所有可以不写

        return view('',['title'=>'tttttt5']);
    }

}
```

## CURD

![fa7fda2e7c90081d988f07ea1a8adc1](D:\wamp\www\s84\markdown\二期\tp5\fa7fda2e7c90081d988f07ea1a8adc1.jpg)

#### 查

```php
public function index()
{
// 原生SQL
$sql = 'SELECT * FROM user';
$list = Db::query($sql);

// 预处理
$sql = 'SELECT * FROM user WHERE id = ?';
$list = Db::query($sql, [5]);

// DB类
$list = Db::table('user')->order(['id' => 'desc'])->select();

// 助手函数
$list = db('user')->order(['id' => 'asc'])->select();

dump($list);
}
```

#### 增

```php
public function insert()
{
    // 原生SQL
   $sql = 'INSERT INTO user (id,name,age) VALUES (null,"k1",66)';
   $result = Db::execute($sql);
    // 预处理
   $sql = 'INSERT INTO user (id,name) VALUES (:id, :name)';
   $result = Db::execute($sql, ['id'=>null, 'name'=>'k2']);

    $data = [
        'name' => 'k5',
        'age' => 99,
        'province' => '西安'
    ];
    // DB类   insertAll() 多条插入
   $result = Db::table('user')->insert($data); // 受影响行数
   $result = Db::table('user')->insertGetId($data); // 自增ID

    // 助手函数
    $result = db('user')->insert($data); // 受影响行数

    dump($result);
}
```
#### 删	

```php
public function delete()
{
    // 原生SQL
   $sql = 'DELETE FROM user WHERE name="k2"';
   $result = Db::execute($sql);
    // DB类
   $result = Db::table('user')->delete(55);
   $result = Db::table('user')->where('name','k1')->delete();

    // 助手函数
    $result = db('user')->delete(54);

    dump($result);
}
```
#### 改

```php
public function update()
{
    // 原生SQL
   $sql = 'UPDATE user SET age="5" WHERE id ="51"';
   $result = Db::execute($sql);

    // DB类
   $result = Db::table('user')->where(['id'=>51])->update(['age'=>88, 'sex'=>1]);

    // 助手函数
    $result = db('user')->where(['id'=>50])->update(['age'=>88, 'sex'=>1]);
    dump($result);

}
```
## 路由

```
ThinkPHP 5.0 在没有启用路由的情况下典型的URL访问规则是：
http://serverName/index.php（或者其它应用入口文件）/模块/控制器/操作/[参数名/参数值...]

```



#### 1.路由文件

​    `application/route.php`

#### 2.路由概念/规则

```
    从规则上区分 路由分为: 动态路由 和 静态路由 两种
    从设置方式上 路由分为: 动态注册 和 静态注册 两种

    路由规则中 包含变量的 就是动态路由,没有包含任何变量的称为静态路由.
    在路由文件中 return数组的 路由形式, 称之为 静态注册
    使用Route类的方法 注册的路由 称之为 动态注册(5.0推荐)
    以上两者 可同时使用.
```
#### 3.定义路由

**Route::rule('路由表达式' , '路由地址' , '请求类型' , '路由参数（数组）' , '变量规则（数组）');**

| 路由表达式: | URL访问规则（包括静态规则和动态规则）,只有符合规则的路由才能正确访问 // 其实就是外号 |
| ----------- | ------------------------------------------------------------ |
| 路由地址:   | 实际访问的地址（可以是控制器操作、类的方法或者闭包）;        |
| 请求        | 包括GET/POST/PUT/DELETE等,如果希望任何请求都能访问使用*号（默认值）。 |
| 路由参数:   | 路由匹配的条件约束或设置参数（用于检测或者解析）;            |
| 路由变量    | 路由规则里面的动态变量以及PATH_INFO里面的参数都称之为路由变量; |
| 变量规则    | 路由规则中的变量的匹配规则（正则表达式）                     |

**支持get请求规则**

Route::get('home', 'index/index/index');

**支持post**

Route::post('ppp', 'index/index/ppp');

**支持GET/POST/delete/...**

Route::**rule**('ooxx','index/index/index');

**支持任意**

****Route::**any**('xxoo','index/index/index');



以上皆为静态路由

###### 动态路由

| 动态路由     |                                   |                                                |                                                            |
| ------------ | --------------------------------- | ---------------------------------------------- | ---------------------------------------------------------- |
| **必传参数** | 路由表达式后面跟变量:**user/:id** | Route::get('user/:id', 'index/index/user');    | 对应user方法里必须要有默认参数,不然就报错                  |
| **可选参数** | []将变量包起来                    | Route::get('user/[:id]', 'index/index/user');  | 可选参数,变量加[],如果输入网页时没ID那么将会输出user默认值 |
| **匹配参数** | [变量]后面加$                     | Route::get('user/[:id]$', 'index/index/user'); | 如果不加$用户在55后面乱输就会报错,user/55/eeres            |

###### 闭包路由

闭包路由  一般用于非法命令,404

```
Route::get('js/[:id]$',function($id){
    if ($id>200){
        return '我们这里没有这么多及时';
    }else{
        return "{$id}";
    }
});
```

###### 路由参数

**ext** :路由后缀检测

例:现在尾巴只允许是.html或.shtml

deny_ext:与上面相反

```
Route::get('userlist','admin/user/index',['ext'=>'html|shtml']);
```

###### 变量规则

第五个参数,用正则来约束变量

```php
// 变量规则  越容易匹配的规则 越往后放 或者加变量尾$ 
// http://s84.com/blog/2019/10/05.html
Route::get('blog/:year/:month/:day', 'index/blog/article',[], ['year'=>'\d{4}','month'=>'\d{2}','day'=>'\d{2}']);
// http://s84.com/blog/5.html
Route::get('blog/:id', 'index/blog/index',[],['id'=>'\d+']);
// http://s84.com/blog/ss.html
Route::get('blog/:name', 'index/blog/read',[], ['name'=>'\w+']);
```

###### 路由分组

```
// Route::rule('路由表达式','路由地址','请求类型','路由参数（数组）','变量规则（数组）');
Route::get('blog/:year/:month/:day', 'index/blog/article',[],['year'=>'\d{4}'],['month'=>'\d{2}'],['day'=>'\d{2}']);
```

Route::group('外号',[

​	'变量'=>[

​		路由地址	,

​		路由数组[],

​		变量规则

​	]

​	'变量'=>[

​		路由地址	,

​		路由数组[],

​		变量规则

​	]

)

```php
Route::group('blog',[
    ':year/:month/:day'=>[
        'index/blog/article',
        [],
        ['year'=>'\d{4}'],
        ['month'=>'\d{2}'],
        ['day'=>'\d{2}']
    ],
    ':id'=>[
        'index/blog/index',
        [],
        ['id'=>'\d+']
    ],
    ':name'=>[
        'index/blog/read',
        [],
        ['name'=>'\w+']
    ],
]);
```

###### 生成URL地址

根据路由生成url网址

在route.php内定义:

```
Route::get('user/[:id]$','index/index/user');
```

调用到url方法:

```
Route::get('url','index/index/url');
```

方法:

```php
public function url()
{	
        echo Url::build('index/index/user', ['id'=>6]);
        echo '<br>';
        echo url('index/index/user', ['id'=>6]);
        echo '<br>';
        echo url('index/blog/article', ['year'=>2016,'month'=>10,'day'=>10]);
        echo '<hr>';
		
    	//带域名,加上true是带本地域名
        echo url('index/blog/read', ['name'=>'laowang'], 'shtml', true);
        echo '<br>';
    	// 指定域名 A站
        echo url('index/blog/read', ['name'=>'laowang'], 'shtml', 'www.acfun.cn');

        echo '<hr>';
    	//带锚点
        echo url('index/blog/read#p1', ['name'=>'laowang'], 'shtml', true);

}	
```

#### 资源路由

5.0支持设置 RESTFul 请求的资源路由，方式如下：
`Route::resource('blog','index/blog');`

​				'外号','模块/文件名(Class名)'

## 控制器

###### 重定向

**跳转**

​	关键词

​	success和error

​	访问jump: `Route::get('jump','admin/index/jump');`

​        跳转:`$this->success('添加成功', url('admin/user/index'));`

​	定义路由::`Route::get('admin/user/index', 'admin/user/index');`

**秒跳:**

​        $this->redirect('http://huaban.com');

**空操作:**

__empty()

`__miss__`路由  自己看手册去

# 实际项目

![RESTful](D:\wamp\www\s84\markdown\二期\tp5\RESTful.PNG)

#### 增删改查 用户管理实例 RESTful资源控制器

1. ###### 生成了 rest模块的 User资源控制器

    php think build --module rest
    php think make:controller rest/User

助手函数   :url()

```php
<a href="{:url('rest/user/index')}" class="btn btn-default">用户列表首页</a> //生成了一个路由=>users.html,users名来自于资源路由Route::resource('users', 'rest/User');
```

遍历,类似于foreach

key是键,id是值

```
{volist name="list" key="k" id=""}

{/volist}
```

###### 输出偶数记录

将查到的东西分两列:,按查到的条目分配,不是按ID基偶数

在volist前标签加上 mod="n"

在volist标签中间写上:

```
{eq name="mod" value="3"}

{/eq}
```

示例

```php
<div class="row mt50">
    <div class="col-md-6">
        <table class="table table-hover bg-info">
            <tr>
                <th>ID</th>
                <th>NAME</th>
                <th>ACTION</th>
            </tr>
            {volist name="list" key="k" id="v" mod="2"}
            {eq name="mod" value="0"}
            <tr>
                <td>{$v['id']}</td>
            </tr>
            {/eq}
            {/volist}
        </table>
    </div>

    <div class="col-md-6">
        <table class="table table-hover bg-info">
            <tr>
                <th>ID</th>
                <th>NAME</th>
                <th>ACTION</th>
            </tr>
            {volist name="list" key="k" id="v" mod="2"}
            {eq name="mod" value="1"}
            <tr>
                <td>{$v['id']}</td>
            </tr>
            {/eq}
            {/volist}
    </div>
</div>
```

#### 添加

读取表单提交过来的参数

1.**request->post()** 

2.input('post.')

#### 编辑

```
<input type="hidden" name="method" value="PUT">
```

doedit里

```
$p = $request->put();
```

## model

生成model 

php think make:model rest/User

###### 断点调试

```
halt($user->pass);//D:\wamp\www\s84\tp5\thinkphp\library\think\Debug.php:193:string 'e10adc3949ba59abbe56e057f20f883e' (length=32)
```

添加数据时,过滤非法字段

反正写入的记录数

```
$result = $user->allowField(true)->save();
$user->id;// 拿到自增ID
```

```
        $p = $request->post();
        $user = new UserModel($p);
        $user->pass = md5($p['pass']);
//        var_dump($user);
//        halt($user->pass);
        // 添加数据,并过滤非法字段
        // 返回写入数据
        $result = $user->allowField(true)->save();
//        echo $user->id;
        if ($result) {
            return $this->success('安排好了','rest/user/index');
        } else {
            return $this->error('添加失败','rest/user/index');

        }
```

###### 盐值加密

```
return password_hash($pass,PASSWORD_DEFAULT);
```

验证密码

## 验证



```
class User extends Validate;
{
    Protected $rule = [
        'name'=>['require','min'=>2,'max'=>20],
        'sex' =>['in'=>'0,1,2'],
        'age' =>['number','between'=>'1,120'],
        'tel'=>['require','regex'=>'/^1[012]\d{9}&/']
    ];
}
```
替换系统自带提示

```
    protected $message =[
        'name.require' =>'你是不是忘了用户名额';
        'name.min' =>'你太短了';
    ];
}
```

## 查

#### 快速查询

##### where 

```
$row = Db::name('user')
     ->where('id',5)
     ->find();

$row = Db::name('user')
    ->where('id','<=',5)
    ->select();
```

and,like

```
$row = Db::name('user')
    ->where('age','<=',20)
    ->where('name','like','%许%')
    ->select();
```

or

```
$row = Db::name('user')
    ->where('sex','0')
    ->whereOr('province','上海')
    ->select();
var_dump($row);
```
{% bili 24897960 %}
#### 批量查询

```
$row = Db::name('user')
     ->where([
         'id'=>['between','10,20'],
         'sex' => ['eq',0]
     ])->select();

$row = Db::name('user')
    ->where('sex',1)
    ->where('id',['between','1,3'],['in',[6, 66, 666]],'or')
    ->select();
```

#### 区间查询

```
  $row = Db::name('user')
            ->where('id', ['>', 2], ['<', 15])
            ->select();
```



#### 多表联查

```
1). 手动多表
$row = Db::field(['u.name'=>'un','l.name'=>'ln'])
    ->table(['hc_user'=> 'u', 'hc_lover'=>'l'])
    ->where('u.id = l.user_id')
    ->select();

//            2). JOIN
$row = Db::table('hc_user')
    ->alias('u')
    ->field(['u.name'=>'uname','l.name'=>'lname'])
    ->join('hc_lover l', 'u.id = l.user_id')
    ->order('uname', 'DESC')
    ->select();

//            3). 视图
$row = Db::view('hc_user', ['name'=>'uname'])
    ->view('hc_lover', ['name'=>'lname'],'hc_user.id = hc_lover.user_id')
    ->select();
```

## 通过url传递id

`<a href="{:url('rest/user/updateNovel',['id'=>$v['id']])}" class="btn btn-xs `

`btn-primary" style="color:#000">`

$id = Request::instance()->param();


