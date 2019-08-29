# 如何在Ubuntu18.04上面安装postgresql11
Postgresql，强大的关系型数据库，号称最像Oracle数据库的数据库，连续蝉联2017、2018年数据库年度冠军，丰富的插件机制使其几乎成为了一个没有短板的数据库，你即可以做全文检索，又可以当时序数据库，甚至借助插件你可以把它当nosql使用，今天咱们来介绍一下如何在Ubuntu18上面安装PG这头大象。

## 1.修改source源
vi /etc/apt/sources.list.d/pgdg.list
```python
# add below line to pgdb.list
deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main
```

## 2.添加源仓库签名
```python
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```

## 3.更新源然后安装PG最新稳定版pg11.5
```python
sudo apt-get update
sudo apt-get install postgresql-11
```