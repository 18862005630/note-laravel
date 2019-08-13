# laravel-excel-export纯数据(未导出图片信息)导出到excel
#### 配置信息
![](https://i.imgur.com/9f5bfwF.png)

## 利用maatwebsite3.1.0扩展包开发该导出功能

#### 1、安装maatwebsite3.1.0
```
//利用阿里云镜像
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
//安装
composer require maatwebsite/excel:~3.1.0
```

#### 2、发布配置
```
php artisan vendor:publish
```

#### 3、生成导出类(App\Exports\UserExport)
```
php artisan make:export UserExport --model=User
```

#### 4、编辑导出类
```
<?php

namespace App\Exports;

use Maatwebsite\Excel\Concerns\FromCollection;
use Maatwebsite\Excel\Concerns\Exportable;
Use Maatwebsite\Excel\Concerns\WithHeadings;

class UserExport implements FromCollection,WithHeadings
{
	use Exportable;

	private $data;
	private $headings;

	//数据注入
	public function __construct($data,$headings){
		$this->data = $data;
		$this->headings = $headings;
	}

    /**
     * 实现接口FromCollection
    * @return \Illuminate\Support\Collection
    */
    public function collection()
    {
        return collect($this->data);
    }

    //实现withHeadings接口
    public function headings():array
    {
    	return $this->headings;
    }
}

```

#### 5、控制器调用导出类实现导出功能
```
<?php

namespace App\Http\Controllers\Index;
use Maatwebsite\Excel\Facades\Excel;
use App\Exports\UserExport;
use App\User;

class IndexController extends Controller
{
    public function index(){
    		$data = User::select('name','email')->get();
    		$headings=['name','email'];
    		return Excel::download(new UserExport($data,$headings),'users.csv');
    }

}

```