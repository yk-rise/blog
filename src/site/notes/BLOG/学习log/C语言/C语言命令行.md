---
{"dg-publish":true,"permalink":"/BLOG/学习log/C语言/C语言命令行/"}
---

[[BLOG/学习log/C语言/C语言\|C语言]]
可以用命令行传递值给C程序，很方便的调试函数
~~~c
#include <stdio.h>

int main(int argc, char* argv[]) {
for (int i = 0; i < argc; i++) {
printf("arg %d: %s\n", i, argv[i]);
}
}
~~~
上面示例中，`main()`函数有两个参数`argc`（argument count）和`argv`（argument variable）。这两个参数的名字可以任意取，但是一般来说，约定俗成就是使用这两个词。

第一个参数`argc`是命令行参数的数量，由于程序名也被计算在内，所以严格地说`argc`是参数数量 + 1。

第二个参数`argv`是一个数组，保存了所有的命令行输入，它的每个成员是一个字符串指针。

例如使用命令
~~~c
$ ./foo hello world
~~~
`argc`是3，表示命令行输入有三个组成部分：`./foo`、`hello`、`world`。数组`argv`用来获取这些输入，`argv[0]`是程序名`./foo`，`argv[1]`是`hello`，`argv[2]`是`world`。一般来说，`argv[1]`到`argv[argc - 1]`依次是命令行的所有参数。`argv[argc]`则是一个空指针 NULL。

==就目前看来要用命令行调试的话，main函数的参数只能是两个，一个数量，一个字符串内容，反正正常写main函数里也不带什么参数，都是调用其他函数的。==

**再次强调，只能传字符串，也可以把数字翻译成对应的字符串**

## 退出状态
C 语言规定，如果`main()`函数没有`return`语句，那么结束运行的时候，默认会添加一句`return 0`，即返回整数`0`。这就是为什么`main()`语句通常约定返回一个整数值，并且返回整数`0`表示程序运行成功。如果返回非零值，就表示程序运行出了问题。

