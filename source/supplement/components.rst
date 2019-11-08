
Script Components
===================

basename()
----------

这个WDL函数允许你根据另一个文件名来命名一个输出文件，例如输入文件名。给它一个文件会去掉路径，只生成文件名(作为字符串)。如果您还给它一个字符串作为次要参数，该函数将尝试从文件名的末尾去掉该字符串，然后返回剩下的内容。

这是超级方便剥离一个文件扩展名，并取代它与其他东西，当你想要例如命名一个任务的输出文件基于一个输入文件名。

基本用法:
^^^^^^^^^

  :: 

	File input_file = "/Users/chris/input.bam"
	String base = basename(input_file)


这产生input.bam。

提供一个后缀字符串:

  :: 

	File input_file = "/Users/chris/input.bam"
	String stripped = basename(input_file, ".bam") 

这将生成输入，然后可以将其与新的后缀字符串组合以生成所需的输出文件名。

call
-----

call组件在工作流主体中用于指定应该执行的特定任务。在最简单的形式中，调用只需要一个任务名称。

我们还可以添加一个代码块来指定任务的输入变量。我们还可以修改call语句来调用别名下的任务，这允许在相同的工作流中使用不同的参数多次运行相同的task。这使得重用代码非常容易;这在实践中是如何工作的，将在快速入门指南的管道选项部分详细解释。

请注意，call语句的执行顺序并不取决于它们在脚本中出现的顺序;相反，它是基于任务调用之间的依赖关系图来确定的。这意味着程序通过评估任务调用的输入是其他任务调用的输出来推断任务调用的顺序。这也将在管道选项一节中详细解释。

用法示例
^^^^^^^^

  :: 

	# in its simplest form 
	call my_task

	# with input variables
	call my_task{
	    input: task_var1= workflow_var1, task_var2= workflow_var2, ...
	}

	# with an alias and input variables
	call my_task as task_alias {
	    input: task_var1= workflow_var1, task_var2= workflow_var2, ...
	}

WDL规范中的更多细节

command
--------

command组件是任务所需的属性。command块的主体用占位符(例如${input_file})指定要运行的文字命令行(基本上是可以在终端shell中运行的任何命令)，这些占位符用于需要填充的命令行变量部分。注意，必须在任务输入定义中定义所有变量占位符。

用法示例
^^^^^^^^^

  :: 

	command {
	    java -jar myExecutable.jar \
		INPUT=${input_file} \
		OUTPUT=${output_basename}.txt
	}

WDL规范中的更多细节

meta
----
meta组件是任务的可选属性。它的目的是存储元数据，如任务的作者或联系人电子邮件。可以使用任何键/值对。

用法示例
^^^^^^^^^

  :: 

	meta {
	    author: "Broad Institute DSDE"
	}

WDL规范中的更多细节

output
-------

output组件是task (mostly*)所需的属性。它用于显式地标识任务命令的输出，以便进行流控制。这里标识的输出将用于构建工作流图，因此，重要的是要包括用作工作流中其他任务的输入的所有输出。

* 从技术上讲，对于不生成在其他任何地方使用的输出的任务(如规范的“Hello World”示例)，不需要输出。但是这种情况非常少见，因为在大多数情况下，当您编写一个实际做一些有用的工作的工作流时，每个任务命令都会产生某种输出。否则，你为什么要运行它?

WDL接受的所有类型的变量都可以包括在这里。输出定义必须包含显式类型声明。

用法示例
^^^^^^^^^

  :: 

	output {
	    File out = "${output_basename}.txt"
	}

WDL规范中的更多细节


parameter_meta
---------------

parameter_meta组件是任务的可选属性。它用于存储任务命令中使用的输入参数和参数的描述，如果变量名不够描述性，这将非常有用。本节中的任何项(即input_file)都必须出现在命令行中。

用法示例
^^^^^^^^^

  :: 

	parameter_meta {
	    input_file: "the BAM file to process"
	    sample_id: "the name of a sample"
	}

WDL规范中的更多细节

runtime
--------

runtime组件是任务的可选属性。它可用于通过键/值对指定环境参数。这些值可以任意设置，但是后端负责解释这些值。Cromwell目前支持三个后端，local, JES和SGE，它们解释了以下runtime属性:

=====================  ========  ======  =====     
Runtime Attribute      LOCAL     JES     SGE
=====================  ========  ======  =====
continueOnReturnCode   x         x       x
cpu                              x       x
disks                            x        
zones                            x        
docker                 x         x       x
failOnStderr           x         x       x
memory                           x       x
preemptible                      x        
bootDiskSizeGb                   x        
=====================  ========  ======  =====


注意，有两个特殊的保留键:

*  docker 要在其上运行任务命令的Docker映像。虽然一般情况下不需要它，但它在云执行服务等上下文中非常有用。例如，如果您计划使用Java执行服务运行Cromwell，则需要使用Docker映像。有关如何生成Docker映像的信息，请阅读本指南。

*  memory 任务的内存需求。这应该是一个带后缀的整数值，比如B、KB、MB或二进制后缀KiB、MiB等等。请注意，如果在本地机器(如笔记本电脑)上运行，克伦威尔将不支持此参数。

使用的例子
^^^^^^^^^^

  :: 

	runtime {
	    docker: "organization/myImage"
	}

WDL规范中的更多细节


task
-----

task组件是WDL脚本的顶级组件。它包含“做某事”所需的所有信息，这些信息围绕着一个命令，该命令附带输入文件和参数的定义，以及输出组件中输出的显式标识。还可以使用runtime、meta和parameter_meta组件为其提供附加(可选)属性。

任务在workflow命令中被“调用”，这是在运行脚本时执行它们的原因。同一个任务可以在同一个工作流中使用不同的参数多次运行，这使得重用代码变得非常容易。这在实践中是如何工作的，将在管道选项一节中详细解释。

用法示例
^^^^^^^^^

  :: 

	task my_task {
	    [ input definitions ]
	    command { ... }
	    output { ... }
	}

WDL规范中的更多细节

variables
----------

注意，这在技术上不是一个正式的WDL组件，比如任务或工作流，因为在WDL规范中没有命名为变量的实体。为了简化文档，我们将变量作为一个组进行讨论，并将它们与适当的组件放在一起。我们当然希望这不会适得其反，反而让大家更加困惑。

与大多数编程语言一样，WDL区分了5种基本类型的变量，也称为基本类型:

* String : 一系列字母数字字符;通常用于存储(短)文本、文件名或在基因组学中存储DNA序列等信息。

* Float :  一个十进制数;例如3.1459(也可以是负数)。

* Int : 一个整数;例如16(也可以是负数)。

* Boolean : 表示二进制值的逻辑元素;例如真或假。

* File :  表示文件的对象，它与文件名本身有一点不同。

这些基本类型可以分为更复杂的数据结构，也称为复合类型:

* Array :  按索引位置存储、排序和检索的元素列表;例如[A,B,C,D]是一个字符串数组或数组[String]，我们可以通过选择第二个元素来选择B元素(索引位置1，因为WDL数组是0索引的)。

* Map :  键值对列表;例如{"color": "blue"， "size": "large"}是字符串到字符串的映射，或者映射[String, String]，在这里我们可以询问对象的颜色。

* Object : 这个有点奇怪，不常用，所以请参阅规范了解更多信息。

您可以在任务本身或工作流中声明变量，使用以下语法:

  :: 

    Type variableName

有时，您可能不希望每次调用任务时都使用变量。要使变量成为可选的(这意味着您不需要在JSON输入文件或工作流调用中设置值)，只需添加?修饰符*,像这样:

  :: 

    Type? variableName

注意，你也可以看到?在类型旁边使用一个空格的修饰符，像这样: Type ? variableName . 这两种格式目前都是允许的(直到 Cromwell v24)，但额外的空间可能在未来的版本中被禁止。

在命令中使用可选变量时，可以指定默认值。这告诉执行引擎，“如果我没有为这个变量提供我自己的值，那么就使用这个默认值。” 它的语法是:

  :: 

    ${default="value" variableName}

此时，不可能在工作流级别使用可选变量。

WDL规范中的更多细节


workflow
---------

工作流组件是WDL脚本所需的顶级组件。它包含调用任务组件的调用语句，以及工作流级别的输入定义。可以通过调用和其他语句将各种任务链接在一起;这些都在管道选项文档中有详细说明。

用法示例
^^^^^^^^^

  :: 

	workflow myWorkflowName {
	    call my_task
	}


WDL规范中的更多细节



