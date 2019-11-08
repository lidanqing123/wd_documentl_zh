

验证语法
=========

没有人喜欢启动一堆工作后，从午餐回来后发现，由于脚本中的语法错误，它们都在几分钟内失败。Missing bracket, schmacket. 

好消息是WDL附带了一个名为wdltool的实用工具工具包，其中包含一个语法验证函数。获取和运行wdltool可执行文件的说明在这里(但是现在不要担心它——在安装所需的所有内容时，我们将再次提供该链接)。

为了验证我们的WDL语法，我们只需在脚本中调用validate函数:

  :: 

    $ java -jar wdltool.jar validate myWorkflow.wdl


这个函数解析WDL脚本并警告我们任何语法错误，如缺少花括号、未定义变量、缺少逗号等。它将解析导入，但是注意，它不能识别错误，比如命令中的输入错误、指定错误的文件名或未能为工作流中运行的程序提供所需的输入。

Example: error if a call references a task that doesn't exist:
---------------------------------------------------------------

  :: 

	$ java -jar wdltool.jar validate myWorkflow.wdl
	ERROR: Call references a task (BADps) that doesn't exist (line 22, col 8)

	  call BADps
	       ^

一旦我们修复了任何语法错误，我们就几乎可以运行我们的脚本了!我们和执行之间只是一步之遥了。嗯。这有点令人担心，不是吗?也许我们可以换个说法?确定。离天堂只有一步之遥了。不，这并不好。好吧，在我们和一个良好编写的WDL脚本在我们首选的执行引擎上顺利运行之间只差一步。好的，我们继续。
