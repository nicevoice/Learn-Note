# MYSQL 访问控制

## 创建用户账号  
`CREATE USER ben IDENTIFIED BY 'password'`

## 重命名用户账号  
`RENAME USER ben`

##  删除用户账号  
`DROP USER ben`

## 设置访问权限  
* 显示指定用户拥有的权限  
`SHOW GRANTS FOR ben`  
* 给于指定用户在`user_test`上的权限， 如果是整个数据库用`GRANT ALL`    
`GRANT SELECT ON user_test.* TO rogee`  
* 去除用户在`user_test`上的一个权限,多个权限用逗号分割, 如果是整个服务器用`REVOKE ALL`  
`REVOKE SELECT ON user_test.* from rogee` 

## 更改口令  
`SET PASSWORD FOR rogee = PASSWORD('new pass')`  

