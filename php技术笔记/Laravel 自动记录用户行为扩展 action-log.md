### Laravel action-log 扩展

给大家推荐我个人写的一个laravel自动记录用户行为的扩展。具体使用请看下文详细介绍。

### 安装

1.方法一：在composer.json中加入

```
{
    "require": {
        "luoyangpeng/action-log": "~1.0"
    }
}
```

2.方法二：

```
composer require luoyangpeng/action-log 
```

### 使用

1.在config/app.php配置文件中加入

```
 'providers' => [
        // ...
        'luoyangpeng\ActionLog\ActionLogServiceProvider',
    ]

'aliases' => [
        // ...
        'ActionLog' => 'luoyangpeng\ActionLog\Facades\ActionLogFacade',
    ]
```

### 配置

1.发布配置文件

```
php artisan vendor:publish
```

2.config/actionlog.php配置文件中加入要记录的模型

```
return [
        '\App\Models\Users',
    ];
```

### 执行数据表迁移文件

```
php artisan migrate
```

### 主动记录用户行为

```
use ActionLog
//$type为操作日志类型 比如登录 添加 删除...
ActionLog::createActionLog($type,$content);
```

### Demo

```
//update

$users = Users::find(1);
$users->name = "myname";
$users->save();

//add

$users = new Users();
$users->name = "myname";
$users->save()

//delete

Users:destroy(1);
```