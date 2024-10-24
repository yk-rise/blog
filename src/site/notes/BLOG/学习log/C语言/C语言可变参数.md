---
{"dg-publish":true,"permalink":"/BLOG/学习log/C语言/C语言可变参数/"}
---

[[BLOG/学习log/C语言/C语言\|C语言]]
C 语言为这种情况提供了一个解决方案，它允许您定义一个函数，能根据具体的需求接受可变数量的参数。

声明方式为：
~~~c
int func_name(int arg1, ...);
~~~
其中，省略号 ... 表示可变参数列表。

~~~c
int func(int, ... )  {
   .
   .
   .
}
 
int main() {
   func(2, 2, 3);
   func(3, 2, 3, 4);
}
~~~
