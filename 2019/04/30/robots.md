# 详解robots协议之robots.txt
网站站长们使用robots.txt文件告诉那些网络蜘蛛们：什么该抓什么不该抓，这就是大名鼎鼎的robots协议【Robots Exclusion Protocol】。此协议是事实上的行业标准，不属于任何组织。

## 需要注意的几个地方
1. robots协议的遵守只能靠道德，蜘蛛们可以忽略此文件。
2. robots.txt是一个你的网站公开的任何人都可读的文件。
3. 对于不想被访问的路径，做好还是自己做好权限验证！

## 该文件该放在那里
一般放在网站的public目录。以rails为例，该位置为：/you/project/path/public/robots.txt

## 下面我们具体举例说明这个robots文件该怎么写！
任何蜘蛛，任何内容都不允许被抓取：
```ruby
User-agent: * # 允许所有的蜘蛛
Disallow: / # 不允许抓取此网站任何网页
```
允许任何蜘蛛抓取网站任何内容：
```ruby
User-agent: *
Disallow:
```
允许任何蜘蛛抓取网站除几个路径的内容：
```ruby
User-agent: *
Disallow: /cgi-bin/
Disallow: /tmp/
Disallow: /junk/
```
仅排斥单个蜘蛛：
```ruby
User-agent: BadBot
Disallow: /
```
只允许单个蜘蛛抓取：
```ruby
User-agent: Google
Disallow:

User-agent: *
Disallow: /
```

由于此协议没有`Allow`配置项，因此只想让蜘蛛抓取单个路径有点尴尬，一种方案是把其他的路径全部Disallow。

我们日常网站中，对于一些敏感的路径比如`/admin`，应该加入Disallow。

## 参考文献
http://www.robotstxt.org/robotstxt.html

