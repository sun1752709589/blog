# 如何在Ubuntu18.04上面安装postgresql11
```python
1. vi /etc/apt/sources.list.d/pgdg.list
   add [deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main]
2. wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
3. sudo apt-get update
4. sudo apt-get install postgresql-11
this will install below packages:
postgresql-client-11	client libraries and client binaries
postgresql-11	core database server
postgresql-contrib-9.x	additional supplied modules (part of the postgresql-xx package in version 10 and later)
libpq-dev	libraries and headers for C language frontend development
postgresql-server-dev-11	libraries and headers for C language backend development
pgadmin4	pgAdmin 4 graphical administration utility
```