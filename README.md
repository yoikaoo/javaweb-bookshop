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
## 阶段性收获
加深了对javaee三层架构（Web、Service、DAO）的理解
+ DAO层：操作数据库表：根据业务需求,写CRUD 
+ Service层：根据业务需求对DAO中方法再封装
+ Web层: 与浏览器交互

    - Servlet:
    1. 接收浏览器请求(用户的请求参数……)
    2. 运用Service层的方法实现功能
    3. 控制视图跳转(重定向,请求转发)

## 功能实现
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

## 实现思路
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
1. 创建数据库表
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
2. 创建与用户表相对应的JavaBean 
```java
package yoika.Bean;
public class userbean {
    private Integer id;
    private String username;
    private String password;
    private String email;

    public userbean() {
    }

    public userbean(Integer id, String username, String password, String email) {
        this.id = id;
        this.username = username;
        this.password = password;
        this.email = email;
    }

    @Override
    public String toString() {
        return "userbean{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", email='" + email + '\'' +
                '}';
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public Integer getId() {
        return id;
    }

    public String getUsername() {
        return username;
    }

    public String getPassword() {
        return password;
    }

    public String getEmail() {
        return email;
    }
}
```



