上一次，我们完成[域名解析](https://mp.weixin.qq.com/s/9Tn5d2ey2lr06oGPZSp6Qw)后，发现浏览器地址栏里的域名被提示为不安全，就是因为它还是个宝宝，没有升级为 HTTPS 证书。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-01.png)

那怎么升级为 HTTPS 证书呢？可以直接通过阿里云购买 SSL 证书，但特么巨贵！

本来想尝试一下 AWS 的免费 SSL 证书，但卡到验证码这一步就是收不到信息。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-02.png)

索性就还用 FreeSSL 吧。

FreeSSL.cn 是一个提供免费HTTPS证书申请的网站，网址如下：

>https://freessl.cn

输入域名 tobebetterjavaer.com 选择 trustAsia 品牌证书，点击「创建」，这次我选择的是三年期自动化（刚好我的服务器申请的是三年，域名也是三年），9.9 元，还是非常良心的。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-03.png)

微信/支付宝支付完成后会跳到证书的订单列表。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-04.png)

选择「更多操作」里的订单详情，会跳转到 CertCloud 页的管理订单。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-05.png)

点击「提交 CSR」后点击「提交」。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-06.png)

接下来就到了域名验证环节，点击「获取验证信息」。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-07.png)

切换到域名解析设置页，准备添加记录。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-08.png)

按照 CertCloud 提供的域名验证信息，添加记录。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-09.png)

添加完成后切换到 CertCloud，点击「域名验证」。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-10.png)

如果不确定上一步的记录是否添加成功，可以点击「诊断」按钮进行测试，如果没有问题会提示匹配成功的信息。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-11.png)

之后，点击「我已完成配置，检测一下」，如果没有问题，会先提示等待 CA 颁发证书，之后再次检测会提示「证书已签发，请刷新页面查看」。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-12.png)

好的，直接刷新页面，可以看到订单状态已经变成「已签发」的状态。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-13.png)

点击证书操作中的「下载证书」，选择适用于 Nginx 的 PEM 格式证书，点击下载。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-14.png)

使用 [Tabby 终端](https://mp.weixin.qq.com/s/HeUAPe4LqqjfzIeWDe8KIg)的「SFTP」将证书上传到网站的云服务器。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-15.png)

打开[宝塔面板](https://mp.weixin.qq.com/s/ditN9J80rSWwnYRumwb4ww)，准备配置 Nginx 的 SSL 证书。将以下信息复制到 Nginx 的配置文件中，保存后重新加载配置。

```
# HTTPS server

server {
    listen       443 ssl;
    server_name  localhost;

    ssl_certificate      /home/cert/nginx/tobebetterjavaer.com_cert_chain.pem;
    ssl_certificate_key  /home/cert/nginx/tobebetterjavaer.com_key.key;

    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers  on;

    location / {
        root   /home/www;
        index  index.html index.htm;
    }
}
```

记得在宝塔面板和云服务器后台放行 443 端口。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-16.png)

在地址栏访问 `https://tobebetterjavaer.com` 就可以看到我们的域名已经升级为 HTTPS 了（安全锁的小图标也显示出来了）。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-17.png)

这时候，如果我们访问 80 端口的 http，仍然是可以的。只不过仍然会显示一个不安全的提示。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-18.png)

此时，我们需要将 HTTP 重定向到 HTTPS。

```
server {
    listen       80;
    server_name  tobebetterjavaer.com www.tobebetterjavaer.com;
    return 301 https://$server_name$request_uri;
}
```

注释掉原来的 80 端口监听，改为 return 跳转。


![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-19.png)

再次刷新原来的 HTTP 访问链接，可以看到已经跳转到 HTTPS 了，如果你查看地址栏的话，也会看到地址变成了 `https://tobebetterjavaer.com`。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/szjy/tobebetterjavaer-https-20.png)


顺带再给大家分享一个好消息，《Java 程序员进阶之路》网站 PV 访问人数已经突破了 1000，来到了 1168，又一个小小的里程碑~

![](https://img-blog.csdnimg.cn/09c38e3385254f3081e59ce9cd350229.png)

三年之后又三年，希望这个小破站能自力更生地活下去。目前已有的花费有：

- 阿里云服务器：3 年 204 元
- 域名：3 年 273 元
- SSL 证书：3 年 9.9 元

希望能给学习 Java 的小伙伴提供一点点帮助，二哥就感觉值了！