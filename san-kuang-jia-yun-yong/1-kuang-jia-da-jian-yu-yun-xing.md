1 JDK要求1.8

代码中使用了[j](http://www.baidu.com/link?url=MHAA3X9Hi2sLn1Q6aCJiCTVj3fO9xxbi1y6xpxe51s6CfpUi3k2_tjb7vDqvz_fi)ava8的lambda表达式

2 导入项目

使用idea导入maven项目

![](/assets/idea1.png)

选择文件夹

![](/assets/idea2.png)

选maven

![](/assets/idea3.png)

按下图勾选

![](/assets/idea4.png)

![](/assets/idea5.png)

点击next

![](/assets/idea6.png)

输入项目名称

![](/assets/idea7.png)

如果报错，可以尝试rebuild一下

![](/assets/idea8.png)

3 初始化数据库

默认为mysql数据库

创建数据库，并执行sql，完成数据库基础表和数据的初始化

4 修改数据库配置参数

找到application.yml\(或者application.propertie\)

![](/assets/mysql1.png)

根据自己的数据库信息进行配置，端口号、数据库名、用户名、密码。

![](/assets/mysql2.png)

5 安装redis

因为项目中使用redis保存每个用户的token，所以运行前必须保证redis服务启动

Windows版redis下载路径

[https://github.com/MicrosoftArchive/redis/releases](https://github.com/MicrosoftArchive/redis/releases)

安装zip的免安装版本即可

双击redis server.exe即可运行

6 启动系统

![](/assets/start.png)

右键Run As或者DebugAs-&gt;JavaApplication

7 在浏览器输入http://localhost:8080

出现下面界面，说明启动成功![](/assets/loginjpg.png)

