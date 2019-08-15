# laravel-notification 消息通知(toDatabase)
#### 1、创建消息通知类
```
php artisan make:notification InvoicePaid
```

#### 2、编辑消息通知类
```
<?php

namespace App\Notifications;

use Illuminate\Bus\Queueable;
use Illuminate\Notifications\Notification;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Notifications\Messages\MailMessage;

class InvoicePaid extends Notification
{
    use Queueable;

    private $content;

    /**
     * Create a new notification instance.
     *
     * @return void
     */
    public function __construct($content)
    {
        $this->content = $content;
    }

    /**
     * Get the notification's delivery channels.
     *
     * @param  mixed  $notifiable
     * @return array
     */
    public function via($notifiable)
    {
        return ['database'];
    }

    /**
     * Get the mail representation of the notification.
     *
     * @param  mixed  $notifiable
     * @return \Illuminate\Notifications\Messages\MailMessage
     */
    public function toMail($notifiable)
    {
        return (new MailMessage)
                    ->line('The introduction to the notification.')
                    ->action('Notification Action', url('/'))
                    ->line('Thank you for using our application!');
    }

    /**
     * Get the array representation of the notification.
     *
     * @param  mixed  $notifiable
     * @return array
     */
    public function toArray($notifiable)
    {
        return [
            //
        ];
    }
	
	//上面via方法定义何种方式发送消息，下面就得实现对应的方法
    public function toDatabase($notifiable){
		//json格式数据，纬度可自定义
        return [
            'uid'=>99,
            'money'=>'$100',
            'content'=>$this->content
        ];
    }
}

```

#### 3、创建数据表
```
php artisan notifications:table
php artisan migrate
```

#### 4、控制器中发送消息
##### 4.1 使用Notifiable实现单个用户的消息发送
```
<?php
namespace App\Http\Controllers\Index;
use App\Http\Controllers\Controller;
use App\User;
use App\Notifications\InvoicePaid;
use Illuminate\Notifications\Notifiable;

class IndexController extends Controller
{
	
    public function index(){    		
    	//消息通知发送
    	$user = User::find(1);
    	$user->notify(new InvoicePaid('消息功能测试'));    		
    		
    }
	
	//消息展示
    public function show()
    {
        $user = User::find(1);
        foreach ($user->Notifications as $notification) {
            dump($notification->data);
        }
    }

}

```

##### 4.2 使用Notification实现多用户群发
```
<?php
namespace App\Http\Controllers\Index;
use App\Http\Controllers\Controller;
use App\User;
use App\Notifications\InvoicePaid;
use Notification;
class IndexController extends Controller
{
	
    public function index(){    		
    	//消息通知发送 
    	$users = User::whereBetween('id',[1,5])->get();    		
    	Notification::send($users, new InvoicePaid('消息功能测试'));
    		
    }
	
	//消息展示
    public function show()
    {
        $user = User::find(1);
        foreach ($user->Notifications as $notification) {
            dump($notification->data);
        }
    }

}

```