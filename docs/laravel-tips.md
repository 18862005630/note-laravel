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


#### stripslashes
过滤json数据中的转义字符\
$data=stripslashes($data);


#### 前端脚本
1 勾勒页面标签结构（外框描绘，结构清晰）
```
javascript: (function() { var elements = document.body.getElementsByTagName('*'); var items = []; for (var i = 0; i < elements.length; i++) { if (elements[i].innerHTML.indexOf('html * { outline: 1px solid red }') != -1) { items.push(elements[i]); } } if (items.length > 0) { for (var i = 0; i < items.length; i++) { items[i].innerHTML = ''; } } else { document.body.innerHTML += '<style>html * { outline: 1px solid red }</style>'; } })();
```
1 勾勒页面标签结构(标签背景色描绘，好看但不清晰)
```
javascript: (function() { var css = document.createElement("style"); css.innerHTML = `* { background-color: rgba(255,0,0,.2); } * * { background-color: rgba(0,255,0,.2); } * * * { background-color: rgba(0,0,255,.2); } * * * * { background-color: rgba(255,0,255,.2); } * * * * * { background-color: rgba(0,255,255,.2); } * * * * * * { background-color: rgba(255,255,0,.2); } * * * * * * * { background-color: rgba(255,0,0,.2); } * * * * * * * * { background-color: rgba(0,255,0,.2); } * * * * * * * * * { background-color: rgba(0,0,255,.2); } * * * * * * * * * * { background-color: rgba(0,0,255,.2); } `; document.querySelector("head").appendChild(css); })();
```
1 git历史记录查看（新开页面，以轮播形式查看历史记录）
```
javascript: (function() { var url = window.location.href; var regEx = /^(https?\:\/\/)(www\.)?(github|gitlab|bitbucket)\.(com|org)\/(.*)$/i; if (regEx.test(url)) { url = url.replace(regEx, "$1$3.githistory.xyz/$5"); window.open(url, "_blank"); } else { alert("Not a Git File URL"); } })();
```