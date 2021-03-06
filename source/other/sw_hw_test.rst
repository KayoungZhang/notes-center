
.. 标题文字下的符号长度都要大于标题长度

《调试九法：软硬件错误的排除之道》
=====================================

调试规则九法：
  
 **- 1. 理解系统**
 
仔细查阅手册每个细节。

 **- 2. 制造失败**
 
从头开始复制现象，查找不受控制的条件，记录每件事情，不要过于相信统计数据。

 **- 3. 不要想，而要看**
 
查看细节，并使用外部调试工具

 **- 4. 分而治之**
 
主次逼近缩小搜索范围，使用易于查看的测试模式。

 **- 5. 一次只改一个地方**
 
隔离关键因素，一次只改动一个测试，与正常情况进行对比，确定自从上一次正常工作以来改动的地方。

 **- 6. 保持审计跟踪**
 
记下你的每步操作、顺序和结果，记录任何细节，最好用计算机记录下来附加到bug报告中，可以之道修订版本信息。

 **- 7. 检查插头**
 
质疑你的假设，尤其是一些显而易见的假设，从头开始检查，对工具进行测试。

 **- 8. 获得全新观点**
 
征求别人的意见，获取专业知识，听取别人的经验，报告症状而不要讲你的理论。

 **- 9. 如果你不修复bug，它将依然存在** 
 
bug从来不会自己消失，从根本上解决问题。


