##### 1、前言
>为提高代码质量，方便地对代码质量进行分析并给出合理的解决方案，引入代码质量管理工具。<br>
>>SonarQube是一个开源的代码质量管理系统，支持 25+ 种语言，可以通过使用插件机制与 IDEA和 JIRA 等其他外部工具集成，从而实现了对代码的质量的全面自动化分析和管理。

##### 2、SonarQube概述  
```
SonarQube 不是简单地将各种质量检测工具的结果（例如 FindBugs，PMD 等）直接展现给客户，而是通过不同的插件算法
来对这些结果进行再加工, 最终以量化的方式来衡量代码质量，从而方便地对不同规模和种类的工程进行相应的代码质量管理。
```
- SonarQube 可以从七个维度检测代码质量:
1. 复杂度分布(complexity): 代码复杂度过高将难以理解
2. 重复代码(duplications): 程序中包含大量复制、粘贴的代码而导致代码臃肿，sonar可以展示源码中重复严重的地方
3. 单元测试统计(unit tests): 统计并展示单元测试覆盖率，开发或测试可以清楚测试代码的覆盖情况
4. 代码规则检查(coding rules): 通过Findbugs,PMD,CheckStyle等检查代码是否符合规范
5. 注释率(comments): 若代码注释过少，特别是人员变动后，其他人接手比较难接手；若过多，又不利于阅读
6. 潜在的Bug(potential bugs): 通过Findbugs,PMD,CheckStyle等检测潜在的bug
7. 结构与设计(architecture & design): 找出循环，展示包与包、类与类之间的依赖、检查程序之间耦合度
- SonarQube平台包含了四个组件：分析器，服务端，安装在服务端的插件以及数据库。
![alt](https://github.com/lxhzmy/reference-book/blob/master/picture/sonar1.jpg "sonar")

- 分析器负责一行一行地执行代码分析。它能够提供技术债务，代码覆盖率，代码复杂度，检测出的问题等信息。检查出来的问题可以是bug，
潜在的bug，或者是可能在将来导致错误的问题等。当分析完成后，结果可以通过由SonarQube服务端提供的网页进行浏览。
SonarQube的web服务器简化了SonarQube实例的配置，插件的安装等过程，并提供一个直观的结果概览图。 

##### 3、代码分析
###### 3.1 Analyzing with SonarQube Scanner
```
1. 下载sonar-scanner解压，将bin文件加入环境变量path中，如我的路径E:\sonar\sonar-scanner\bin将此路径加入path中。

2. 修改sonar scanner配置文件， conf/sonar-scanner.properties。配置需要访问的sonar服务和mysql服务器地址、用户密码。

3. 查看服务是否Ok sonar-scanner -h 

4. 将sonar-project.properties 放入需要扫描的project中
# must be unique in a given SonarQube instance
sonar.projectKey=HCMessage-server
# this is the name and version displayed in the SonarQube UI. Was mandatory prior to SonarQube 6.1.
sonar.projectName=HCMessage-server
sonar.projectVersion=1.0
sonar.java.binaries=build/classes/java/main
# Path is relative to the sonar-project.properties file. Replace "\" by "/" on Windows.
# This property is optional if sonar.modules is set.
sonar.sources=.
# Encoding of the source code. Default is default system encoding
sonar.sourceEncoding=UTF-8

5. 启动分析 sonar-scanner -X
```
###### 3.2 Analyzing with SonarQube Scanner for Gradle 
```
1. 引入插件build.gradle
buildscript {
    repositories {
        maven { url 'https://plugins.gradle.org/m2/' }
    }
    dependencies {
        classpath "org.sonarsource.scanner.gradle:sonarqube-gradle-plugin:2.6.2"
    }
}

apply plugin: "org.sonarqube"

2. 添加sonarqube配置参数
sonarqube {
    properties {
        property "sonar.host.url", "http://10.164.206.60:9000/sonar"
        property "sonar.projectKey", "Outreach"
    }
}

2. 启动分析 gradle sonarqube

```
###### 3.3 Analyzing with SonarQube Scanner for Jenkins 
TBD

##### 4、结果展示
SonarQube 进行代码分析之后，会将分析结果发送至SonarQube服务端，可以通过浏览器查看分析结果。

##### 5、SonarQube和持续集成

##### 6、结束
SonarQube 的使用使代码质量控制变地更加容易，并且能够减少真实的和潜在的bug的数量。开发者们能够更加专注于逻辑本身，花费更多时间进行业务分析，并为具体的案例寻找最有效的解决方案。使用了SonarQube 进行代码检查之后，管理者们开始跟踪代码分析的结果，因为他们相信这些结果能够较好地反应项目的开发状况。

[sonar_jiegou]:https://github.com/lxhzmy/reference-book/blob/master/picture/sonar1.jpg
