
执行!
======

目前，Cromwell是我们所知道的唯一支持WDL的全功能执行引擎。

Cromwell
---------

Cromwell是一个用Java编写的开源(BSD 3-clause)执行引擎，它支持在三种类型的平台上运行WDL: 本地机(例如您的笔记本电脑)、通过作业调度器(例如GridEngine)访问的本地集群/计算场或云平台(例如谷歌云或Amazon AWS)。

.. image:: images/execution1.png

它是以美国演员詹姆斯·克伦威尔(James Cromwell)的名字命名的，他是《宝贝》(Babe)和《星际迷航:第一次接触》(star Trek: First Contact)等伟大电影中的明星，因此它的吉祥物是“变形猪杰米”(Jamie the warp pig)。

Cromwell可执行文件可以作为预编译的jar文件从Cromwell GitHub存储库中获得。它需要Java 8来运行。

基本的命令语法如下:

  :: 
    
    java -jar cromwell.jar <action> <parameters>

关于通过Cromwell运行WDLs的更详细的例子可以在WDL文档的教程部分找到。


Cromwell本地运行WDL
--------------------

在本地机器上运行是世界上最简单的事情。假设您已经验证了一个名为myWorkflow.wdl的WDL脚本和一个名为myWorkflow_inputs.json的输入JSON文件，您只需调用Cromwell的运行函数，如下所示:

  :: 

    java -jar Cromwell.jar run myWorkflow.wdl --inputs myWorkflow_inputs.json

这将运行您的工作流。将看到来自Cromwell引擎将为你更新在它走过的步骤的信息。

.. note:: 
    注意，通常由工具本身输出到终端的任何消息实际上都不会显示在运行脚本的终端中。相反，Cromwell将此输出保存在执行文件夹中名为stderr的日志文件中。

默认情况下，你可以在这个文件夹中找到所有生成的文件(输出和日志):

.. image:: images/execution2.png


在其他平台上运行
-----------------

在集群或云平台上运行可能比这更复杂一些，所以我们在这个快速入门指南中不介绍它们。我们提供了一些指向将Cromwell封装在 `工具包 <https://software.broadinstitute.org/wdl/toolkit>`_ 页面上的平台或集成的指针。



这是它!现在您已经准备好自己编写一些脚本了，所以请直接进入教程部分。

