
Components
===========

basename()
----------


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



task
-----

variables
----------




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



