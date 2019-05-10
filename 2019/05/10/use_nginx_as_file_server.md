# 如何将nginx当做静态资源服务器
众所周知，nginx是一款优秀的反向代理服务器。nginx以其强大的性能俘获了不少开发者的芳心。然而nginx还有一个很重要的功能就是当做静态资源服务器，今天来分享一下如何配置。其基本配置有二种方式：alias和root，下面就来具体示例说明一下。

假设我的域名是：alipay.com并且已经配置好域名解析。

### alias
```ruby
server{
    listen 80;
    server_name test.alipay.com;
    location /doc/ {
        alias /you/file/dir/;
        autoindex on; # 索引，当输入某和路径时列出此路径下所有文件
        autoindex_exact_size on; # 显示文件大小
        autoindex_localtime on; # 显示文件时间
    }
}
```
此时，通过浏览器访问http://test.alipay.com/doc/test.txt，则访问服务器的文件是/you/file/dir/test.txt。即使用`/you/file/dir/`替换了`/doc/`。

### root
```ruby
server{
    listen 80;
    server_name test.alipay.com;
    location /doc/ {
        root /you/file/dir;
    }
}
```
此时，通过浏览器访问http://test.alipay.com/doc/test.txt，则访问服务器的文件是/you/file/dir/doc/test.txt。即`/you/file/dir`+`/doc/test.txt`


### alias和root的区别
只需记住一点，alias英文意思就是：别名，在这儿恰如其名。

1. root的处理结果是：root路径＋location路径；alias的处理结果是：使用alias路径替换location路径
2. 使用alias时，目录名后面一定要加`/`。
3. alias只能位于location块中。（root可以不放在location中）

### 实际配置时需要注意的点
由于nginx的worker使用www-data用户跑起来的，所以你的`/you/file/dir/`路径用户www-data一定要有访问权限，否则当你访问文件时nginx会直接返回403无权限拒绝访问。

可参考：https://stackoverflow.com/questions/16808813/nginx-serve-static-file-and-got-403-forbidden