#构建块

每个Gradle构建都包括三个基本的构建块：项目(projects)、任务(tasks)和属性(properties)，每个构建至少包括一个项目，项目包括一个或者多个任务，项目和任务都有很多个属性来控制构建过程。

Gradle运用了领域驱动的设计理念（DDD）来给自己的领域构建软件建模，因此Gradle的项目和任务都在Gradle的API中有一个直接的class来表示，接下来我们来深入了解每一个组件和它对应的API。

##项目

在Gradle术语里项目表示你想构建的一个组件(比如一个JAR文件)，或者你想完成的一个目标(比如打包app)，如果你以前使用过Maven，你应该听过类似的概念。与Maven pom.xml相对应的是build.gradle文件，每个Gradle脚本至少定义了一个项目。当开始构建过程后，Gradle基于你的配置实例化org.gradle.api.Project这个类以及让这个项目通过project变量来隐式的获得。下图列出了API接口和最重要的方法。

![](/images/dag24.png)

一个项目可以创建新任务、添加依赖和配置、应用插件和其他脚本，许多属性比如name和description都是可以通过getter和setter方法来访问。

Project实例允许你访问你项目所有的Gradle特性，比如任务的创建和依赖了管理，记住一点当访问你项目的属性和方法时你并不需要显式的使用project变量--Gradle假定你的意思是Project实例，看看下面这个例子：

	//没有使用project变量来设置项目的描述
	setDescription("myProject")
	//使用Grovvy语法来访问名字和描述
	println "Description of project $name: " + project.description

在之前的章节，我们只处理到单个peoject的构建，Gradle支持多项目的构建，软件设计一个很重要的概念是模块化，当一个软件系统变得越复杂，你越想把它分解成一个个功能性的模块，模块之间可以相互依赖，每个模块有自己的build.gradle脚本。

##任务

我们在第二章的时候就创建了一些简单的任务，你应该了解两个概念：任务动作(actions)和任务依赖，一个动作就是任务执行的时候一个原子的工作，这可以简单到打印hello world,也可以复杂到编译源代码。很多时候一个任务需要在另一个任务之后执行，尤其是当一个任务的输入依赖于另一个任务的输出时，比如项目打包成JAR文件之前先要编译成class文件，让我们来看看Gradle API中任务的表示：org.gradle.api.Task 接口。

![](/images/dag25.png)


##属性

每个Project和Task实例都提供了setter和getter方法来访问属性，属性可以是任务的描述或者项目的版本号，在后续的章节，你会在具体例子中读取和修改这些属性值，有时候你要定义你自己的属性，比如，你想定义一个变量来引用你在构建脚本中多次使用的一个文件，Gradle允许你通过外部属性来定义自己的变量。

###外部属性

外部属性一般存储在键值对中，要添加一个属性，你需要使用ext命名空间，看一个例子：

	//Only initial declaration of extra property requires you to use ext namespace
	project.ext.myProp = 'myValue'
	ext {
	   someOtherProp = 123
	}

	//Using ext namespace to access extra property is optional
	assert myProp == 'myValue'
	println project.someOtherProp
	ext.someOtherProp = 567	

相似的，外部属性可以定义在一个属性文件中：
通过在<USER_HOME>/.gradle路径或者项目根目录下的 gradle.properties文件来定义属性可以直接注入到你的项目中，他们可以通过 project实例来访问，注意<USER_HOME>/.gradle目录下只能有一个 Gradle属性文件即使你有多个项目，在属性文件中定义的属性可以被所有的项目访问，假设你在你的gradle.properties文件中定义了下面的属性：

	exampleProp = myValue
	someOtherProp = 455

你可以在项目中访问这两个变量：

	assert project.exampleProp == 'myValue'

	task printGradleProperty << {
	   println "Second property: $someOtherProp"
	}

**定义属性的其他方法**

你也可以通过下面的方法来定义属性：

* 通过-P命令行选项来定义项目属性
* 通过-D命令行选项来定义系统属性
* 环境属性遵循这个模式： `ORG_GRADLE_PROJECT_propertyName=someValue`



