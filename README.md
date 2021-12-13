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
+ 编写操作用户表的UserDAO
UserDAO直接与存储用户数据的用户表进行交互。实现如下功能：
    1. 注册：将用户输入的用户名、密码、邮箱添加进用户表
    2. 判断用户是否存在：查询用户表中是否有与用户输入的用户名相一致的字段
    3. 判断用户账号密码是否输入正确：查询用户表中是否有与用户输入的用户名、密码相一致的字段
+ UserDAOTest类进行测试

#### Service层：编写UesrService， 封装UserDAO中的方法
+ 测试UserService
#### Web层：编写实现登录/注册功能的Servlet程序
+ 实现注册功能的Servelt程序
    + RegisterServlet:
    1. 获取用户请求的数据
    2. 判断验证码是否正确
         + 错误：返回注册页面并提示
         + 正确：判断注册用户是否已经被注册 
             + 错误：返回注册页面并提示
             + 正确：跳转到注册成功页面 
+ 实现登录功能的Servlet程序
    + LoginServlet: 
    1. 获取用户请求信息
    2. 判断用户是否存在
        + 不存在：返回登入页面并提示
        + 存在：判断用户账号密码是否输入正确
            + 正确：跳转登入成功页面
            + 错误：返回登入页面并提示

