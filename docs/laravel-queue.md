# Laravel Queue
包含关系：
连接 》 队列 》 队列任务

1、配置文件.env中更改QUEUE_DRIVER=redis

2、生成队列文件Jobs/SendMail.php
php artisan make:job SendMail

3、在SendMail中的handle方法中编写队列任务

4、启动队列监听
php artisan queue:work

5、其他控制器中调用队列,延迟10秒执行
\App\Jobs\SendMail::dispatch()->delay(10);

6、安装predis扩展
composer require predis/predis ^1.1
