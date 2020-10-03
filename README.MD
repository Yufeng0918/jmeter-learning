#JMeter

## 01. 常用压力测试工具对比
- loadrunner: 性能稳定，压测结果及细粒度大，可以自定义脚本进行压测，但是太过于重大，功能比较繁多
- apache ab: 模拟多线程并发请求,ab命令对发出负载的计算机要求很低，既不会占用很多CPU，也不会占用太多的内存，但却会给目标服务器造成巨大的负载, 简单DDOS攻击等
- webbench: webbench首先fork出多个子进程，每个子进程都循环做web访问测试。子进程把访问的结果通过pipe告诉父进程，父进程做最终的统计结果。
***


## 02. JMeter4.x基础知识讲解和压测实操
### 压测不同的协议和应用
- Web - HTTP, HTTPS (Java, NodeJS, PHP, ASP.NET, …)
- SOAP / REST Webservices
- FTP
- Database via JDBC
- LDAP  轻量目录访问协议
- Message-oriented middleware (MOM) via JMS
- Mail - SMTP(S), POP3(S) and IMAP(S)
- TCP等等

## 使用场景及优点
- 功能测试
- 压力测试
- 分布式压力测试
- 纯java开发
- 上手容易，高性能
- 提供测试数据分析
- 各种报表数据图形展示
***


## 03. 本地快速安装Jmeter4.x
- 需要安装JDK8。或者JDK9,JDK10
- 快速下载: linux：http://mirrors.tuna.tsinghua.edu.cn/apache//jmeter/binaries/apache-jmeter-4.0.tgz
- 文档地址：http://jmeter.apache.org/usermanual/get-started.html
- 建议安装JDK环境，虽然JRE也可以，但是压测https需要JDK里面的 keytool工具
***

	
## 04. Jmeter目录文件讲解
- bin:核心可执行文件，包含配置
    + jmeter.bat: windows启动文件：
    + jmeter: mac或者linux启动文件：
    + jmeter-server：mac或者Liunx分布式压测使用的启动文件
    + jmeter.properties: 核心配置文件
- extras：插件拓展的包
- lib:核心的依赖包
    + ext:核心包
    + junit:单元测试包
***


## 05. Jmeter语言版本中英文切换
- 控制台修改
    + menu -> options -> choose language
- 配置文件修改	
    + bin目录 -> jmeter.properties, language=zh_CN 
***


## 06. 使用SpringBoot 2.0快速编写API测试接口
- 使用java的框架springBoot快速编写几个API接口测试
- https://spring.io/guides/gs/spring-boot/
    + 模拟GET请求，用户列表接口
	+ 模拟POST请求，用户登录接口
- http://localhost:8080/users
***


## 07. Jmeter基础功能组件介绍线程组和Sampler
- 添加->threads->线程组（控制总体并发）
    + 线程数：虚拟用户数。一个虚拟用户占用一个进程或线程
    + 准备时长（Ramp-Up Period(in seconds)）：全部线程启动的时长，比如100个线程，20秒，则表示20秒内100个线程都要启动完成，每秒启动5个线程
    + 循环次数：每个线程发送的次数，假如值为5，100个线程，则会发送500次请求，可以勾选永远循环
- 线程组->添加-> Sampler(采样器) -> Http （一个线程组下面可以增加几个Sampler）
    + 名称：采样器名称
    + 注释：对这个采样器的描述
    + web服务器：
        + 默认协议是http
        + 默认端口是80
        + 服务器名称或IP ：请求的目标服务器名称或IP地址
    + 路径：服务器URL
- 查看测试结果
    + 线程组->添加->监听器->察看结果树
***


## 08. Jmeter的断言基本使用
#### 增加断言: 线程组 -> 添加 -> 断言 -> 响应断言  
- apply to(应用范围):
    + Main sample only: 仅当前父取样器 进行断言，一般一个请求，如果发一个请求会触发多个，则就有sub sample（比较少用）
- 要测试的响应字段：
    + 响应文本：即响应的数据，比如json等文本
    + 响应代码：http的响应状态码，比如200，302，404这些
    + 响应信息：http响应代码对应的响应信息，例如：OK, Found
    + Response Header: 响应头
- 模式匹配规则：
    + 包括：包含在里面就成功
    + 匹配：响应内容完全匹配，不区分大小写
    + equals：完全匹配，区分大小写
#### 断言结果监听器: 线程组-> 添加 -> 监听器 -> 断言结果
- 里面的内容是sampler采样器的名称
- 断言失败，查看结果树任务结果颜色标红(通过结果数里面双击不通过的记录，可以看到错误信息)
#### 每个sample下面可以加单独的结果树，然后同时加多个断言，最外层可以加个结果树进行汇总
***



## 09. Jmeter实战之压测结果聚合报告分析
#### 新增聚合报告：线程组->添加->监听器->聚合报告（Aggregate Report）
#### 详解
- lable: sampler的名称
- Samples: 一共发出去多少请求,例如10个用户，循环10次，则是 100
- Average: 平均响应时间
- Median: 中位数，也就是 50％ 用户的响应时间
- 90% Line: 90％ 用户的响应不会超过该时间
- 95% Line: 95％ 用户的响应不会超过该时间
- 99% Line: 99％ 用户的响应不会超过该时间
- min: 最小响应时间
- max: 最大响应时间
- Error%: 错误的请求的数量/请求的总数
- Throughput: 吞吐量——默认情况下表示每秒完成的请求数（Request per Second) 可类比为qps
- KB/Sec: 每秒接收数据量
***


## 10. Jmeter定义变量实战
- 用户自定义
    - 很多变量在全局中都有使用，或者测试数据更改，可以在一处定义，四处使用, 比如服务器地址
    - 线程组->add -> Config Element(配置原件)-> User Definde Variable（用户定义的变量）
    - 引用方式${XXX}，在接口中变量中使用
    - 原始查看结果树和非原生查看（基础按钮）
- CSV 文件
    - 线程组->add -> Config Element(配置原件)-> CSV data set config (CSV数据文件设置)
    - 如果是多个参数需要同时引用，则在CSV数据文件里面设置加多个字段 
	    + Variabled names(comma-delitited):  csv_name,csv_pwd
***



## 11. Jmeter压测实战之JDBC request压测Mysql讲解
#### 配置
- Thread Group -> add -> sampler -> jdbc request
- jar包添加  mysql-connector-java-5.1.30.jar 
- JDBC connection Configuration 配置
    + JDBC request->add -> config element -> JDBC connection configuration
        + Max Number of connections : 最大连接数
        + MAX wait :最大等待时间
        + Auto Commit: 是否自动提交事务
        + DataBase URL : 数据库连接地址 jdbc:mysql://127.0.0.1:3306/blog
        + JDBC Driver Class : 数据库驱动，选择对应的mysql
        + username:数据库用户名
        + password:数据库密码
#### Sampler
- Debug Sampler使用（结果树中查看）
    + Thread Group -> add -> sampler -> debug sampler
- 参数讲解：(sql结尾不要加";")
    + variable name of pool declared in JDBC connection configuration（和配置文件同名）
    + Query Type 查询类型
    + parameter values 参数值
    + parameter types  参数类型
    + variable names  sql执行结果变量名
    + result variable names 所有结果当做一个对象存储
    + query timeouts  查询超时时间 
    + handle results  处理结果集
***



## 12. 分布式压测介绍
#### 分布式压测	
- 普通压测：单台机可以对目标机器产生的压力比较小，受限因素包括CPU，网络，IO等
- 分布式压测：利用多台机器向目标机器产生压力，模拟几万用户并发访问
#### Jmeter分布式压测原理
- 总控机器的节点master，其他产生压力的机器叫“肉鸡” server
- master会把压测脚本发送到 server上面
- 执行的时候，server上只需要把jmeter-server打开就可以了，不用启动jmeter
- 结束后，server会把压测数据回传给master,然后master汇总输出报告
- 配置详情
***
			

## 13. Linux 部署
#### SpringBoot 接口打包，并用jar包方式部署
- mvn package && java -jar target/gs-spring-boot-0.1.0.jar
#### 阿里云服务器介绍和ECS服务器使用
- 阿里云服务器介绍和购买ECS服务器等
#### 阿里云Linux服务器下安装启动JDK8并配置环境变量
#### 部署java项目到阿里云服务器和守护进程讲解
- 部署项目到阿里云，并启动，公网可以访问
    + 关闭防火墙
    + 阿里云控制台安全策略，开放端口
- 守护进程：nohup java -jar xxxxx &
#### 阿里云Linux服务器安装Jmeter 4.0
- wget http://apache.osuosl.org//jmeter/binaries/apache-jmeter-4.0.tgz
***



## 14. Jmeter非GUI界面 参数讲解
#### 非GUI界面，压测参数讲解
-h 帮助
-n 非GUI模式
-t 指定要运行的 JMeter 测试脚本文件
-l 记录结果的文件 每次运行之前，(要确保之前没有运行过,即xxx.jtl不存在，不然报错)
-r Jmter.properties文件中指定的所有远程服务器
-e 在脚本运行结束后生成html报告
-o 用于存放html报告的目录（目录要为空，不然报错）
#### 官方配置文件地址 http://jmeter.apache.org/usermanual/get-started.html
- jmeter -n -t linux_users_api.jmx -l result.jtl -e -o /usr/local/softwate/jmeter/temp/ResultReport
- jmeter -n -t /Users/jack/Desktop/linux_users_api.jmx -l result.jtl -e -o /Users/jack/Desktop/person/jmeter/temp
#### 阿里云Linux服务器下非GUI执行jmeter压测
- 在阿里云服务器上以非GUI界面去执行JMX压测脚本
- jmx目录：/usr/local/software/jmeter/temp
- jmeter -n -t /usr/local/software/jmeter/temp/linux_users_api.jmx -l /usr/local/software/jmeter/temp/jtl/result.jtl 
***



## 15. 可视化压测报告细讲
#### Jmeter压测实战之jtl文件生成和查看
- 利用软件从阿里云Centos服务器下载压测报告，讲解Jtl文件，并怎么查看文件
- 可以通过打开jmeter，新建线程组->summary report->浏览文件 进行查看
### Jmeter压测接口的性能优化
- 使用非GUI模式：jmeter -n -t test.jmx -l result.jtl
- 少使用Listener， 如果使用-l参数，它们都可以被删除或禁用。
- 在加载测试期间不要使用“查看结果树”或“查看结果”表监听器，只能在脚本阶段使用它们来调试脚本。
- 包含控制器在这里没有帮助，因为它将文件中的所有测试元素添加到测试计划中。]
- 不要使用功能模式,使用CSV输出而不是XML
- 只保存你需要的数据,尽可能少地使用断言
- 如果测试需要大量数据，可以提前准备好测试数据放到数据文件中，以CSV Read方式读取。
- 用内网压测，减少其他带宽影响压测结果
- 如果压测大流量，尽量用多几个节点以非GUI模式向服务器施压
- http://jakarta.apache.org/jmeter/usermanual/best-practices.html#lean_mean
### Jmeter压测生成多维度图形化HTML测试报告
- jmeter -n -t /usr/local/software/jmeter/temp/linux_users_api.jmx -l /usr/local/software/jmeter/temp/jtl/result.jtl -e -o /usr/local/software/jmeter/temp/result
- dashboard讲解
    + Test and Report informations
        + Source file：jtl文件名
        + Start Time ：压测开始时间
        + End Time ：压测结束时间
        + Filter for display：过滤器
        + Lable:sampler采样器名称	
    + APDEX(Application performance Index)
        + apdex:应用程序性能指标,范围在0~1之间，1表示达到所有用户均满意
        + T(Toleration threshold)：可接受阀值
        + F(Frustration threshold)：失败阀值
    + Requests Summary
        + OK:成功率
        + KO:失败率
    + Statistics 统计数据
        + lable:sampler采样器名称
        + samples:请求总数，并发数*循环次数
        + KO:失败次数
        + Error%:失败率
        + Average:平均响应时间
        + Min:最小响应时间
        + Max:最大响应时间
        + 90th pct: 90%的用户响应时间不会超过这个值（关注这个就可以了）
        + 2ms,3ms,4,5,2,6,8,3,9
        + 95th pct: 95%的用户响应时间不会超过这个值
        + 99th pct: 99%的用户响应时间不会超过这个值 (存在极端值)
        + throughtput:Request per Second吞吐量 qps
        + received:每秒从服务器接收的数据量
        + send：每秒发送的数据量
#### Jmeter图形化HTML压测报告Charts报表讲解
- 讲解压测报告 html里面Charts的核心指标
- Over Time（随着时间的变化）
    + Response Times Over Time：响应时间变化趋势
    + Response Time Percentiles Over Time (successful responses)：最大，最小，平均，用户响应时间分布
    + Active Threads Over Time：并发用户数趋势
    + Bytes Throughput Over Time：每秒接收和请求字节数变化，蓝色表示发送，黄色表示接受
    + Latencies Over Time：平均响应延时趋势
    + Connect Time Over Time	：连接耗时趋势
- Throughput
    + Hits Per Second (excluding embedded resources):每秒点击次数
    + Codes Per Second (excluding embedded resources)：每秒状态码数量
    + Transactions Per Second：即TPS，每秒事务数
    + Response Time Vs Request：响应时间和请求数对比
    + Latency Vs Request：延迟时间和请求数对比
- Response Times
    + Response Time Percentiles：响应时间百分比
    + Response Time Overview：响应时间概述
    + Time Vs Threads：活跃线程数和响应时间
    + Response Time Distribution：响应时间分布图
***




## 16. 多节点JMeter分布式压测实战
#### Jmeter4.0分布式压测准备工作
- 压测注意事项
    + 系统上的防火墙被关闭或正确的端口被打开。
    + 所有的客户端都在同一个子网上。
    + 如果使用192.x.x.x或10.x.x.x IP地址，则服务器位于同一子网中。 如果服务器不使用192.xx或10.xx IP地址，则不应该有任何问题。
    + 确保JMeter可以访问服务器。
    + 确保在所有系统上使用相同版本的JMeter和Java。 混合版本将无法正常工作。
    + 您已为RMI设置SSL或将其禁用。
    + 压测注意事项：一定要用内网IP，不用用公网IP,用ping去检查
- 专业名字
    + master:司令
    + slave：奴隶
    + target：目标
    + nohup ./jmeter-server &
	+ ps -ef|grep jmeter-server
#### Jmeter分布式压测实战，压测接口
- 关注CPU和内存使用
    + -r to trigger remote host
    + ./jmeter -n -t /Users/jack/Desktop/remote.jmx -r -l /Users/jack/Desktop/jtl/result.jtl -e -o /Users/jack/Desktop/result
    + Creating summariser <summary>
    + Created the tree successfully using /Users/jack/Desktop/remote.jmx
    + Configuring remote engine: 172.20.10.3:8899
    + Using local port: 8899
    + Configuring remote engine: 172.20.10.11:8899
    + Starting remote engines
    + Starting the test @ Thu Mar 29 23:21:13 CST 2018 (1522336873931)
    + Remote engines have been started
    + Waiting for possible Shutdown/StopTestNow/Heapdump message on port 4445
    + summary =      4 in 00:00:22 =    0.2/s Avg:  5582 Min:    94 Max: 21006 Err:     1 (25.00%)
    Tidying up remote @ Thu Mar 29 23:21:36 CST 2018 (1522336896842)
- 压测修改master节点信息
    + jemeter.properties 值是slave机器的ip+端口号，如果有多个，用逗号分隔
    + remote_hosts=192.168.0.102:8899,192.168.0.101:8899
    + server.rmi.ssl.disable=true
- 启动slave机器，注意要同个网段，ip地址用内网ip
    + ./jmeter-server
    + Using local port: 8899
    + server.rmi.localport=8899 表示slave server启动显示的端口
    + server_port=8899  表示master机器要远程连接的端口   即 remote_hosts=xxxx:8899
    + Created remote object: UnicastServerRef2 [liveRef: [endpoint:[192.168.0.102:8899](local),objID:[3a585a4d:162724586ab:-7fff, 3963132813614033916]]]

