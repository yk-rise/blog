---
{"dg-publish":true,"permalink":"/技术/FPGA/xilinx acap/xilinx acap/"}
---


[[技术/FPGA/FPGA\|FPGA]]

##### 设计中心链接
https://china.xilinx.com/support/documentation-navigation/design-hubs.html

##### 教程链接：
https://xilinx.github.io/Embedded-Design-Tutorials/docs-cn/Versal-EDT/docs/2-cips-noc-ip-config.html

# 什么是acap

Xilinx的ACAP是一种高性能的可编程计算平台，可用于构建专用的加速器来实现高效的数据处理。



![Pasted image 20230103140958.png](/img/user/%E6%96%87%E4%BB%B6/Pasted%20image%2020230103140958.png)

**异构加速**
是指在系统中使用不同体系结构的处理器的联合计算方式。

# 使用
对于这一点，赛灵思给出了两种方案：

如果你有过使用FPGA的经历的硬件开发者，那么基于Vivado的那套传统的开发流程仍然是适用的，你可以像使用FPGA那样去使用ACAP；

如果你是没有任何硬件开发背景的软件开发者，也别不用担心，你同样可以用Versal ACAP来加速你的应用，方法就是使用Vitis软件平台。

## 流程
1. Vivado是Xilinx的硬件设计工具，支持ACAP的开发。
    
2.  创建新的ACAP设计。在Vivado中，选择“新建设计”，然后选择“ACAP”作为设计类型。
    
3.  配置ACAP设备。在Vivado中，选择ACAP设备，并调整其配置设置。
    
4.  编写ACAP软件。使用Xilinx ACAP SDK，编写C/C++代码来实现您想要加速的算法。
    
5.  编译和下载ACAP软件。使用Vivado工具，将ACAP软件编译成可以在ACAP设备上运行的二进制文件。然后将该文件下载到ACAP设备上。
    
6.  运行ACAP软件。在ACAP设备上运行您编写的ACAP软件，以便它能够实现加速

# 文档链接
UG1504 - Versal ACAP 系统和解决方案规划方法指南（中文版） (v2020.2)

https://link.zhihu.com/?target=https%3A//china.xilinx.com/support/documentation/sw_manuals/xilinx2020_2/c_ug1504-acap-system-solution-planning-methodology.pdf

UG1387 - Versal ACAP 硬件、IP 和平台开发方法指南（中文版） (v2020.2)

https://link.zhihu.com/?target=https%3A//china.xilinx.com/support/documentation/sw_manuals/xilinx2020_2/c_ug1387-acap-hardware-ip-platform-dev-methodology.pdf

UG1273 - Versal ACAP 设计指南（中文版） (v2020.2)

https://link.zhihu.com/?target=https%3A//china.xilinx.com/support/documentation/sw_manuals/xilinx2020_2/c_ug1273-versal-acap-design.pdf

