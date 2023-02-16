# SSL-自签名证书

工具：openssl

网址：https://www.136.la/shida/show-135582.html

```shell
## 生成私钥和证书
openssl req -newkey rsa:2048 -keyout my.key -subj "/C=XX/O=XX/OU=XX/OU=XX/OU=XX/CN=wzj.com" -x509 -days 36500 -out my.crt 
# 输入m'y
```

生成过程：

![image-20220324140854319](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141605379.png)

生成结果：

![image-20220324140335711](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141605625.png)

```shell
## 生成公钥
## 证书中包含了公钥, 因此直接使用证书生成
openssl x509 -in my.crt -outform PEM -out my.pem
```

生成结果：

![image-20220324140503668](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141606296.png)



得到了三个文件, 私钥`my.key`, 公钥`my.pem`, 证书`my.crt`

```shell
# 生成pfx文件
# pfx需要私钥, 公钥, 证书
openssl pkcs12 -export -out my.pfx -inkey my.key -in my.pem -certfile my.crt
```

生成过程：

![image-20220324141321913](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141606720.png)

生成结果：

![image-20220324141050860](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141606475.png)

