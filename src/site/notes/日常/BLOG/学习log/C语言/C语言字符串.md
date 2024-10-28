---
{"dg-publish":true,"permalink":"//blog/log/c/c/"}
---

[[日常/BLOG/学习log/C语言/C语言\|C语言]]
在 C 语言中，字符串实际上是使用空字符 \0 结尾的一维字符数组。因此，\0 是用于标记字符串的结束。

![Pasted image 20241014111303.png](/img/user/Pasted%20image%2020241014111303.png)

关于 sizeof() 和 strlen()

-  1、**sizeof()** 计算字符串的长度，包含末尾的 **'\0'**
-  2、**strlen()** 计算字符串的长度，不包含字符串末尾的 **'\0'**

