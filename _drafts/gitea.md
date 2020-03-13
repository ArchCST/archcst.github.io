# Gitea 配置
## 为 Gitea 分配数据库和数据库用户
按照 [官方英文文档](https://docs.gitea.io/en-us/database-prep/) 配置。
为 Gitea 创建一个数据库，首先通过 `mysql -u root -p` 登陆数据库，然后再 MySQL 命令提示符下：
```sql
# 取消对老版本密码的支持
SET old_passwords=0; 

# 添加 Gitea 的数据库用户
CREATE USER 'gitea' IDENTIFIED BY '密码'; -- 远程数据库的话用户名后面要跟上『@'Gite服务器的ip地址'』

# 创建数据库，注意使用官方指定的字符集
CREATE DATABASE giteadb CHARACTER SET 'utf8mb4' COLLATE 'utf8mb4_unicode_ci';

# 给 Gitea 用户所有 giteadb 库的权限
GRANT ALL PRIVILEGES ON giteadb.* TO 'gitea';
FLUSH PRIVILEGES;
```
然后 `exit` 退出 MySQL，用 `mysql -u gitea -p` 登陆看看，能登陆就ok了

## 安装
访问 CVM 的 `公网IP:3000` 进入 Gitea 首页，点击右上方注册登录都可以，就进入初始配置页面了。
数据库如果和Gitea 放一起的话，数据库主机就填 127.0.0.1:3306 即可。
字符集一定选成创建时一致的字符集， `utf8mb4`
数据库名要填成创建的名称，`giteadb`

