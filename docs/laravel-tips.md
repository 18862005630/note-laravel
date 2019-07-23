# laravel-tips

#### base64_encode以及base64_decode
base64_encode对字符串加密
应用场景：如emoji的存储入库，需要加密存入数据库，取出时要解密

若解密导致字符串编码不一致，需要转编码
    ![](https://i.imgur.com/Q555uvV.png)
    

