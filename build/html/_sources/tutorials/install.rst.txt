
(howto) 安装WDL车间的软件
==========================

要参加我们的实践工作坊，你需要下载以下程序:

wdltool.jar
------------

`wdltool <https://github.com/broadinstitute/wdltool>`_ 工具包是一个实用程序包，它提供了编写和运行WDL脚本的辅助功能，包括语法验证和输入模板生成。您可以在 `这里 <https://github.com/broadinstitute/wdltool/releases>`_ 下载预编译的可执行文件的最新版本。

cromwell.jar
------------

Cromwell是一个执行引擎，能够运行用WDL编写的脚本，描述涉及命令行工具的数据处理和分析工作流(比如实现GATK `最佳实践 <https://www.broadinstitute.org/gatk/guide/best-practices.php>`_ 的管道)。最新的版本可以在 `这里 <https://github.com/broadinstitute/cromwell/releases>`_ 以预编译的jar的形式下载.

gatk.jar
---------

我们的教程使用来自GATK (GenomeAnalysisToolkit)的工具来演示如何编写执行实际数据处理和分析任务的WDL脚本;为了遵循他们，你需要安装GATK。您可以按照以下 `说明 <http://gatkforums.broadinstitute.org/gatk/discussion/2899/howto-install-all-software-packages-required-to-follow-the-gatk-best-practices/p1#gatk>`_ 进行操作。注意，你不必在上面链接的文章中安装所有的东西，只要GATK就可以了。

为了运行这些工具，您需要安装Java version 8，您可以在 `这里 <http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html>`_ 找到它。为了使运行wdltool、cromwell和gatk更容易，您应该为 `每个终端配置文件添加一个环境变量 <https://www.google.com/amp/s/www.cyberciti.biz/faq/set-environment-variable-linux/amp/>`_ ，指向适当的jar文件。我们将使用环境变量$gatk、$cromwell和$wdltool。

此时，您应该能够通过在终端中为cromwell和wdl jar文件调用java -jar <environment variable here> -help 来测试是否一切正常。如果它们起作用，您将看到打印出的文本描述您可以使用每个工具调用哪些函数。要测试gatk，使用--help而不是-help。


SublimeText
-----------

WDL可以用任何文本编辑程序编写，但是对于这个研讨会，我们将使用SublimeText。这是一个简单而有效的程序，您可以在 `这里 <http://www.sublimetext.com/>`_ 下载。这个程序还允许WDL的语法高亮显示，您可以按照 `这里 <https://github.com/broadinstitute/wdl/tree/develop/highlighters/SublimeText>`_ 的说明进行安装。

DATA BUNDLE
------------

最后，也是最重要的，您将需要我们为本次研讨会准备的数据包。它包含了我们将用于动手实践的材料，尽管它很小，但是如果每个人都在研讨会开始时等待下载它，我们经常会遇到下载滞后的问题。您可以在 `这里 <https://drive.google.com/open?id=0B7akc6CTmxIHVUVyS2FxYmM3a1k>`_ 找到数据包。


