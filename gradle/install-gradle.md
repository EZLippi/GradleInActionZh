##安装Gradle

Gradle的安装非常方便，下载ZIP包，解压到本地目录，设置 GRADLE_HOME 环境变量并将 GRADLE_HOME/bin 加到 PATH 环境变量中，安装就完成了。用户可以运行gradle -v命令验证安装，这些初始的步骤和Maven没什么两样。我这里安装的Gradle版本是1.10，详细信息见下：

	ᐅ gradle -v

	------------------------------------------------------------
	Gradle 1.10
	------------------------------------------------------------
	Build time:   2013-12-17 09:28:15 UTC
	Build number: none
	Revision:     36ced393628875ff15575fa03d16c1349ffe8bb6
	Groovy:       1.8.6
	Ant:          Apache Ant(TM) version 1.9.2 compiled on July 8 2013
	Ivy:          2.2.0
	JVM:          1.7.0_45 (Oracle Corporation 24.45-b08)
	OS:           Mac OS X 10.9.2 x86_64

