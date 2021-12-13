# 书城项目
## 项目概要
```
该项目主要实现了用户的注册和登录功能。
通过使用Servlet，JavaEE三层（Web，Service，Dao层），以及Web关联的一些基础知识进行项目的开发。
项目中还涉及到一些常用的工具类：
例如
  数据库连接池工具：druid 
  数据库操作工具：commons-dbutils
```
## 项目收获


# 实现用户登入注册功能-第一阶段
### 阶段性收获
加深了对javaee三层架构（Web、Service、DAO）的理解
+ DAO层：操作数据库表：根据业务需求,写CRUD 
+ Service层：根据业务需求对DAO中方法再封装
+ Web层: 与浏览器交互

    - Servlet:
    1. 接收浏览器请求(用户的请求参数……)
    2. 运用Service层的方法实现功能
    3. 控制视图跳转(重定向,请求转发)

### 功能实现
+ 需求1：用户注册
1. 访问注册页面 
2. 填写注册信息，提交给服务器 
3. 服务器保存用户 
4. 当用户已经存在----提示用户注册失败，用户名已存在。
5. 当用户不存在-----注册成功

+ 需求 2：用户登陆
1. 访问登陆页面 
2. 填写用户名密码后提交 
3. 服务器判断用户是否存在 
4. 如果登陆失败 --->>>> 返回用户名或者密码错误信息 
5. 如果登录成功 --->>>>跳转到成功页面

### 实现思路
+ 实现步骤：
1. 根据要求创建存储用户信息数据的用户表
2. 编写与用户表数据相对应的JavaBean
3. DAO层：编写操作用户表的UserDAO
    + 注册
    + 判断用户是否存在
    + 判断用户名/密码是否输入正确
4. Service层：编写UserService，封装UserDAO中的方法 
5. Web层：编写servlet程序分别实现注册/登录功能

## 实现流程

### 准备阶段
#### 构建静态网页
+ 导入web资源文件：

#### 创建src目录:
+ web层    
+ service层
+ dao层
+ bean包
+ utils包
+ 测试包

#### 导入jar包

### 编码阶段
#### 创建数据库表
```sql
//创建BookShop数据库
  create database BookShop;

  //创建用户表
  create table t_user(
    `id` int primary key auto_increment,
    `username` varchar(20) not null unique,
    `password` varchar(32) not null,
    `email` varchar(200),
  );
```
#### 创建与用户表相对应的JavaBean
#### DAO层：编写DAO操作用户表
+ 导入工具类Jdbcutils、相关配置文件
+ 通过JdbcUtilsTest类进行测试  
+ 导入BaseDAO（这里自己手写）
```java
package yoika.dao;

import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;
import yoika.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

public class BaseDAO {
    private QueryRunner queryRunner = new QueryRunner();

    //update()方法执行 Insert/Update/Delete语句
    public int update(String sql,Object... args){
    //使用JdbcUtils工具类创建连接池
    //与数据库进行连接
        Connection connection = JdbcUtils.getConnection();
        try {
            return queryRunner.update(connection,sql,args);
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.close(connection);
        }
     //返回-1执行失败
        return  -1;
    }

   //查询返回一个 javaBean 的 sql 语句（查询结果为一列）
    public <T> Object queryForOne(Class<T> type,String sql,Object...args){
        Connection connection = JdbcUtils.getConnection();

        try {
            return  queryRunner.query(connection,sql,args,new BeanHandler<T>(type));
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.close(connection);
        }
           return  null;
    }

   //查询返回多个javaBean的sql语句（查询结果为多列）
    public <T> List<T> queryForList(Class<T> type,String sql,Object... args ){
        Connection connection = JdbcUtils.getConnection();
        try {
            queryRunner.query(connection,sql,new BeanListHandler<T>(type),args);
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.close(connection);
        }
   return null;
    }

  //查询返回一列一行的sql语句

  public Object queryForSingleValue(String sql, Object... args){
      Connection conn = JdbcUtils.getConnection();
      try {
          return queryRunner.query(conn, sql, new ScalarHandler(), args);
      } catch (Exception e) {
          e.printStackTrace();
      } finally {
          JdbcUtils.close(conn);
      }
      return null;
  }
}
```
+ 编写操作用户表的UserDAO
UserDAO直接与存储用户数据的用户表进行交互。实现如下功能：
    1. 注册：将用户输入的用户名、密码、邮箱添加进用户表
    2. 判断用户是否存在：查询用户表中是否有与用户输入的用户名相一致的字段
    3. 判断用户账号密码是否输入正确：查询用户表中是否有与用户输入的用户名、密码相一致的字段
```java
package yoika.dao;
import yoika.Bean.userbean;
public class UserDAO extends BaseDAO{

    //注册:注册成功返回1，注册失败返回-1

    public int saveUser(userbean user){
     String sql="insert into t_user(`username`,`password`,`email`) values(?,?,?)";
     return update(sql,user.getUsername(),user.getPassword(),user.getEmail() );

    }

   //判断用户是否存在：不存在返回null，存在返回匹配的javabean
    public Object queryForUsername(String usename){
        String sql ="select * from t_user where username=?";
        return queryForOne(userbean.class,sql,usename);
    }
   //判断用户名/密码是否输入正确：不正确返回null，正确存在返回匹配的javabean
     public  Object queryForUsernameAndPassword(String username,String password){
        String sql="select * from t_user where username=? and password=?";
       return  queryForOne(userbean.class,sql,username,password);
     }
}
```
+ UserDAOTest类进行测试

#### Service层：编写UesrService， 封装UserDAO中的方法
```java
package yoika.service;


import yoika.Bean.userbean;
import yoika.dao.UserDAO;

public class UserService {
   //创建UserDAO对象
    UserDAO userDAO = new UserDAO();

   //注册
   //被注册返回false
    public boolean registUser(userbean user){
        int i = userDAO.saveUser(user);
       if (i==-1){
           return false;
       }else {
           return true;
       }
    }

    //判断用户是否存在
    //不存在返回false
    public  boolean existUser(String username){
        if (userDAO.queryForUsername(username)==null){
            return false;
        }else {
            return true;
        }
    }
   //判断用户名/密码是否输入正确
   //不正确返回false
   public boolean login(String username,String password){
       if(userDAO.queryForUsernameAndPassword(username,password)==null){
         return false;
       }else {
           return true;
       }
   }
}
```
+ 测试UserService
