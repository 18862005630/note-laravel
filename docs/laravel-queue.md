# Laravel Queue
包含关系：
连接 》 队列 》 队列任务

1、配置文件.env中更改QUEUE_DRIVER=redis

2、生成队列文件Jobs/SendMail.php
php artisan make:job SendMail

3、在SendMail中的handle方法中编写队列任务

4、启动队列监听
php artisan queue:work（此命令针对的是默认连接，若自定义连接和队列名需指定）
php artisan queue:work --queue=email(指定监听队列email)

5、其他控制器中调用队列,延迟10秒执行
\App\Jobs\SendMail::dispatch($data)->onConnection('edm')->onQueue('email')->delay(10);

6、安装predis扩展
composer require predis/predis ^1.1
```
config/queue.php配置

    'redis' => [
            'driver' => 'redis',
            'connection' => 'default',
            'queue' => env('REDIS_QUEUE', 'default'),
            'retry_after' => 2,
            'block_for' => null,
        ],
     'edm' => [
            'driver' => 'redis',
            'connection' => 'default',
            'queue' => ['default','email'],
            'retry_after' => 2,
            'block_for' => 12,
        ]
```

官方自带的队列监听扩展：laravel horizon
安装：
1、composer require laravel/horizon
2、php artisan vendor:publish --provider="Laravel\Horizon\HorizonServiceProvider"
运行：
php artisan horizon
查看：站点域名/horizon

其他命令：
暂停：php artisan horizon:pause
继续 php artisan horizon:continue
执行完所有任务后退出 php artisan horizon:terminate