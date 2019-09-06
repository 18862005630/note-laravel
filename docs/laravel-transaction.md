##### laravel-transaction
```
DB::connection('marketing')->beginTransaction();
try{
	//数据操作
	DB::connection('marketing')->commit();
}catch(\Exception $e){
	DB::connection('marketing')->rollBack();
	Log::channel('database')->info($e);
}
```

```
//多数据库操作(当前类继承BaseController)
$this->beginTransaction(['marketing','deal']);
try{
	//数据操作
	$this->commit();
}catch(\Exception $e){
	$this->rollBack();
	Log::channel('database')->info($e);
}


//BaseController  

<?php 
namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use Dingo\Api\Routing\Helpers;
use DB;

class BaseController extends Controller{
    use Helpers; 

    protected $state = 200;
    protected $txDatabases = [];

    public function getStatusCode(){
        return $this->statusCode;
    }

    public function setStatusCode($statusCode){
        $this->statusCode = $statusCode;
        return $this;
    }

    public function responseNotFound($msg = 'Failed'){
        return $this->responseError($msg);
    }

    private function responseError($msg){
        return json_encode([
                'state' => $this->getStatusCode(),
                'info' => $msg,
            ],JSON_UNESCAPED_UNICODE);
    }
    /**
     * Open multiple databases transactions
     *
     * @param  array   $txDatabases
     * @return null
     */
    public function beginTransaction($txDatabases){
        $this->txDatabases = $txDatabases;
        foreach($this->txDatabases as $db){
            DB::connection($db)->beginTransaction();
        }
    }
    /**
     * Commit multiple databases transactions
     *
     * @return null
     */
    public function commit(){
        foreach($this->txDatabases as $db){
            DB::connection($db)->commit();
        }
    }
    /**
     * Rollback multiple databases transactions
     *
     * @return null
     */
    public function rollBack(){
        foreach($this->txDatabases as $db){
            DB::connection($db)->rollBack();
        }
    }
    
}



```