# 黑马-苍穹外卖项目学习笔记

## ——BY mozijie

## 0.说明

> 该课程是黑马程序员springboot的经典项目课程
> 
> 课程地址：https://www.bilibili.com/video/BV1TP411v7v6/?spm_id_from=333.788.comment.all.click&vd_source=7c23485904ea3584fb46e603e43ca789
> 
>


# 苍穹外卖-day01

## 2025.07.06学完

## 课程内容

- 软件开发整体介绍
- 苍穹外卖项目介绍
- 开发环境搭建
- 导入接口文档
- Swagger



**项目整体效果展示：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106160753928.png" alt="image-20221106160753928" style="zoom:67%;" />

​																		        管理端-外卖商家使用

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106160905554.png" alt="image-20221106160905554" style="zoom:67%;" />

​																			    用户端-点餐用户使用

当我们完成该项目的学习，可以培养以下能力：

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106161507973.png" alt="image-20221106161507973" style="zoom:67%;" />

## 1. 软件开发整体介绍

作为一名软件开发工程师,我们需要了解在软件开发过程中的开发流程， 以及软件开发过程中涉及到的岗位角色，角色的分工、职责， 并了解软件开发中涉及到的三种软件环境。那么这一小节，我们将从 软件开发流程、角色分工、软件环境 三个方面整体介绍一下软件开发。



### 1.1 软件开发流程

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106162815172.png" alt="image-20221106162815172"  style="zoom:80%;" />



**1). 第1阶段: 需求分析**

完成需求规格说明书、产品原型编写。  

需求规格说明书， 一般来说就是使用 Word 文档来描述当前项目的各个组成部分，如：系统定义、应用环境、功能规格、性能需求等，都会在文档中描述。**例如：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106163758703.png" alt="image-20221106163758703" style="zoom: 50%;" />

产品原型，一般是通过网页(html)的形式展示当前的页面展示什么样的数据, 页面的布局是什么样子的，点击某个菜单，打开什么页面，点击某个按钮，出现什么效果，都可以通过产品原型看到。 **例如：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106163925031.png" alt="image-20221106163925031" style="zoom:50%;" />





**2). 第2阶段: 设计**

设计的内容包含 UI设计、数据库设计、接口设计。

UI设计：用户界面的设计，主要设计项目的页面效果，小到一个按钮，大到一个页面布局，还有人机交互逻辑的体现。**例如：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106165303946.png" alt="image-20221106165303946" style="zoom:50%;" />

数据库设计：需要设计当前项目中涉及到哪些数据库，每一个数据库里面包含哪些表，这些表结构之间的关系是什么样的，表结构中包含哪些字段。**例如：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106165554917.png" alt="image-20221106165554917" style="zoom:50%;" />

接口设计：通过分析原型图，首先，粗粒度地分析每个页面有多少接口，然后，再细粒度地分析每个接口的传入参数，返回值参数，同时明确接口路径及请求方式。**例如：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106171538880.png" alt="image-20221106171538880" style="zoom:50%;" />



**3). 第3阶段: 编码**

编写项目代码、并完成单元测试。

项目代码编写：作为软件开发工程师，我们需要对项目的模块功能分析后，进行编码实现。

单元测试：编码实现完毕后，进行单元测试，单元测试通过后再进入到下一阶段。**例如：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106175351594.png" alt="image-20221106175351594" style="zoom:50%;" />





**4). 第4阶段: 测试**

在该阶段中主要由测试人员, 对部署在测试环境的项目进行功能测试, 并出具测试报告。



**5). 第5阶段: 上线运维**

在项目上线之前， 会由运维人员准备服务器上的软件环境安装、配置， 配置完毕后， 再将我们开发好的项目，部署在服务器上运行。



### 1.2 角色分工

在对整个软件开发流程熟悉后， 我们还有必要了解一下在整个软件开发流程中涉及到的岗位角色，以及各个角色的职责分工。

![image-20210725234015404](mozijie_notebook.assets/image-20210725234015404.png)

| 岗位/角色                           | 对应阶段                      | 职责/分工                                  |
| :---------------------------------- | ----------------------------- | :----------------------------------------- |
| 项目经理                            | 全阶段                        | 对整个项目负责，任务分配、把控进度         |
| 产品经理                            | 需求分析                      | 进行需求调研，输出需求调研文档、产品原型等 |
| UI设计师                            | 设计                          | 根据产品原型输出界面效果图                 |
| 架构师                              | 设计                          | 项目整体架构设计、技术选型等               |
| <font color='red'>开发工程师</font> | <font color='red'>编码</font> | <font color='red'>功能代码实现</font>      |
| 测试工程师                          | 测试                          | 编写测试用例，输出测试报告                 |
| 运维工程师                          | 上线运维                      | 软件环境搭建、项目上线                     |



上述我们讲解的角色分工, 是在一个项目组中比较标准的角色分工, 但是在实际的项目中, 有一些项目组由于人员配置紧张, 可能并没有专门的架构师或测试人员, 这个时候可能需要有项目经理或者程序员兼任。



### 1.3 软件环境

作为软件开发工程师，在编码的过程中就不可避免地会接触多种软件环境，我们主要来分析在工作中经常遇到的三套环境， 分别是: 开发环境、测试环境、生产环境。 接下来，我们分别介绍一下这三套环境的作用和特点。



**1). 开发环境(development)**

我们作为软件开发人员，在开发阶段使用的环境，就是开发环境，一般外部用户无法访问。

比如，我们在开发中使用的MySQL数据库和其他的一些常用软件，我们可以安装在本地， 也可以安装在一台专门的服务器中， 这些应用软件仅仅在软件开发过程中使用， 项目测试、上线时，我们不会使用这套环境了，这个环境就是开发环境。



**2). 测试环境(testing)**

当软件开发工程师，将项目的功能模块开发完毕，并且单元测试通过后，就需要将项目部署到测试服务器上，让测试人员对项目进行测试。那这台测试服务器就是专门给测试人员使用的环境， 也就是测试环境，用于项目测试，一般外部用户无法访问。



**3). 生产环境(production)**

当项目开发完毕，并且由测试人员测试通过之后，就可以上线项目，将项目部署到线上环境，并正式对外提供服务，这个线上环境也称之为生产环境。



​			        **开发环境**                                                         **测试环境**                                                **生产环境**

![image-20221106182912829](mozijie_notebook.assets/image-20221106182912829.png)



首先，会在开发环境中进行项目开发，往往开发环境大多数都是本地的电脑环境和局域网内的环境，当开发完毕后，然后会把项目部署到测试环境，测试环境一般是一台独立测试服务器的环境，项目测试通过后，最终把项目部署到生产环境，生产环境可以是机房或者云服务器等线上环境。



## 2. 苍穹外卖项目介绍

在开发苍穹外卖这个项目之前，我们需要全方位的来介绍一下当前我们学习的这个项目。接下来，我们将从项目简介、产品原型、技术选型三个方面来介绍苍穹外卖这个项目。



### 2.1 项目介绍

本项目（苍穹外卖）是专门为餐饮企业（餐厅、饭店）定制的一款软件产品，包括 系统管理后台 和 小程序端应用 两部分。其中系统管理后台主要提供给餐饮企业内部员工使用，可以对餐厅的分类、菜品、套餐、订单、员工等进行管理维护，对餐厅的各类数据进行统计，同时也可进行来单语音播报功能。小程序端主要提供给消费者使用，可以在线浏览菜品、添加购物车、下单、支付、催单等。

![image-20221106185252326](mozijie_notebook.assets/image-20221106185252326.png)



接下来，通过功能架构图来展示**管理端**和**用户端**的具体业务功能模块。

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106194424735.png" alt="image-20221106194424735" style="zoom: 67%;" />

**1). 管理端功能**

员工登录/退出 , 员工信息管理 , 分类管理 , 菜品管理 , 套餐管理 , 菜品口味管理 , 订单管理 ，数据统计，来单提醒。

**2). 用户端功能**

 微信登录 , 收件人地址管理 , 用户历史订单查询 , 菜品规格查询 , 购物车功能 , 下单 , 支付、分类及菜品浏览。



### 2.2 产品原型

**产品原型**，用于展示项目的业务功能，一般由产品经理进行设计。

> **<font color='red'>注意事项：</font>** 产品原型主要用于展示项目的功能，并不是最终的页面效果。



在课程资料的产品原型文件夹下,提供了两份产品原型。

![image-20221106195023552](mozijie_notebook.assets/image-20221106195023552.png) 

**管理端原型图：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106195259858.png" alt="image-20221106195259858" style="zoom:50%;" />

**用户端原型图：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106195354556.png" alt="image-20221106195354556" style="zoom:50%;" />

**1). 管理端**

餐饮企业内部员工使用。 主要功能有: 

| 模块      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| 登录/退出 | 内部员工必须登录后,才可以访问系统管理后台                    |
| 员工管理  | 管理员可以在系统后台对员工信息进行管理，包含查询、新增、编辑、禁用等功能 |
| 分类管理  | 主要对当前餐厅经营的 菜品分类 或 套餐分类 进行管理维护， 包含查询、新增、修改、删除等功能 |
| 菜品管理  | 主要维护各个分类下的菜品信息，包含查询、新增、修改、删除、启售、停售等功能 |
| 套餐管理  | 主要维护当前餐厅中的套餐信息，包含查询、新增、修改、删除、启售、停售等功能 |
| 订单管理  | 主要维护用户在移动端下的订单信息，包含查询、取消、派送、完成，以及订单报表下载等功能 |
| 数据统计  | 主要完成对餐厅的各类数据统计，如营业额、用户数量、订单等     |



**2). 用户端**

移动端应用主要提供给消费者使用。主要功能有:

| 模块        | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| 登录/退出   | 用户需要通过微信授权后登录使用小程序进行点餐                 |
| 点餐-菜单   | 在点餐界面需要展示出菜品分类/套餐分类, 并根据当前选择的分类加载其中的菜品信息, 供用户查询选择 |
| 点餐-购物车 | 用户选中的菜品就会加入用户的购物车, 主要包含 查询购物车、加入购物车、删除购物车、清空购物车等功能 |
| 订单支付    | 用户选完菜品/套餐后, 可以对购物车菜品进行结算支付, 这时就需要进行订单的支付 |
| 个人信息    | 在个人中心页面中会展示当前用户的基本信息, 用户可以管理收货地址, 也可以查询历史订单数据 |



### 2.3 技术选型

关于本项目的技术选型, 我们将会从 用户层、网关层、应用层、数据层 这几个方面进行介绍，主要用于展示项目中使用到的技术框架和中间件等。

![image-20221106185646994](mozijie_notebook.assets/image-20221106185646994.png)



**1). 用户层**

本项目中在构建系统管理后台的前端页面，我们会用到H5、Vue.js、ElementUI、apache echarts(展示图表)等技术。而在构建移动端应用时，我们会使用到微信小程序。



**2). 网关层**

Nginx是一个服务器，主要用来作为Http服务器，部署静态资源，访问性能高。在Nginx中还有两个比较重要的作用： 反向代理和负载均衡， 在进行项目部署时，要实现Tomcat的负载均衡，就可以通过Nginx来实现。



**3). 应用层**

SpringBoot： 快速构建Spring项目, 采用 "约定优于配置" 的思想, 简化Spring项目的配置开发。

SpringMVC：SpringMVC是spring框架的一个模块，springmvc和spring无需通过中间整合层进行整合，可以无缝集成。

Spring Task:  由Spring提供的定时任务框架。

httpclient:  主要实现了对http请求的发送。

Spring Cache:  由Spring提供的数据缓存框架

JWT:  用于对应用程序上的用户进行身份验证的标记。

阿里云OSS:  对象存储服务，在项目中主要存储文件，如图片等。

Swagger： 可以自动的帮助开发人员生成接口文档，并对接口进行测试。

POI:  封装了对Excel表格的常用操作。

WebSocket: 一种通信网络协议，使客户端和服务器之间的数据交换更加简单，用于项目的来单、催单功能实现。



**4). 数据层**

MySQL： 关系型数据库, 本项目的核心业务数据都会采用MySQL进行存储。

Redis： 基于key-value格式存储的内存数据库, 访问速度快, 经常使用它做缓存。

Mybatis： 本项目持久层将会使用Mybatis开发。

pagehelper:  分页插件。

spring data redis:  简化java代码操作Redis的API。



**5). 工具**

git: 版本控制工具, 在团队协作中, 使用该工具对项目中的代码进行管理。

maven: 项目构建工具。

junit：单元测试工具，开发人员功能实现完毕后，需要通过junit对功能进行单元测试。

postman:  接口测工具，模拟用户发起的各类HTTP请求，获取对应的响应结果。



## 3. 开发环境搭建

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106200821282.png" alt="image-20221106200821282" style="zoom: 67%;" />

开发环境搭建主要包含**前端环境**和**后端环境**两部分。作为服务端开发工程师， 我们课程学习的重心应该放在后端的业务代码上， 前端的页面我们只需要导入资料中的nginx， 前端页面的代码我们只需要能看懂即可。



### 3.1 前端环境搭建

**1). 前端工程基于 nginx** 

从资料中找到前端运行环境的nginx，移动到**非中文目录**下。

![image-20221106202239828](mozijie_notebook.assets/image-20221106202239828.png)

**sky**目录中存放了管理端的前端资源，具体如下：

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106202517738.png" alt="image-20221106202517738" style="zoom:50%;" />

**2). 启动nginx，访问测试** 

双击 nginx.exe 即可启动 nginx 服务，访问端口号为 80

http://localhost:80

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221106202720190.png" alt="image-20221106202720190" style="zoom:50%;" />

### 3.2 后端环境搭建

#### 3.2.1 熟悉项目结构

后端工程基于 maven 进行项目构建，并且进行分模块开发。

**1). 从当天资料中找到后端初始工程：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107092342140.png" alt="image-20221107092342140" style="zoom:67%;" />

**2). 用 IDEA 打开初始工程，了解项目的整体结构：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107092540194.png" alt="image-20221107092540194" style="zoom: 50%;" />

对工程的每个模块作用说明：

| **序号** | **名称**     | **说明**                                                     |
| -------- | ------------ | ------------------------------------------------------------ |
| 1        | sky-take-out | maven父工程，统一管理依赖版本，聚合其他子模块                |
| 2        | sky-common   | 子模块，存放公共类，例如：工具类、常量类、异常类等           |
| 3        | sky-pojo     | 子模块，存放实体类、VO、DTO等                                |
| 4        | sky-server   | 子模块，后端服务，存放配置文件、Controller、Service、Mapper等 |

对项目整体结构了解后，接下来我们详细分析上述的每个子模块：



- **sky-common:** 模块中存放的是一些公共类，可以供其他模块使用

  <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107093606590.png" alt="image-20221107093606590" style="zoom: 50%;" />

  

  分析sky-common模块的每个包的作用：

  | 名称        | 说明                           |
  | ----------- | ------------------------------ |
  | constant    | 存放相关常量类                 |
  | context     | 存放上下文类                   |
  | enumeration | 项目的枚举类存储               |
  | exception   | 存放自定义异常类               |
  | json        | 处理json转换的类               |
  | properties  | 存放SpringBoot相关的配置属性类 |
  | result      | 返回结果类的封装               |
  | utils       | 常用工具类                     |

  

- **sky-pojo:** 模块中存放的是一些 entity、DTO、VO

  <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107094611987.png" alt="image-20221107094611987" style="zoom: 50%;" />

  分析sky-pojo模块的每个包的作用：

  | **名称** | **说明**                                     |
  | -------- | -------------------------------------------- |
  | Entity   | 实体，通常和数据库中的表对应                 |
  | DTO      | 数据传输对象，通常用于程序中各层之间传递数据 |
  | VO       | 视图对象，为前端展示数据提供的对象           |
  | POJO     | 普通Java对象，只有属性和对应的getter和setter |

  

- **sky-server:** 模块中存放的是 配置文件、配置类、拦截器、controller、service、mapper、启动类等

  <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107094852361.png" alt="image-20221107094852361" style="zoom:50%;" />

  分析sky-server模块的每个包的作用：

  | 名称           | 说明             |
  | -------------- | ---------------- |
  | config         | 存放配置类       |
  | controller     | 存放controller类 |
  | interceptor    | 存放拦截器类     |
  | mapper         | 存放mapper接口   |
  | service        | 存放service类    |
  | SkyApplication | 启动类           |



#### 3.2.2 Git版本控制

使用Git进行项目代码的版本控制，具体操作：

**1). 创建Git本地仓库**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107164030050.png" alt="image-20221107164030050" style="zoom:50%;" /><img src="../../../../Javaweb-Sky-Delivery/墨子杰自用讲义/day01-02/assets/image-20221107135640100.png" alt="image-20221107135640100" style="zoom:50%;" />



当Idea中出现：

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107135819662.png" alt="image-20221107135819662" style="zoom:67%;" />

说明本地仓库创建成功。



**2). 创建Git远程仓库**

访问https://gitee.com/，新建仓库

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107140517031.png" alt="image-20221107140517031" style="zoom:50%;" />

点击 创建 

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107140834161.png" alt="image-20221107140834161" style="zoom:50%;" />



**3). 将本地文件推送到Git远程仓库**



1. **提交文件至本地仓库**

   忽略以下类型文件

   <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107164324396.png" alt="image-20221107164324396" style="zoom:50%;" />

   开始提交

   <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107164654572.png" alt="image-20221107164654572" style="zoom:50%;" />

   中间出现：点击commit

   <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107164813643.png" alt="image-20221107164813643" style="zoom:50%;" />

2. **添加Git远程仓库地址**

   复制远程地址：

   <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107141339343.png" alt="image-20221107141339343" style="zoom:50%;" />

   添加地址：

   <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107141634614.png" alt="image-20221107141634614" style="zoom:50%;" />

   <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107141813693.png" alt="image-20221107141813693" style="zoom:50%;" />

3. **推送**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107141934132.png" alt="image-20221107141934132" style="zoom:50%;" />

成功推送至远程仓库

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107165149915.png" alt="image-20221107165149915" style="zoom:50%;" />



#### 3.2.3 数据库环境搭建

1. **从资料中找到sky.sql**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107101030138.png" alt="image-20221107101030138" style="zoom: 67%;" />

直接打开sky.sql文件

![image-20221107101238205](mozijie_notebook.assets/image-20221107101238205.png)

通过该sql文件直接可创建数据库，所以不需要提前创建数据库，直接导入该文件执行即可。

2. **执行sky.sql文件**

<img src="../../../../Javaweb-Sky-Delivery/墨子杰自用讲义/day01-02/assets/image-20221107102152285.png" alt="image-20221107102152285" style="zoom:50%;" /><img src="../../../../Javaweb-Sky-Delivery/墨子杰自用讲义/day01-02/assets/image-20221107102416034.png" alt="image-20221107102416034" style="zoom:50%;" /><img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107104738200.png" alt="image-20221107104738200" style="zoom:50%;" />

执行完成后，共创建出11张表

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107105103816.png" alt="image-20221107105103816" style="zoom: 80%;" />

每张表的说明：

| **序号** | **表名**      | **中文名**     |
| -------- | ------------- | -------------- |
| 1        | employee      | 员工表         |
| 2        | category      | 分类表         |
| 3        | dish          | 菜品表         |
| 4        | dish_flavor   | 菜品口味表     |
| 5        | setmeal       | 套餐表         |
| 6        | setmeal_dish  | 套餐菜品关系表 |
| 7        | user          | 用户表         |
| 8        | address_book  | 地址表         |
| 9        | shopping_cart | 购物车表       |
| 10       | orders        | 订单表         |
| 11       | order_detail  | 订单明细表     |

我们目前先简单了解大概有哪些表, 每张表存储什么数据, 有一个印象。对于具体的表结构, 以及表结构中的字段, 可以参考资料中的**《数据库设计文档》**，同时在讲解具体的功能开发时, 我们也会再详细介绍。

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107142344642.png" alt="image-20221107142344642" style="zoom:67%;" />



#### 3.2.4 前后端联调

后端的初始工程中已经实现了**登录**功能，直接进行前后端联调测试即可

实现思路：

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107110539832.png" alt="image-20221107110539832" style="zoom:67%;" />

**0.后端编译、启动、前端启动**

**首先后端编译**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20250703222213514.png" alt="image-20250703222213514" style="zoom: 67%;" />

**然后后端启动**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20250703222322204.png" alt="image-20250703222322204" style="zoom:67%;" />

**然后前端启动**

![image-20250703222401465](mozijie_notebook.assets/image-20250703222401465.png)

**在浏览器输入 http://localhost 进入**

**注意：如果点击登录出现问题**

大概是因为与数据库链接出问题

我的电脑的数据库账号和密码分别为root和123456

代码里面默认的是root和root

在配置文件里面改一下就行：

![image-20250703222632069](mozijie_notebook.assets/image-20250703222632069.png)

**1.Controller层**

在sky-server模块中，com.sky.controller.admin.EmployeeController

```java
/**
 * 登录
 *
 * @param employeeLoginDTO
 * @return
 */
@PostMapping("/login")
public Result<EmployeeLoginVO> login(@RequestBody EmployeeLoginDTO employeeLoginDTO) {
    log.info("员工登录：{}", employeeLoginDTO);
    //调用service方法查询数据库
    Employee employee = employeeService.login(employeeLoginDTO);

    // 创建一个 Map 对象用于存储 JWT 的声明（payload 部分）
        Map<String, Object> claims = new HashMap<>();

    // 向 claims 中添加员工ID声明：键为常量 EMP_ID，值为员工对象ID
    claims.put(JwtClaimsConstant.EMP_ID, employee.getId());

    // 生成 JWT 令牌：使用工具类传入密钥/有效期/声明数据
    String token = JwtUtil.createJWT(
            jwtProperties.getAdminSecretKey(),  // 从配置获取加密密钥
            jwtProperties.getAdminTtl(),        // 从配置获取令牌有效期（毫秒）
            claims);                           // 包含员工ID的声明数据

    // 使用建造者模式构建返回给前端的响应对象
    EmployeeLoginVO employeeLoginVO = EmployeeLoginVO.builder()
            .id(employee.getId())       // 设置员工ID
            .userName(employee.getUsername()) // 设置用户名
            .name(employee.getName())   // 设置真实姓名
            .token(token)               // 设置生成的JWT令牌
            .build();                   // 构建不可变对象

    return Result.success(employeeLoginVO);
}
```

**2.Service层**

在sky-server模块中，com.sky.service.impl.EmployeeServiceImpl

```java
/**
 * 员工登录
 *
 * @param employeeLoginDTO
 * @return
 */
public Employee login(EmployeeLoginDTO employeeLoginDTO) {
    String username = employeeLoginDTO.getUsername();
    String password = employeeLoginDTO.getPassword();

    //1、根据用户名查询数据库中的数据
    Employee employee = employeeMapper.getByUsername(username);

    //2、处理各种异常情况（用户名不存在、密码不对、账号被锁定）
    if (employee == null) {
        //账号不存在
        throw new AccountNotFoundException(MessageConstant.ACCOUNT_NOT_FOUND);
    }

    //密码比对
    if (!password.equals(employee.getPassword())) {
        //密码错误
        throw new PasswordErrorException(MessageConstant.PASSWORD_ERROR);
    }

    if (employee.getStatus() == StatusConstant.DISABLE) {
        //账号被锁定
        throw new AccountLockedException(MessageConstant.ACCOUNT_LOCKED);
    }

    //3、返回实体对象
    return employee;
}
```

**3.Mapper层**

在sky-server模块中，com.sky.mapper.EmployeeMapper

```java
package com.sky.mapper;

import com.sky.entity.Employee;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

@Mapper
public interface EmployeeMapper {

    /**
     * 根据用户名查询员工
     * @param username
     * @return
     */
    @Select("select * from employee where username = #{username}")
    Employee getByUsername(String username);

}

```

注：可以通过**断点调试跟踪后端程序的执行过程**



#### 3.2.5 nginx反向代理和负载均衡

对登录功能测试完毕后，接下来，我们思考一个问题：**前端发送的请求，是如何请求到后端服务的？**

前端请求地址：http://localhost/api/employee/login

后端接口地址：http://localhost:8080/admin/employee/login



​              **前端请求地址**                                                                                       **后端接口地址**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107151607921.png" alt="image-20221107151607921" style="zoom:50%;" />                                  <img src="../../../../Javaweb-Sky-Delivery/墨子杰自用讲义/day01-02/assets/image-20221107151623005.png" alt="image-20221107151623005" style="zoom: 33%;" />

很明显，两个地址不一致，那是如何请求到后端服务的呢？

![image-20221107152041371](mozijie_notebook.assets/image-20221107152041371.png)



**1). nginx反向代理**

**nginx 反向代理**，就是将前端发送的动态请求由 nginx 转发到后端服务器

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107152112092.png" alt="image-20221107152112092" style="zoom:67%;" />

那为什么不直接通过浏览器直接请求后台服务端，需要通过nginx反向代理呢？



**nginx 反向代理的好处：**

- 提高访问速度

  因为nginx本身可以进行缓存，如果访问的同一接口，并且做了数据缓存，nginx就直接可把数据返回，不需要真正地访问服务端，从而提高访问速度。

- 进行负载均衡

  所谓负载均衡,就是把大量的请求按照我们指定的方式均衡的分配给集群中的每台服务器。

- 保证后端服务安全

  因为一般后台服务地址不会暴露，所以使用浏览器不能直接访问，可以把nginx作为请求访问的入口，请求到达nginx后转发到具体的服务中，从而保证后端服务的安全。

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107153808368.png" alt="image-20221107153808368" style="zoom:67%;" />

**nginx 反向代理的配置方式：**

```nginx
server{
    listen 80;
    server_name localhost;
    
    location /api/{
        proxy_pass http://localhost:8080/admin/; #反向代理
    }
}
```

**proxy_pass：**该指令是用来设置代理服务器的地址，可以是主机名称，IP地址加端口号等形式。

如上代码的含义是：监听80端口号， 然后当我们访问 http://localhost:80/api/../..这样的接口的时候，它会通过 location /api/ {} 这样的反向代理到 http://localhost:8080/admin/上来。



接下来，进到nginx-1.20.2\conf，打开nginx配置

```nginx
# 反向代理,处理管理端发送的请求
location /api/ {
	proxy_pass   http://localhost:8080/admin/;
    #proxy_pass   http://webservers/admin/;
}
```

当在访问http://localhost/api/employee/login，nginx接收到请求后转到http://localhost:8080/admin/，故最终的请求地址为http://localhost:8080/admin/employee/login，和后台服务的访问地址一致。



**2). nginx 负载均衡**

当如果服务以集群的方式进行部署时，那nginx在转发请求到服务器时就需要做相应的负载均衡。其实，负载均衡从本质上来说也是基于反向代理来实现的，最终都是转发请求。



**nginx 负载均衡的配置方式：**

```nginx
upstream webservers{
    server 192.168.100.128:8080;
    server 192.168.100.129:8080;
}
server{
    listen 80;
    server_name localhost;
    
    location /api/{
        proxy_pass http://webservers/admin;#负载均衡
    }
}
```

**upstream：**如果代理服务器是一组服务器的话，我们可以使用upstream指令配置后端服务器组。

如上代码的含义是：监听80端口号， 然后当我们访问 http://localhost:80/api/../..这样的接口的时候，它会通过 location /api/ {} 这样的反向代理到 http://webservers/admin，根据webservers名称找到一组服务器，根据设置的负载均衡策略(默认是轮询)转发到具体的服务器。

**注：**upstream后面的名称可自定义，但要上下保持一致。



**nginx 负载均衡策略：**

| **名称**   | **说明**                                               |
| ---------- | ------------------------------------------------------ |
| 轮询       | 默认方式                                               |
| weight     | 权重方式，默认为1，权重越高，被分配的客户端请求就越多  |
| ip_hash    | 依据ip分配方式，这样每个访客可以固定访问一个后端服务   |
| least_conn | 依据最少连接方式，把请求优先分配给连接数少的后端服务   |
| url_hash   | 依据url分配方式，这样相同的url会被分配到同一个后端服务 |
| fair       | 依据响应时间方式，响应时间短的服务将会被优先分配       |

具体配置方式：

**轮询：**

```nginx
upstream webservers{
    server 192.168.100.128:8080;
    server 192.168.100.129:8080;
}
```

**weight:**

```nginx
upstream webservers{
    server 192.168.100.128:8080 weight=90;
    server 192.168.100.129:8080 weight=10;
}
```

**ip_hash:**

```nginx
upstream webservers{
    ip_hash;
    server 192.168.100.128:8080;
    server 192.168.100.129:8080;
}
```

**least_conn:**

```nginx
upstream webservers{
    least_conn;
    server 192.168.100.128:8080;
    server 192.168.100.129:8080;
}
```

**url_hash:**

```nginx
upstream webservers{
    hash &request_uri;
    server 192.168.100.128:8080;
    server 192.168.100.129:8080;
}
```

**fair:**

```nginx
upstream webservers{
    server 192.168.100.128:8080;
    server 192.168.100.129:8080;
    fair;
}
```



### 3.3 完善登录功能

**问题：**员工表中的密码是明文存储，安全性太低。

![image-20221107160529803](mozijie_notebook.assets/image-20221107160529803.png)

**解决思路：**

1. 将密码加密后存储，提高安全性

   <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107161918913.png" alt="image-20221107161918913" style="zoom:50%;" />

2. 使用MD5加密方式对明文密码加密

   <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107160739680.png" alt="image-20221107160739680" style="zoom:50%;" />

**实现步骤：**

1. 修改数据库中明文密码，改为MD5加密后的密文

   打开employee表，修改密码

   ![image-20221107161446710](mozijie_notebook.assets/image-20221107161446710.png)

2. 修改Java代码，前端提交的密码进行MD5加密后再跟数据库中密码比对

   打开EmployeeServiceImpl.java，修改比对密码

   ```java
   /**
    * 员工登录
    *
    * @param employeeLoginDTO
    * @return
    */
   public Employee login(EmployeeLoginDTO employeeLoginDTO) {
   
       //1、根据用户名查询数据库中的数据
   
       //2、处理各种异常情况（用户名不存在、密码不对、账号被锁定）
       //.......
       //密码比对
       // TODO 后期需要进行md5加密，然后再进行比对
       password = DigestUtils.md5DigestAsHex(password.getBytes());
       if (!password.equals(employee.getPassword())) {
           //密码错误
           throw new PasswordErrorException(MessageConstant.PASSWORD_ERROR);
       }
   
       //........
   
       //3、返回实体对象
       return employee;
   }
   ```

   

## 4. 导入接口文档

接下来，就要进入到项目的业务开发了，而我们的开发方式就是基本当前企业主流的前后端分离开发方式，那么这种方式就要求我们之前需要先将接口定义好，这样前后端人员才能并行开发，所以，这个章节就需要将接口文档导入到管理平台，为我们后面业务开发做好准备。其实，在真实的企业开发中，接口设计过程其实是一个非常漫长的过程，可能需要多次开会讨论调整，甚至在开发的过程中才会发现某些接口定义还需要再调整，这种情况其实是非常常见的，但是由于项目时间原因，所以选择一次性导入所有的接口，在开发业务功能过程当中，也会带着大家一起来分析一下对应的接口是怎么确定下来的，为什么要这样定义，从而培养同学们的接口设计能力。



### 4.1 前后端分离开发流程

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107181446913.png" alt="image-20221107181446913" style="zoom:67%;" />

第一步：定义接口，确定接口的路径、请求方式、传入参数、返回参数。

第二步：前端开发人员和后端开发人员并行开发，同时，也可自测。

第三步：前后端人员进行连调测试。

第四步：提交给测试人员进行最终测试。



### 4.2 操作步骤

**注意：这里提到的接口管理网站YApi并不好使 ， 以后请使用java自带的Swagger或者网页端的apifox。这个YApi可以不管他**

将课程资料中提供的项目接口导入YApi。访问地址：https://yapi.pro/


**1). 从资料中找到项目接口文件**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107184823104.png" alt="image-20221107184823104" style="zoom:80%;" />

**2). 导入到YApi平台**

在YApi平台创建出两个项目

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107185304117.png" alt="image-20221107185304117" style="zoom: 50%;" />

选择苍穹外卖-管理端接口.json导入

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107185630516.png" alt="image-20221107185630516" style="zoom:50%;" />

导入成功

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107185742061.png" alt="image-20221107185742061" style="zoom:50%;" />



另一个用户端json文件也执行相同操作。



## 5. Swagger

### 5.1 介绍

Swagger 是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务(<https://swagger.io/>)。 它的主要作用是：

1. 使得前后端分离开发更加方便，有利于团队协作

2. 接口的文档在线自动生成，降低后端开发人员编写接口文档的负担

3. 功能测试 

   Spring已经将Swagger纳入自身的标准，建立了Spring-swagger项目，现在叫Springfox。通过在项目中引入Springfox ，即可非常简单快捷的使用Swagger。

knife4j是为Java MVC框架集成Swagger生成Api文档的增强解决方案,前身是swagger-bootstrap-ui,取名kni4j是希望它能像一把匕首一样小巧,轻量,并且功能强悍!

目前，一般都使用knife4j框架。



### 5.2 使用步骤

1. 导入 knife4j 的maven坐标

   在pom.xml中添加依赖

   ```xml
   <dependency>
      <groupId>com.github.xiaoymin</groupId>
      <artifactId>knife4j-spring-boot-starter</artifactId>
   </dependency>
   ```

2. 在配置类中加入 knife4j 相关配置

   WebMvcConfiguration.java

   ```java
   /**
        * 通过knife4j生成接口文档
        * @return
   */
   @Bean
   public Docket docket() {
       ApiInfo apiInfo = new ApiInfoBuilder()
               .title("苍穹外卖项目接口文档")
               .version("2.0")
               .description("苍穹外卖项目接口文档")
               .build();
       Docket docket = new Docket(DocumentationType.SWAGGER_2)
               .apiInfo(apiInfo)
               .select()
               .apis(RequestHandlerSelectors.basePackage("com.sky.controller"))
               .paths(PathSelectors.any())
               .build();
       return docket;
   }
   ```

   

3. 设置静态资源映射，否则接口文档页面无法访问

   WebMvcConfiguration.java

   ```java
   /**
        * 设置静态资源映射
        * @param registry
   */
   protected void addResourceHandlers(ResourceHandlerRegistry registry) {
           registry.addResourceHandler("/doc.html").addResourceLocations("classpath:/META-INF/resources/");
           registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");
   }
   ```

4. 访问测试

   接口文档访问路径为 http://ip:port/doc.html ---> http://localhost:8080/doc.html

   <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107173033632.png" alt="image-20221107173033632" style="zoom:50%;" />

   接口测试:测试登录功能

   <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107173137506.png" alt="image-20221107173137506" style="zoom:50%;" />

**思考：**通过 Swagger 就可以生成接口文档，那么我们就不需要 Yapi 了？

1、Yapi 是设计阶段使用的工具，管理和维护接口

2、Swagger 在开发阶段使用的框架，帮助后端开发人员做后端的接口测试



### 5.3 常用注解

通过注解可以控制生成的接口文档，使接口文档拥有更好的可读性，常用注解如下：

| **注解**          | **说明**                                               |
| ----------------- | ------------------------------------------------------ |
| @Api              | 用在类上，例如Controller，表示对类的说明               |
| @ApiModel         | 用在类上，例如entity、DTO、VO                          |
| @ApiModelProperty | 用在属性上，描述属性信息                               |
| @ApiOperation     | 用在方法上，例如Controller的方法，说明方法的用途、作用 |

接下来，使用上述注解，生成可读性更好的接口文档

在sky-pojo模块中

EmployeeLoginDTO.java

```java
package com.sky.dto;

import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;

import java.io.Serializable;

@Data
@ApiModel(description = "员工登录时传递的数据模型")
public class EmployeeLoginDTO implements Serializable {

    @ApiModelProperty("用户名")
    private String username;

    @ApiModelProperty("密码")
    private String password;

}

```

EmployeeLoginVo.java

```java
package com.sky.vo;

import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
@ApiModel(description = "员工登录返回的数据格式")
public class EmployeeLoginVO implements Serializable {

    @ApiModelProperty("主键值")
    private Long id;

    @ApiModelProperty("用户名")
    private String userName;

    @ApiModelProperty("姓名")
    private String name;

    @ApiModelProperty("jwt令牌")
    private String token;

}
```

在sky-server模块中

EmployeeController.java

```java
package com.sky.controller.admin;

import com.sky.constant.JwtClaimsConstant;
import com.sky.dto.EmployeeLoginDTO;
import com.sky.entity.Employee;
import com.sky.properties.JwtProperties;
import com.sky.result.Result;
import com.sky.service.EmployeeService;
import com.sky.utils.JwtUtil;
import com.sky.vo.EmployeeLoginVO;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.HashMap;
import java.util.Map;

/**
 * 员工管理
 */
@RestController
@RequestMapping("/admin/employee")
@Slf4j
@Api(tags = "员工相关接口")
public class EmployeeController {

    @Autowired
    private EmployeeService employeeService;
    @Autowired
    private JwtProperties jwtProperties;

    /**
     * 登录
     *
     * @param employeeLoginDTO
     * @return
     */
    @PostMapping("/login")
    @ApiOperation(value = "员工登录")
    public Result<EmployeeLoginVO> login(@RequestBody EmployeeLoginDTO employeeLoginDTO) 	{
        //..............

        
    }

    /**
     * 退出
     *
     * @return
     */
    @PostMapping("/logout")
    @ApiOperation("员工退出")
    public Result<String> logout() {
        return Result.success();
    }

}

```

启动服务：访问http://localhost:8080/doc.html

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221107175649468.png" alt="image-20221107175649468" style="zoom:50%;" />

# 苍穹外卖-day02

## 2025.07.06开始学-学到30集-5.1

## 课程内容

- 新增员工
- 员工分页查询
- 启用禁用员工账号
- 编辑员工
- 导入分类模块功能代码



**功能实现：**员工管理、菜品分类管理。

**员工管理效果：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112172846316.png" alt="image-20221112172846316" style="zoom:50%;" /> 



**菜品分类管理效果：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112172925354.png" alt="image-20221112172925354" style="zoom:50%;" /> 



## 1. 新增员工

### 1.1 需求分析和设计

#### 1.1.1 产品原型

一般在做需求分析时，往往都是对照着产品原型进行分析，因为产品原型比较直观，便于我们理解业务。

后台系统中可以管理员工信息，通过新增员工来添加后台系统用户。

**新增员工原型：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111161120975.png" alt="image-20221111161120975" style="zoom: 50%;" /> 



当填写完表单信息, 点击"保存"按钮后, 会提交该表单的数据到服务端, 在服务端中需要接受数据, 然后将数据保存至数据库中。

**注意事项：**

1. 账号必须是唯一的
2. 手机号为合法的11位手机号码
3. 身份证号为合法的18位身份证号码
4. 密码默认为123456



#### 1.1.2 接口设计

找到资料-->项目接口文档-->苍穹外卖-管理端接口.html

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111162912753.png" alt="image-20221111162912753"  /> 

![image-20221111162930483](mozijie_notebook.assets/image-20221111162930483-1751799985803-1.png)

明确新增员工接口的**请求路径、请求方式、请求参数、返回数据**。



本项目约定：

- **管理端**发出的请求，统一使用**/admin**作为前缀。
- **用户端**发出的请求，统一使用**/user**作为前缀。



#### 1.1.3 表设计

新增员工，其实就是将我们新增页面录入的员工数据插入到employee表。

**employee表结构：**

| **字段名**  | **数据类型** | **说明**     | **备注**    |
| ----------- | ------------ | ------------ | ----------- |
| id          | bigint       | 主键         | 自增        |
| name        | varchar(32)  | 姓名         |             |
| username    | varchar(32)  | 用户名       | 唯一        |
| password    | varchar(64)  | 密码         |             |
| phone       | varchar(11)  | 手机号       |             |
| sex         | varchar(2)   | 性别         |             |
| id_number   | varchar(18)  | 身份证号     |             |
| status      | Int          | 账号状态     | 1正常 0锁定 |
| create_time | Datetime     | 创建时间     |             |
| update_time | datetime     | 最后修改时间 |             |
| create_user | bigint       | 创建人id     |             |
| update_user | bigint       | 最后修改人id |             |

其中，employee表中的status字段已经设置了默认值1，表示状态正常。

![image-20221111180159188](mozijie_notebook.assets/image-20221111180159188.png)  



### 1.2 代码开发

#### 1.2.1 设计DTO类

**根据新增员工接口设计对应的DTO**

前端传递参数列表：

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111164002448.png" alt="image-20221111164002448" style="zoom:50%;" /> 

**思考：**是否可以使用对应的实体类来接收呢？

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111164453341.png" alt="image-20221111164453341" style="zoom:50%;" /> 

**注意：**当前端提交的数据和实体类中对应的属性差别比较大时，建议使用DTO来封装数据

由于上述传入参数和实体类有较大差别，所以自定义DTO类。

进入sky-pojo模块，在com.sky.dto包下，已定义EmployeeDTO

```java
package com.sky.dto;

import lombok.Data;

import java.io.Serializable;

@Data
public class EmployeeDTO implements Serializable {

    private Long id;

    private String username;

    private String name;

    private String phone;

    private String sex;

    private String idNumber;

}
```



#### 1.2.2 Controller层

 **EmployeeController中创建新增员工方法**

进入到sky-server模块中，在com.sky.controller.admin包下，在EmployeeController中创建新增员工方法，接收前端提交的参数。

```java
	/**
     * 新增员工
     * @param employeeDTO
     * @return
     */
    @PostMapping
    @ApiOperation("新增员工")
    public Result save(@RequestBody EmployeeDTO employeeDTO){
        log.info("新增员工：{}",employeeDTO);
        employeeService.save(employeeDTO);//该方法后续步骤会定义
        return Result.success();
    }
```

**注：**Result类定义了后端统一返回结果格式。

进入sky-common模块，在com.sky.result包下定义了Result.java

```java
package com.sky.result;

import lombok.Data;

import java.io.Serializable;

/**
 * 后端统一返回结果
 * @param <T>
 */
@Data
public class Result<T> implements Serializable {

    private Integer code; //编码：1成功，0和其它数字为失败
    private String msg; //错误信息
    private T data; //数据

    public static <T> Result<T> success() {
        Result<T> result = new Result<T>();
        result.code = 1;
        return result;
    }

    public static <T> Result<T> success(T object) {
        Result<T> result = new Result<T>();
        result.data = object;
        result.code = 1;
        return result;
    }

    public static <T> Result<T> error(String msg) {
        Result result = new Result();
        result.msg = msg;
        result.code = 0;
        return result;
    }

}
```



#### 1.2.3 Service层接口

**在EmployeeService接口中声明新增员工方法**

进入到sky-server模块中,com.sky.server.EmployeeService

```java
	/**
     * 新增员工
     * @param employeeDTO
     */
    void save(EmployeeDTO employeeDTO);
```



#### 1.2.4 Service层实现类

**在EmployeeServiceImpl中实现新增员工方法**

com.sky.server.impl.EmployeeServiceImpl中创建方法

```java
	/**
     * 新增员工
     *
     * @param employeeDTO
     */
    public void save(EmployeeDTO employeeDTO) {
        Employee employee = new Employee();

        //对象属性拷贝
        BeanUtils.copyProperties(employeeDTO, employee);

        //设置账号的状态，默认正常状态 1表示正常 0表示锁定
        employee.setStatus(StatusConstant.ENABLE);

        //设置密码，默认密码123456
        employee.setPassword(DigestUtils.md5DigestAsHex(PasswordConstant.DEFAULT_PASSWORD.getBytes()));

        //设置当前记录的创建时间和修改时间
        employee.setCreateTime(LocalDateTime.now());
        employee.setUpdateTime(LocalDateTime.now());

        //设置当前记录创建人id和修改人id
        employee.setCreateUser(10L);//目前写个假数据，后期修改
        employee.setUpdateUser(10L);

        employeeMapper.insert(employee);//后续步骤定义
    }
```



在sky-common模块com.sky.constants包下已定义StatusConstant.java

```java
package com.sky.constant;

/**
 * 状态常量，启用或者禁用
 */
public class StatusConstant {

    //启用
    public static final Integer ENABLE = 1;

    //禁用
    public static final Integer DISABLE = 0;
}

```



#### 1.2.5 Mapper层

**在EmployeeMapper中声明insert方法**

com.sky.EmployeeMapper中添加方法

```java
	/**
     * 插入员工数据
     * @param employee
     */
    @Insert("insert into employee (name, username, password, phone, sex, id_number, create_time, update_time, create_user, update_user,status) " +
            "values " +
            "(#{name},#{username},#{password},#{phone},#{sex},#{idNumber},#{createTime},#{updateTime},#{createUser},#{updateUser},#{status})")
    void insert(Employee employee);
```



在application.yml中已开启驼峰命名，故id_number和idNumber可对应。

```yaml
mybatis:
  configuration:
    #开启驼峰命名
    map-underscore-to-camel-case: true
```



### 1.3 功能测试

代码已经发开发完毕，对新增员工功能进行测试。

**功能测试实现方式：**

- 通过接口文档测试
- 通前后端联调测试

接下来我们使用上述两种方式分别测试。



#### 1.3.1 接口文档测试

**启动服务：**访问http://localhost:8080/doc.html，进入新增员工接口

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111184934153.png" alt="image-20221111184934153" style="zoom:50%;" />   

json数据：

```json
{
  "id": 0,
  "idNumber": "111222333444555666",
  "name": "xiaozhi",
  "phone": "13812344321",
  "sex": "1",
  "username": "小智"
}
```

响应码：401 报错

**通过断点调试：**进入到JwtTokenAdminInterceptor拦截器

```java
 	/**
     * 校验jwt
     *
     * @param request
     * @param response
     * @param handler
     * @return
     * @throws Exception
     */
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //判断当前拦截到的是Controller的方法还是其他资源
        if (!(handler instanceof HandlerMethod)) {
            //当前拦截到的不是动态方法，直接放行
            return true;
        }

        //1、从请求头中获取令牌 jwtProperties.getAdminTokenName()获取为token
        String token = request.getHeader(jwtProperties.getAdminTokenName());

        //2、校验令牌
        try {
            log.info("jwt校验:{}", token);
            Claims claims = JwtUtil.parseJWT(jwtProperties.getAdminSecretKey(), token);
            Long empId = Long.valueOf(claims.get(JwtClaimsConstant.EMP_ID).toString());
            log.info("当前员工id：", empId);
            //3、通过，放行
            return true;
        } catch (Exception ex) {
            //4、不通过，响应401状态码
            response.setStatus(401);
            return false;
        }
    }
```

**报错原因：**由于JWT令牌校验失败，导致EmployeeController的save方法没有被调用

**解决方法：**调用员工登录接口获得一个合法的JWT令牌

使用admin用户登录获取令牌

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111185649636.png" alt="image-20221111185649636" style="zoom: 50%;" /> 

**添加令牌：**

将合法的JWT令牌添加到全局参数中

文档管理-->全局参数设置-->添加参数

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111185901726.png" alt="image-20221111185901726" style="zoom: 50%;" /> 

**接口测试：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111190132481.png" alt="image-20221111190132481" style="zoom:50%;" /> 

其中，请求头部含有JWT令牌

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111190248268.png" alt="image-20221111190248268" style="zoom:50%;" /> 

**查看employee表：**

![image-20221111191624908](mozijie_notebook.assets/image-20221111191624908.png)

测试成功。



#### 1.3.2 前后端联调测试

启动nginx,访问 http://localhost

登录-->员工管理-->添加员工

**注意这里的前端代码对手机号和身份证号也写了判断：**

**手机号开头必须是13，身份证号开头必须是11。我服了，哪有前端这么写的，不符合实际情况**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111192339271.png" alt="image-20221111192339271" style="zoom:80%;" /> 

保存后，查看employee表

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111192446950.png" alt="image-20221111192446950"  /> 

测试成功。



**注意：**由于开发阶段前端和后端是并行开发的，后端完成某个功能后，此时前端对应的功能可能还没有开发完成，
导致无法进行前后端联调测试。所以在开发阶段，后端测试主要以接口文档测试为主。



### 1.4 代码完善

目前，程序存在的问题主要有两个：

- 录入的用户名已存，抛出的异常后没有处理
- 新增员工时，创建人id和修改人id设置为固定值

接下来，我们对上述两个问题依次进行分析和解决。



#### 1.4.1 问题一

**描述：**录入的用户名已存，抛出的异常后没有处理

**分析：**

新增username=zhangsan的用户，若employee表中之前已存在。

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111193700895.png" alt="image-20221111193700895"  /> 



**注意：这里如果响应码是401，而不是500，说明token过期了，要重新创建一个token配置**

**具体操作可以见1.3.1**

后台报错信息：

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111194049620.png" alt="image-20221111194049620" style="zoom:80%;" /> 

查看employee表结构：

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111194131787.png" alt="image-20221111194131787" style="zoom:50%;" /> 

发现，username已经添加了唯一约束，不能重复。

**解决：**

通过全局异常处理器来处理。

进入到sky-server模块，com.sky.hander包下，GlobalExceptionHandler.java添加方法

```java
	/**
     * 处理SQL异常
     * @param ex
     * @return
     */
    @ExceptionHandler
    public Result exceptionHandler(SQLIntegrityConstraintViolationException ex){
        //Duplicate entry 'zhangsan' for key 'employee.idx_username'
        String message = ex.getMessage();
        if(message.contains("Duplicate entry")){
            String[] split = message.split(" ");
            String username = split[2];
            String msg = username + MessageConstant.ALREADY_EXISTS;
            return Result.error(msg);
        }else{
            return Result.error(MessageConstant.UNKNOWN_ERROR);
        }
    }
```

进入到sky-common模块，在MessageConstant.java添加

```java
public static final String ALREADY_EXISTS = "已存在";
```

再次，接口测试：

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111195521095.png" alt="image-20221111195521095" style="zoom:50%;" /> 



####  1.4.2 问题二

**描述**：新增员工时，创建人id和修改人id设置为固定值

**分析：**

```java
	/**
     * 新增员工
     *
     * @param employeeDTO
     */
    public void save(EmployeeDTO employeeDTO) {
        Employee employee = new Employee();
        //................
        //////////当前设置的id为固定值10//////////
        employee.setCreateUser(10L);
        employee.setUpdateUser(10L);
        //////////////////////////////////////
        //.................................

        employeeMapper.insert(employee);//后续步骤定义
    }
```

**解决：**

通过某种方式动态获取当前登录员工的id。

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111201922482.png" alt="image-20221111201922482" style="zoom:50%;" /> 

员工登录成功后会生成JWT令牌并响应给前端：

在sky-server模块

```java
package com.sky.controller.admin;
/**
 * 员工管理
 */
@RestController
@RequestMapping("/admin/employee")
@Slf4j
@Api(tags = "员工相关接口")
public class EmployeeController {

    @Autowired
    private EmployeeService employeeService;
    @Autowired
    private JwtProperties jwtProperties;

    /**
     * 登录
     *
     * @param employeeLoginDTO
     * @return
     */
    @PostMapping("/login")
    @ApiOperation(value = "员工登录")
    public Result<EmployeeLoginVO> login(@RequestBody EmployeeLoginDTO employeeLoginDTO) {
        //.........

        //登录成功后，生成jwt令牌
        Map<String, Object> claims = new HashMap<>();
        claims.put(JwtClaimsConstant.EMP_ID, employee.getId());
        String token = JwtUtil.createJWT(
                jwtProperties.getAdminSecretKey(),
                jwtProperties.getAdminTtl(),
                claims);

        //............

        return Result.success(employeeLoginVO);
    }

}
```

后续请求中，前端会携带JWT令牌，通过JWT令牌可以解析出当前登录员工id：

JwtTokenAdminInterceptor.java

```java
package com.sky.interceptor;

/**
 * jwt令牌校验的拦截器
 */
@Component
@Slf4j
public class JwtTokenAdminInterceptor implements HandlerInterceptor {

    @Autowired
    private JwtProperties jwtProperties;

    /**
     * 校验jwt
     *
     * @param request
     * @param response
     * @param handler
     * @return
     * @throws Exception
     */
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
       
		//..............
        
        //1、从请求头中获取令牌
        String token = request.getHeader(jwtProperties.getAdminTokenName());

        //2、校验令牌
        try {
            log.info("jwt校验:{}", token);
            Claims claims = JwtUtil.parseJWT(jwtProperties.getAdminSecretKey(), token);
            Long empId = Long.valueOf(claims.get(JwtClaimsConstant.EMP_ID).toString());
            log.info("当前员工id：", empId);
            //3、通过，放行
            return true;
        } catch (Exception ex) {
            //4、不通过，响应401状态码
            response.setStatus(401);
            return false;
        }
    }
}
```

**思考：**解析出登录员工id后，如何传递给Service的save方法？

通过ThreadLocal进行传递。



#### 1.4.3 ThreadLocal

**介绍：**

ThreadLocal 并不是一个Thread，而是Thread的局部变量。
ThreadLocal为每个线程提供单独一份存储空间，具有线程隔离的效果，只有在线程内才能获取到对应的值，线程外则不能访问。

**常用方法：**

- public void set(T value) 	设置当前线程的线程局部变量的值
- public T get() 		返回当前线程所对应的线程局部变量的值
- public void remove()        移除当前线程的线程局部变量

对ThreadLocal有了一定认识后，接下来继续解决**问题二**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111212349365.png" alt="image-20221111212349365" style="zoom:67%;" /> 

初始工程中已经封装了 ThreadLocal 操作的工具类：

在sky-common模块

```java
package com.sky.context;

public class BaseContext {

    public static ThreadLocal<Long> threadLocal = new ThreadLocal<>();

    public static void setCurrentId(Long id) {
        threadLocal.set(id);
    }

    public static Long getCurrentId() {
        return threadLocal.get();
    }

    public static void removeCurrentId() {
        threadLocal.remove();
    }

}
```

在拦截器中解析出当前登录员工id，并放入线程局部变量中：

在sky-server模块中，拦截器：

```java
package com.sky.interceptor;

/**
 * jwt令牌校验的拦截器
 */
@Component
@Slf4j
public class JwtTokenAdminInterceptor implements HandlerInterceptor {

    @Autowired
    private JwtProperties jwtProperties;

    /**
     * 校验jwt
     *
     * @param request
     * @param response
     * @param handler
     * @return
     * @throws Exception
     */
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        
		//.............................
       
        //2、校验令牌
        try {
            //.................
            Claims claims = JwtUtil.parseJWT(jwtProperties.getAdminSecretKey(), token);
            Long empId = Long.valueOf(claims.get(JwtClaimsConstant.EMP_ID).toString());
            log.info("当前员工id：", empId);
            /////将用户id存储到ThreadLocal////////
            BaseContext.setCurrentId(empId);
            ////////////////////////////////////
            //3、通过，放行
            return true;
        } catch (Exception ex) {
            //......................
        }
    }
}
```

在Service中获取线程局部变量中的值：

```java
	/**
     * 新增员工
     *
     * @param employeeDTO
     */
    public void save(EmployeeDTO employeeDTO) {
        //.............................

        //设置当前记录创建人id和修改人id
        employee.setCreateUser(BaseContext.getCurrentId());//目前写个假数据，后期修改
        employee.setUpdateUser(BaseContext.getCurrentId());

        employeeMapper.insert(employee);
    }
```

测试：使用admin(id=1)用户登录后添加一条记录

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111214044510.png" alt="image-20221111214044510" style="zoom:50%;" /> 

查看employee表记录

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111214354053.png" alt="image-20221111214354053" style="zoom:50%;" /> 



### 1.5 代码提交

点击提交：

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112092139583.png" alt="image-20221112092139583" style="zoom: 50%;" /> 

提交过程中，出现提示：

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112092239578.png" alt="image-20221112092239578" style="zoom:50%;" /> 

继续push:

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112092332837.png" alt="image-20221112092332837" style="zoom:50%;" /> 

推送成功：

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112092427252.png" alt="image-20221112092427252" style="zoom: 80%;" /> 



## 2. 员工分页查询

### 2.1 需求分析和设计

#### 2.1.1 产品原型

系统中的员工很多的时候，如果在一个页面中全部展示出来会显得比较乱，不便于查看，所以一般的系统中都会以分页的方式来展示列表数据。而在我们的分页查询页面中, 除了分页条件以外，还有一个查询条件 "员工姓名"。

**查询员工原型：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111215309289.png" alt="image-20221111215309289"  /> 

**业务规则**：

- 根据页码展示员工信息
- 每页展示10条数据
- 分页查询时可以根据需要，输入员工姓名进行查询



#### 2.1.2 接口设计

找到资料-->项目接口文档-->苍穹外卖-管理端接口.html

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221111220031113.png" alt="image-20221111220031113" style="zoom:67%;" />

![image-20221111220041965](mozijie_notebook.assets/image-20221111220041965-1751800168112-9.png)

**注意事项：**

- 请求参数类型为Query，不是json格式提交，在路径后直接拼接。/admin/employee/page?name=zhangsan
- 返回数据中records数组中使用Employee实体类对属性进行封装。



### 2.2 代码开发

#### 2.2.1 设计DTO类

根据请求参数进行封装，在sky-pojo模块中

```java
package com.sky.dto;

import lombok.Data;

import java.io.Serializable;

@Data
public class EmployeePageQueryDTO implements Serializable {

    //员工姓名
    private String name;

    //页码
    private int page;

    //每页显示记录数
    private int pageSize;

}

```



#### 2.2.2 封装PageResult

后面所有的分页查询，统一都封装为PageResult对象。

在sky-common模块

```java
package com.sky.result;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;
import java.util.List;

/**
 * 封装分页查询结果
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
public class PageResult implements Serializable {

    private long total; //总记录数

    private List records; //当前页数据集合

}

```

员工信息分页查询后端返回的对象类型为: Result<PageResult>

```java
package com.sky.result;

import lombok.Data;

import java.io.Serializable;

/**
 * 后端统一返回结果
 * @param <T>
 */
@Data
public class Result<T> implements Serializable {

    private Integer code; //编码：1成功，0和其它数字为失败
    private String msg; //错误信息
    private T data; //数据

    public static <T> Result<T> success() {
        Result<T> result = new Result<T>();
        result.code = 1;
        return result;
    }

    public static <T> Result<T> success(T object) {
        Result<T> result = new Result<T>();
        result.data = object;
        result.code = 1;
        return result;
    }

    public static <T> Result<T> error(String msg) {
        Result result = new Result();
        result.msg = msg;
        result.code = 0;
        return result;
    }

}
```



#### 2.2.3 Controller层

在sky-server模块中，com.sky.controller.admin.EmployeeController中添加分页查询方法。

```java
	/**
     * 员工分页查询
     * @param employeePageQueryDTO
     * @return
     */
    @GetMapping("/page")
    @ApiOperation("员工分页查询")
    public Result<PageResult> page(EmployeePageQueryDTO employeePageQueryDTO){
        log.info("员工分页查询，参数为：{}", employeePageQueryDTO);
        PageResult pageResult = employeeService.pageQuery(employeePageQueryDTO);//后续定义
        return Result.success(pageResult);
    }
```



#### 2.2.4 Service层接口

在EmployeeService接口中声明pageQuery方法：

```java
	/**
     * 分页查询
     * @param employeePageQueryDTO
     * @return
     */
    PageResult pageQuery(EmployeePageQueryDTO employeePageQueryDTO);
```



#### 2.2.5 Service层实现类

在EmployeeServiceImpl中实现pageQuery方法：

```java
	/**
     * 分页查询
     *
     * @param employeePageQueryDTO
     * @return
     */
    public PageResult pageQuery(EmployeePageQueryDTO employeePageQueryDTO) {
        // select * from employee limit 0,10
        //开始分页查询
        PageHelper.startPage(employeePageQueryDTO.getPage(), employeePageQueryDTO.getPageSize());

        Page<Employee> page = employeeMapper.pageQuery(employeePageQueryDTO);//后续定义

        long total = page.getTotal();
        List<Employee> records = page.getResult();

        return new PageResult(total, records);
    }
```

**注意：**此处使用 mybatis 的分页插件 PageHelper 来简化分页代码的开发。底层基于 mybatis 的拦截器实现。

故在pom.xml文中添加依赖(初始工程已添加)

```xml
<dependency>
   <groupId>com.github.pagehelper</groupId>
   <artifactId>pagehelper-spring-boot-starter</artifactId>
   <version>${pagehelper}</version>
</dependency>
```



#### 2.2.6 Mapper层

在 EmployeeMapper 中声明 pageQuery 方法：

```java
	/**
     * 分页查询
     * @param employeePageQueryDTO
     * @return
     */
    Page<Employee> pageQuery(EmployeePageQueryDTO employeePageQueryDTO);
```

在 src/main/resources/mapper/EmployeeMapper.xml 中编写SQL：

```sql
<select id="pageQuery" resultType="com.sky.entity.Employee">
        select * from employee
        <where>
            <if test="name != null and name != ''">
                and name like concat('%',#{name},'%')
            </if>
        </where>
        order by create_time desc
    </select>
```



### 2.3 功能测试

可以通过接口文档进行测试，也可以进行前后端联调测试。

接下来使用两种方式分别测试：



#### 2.3.1 接口文档测试

**重启服务：**访问http://localhost:8080/doc.html，进入员工分页查询

这里将application.yml 里面的admin-ttl参数修改一下。

以后就不会遇到token失效的问题了

```yaml
    # 设置jwt过期时间,这里设置约3年，防止每次请求都需要重新登录
    admin-ttl: 45768000000
```



<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112101848657.png" alt="image-20221112101848657" style="zoom:67%;" /> 

**响应结果：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112101946022.png" alt="image-20221112101946022" style="zoom:50%;" /> 



#### 2.3.2 前后端联调测试

**点击员工管理**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112102441810.png" alt="image-20221112102441810" style="zoom:50%;" />  



**输入员工姓名为zhangsan**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112102540938.png" alt="image-20221112102540938" style="zoom:50%;" /> 

不难发现，**最后操作时间格式**不清晰，在**代码完善**中解决。

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112102745437.png" alt="image-20221112102745437" style="zoom:50%;" /> 



### 2.4 代码完善

**问题描述：**操作时间字段显示有问题。

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112103235539.png" alt="image-20221112103235539" style="zoom:50%;" /> 



**解决方式：**

**1).  方式一**

在属性上加上注解，对日期进行格式化

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112103501581.png" alt="image-20221112103501581" style="zoom:67%;" /> 

但这种方式，需要在每个时间属性上都要加上该注解，使用较麻烦，不能全局处理。



**2).  方式二（推荐 )**

在WebMvcConfiguration中扩展SpringMVC的消息转换器，统一对日期类型进行格式处理

```java
	/**
     * 扩展Spring MVC框架的消息转化器
     * @param converters
     */
    protected void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
        log.info("扩展消息转换器...");
        //创建一个消息转换器对象
        MappingJackson2HttpMessageConverter converter = new MappingJackson2HttpMessageConverter();
        //需要为消息转换器设置一个对象转换器，对象转换器可以将Java对象序列化为json数据
        converter.setObjectMapper(new JacksonObjectMapper());
        //将自己的消息转化器加入容器中
        converters.add(0,converter);
    }
```

添加后，再次测试

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112104305608.png" alt="image-20221112104305608" style="zoom:67%;" /> 

时间格式定义，sky-common模块中

```java
package com.sky.json;

public class JacksonObjectMapper extends ObjectMapper {

	//.......
    public static final String DEFAULT_DATE_TIME_FORMAT = "yyyy-MM-dd HH:mm";
    //.......

    }
}
```

### 2.5 代码提交

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112105011214.png" alt="image-20221112105011214" style="zoom:50%;" /> 

后续步骤和新增员工代码提交一致，不再赘述。



## 3. 启用禁用员工账号

### 3.1 需求分析与设计

#### 3.1.1 产品原型

在员工管理列表页面，可以对某个员工账号进行启用或者禁用操作。账号禁用的员工不能登录系统，启用后的员工可以正常登录。如果某个员工账号状态为正常，则按钮显示为 "禁用"，如果员工账号状态为已禁用，则按钮显示为"启用"。

**启禁用员工原型：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112112359233.png" alt="image-20221112112359233" style="zoom:67%;" /> 

**业务规则：**

- 可以对状态为“启用” 的员工账号进行“禁用”操作
- 可以对状态为“禁用”的员工账号进行“启用”操作
- 状态为“禁用”的员工账号不能登录系统



#### 3.1.2 接口设计

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112112728333-1751800049054-3.png" alt="image-20221112112728333" style="zoom:80%;" />

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112112739680.png" alt="image-20221112112739680" style="zoom: 80%;" />



1). 路径参数携带状态值。

2). 同时，把id传递过去，明确对哪个用户进行操作。

3). 返回数据code状态是必须，其它是非必须。



### 3.2 代码开发

#### 3.2.1 Controller层

在sky-server模块中，根据接口设计中的请求参数形式对应的在 EmployeeController 中创建启用禁用员工账号的方法：

```java
	/**
     * 启用禁用员工账号
     * @param status
     * @param id
     * @return
     */
    @PostMapping("/status/{status}")
    @ApiOperation("启用禁用员工账号")
    public Result startOrStop(@PathVariable Integer status,Long id){
        // 路径参数要使用@PathVariable
        log.info("启用禁用员工账号：{},{}",status,id);
        employeeService.startOrStop(status,id);//后绪步骤定义
        return Result.success();
    }
```



#### 3.2.2 Service层接口

在 EmployeeService 接口中声明启用禁用员工账号的业务方法：

```java
	/**
     * 启用禁用员工账号
     * @param status
     * @param id
     */
    void startOrStop(Integer status, Long id);
```



#### 3.2.3 Service层实现类

在 EmployeeServiceImpl 中实现启用禁用员工账号的业务方法：

```java
	/**
     * 启用禁用员工账号
     *
     * @param status
     * @param id
     */
    public void startOrStop(Integer status, Long id) {
        // 更新员工账号状态
        // update employee set status = ? where id = ?;
        Employee employee = Employee.builder()
                .status(status)
                .id(id)
                .build();

        employeeMapper.update(employee);
    }
```



#### 3.2.4 Mapper层

在 EmployeeMapper 接口中声明 update 方法：

```java
	/**
     * 根据主键动态修改属性
     * @param employee
     */
    void update(Employee employee);
```

在 EmployeeMapper.xml 中编写SQL：

```sql
<update id="update" parameterType="Employee">
        update employee
        <set>
            <if test="name != null">name = #{name},</if>
            <if test="username != null">username = #{username},</if>
            <if test="password != null">password = #{password},</if>
            <if test="phone != null">phone = #{phone},</if>
            <if test="sex != null">sex = #{sex},</if>
            <if test="idNumber != null">id_Number = #{idNumber},</if>
            <if test="updateTime != null">update_Time = #{updateTime},</if>
            <if test="updateUser != null">update_User = #{updateUser},</if>
            <if test="status != null">status = #{status},</if>
        </set>
        where id = #{id}
    </update>
```



### 3.3 功能测试

#### 3.3.1 接口文档测试

**测试前，**查询employee表中员工账号状态

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112143142457.png" alt="image-20221112143142457" style="zoom:67%;" /> 

**开始测试**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112143316357.png" alt="image-20221112143316357" style="zoom:50%;" /> 

**测试完毕后**，再次查询员工账号状态

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112143428676.png" alt="image-20221112143428676" style="zoom: 67%;" /> 



#### 3.3.2 前后端联调测试

**测试前：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112143552246.png" alt="image-20221112143552246" style="zoom: 33%;" /> 

**点击启用:**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112143655318.png" alt="image-20221112143655318" style="zoom:33%;" /> 



### 3.4 代码提交

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112143827631.png" alt="image-20221112143827631" style="zoom:50%;" /> 

后续步骤和上述功能代码提交一致，不再赘述。



## 4. 编辑员工

### 4.1 需求分析与设计

#### 4.1.1 产品原型

在员工管理列表页面点击 "编辑" 按钮，跳转到编辑页面，在编辑页面回显员工信息并进行修改，最后点击 "保存" 按钮完成编辑操作。

**员工列表原型：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112144731759.png" alt="image-20221112144731759" style="zoom: 67%;" /> 

**修改页面原型**：

注：点击修改时，数据应该正常回显到修改页面。

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112144842825.png" alt="image-20221112144842825" style="zoom: 67%;" /> 



#### 4.1.2 接口设计

根据上述原型图分析，编辑员工功能涉及到两个接口：

- 根据id查询员工信息
- 编辑员工信息

**1). 根据id查询员工信息**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112145607939.png" alt="image-20221112145607939" style="zoom: 80%;" />

 <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112145619775-1751800091788-5.png" alt="image-20221112145619775" style="zoom:80%;" />



**2). 编辑员工信息**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112145643769.png" alt="image-20221112145643769" style="zoom: 80%;" /> 

![image-20221112145659035](mozijie_notebook.assets/image-20221112145659035-1751800119216-7.png)

**注:因为是修改功能，请求方式可设置为PUT。**



### 4.2 代码开发

#### 4.2.1 回显员工信息功能

**1). Controller层**

在 EmployeeController 中创建 getById 方法：

```java
	/**
     * 根据id查询员工信息
     * @param id
     * @return
     */
    @GetMapping("/{id}")
    @ApiOperation("根据id查询员工信息")
    public Result<Employee> getById(@PathVariable Long id){
        Employee employee = employeeService.getById(id);
        return Result.success(employee);
    }
```



**2). Service层接口**

在 EmployeeService 接口中声明 getById 方法：

```java
    /**
     * 根据id查询员工
     * @param id
     * @return
     */
    Employee getById(Long id);
```



**3). Service层实现类**

在 EmployeeServiceImpl 中实现 getById 方法：

```java
 	/**
     * 根据id查询员工
     *
     * @param id
     * @return
     */
    public Employee getById(Long id) {
        Employee employee = employeeMapper.getById(id);
        employee.setPassword("****");// 这里是为了防止看到数据库里的密码，导致密码泄露
        return employee;
    }
```



**4). Mapper层**

在 EmployeeMapper 接口中声明 getById 方法：

```java
	/**
     * 根据id查询员工信息
     * @param id
     * @return
     */
    @Select("select * from employee where id = #{id}")
    Employee getById(Long id);
```



#### 4.2.2 修改员工信息功能

**1). Controller层**

在 EmployeeController 中创建 update 方法：

```java
	/**
     * 编辑员工信息
     * @param employeeDTO
     * @return
     */
    @PutMapping
    @ApiOperation("编辑员工信息")
    public Result update(@RequestBody EmployeeDTO employeeDTO){
        log.info("编辑员工信息：{}", employeeDTO);
        employeeService.update(employeeDTO);
        return Result.success();
    }
```



**2). Service层接口**

在 EmployeeService 接口中声明 update 方法：

```java
    /**
     * 编辑员工信息
     * @param employeeDTO
     */
    void update(EmployeeDTO employeeDTO);
```



**3). Service层实现类**

在 EmployeeServiceImpl 中实现 update 方法：

```java
 	/**
     * 编辑员工信息
     *
     * @param employeeDTO
     */
    public void update(EmployeeDTO employeeDTO) {
        Employee employee = new Employee();
        BeanUtils.copyProperties(employeeDTO, employee);

        employee.setUpdateTime(LocalDateTime.now());
        employee.setUpdateUser(BaseContext.getCurrentId());

        employeeMapper.update(employee);
    }
```

在实现**启用禁用员工账号**功能时，已实现employeeMapper.update(employee)，在此不需写Mapper层代码。



### 4.3 功能测试

#### 4.3.1 接口文档测试

分别测试**根据id查询员工信息**和**编辑员工信息**两个接口

**1). 根据id查询员工信息**

查询employee表中的数据，以id=4的记录为例

![image-20221112154253995](mozijie_notebook.assets/image-20221112154253995.png)

开始测试

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112154411245.png" alt="image-20221112154411245" style="zoom:50%;" /> 

获取到了id=4的相关员工信息

**2). 编辑员工信息**

修改id=4的员工信息，**name**由**zhangsan**改为**张三丰**，**username**由**张三**改为**zhangsanfeng**。

 <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112155001414.png" alt="image-20221112155001414" style="zoom:50%;" /> 

查看employee表数据

![image-20221112155029547](mozijie_notebook.assets/image-20221112155029547.png)



#### 4.3.2 前后端联调测试

进入到员工列表查询

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112155206712.png" alt="image-20221112155206712" style="zoom:50%;" /> 

对员工姓名为杰克的员工数据修改，点击修改，数据已回显

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112155430652.png" alt="image-20221112155430652" style="zoom:50%;" /> 

修改后，点击保存

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112155559298.png" alt="image-20221112155559298" style="zoom:50%;" /> 

### 4.4 代码提交

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112155735984.png" alt="image-20221112155735984" style="zoom:50%;" /> 

后续步骤和上述功能代码提交一致，不再赘述。



## 5. 导入分类模块功能代码￥￥￥

### 5.1 需求分析与设计

#### 5.1.1 产品原型

后台系统中可以管理分类信息，分类包括两种类型，分别是 **菜品分类** 和 **套餐分类** 。

先来分析**菜品分类**相关功能。

**新增菜品分类：**当我们在后台系统中添加菜品时需要选择一个菜品分类，在移动端也会按照菜品分类来展示对应的菜品。

**菜品分类分页查询：**系统中的分类很多的时候，如果在一个页面中全部展示出来会显得比较乱，不便于查看，所以一般的系统中都会以分页的方式来展示列表数据。

**根据id删除菜品分类：**在分类管理列表页面，可以对某个分类进行删除操作。需要注意的是当分类关联了菜品或者套餐时，此分类不允许删除。

**修改菜品分类：**在分类管理列表页面点击修改按钮，弹出修改窗口，在修改窗口回显分类信息并进行修改，最后点击确定按钮完成修改操作。

**启用禁用菜品分类：**在分类管理列表页面，可以对某个分类进行启用或者禁用操作。

**分类类型查询：**当点击分类类型下拉框时，从数据库中查询所有的菜品分类数据进行展示。



**分类管理原型：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112160907419.png" alt="image-20221112160907419" style="zoom:50%;" /> 

**业务规则：**

- 分类名称必须是唯一的
- 分类按照类型可以分为菜品分类和套餐分类
- 新添加的分类状态默认为“禁用”



#### 5.1.2 接口设计

根据上述原型图分析，菜品分类模块共涉及6个接口。

- 新增分类
- 分类分页查询
- 根据id删除分类
- 修改分类
- 启用禁用分类
- 根据类型查询分类

接下来，详细地分析每个接口。

找到资料-->项目接口文档-->苍穹外卖-管理端接口.html

**1). 新增分类**

![image-20221112163011291](mozijie_notebook.assets/image-20221112163011291-1751800189299-11.png)

 <img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112163044547.png" alt="image-20221112163044547" style="zoom:67%;" />

**2). 分类分页查询**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112163209383.png" alt="image-20221112163209383" style="zoom:50%;" />

![image-20221112163233212](mozijie_notebook.assets/image-20221112163233212-1751800213082-13.png)

**3). 根据id删除分类**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112163323554.png" alt="image-20221112163323554" style="zoom:50%;" /> 

**4). 修改分类**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112163424457.png" alt="image-20221112163424457" style="zoom:50%;" /> 

![image-20221112163445296](mozijie_notebook.assets/image-20221112163445296-1751800244158-15.png)

**5). 启用禁用分类**

![image-20221112163557247](mozijie_notebook.assets/image-20221112163557247-1751800261245-17.png)

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112163622896.png" alt="image-20221112163622896" style="zoom:50%;" />

**6). 根据类型查询分类**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112163806318.png" alt="image-20221112163806318" style="zoom:50%;" /> 

![image-20221112163839168](mozijie_notebook.assets/image-20221112163839168-1751800274975-19.png)

#### 5.1.3 表设计

**category表结构：**

| **字段名**  | **数据类型** | **说明**     | **备注**            |
| ----------- | ------------ | ------------ | ------------------- |
| id          | bigint       | 主键         | 自增                |
| name        | varchar(32)  | 分类名称     | 唯一                |
| type        | int          | 分类类型     | 1菜品分类 2套餐分类 |
| sort        | int          | 排序字段     | 用于分类数据的排序  |
| status      | int          | 状态         | 1启用 0禁用         |
| create_time | datetime     | 创建时间     |                     |
| update_time | datetime     | 最后修改时间 |                     |
| create_user | bigint       | 创建人id     |                     |
| update_user | bigint       | 最后修改人id |                     |



### 5.2 代码导入

导入资料中的分类管理模块功能代码即可

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112164259732.png" alt="image-20221112164259732" style="zoom:50%;" /> 

可按照mapper-->service-->controller依次导入，这样代码不会显示相应的报错。

进入到sky-server模块中

#### 5.2.1 Mapper层

**DishMapper.java**

```java
package com.sky.mapper;

import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

@Mapper
public interface DishMapper {

    /**
     * 根据分类id查询菜品数量
     * @param categoryId
     * @return
     */
    @Select("select count(id) from dish where category_id = #{categoryId}")
    Integer countByCategoryId(Long categoryId);

}

```

**SetmealMapper.java**

```java
package com.sky.mapper;

import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

@Mapper
public interface SetmealMapper {

    /**
     * 根据分类id查询套餐的数量
     * @param id
     * @return
     */
    @Select("select count(id) from setmeal where category_id = #{categoryId}")
    Integer countByCategoryId(Long id);

}
```

**CategoryMapper.java**

```java
package com.sky.mapper;

import com.github.pagehelper.Page;
import com.sky.dto.CategoryPageQueryDTO;
import com.sky.entity.Category;
import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Mapper;

import java.util.List;

@Mapper
public interface CategoryMapper {

    /**
     * 插入数据
     * @param category
     */
    @Insert("insert into category(type, name, sort, status, create_time, update_time, create_user, update_user)" +
            " VALUES" +
            " (#{type}, #{name}, #{sort}, #{status}, #{createTime}, #{updateTime}, #{createUser}, #{updateUser})")
    void insert(Category category);

    /**
     * 分页查询
     * @param categoryPageQueryDTO
     * @return
     */
    Page<Category> pageQuery(CategoryPageQueryDTO categoryPageQueryDTO);

    /**
     * 根据id删除分类
     * @param id
     */
    @Delete("delete from category where id = #{id}")
    void deleteById(Long id);

    /**
     * 根据id修改分类
     * @param category
     */
    void update(Category category);

    /**
     * 根据类型查询分类
     * @param type
     * @return
     */
    List<Category> list(Integer type);
}
```

**CategoryMapper.xml**,进入到resources/mapper目录下

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.sky.mapper.CategoryMapper">

    <select id="pageQuery" resultType="com.sky.entity.Category">
        select * from category
        <where>
            <if test="name != null and name != ''">
                and name like concat('%',#{name},'%')
            </if>
            <if test="type != null">
                and type = #{type}
            </if>
        </where>
        order by sort asc , create_time desc
    </select>

    <update id="update" parameterType="Category">
        update category
        <set>
            <if test="type != null">
                type = #{type},
            </if>
            <if test="name != null">
                name = #{name},
            </if>
            <if test="sort != null">
                sort = #{sort},
            </if>
            <if test="status != null">
                status = #{status},
            </if>
            <if test="updateTime != null">
                update_time = #{updateTime},
            </if>
            <if test="updateUser != null">
                update_user = #{updateUser}
            </if>
        </set>
        where id = #{id}
    </update>

    <select id="list" resultType="Category">
        select * from category
        where status = 1
        <if test="type != null">
            and type = #{type}
        </if>
        order by sort asc,create_time desc
    </select>
</mapper>

```



#### 5.2.2 Service层

**CategoryService.java**

```java
package com.sky.service;

import com.sky.dto.CategoryDTO;
import com.sky.dto.CategoryPageQueryDTO;
import com.sky.entity.Category;
import com.sky.result.PageResult;
import java.util.List;

public interface CategoryService {

    /**
     * 新增分类
     * @param categoryDTO
     */
    void save(CategoryDTO categoryDTO);

    /**
     * 分页查询
     * @param categoryPageQueryDTO
     * @return
     */
    PageResult pageQuery(CategoryPageQueryDTO categoryPageQueryDTO);

    /**
     * 根据id删除分类
     * @param id
     */
    void deleteById(Long id);

    /**
     * 修改分类
     * @param categoryDTO
     */
    void update(CategoryDTO categoryDTO);

    /**
     * 启用、禁用分类
     * @param status
     * @param id
     */
    void startOrStop(Integer status, Long id);

    /**
     * 根据类型查询分类
     * @param type
     * @return
     */
    List<Category> list(Integer type);
}

```

**EmployeeServiceImpl.java**

```java
package com.sky.service.impl;

import com.github.pagehelper.Page;
import com.github.pagehelper.PageHelper;
import com.sky.constant.MessageConstant;
import com.sky.constant.PasswordConstant;
import com.sky.constant.StatusConstant;
import com.sky.context.BaseContext;
import com.sky.dto.EmployeeDTO;
import com.sky.dto.EmployeeLoginDTO;
import com.sky.dto.EmployeePageQueryDTO;
import com.sky.entity.Employee;
import com.sky.exception.AccountLockedException;
import com.sky.exception.AccountNotFoundException;
import com.sky.exception.PasswordErrorException;
import com.sky.mapper.EmployeeMapper;
import com.sky.result.PageResult;
import com.sky.service.EmployeeService;
import org.springframework.beans.BeanUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.util.DigestUtils;

import java.time.LocalDateTime;
import java.util.List;

@Service
public class EmployeeServiceImpl implements EmployeeService {

    @Autowired
    private EmployeeMapper employeeMapper;

    /**
     * 员工登录
     *
     * @param employeeLoginDTO
     * @return
     */
    public Employee login(EmployeeLoginDTO employeeLoginDTO) {
        String username = employeeLoginDTO.getUsername();
        String password = employeeLoginDTO.getPassword();

        //1、根据用户名查询数据库中的数据
        Employee employee = employeeMapper.getByUsername(username);

        //2、处理各种异常情况（用户名不存在、密码不对、账号被锁定）
        if (employee == null) {
            //账号不存在
            throw new AccountNotFoundException(MessageConstant.ACCOUNT_NOT_FOUND);
        }

        //密码比对
        // TODO 后期需要进行md5加密，然后再进行比对
        password = DigestUtils.md5DigestAsHex(password.getBytes());
        if (!password.equals(employee.getPassword())) {
            //密码错误
            throw new PasswordErrorException(MessageConstant.PASSWORD_ERROR);
        }

        if (employee.getStatus() == StatusConstant.DISABLE) {
            //账号被锁定
            throw new AccountLockedException(MessageConstant.ACCOUNT_LOCKED);
        }

        //3、返回实体对象
        return employee;
    }

    /**
     * 新增员工
     *
     * @param employeeDTO
     */
    public void save(EmployeeDTO employeeDTO) {
        Employee employee = new Employee();

        //对象属性拷贝
        BeanUtils.copyProperties(employeeDTO, employee);

        //设置账号的状态，默认正常状态 1表示正常 0表示锁定
        employee.setStatus(StatusConstant.ENABLE);

        //设置密码，默认密码123456
        employee.setPassword(DigestUtils.md5DigestAsHex(PasswordConstant.DEFAULT_PASSWORD.getBytes()));

        //设置当前记录的创建时间和修改时间
        employee.setCreateTime(LocalDateTime.now());
        employee.setUpdateTime(LocalDateTime.now());

        //设置当前记录创建人id和修改人id
        employee.setCreateUser(BaseContext.getCurrentId());//目前写个假数据，后期修改
        employee.setUpdateUser(BaseContext.getCurrentId());

        employeeMapper.insert(employee);
    }

    /**
     * 分页查询
     *
     * @param employeePageQueryDTO
     * @return
     */
    public PageResult pageQuery(EmployeePageQueryDTO employeePageQueryDTO) {
        // select * from employee limit 0,10
        //开始分页查询
        PageHelper.startPage(employeePageQueryDTO.getPage(), employeePageQueryDTO.getPageSize());

        Page<Employee> page = employeeMapper.pageQuery(employeePageQueryDTO);

        long total = page.getTotal();
        List<Employee> records = page.getResult();

        return new PageResult(total, records);
    }

    /**
     * 启用禁用员工账号
     *
     * @param status
     * @param id
     */
    public void startOrStop(Integer status, Long id) {
        Employee employee = Employee.builder()
                .status(status)
                .id(id)
                .build();

        employeeMapper.update(employee);
    }

    /**
     * 根据id查询员工
     *
     * @param id
     * @return
     */
    public Employee getById(Long id) {
        Employee employee = employeeMapper.getById(id);
        employee.setPassword("****");
        return employee;
    }

    /**
     * 编辑员工信息
     *
     * @param employeeDTO
     */
    public void update(EmployeeDTO employeeDTO) {
        Employee employee = new Employee();
        BeanUtils.copyProperties(employeeDTO, employee);

        employee.setUpdateTime(LocalDateTime.now());
        employee.setUpdateUser(BaseContext.getCurrentId());

        employeeMapper.update(employee);
    }

}
```



#### 5.2.3 Controller层

**CategoryController.java**

```java
package com.sky.controller.admin;

import com.sky.dto.CategoryDTO;
import com.sky.dto.CategoryPageQueryDTO;
import com.sky.entity.Category;
import com.sky.result.PageResult;
import com.sky.result.Result;
import com.sky.service.CategoryService;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;
import java.util.List;

/**
 * 分类管理
 */
@RestController
@RequestMapping("/admin/category")
@Api(tags = "分类相关接口")
@Slf4j
public class CategoryController {

    @Autowired
    private CategoryService categoryService;

    /**
     * 新增分类
     * @param categoryDTO
     * @return
     */
    @PostMapping
    @ApiOperation("新增分类")
    public Result<String> save(@RequestBody CategoryDTO categoryDTO){
        log.info("新增分类：{}", categoryDTO);
        categoryService.save(categoryDTO);
        return Result.success();
    }

    /**
     * 分类分页查询
     * @param categoryPageQueryDTO
     * @return
     */
    @GetMapping("/page")
    @ApiOperation("分类分页查询")
    public Result<PageResult> page(CategoryPageQueryDTO categoryPageQueryDTO){
        log.info("分页查询：{}", categoryPageQueryDTO);
        PageResult pageResult = categoryService.pageQuery(categoryPageQueryDTO);
        return Result.success(pageResult);
    }

    /**
     * 删除分类
     * @param id
     * @return
     */
    @DeleteMapping
    @ApiOperation("删除分类")
    public Result<String> deleteById(Long id){
        log.info("删除分类：{}", id);
        categoryService.deleteById(id);
        return Result.success();
    }

    /**
     * 修改分类
     * @param categoryDTO
     * @return
     */
    @PutMapping
    @ApiOperation("修改分类")
    public Result<String> update(@RequestBody CategoryDTO categoryDTO){
        categoryService.update(categoryDTO);
        return Result.success();
    }

    /**
     * 启用、禁用分类
     * @param status
     * @param id
     * @return
     */
    @PostMapping("/status/{status}")
    @ApiOperation("启用禁用分类")
    public Result<String> startOrStop(@PathVariable("status") Integer status, Long id){
        categoryService.startOrStop(status,id);
        return Result.success();
    }

    /**
     * 根据类型查询分类
     * @param type
     * @return
     */
    @GetMapping("/list")
    @ApiOperation("根据类型查询分类")
    public Result<List<Category>> list(Integer type){
        List<Category> list = categoryService.list(type);
        return Result.success(list);
    }
}
```

全部导入完毕后，进行编译

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112170742400.png" alt="image-20221112170742400" style="zoom:50%;" /> 

### 5.3 功能测试

重启服务，访问http://localhost:80,进入分类管理

**分页查询：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112171252992.png" alt="image-20221112171252992" style="zoom:50%;" /> 

**分类类型：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112171402210.png" alt="image-20221112171402210" style="zoom:50%;" /> 

**启用禁用：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112171604991.png" alt="image-20221112171604991" style="zoom:50%;" /> 

点击禁用

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112171700774.png" alt="image-20221112171700774" style="zoom:50%;" /> 

**修改：**

回显

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112172117834.png" alt="image-20221112172117834" style="zoom:50%;" />  

修改后

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112172150709.png" alt="image-20221112172150709" style="zoom:50%;" />

**新增：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112172356093.png" alt="image-20221112172356093" style="zoom:50%;" /> 

点击确定，查询列表

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112172439370.png" alt="image-20221112172439370" style="zoom:50%;" /> 

**删除：**

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112172525216.png" alt="image-20221112172525216" style="zoom:50%;" /> 

删除后，查询分类列表

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112172611754.png" alt="image-20221112172611754" style="zoom:50%;" /> 

删除成功

### 5.4 代码提交

<img src="java_notebook/苍穹外卖/mozijie_notebook.assets/image-20221112173205048.png" alt="image-20221112173205048" style="zoom:50%;" /> 

后续步骤和上述功能代码提交一致，不再赘述。





