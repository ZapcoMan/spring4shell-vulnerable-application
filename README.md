README.md
# Spring4Shell PoC 应用程序
# （CVE-2022-22965）概念验证应用程序和漏洞利用

这是一个容易受到Spring4Shell漏洞（CVE-2022-22965）影响的docker化应用程序。提供了WAR的完整Java源代码并可以修改，每次构建docker镜像时都会重新构建WAR文件。然后Tomcat将加载构建好的WAR文件。这个应用程序没有什么特别之处，它是一个简单的“Hello World”应用程序，基于[Spring教程](https://spring.io/guides/gs/handling-form-submission/)。

详细信息: https://www.lunasec.io/docs/blog/spring-rce-vulnerabilities

遇到POC问题？请查看LunaSec的分支：https://github.com/lunasec-io/Spring4Shell-POC，它维护得更积极。

## 要求

1. Docker
2. Python3 + requests库

## 指南

1. 克隆仓库
2. 构建并运行容器: `docker build . -t spring4shell && docker run -p 8080:8080 spring4shell`
3. 应用程序现在应该可以在 http://localhost:8080/helloworld/greeting 访问

![WebPage](screenshots/webpage.png?raw=true)

4. 运行exploit.py脚本:
 `python exploit.py --url "http://localhost:8080/helloworld/greeting"`

![WebPage](screenshots/runexploit_2.png?raw=true)

5. 访问创建的webshell！修改`cmd` GET参数以执行您的命令。（默认为 `http://localhost:8080/shell.jsp`）

![WebPage](screenshots/RCE.png?raw=true)

## 注意事项

**已修复！** ~~截至目前，容器（可能是Tomcat）必须在每次利用之间重启。我正在积极尝试解决这个问题。~~

重新运行利用将在服务器上创建一个额外的{old_filename}_.jsp文件。

欢迎通过PR/DM [@Rezn0k](https://twitter.com/rezn0k) 提出改进意见！

## 致谢

- [@esheavyind](https://twitter.com/esheavyind) 提供了构建PoC的帮助。查看他们的说明：https://gist.github.com/esell/c9731a7e2c5404af7716a6810dc33e1a
- [@LunaSecIO](https://twitter.com/LunaSecIO) 改进了文档和利用脚本
- [@rwincey](https://twitter.com/rwincey) 使利用脚本无需重启Tomcat即可重复运行
