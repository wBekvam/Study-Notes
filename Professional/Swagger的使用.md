# Swagger的使用

> Swagger是一个用于描述和记录**[RESTful API](https://blog.csdn.net/weixin_43737917/article/details/105524365)**的项目

### 1. 简介

​	随着互联网技术的发展，现在的网站架构基本都由原来的后端渲染，变成了：前端渲染、前后端分离的形态，而且前端技术和后端技术在各自的道路上越走越远。 
​	前端和后端的唯一联系，变成了API接口；API文档成了前后端开发人员联系的纽带，变得越来越重要，`swagger`就是一款让你更好的书写API文档的框架。

### 2. Swagger Editor 安装与启动

1. [下载](https://github.com/swagger-api/swagger-editor/releases/download/v2.10.4/swagger-editor.zip)

2. 解压 swagger-editor

3. 全局安装全局安装http-server(http-server是一个简单的零配置命令行http服务器) ，需本地有Node.js环境

   ```shell
   npm install ‐g http‐server
   ```

4. 启动swagger-editor，在swagger-editor文件夹的父目录中：

   ```shell
   http‐server swagger‐editor
   ```

5. 打开浏览器访问：  [http://localhost:8080](http://localhost:8080/)

   ![image-20200506124856498](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\image-20200506124856498.png)

6. 导出文件

   File -> DownLoad Yaml

### 3. Swagger的语法规则

> 更多详情见 [官方文档](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md)

#### 3.1 字段类型与格式定义

| 普通的名字 | type    | format    | 说明                                |
| ---------- | ------- | --------- | ----------------------------------- |
| integer    | integer | int32     | 签署了32位                          |
| long       | integer | int64     | 签署了64位                          |
| float      | number  | float     |                                     |
| double     | number  | double    |                                     |
| string     | string  |           |                                     |
| byte       | string  | byte      | base64编码的字符                    |
| binary     | string  | binary    | 任何的八位字节序列                  |
| boolean    | boolean |           |                                     |
| date       | string  | date      | 所定义的full-date- - - - - -RFC3339 |
| dateTime   | string  | date-time | 所定义的date-time- - - - - -RFC3339 |
| password   | string  | password  | 用来提示用户界面输入需要模糊。      |

#### 3.2 固定字段

| 字段名              | 类型                                         | 描述                                                         |
| ------------------- | -------------------------------------------- | ------------------------------------------------------------ |
| swagger             | string                                       | 必需的。使用指定的规范版本。                                 |
| info                | Info Object                                  | 必需的。提供元数据API。 可以使用元数据的客户如果需要。       |
| host                | string                                       | 主机名或ip服务API。                                          |
| basePath            | string                                       | API的基本路径,这是相对的host。 如果不包括,API是直属host。 必须从价值领先斜杠(/)。 的basePath不支持路径模板。 |
| schemes             | [string]                                     | API的传输协议。 值必须从列表中:"http","https","ws","wss"。 如果schemes不包括,默认使用计划是用于访问大摇大摆的定义本身。 |
| consumes            | [string]                                     | [一个MIME类型的api可以使用列表。 这是可以覆盖全球所有API,但在特定的API调用。 值必须是所描述的Mime类型。](#mimeTypes) |
| produces            | [string]                                     | [MIME类型的api可以产生的列表。 这是可以覆盖全球所有API,但在特定的API调用。 值必须是所描述的Mime类型。](#mimeTypes) |
| paths               | [路径对象](#pathsObject)                     | 必需的。可用的路径和操作的API。                              |
| definitions         | [定义对象](#definitionsObject)               | 一个对象数据类型生产和使用操作。                             |
| parameters          | [参数定义对象](#parametersDefinitionsObject) | 一个对象来保存参数,可以使用在操作。 这个属性不为所有操作定义全局参数。 |
| responses           | [反应定义对象](#responsesDefinitionsObject)  | 一个对象响应,可以跨操作使用。 这个属性不为所有操作定义全球响应。 |
| securityDefinitions | [安全定义对象](#securityDefinitionsObject)   | 安全方案定义规范,可以使用。                                  |
| security            | [(安全需求对\]](#securityRequirementObject)  | 声明的安全计划申请API作为一个整体。 值的列表描述替代安全方案,可以使用(也就是说,有一个逻辑或安全需求之间)。 个人业务可以覆盖这个定义。 |
| tags                | [(标签对\]](#tagObject)                      | [的列表标签使用的规范与额外的元数据。 标签的顺序可以用来反思他们的订单的解析工具。 并不是所有使用的标签操作对象必须声明。 声明的标签不可能组织随机或基于工具的逻辑。 列表中的每个标记名称必须是唯一的。](#operationObject) |
| externalDocs        | [外部文档对象](#externalDocumentationObject) | 额外的外部文档。                                             |
| tags                | (string]                                     | 的标签列表API文档控制。 标签可用于逻辑分组业务的资源或任何其他限定符。 |
| summary             | string                                       | 什么操作的一个简短的总结。 最大swagger-ui可读性,这一领域应小于120个字符。 |
| description         | string                                       | [详细解释操作的行为。GFM语法可用于富文本表示。](https://help.github.com/articles/github-flavored-markdown) |
| externalDocs        | [外部文档对象](#externalDocumentationObject) | 额外的外部文档操作。                                         |
| operationId         | string                                       | 独特的字符串用于识别操作。 id必须是唯一的在所有业务中所描述的API。 工具和库可以使用operationId来唯一地标识一个操作,因此,建议遵循通用的编程的命名约定。 |
| consumes            | [string]                                     | MIME类型的列表操作可以使用。 这将覆盖consumes定义在炫耀的对象。 空值可用于全球定义清楚。 值必须是所描述的Mime类型。 |
| produces            | [string]                                     | MIME类型的列表操作可以产生。 这将覆盖produces定义在炫耀的对象。 空值可用于全球定义清楚。 值必须是所描述的Mime类型。 |
| parameters          | (参数对象 \|引用对象]                        | 适用于该操作的参数列表。 如果已经定义了一个参数道路项目新定义将覆盖它,但不能删除它。 必须不包含重复的参数列表。 一个独特的参数定义的组合的名字和位置。 可以使用列表引用对象链接到参数的定义的对象的参数。 可以有一个“身体”参数。 |
| responses           | [响应对象](#responsesObject)                 | 必需的。返回的列表可能的反应,因为他们执行这个操作。          |
| schemes             | [string]                                     | [传输协议的操作。 值必须从列表中:"http","https","ws","wss"。 的值将覆盖的对象schemes定义。](#swaggerSchemes) |
| deprecated          | boolean                                      | 声明该操作被弃用。 使用声明的操作应该没有。 默认值是false。  |
| security            | [(安全需求对\]](#securityRequirementObject)  | [声明的安全计划申请这个操作。 值的列表描述替代安全方案,可以使用(也就是说,有一个逻辑或安全需求之间)。 这个定义覆盖任何宣布顶级security。 删除一个顶级安全声明,可以使用一个空数组。](#swaggerSecurity) |

### 4. SwaggerUI

> SwaggerUI是用来展示Swagger文档的界面

**安装步骤：**

1. 在本地安装 [Nginx](http://nginx.org/en/download.html)

2. 下载 SwaggerUI 源码： [下载](https://swagger.io/tools/swagger-ui/download/)

3. 解压swagger-ui-master.zip，将dist文件夹下的全部文件拷贝至 nginx的html目录

4. 启动nginx

   ​	可能存在端口被占用，可在conf/nginx.conf下更改默认端口（80）

   ```shell
   start nginx
   ```

5. 浏览器打开页面 http://localhost即可看到文档页面

   ![](D:\笔记\插图\swaggerUI.png)

6. 我们将编写好的yml文件也拷贝至nginx的html目录，这样我们就可以加载我们的swagger文档了

   ![](D:\笔记\插图\swaggerUI01.png)