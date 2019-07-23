# laravel-tips

#### base64_encode以及base64_decode
base64_encode对字符串加密
应用场景：如emoji的存储入库，需要加密存入数据库，取出时要解密

若解密导致字符串编码不一致，需要转编码
```
	$code=mb_detect_encoding(base64_decode($str), array("ASCII",'UTF-8',"GB2312","GBK",'BIG5'));//判断当前字符串的编码格式，不存在即false
	mb_convert_encoding(base64_decode($str), 'UTF-8', $oldcode);//将字符串由原来的编码格式转为UTF-8，第三个参数可以不写
```
 emoji解密后为UTF-8格式，其他字符串解密后为非UTF-8格式，所以统一转为UTF-8   
```
mb_convert_encoding(base64_decode($str), 'UTF-8');
```