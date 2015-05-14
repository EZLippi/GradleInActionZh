##拆分项目文件

到目前为止我们自定义了一个build.gradle和settings.gradle文件，随着你添加越来越多的子项目和任务到build.gradle中，代码的维护性将会下降。通过给每个子项目建立一个单独的build.gradle文件可以解决这个问题。

接下来我们在每个子项目的目录下创建一个build.gradle文件，目录如下：

![](/images/dag46.png)

现在你可以把构建逻辑从原先的build脚本中拆分开来放到合适的位置。

###定义根项目的构建代码

移除了那些与特定子项目相关的代码后，根项目的内容变得非常简单，只需要保留allprojects和subprojects配置块，如下所示：

	allprojects {
		group = 'com.manning.gia'
		version = '0.1'
	}

	subprojects {
		apply plugin: 'java'
	}

###定义子项目的构建代码

接下来你只需要把与特定项目相关的构建代码移到相应的build.gradle文件里就可以了，如下所示：

repository子项目的构建代码：

	dependencies {
		compile project(':model')
	}

web子项目的构建代码：

	apply plugin: 'war'
	apply plugin: 'jetty'

	repositories {
		mavenCentral()
	}

	dependencies {
		compile project(':repository')
		providedCompile 'javax.servlet:servlet-api:2.5'
		runtime 'javax.servlet:jstl:1.1.2'
	}

运行这个多项目构建和之前单独的一个build脚本的结果完全一样，当时你该上了构建代码的可读性和维护性。


