---
{"dg-publish":true,"permalink":"/BLOG/学习log/C语言/C语言指针/"}
---

[[BLOG/学习log/C语言/C语言\|C语言]]
~~~c
*p是指针变量，表示地址为p的内存变量的值
&p是表示变量p的地址
~~~

## 指针数组
指针数组是储存了一组指针，每个指针都可以指向不同的数据对象的数组。

~~~c
#include <stdio.h>
 
int main() {
    int num1 = 10, num2 = 20, num3 = 30;
    
    // 声明一个整数指针数组，包含三个指针
    int *ptrArray[3];
    
    // 将指针指向不同的整数变量
    ptrArray[0] = &num1;
    ptrArray[1] = &num2;
    ptrArray[2] = &num3;
    
    // 使用指针数组访问这些整数变量的值
    printf("Value at index 0: %d\n", *ptrArray[0]);
    printf("Value at index 1: %d\n", *ptrArray[1]);
    printf("Value at index 2: %d\n", *ptrArray[2]);
    
    return 0;
}
~~~

**指针数组和数组指针的区别**

**指针数组**
指针数组：指针数组可以说成是”指针的数组”，首先这个变量是一个数组。
其次，”指针”修饰这个数组，意思是说这个数组的所有元素都是指针类型。
在 32 位系统中，指针占四个字节。

**数组指针**
数组指针：数组指针可以说成是”数组的指针”，首先这个变量是一个指针。
其次，”数组”修饰这个指针，意思是说这个指针存放着一个数组的首地址，或者说这个指针指向一个数组的首地址。

## 函数指针
函数指针是指向函数的指针变量。通常我们说的指针变量是指向一个整型、字符型或数组等变量，而函数指针是指向函数。函数指针可以像一般函数一样，用于调用函数、传递参数。
~~~c
#include <stdio.h>
 
int max(int x, int y)
{
    return x > y ? x : y;
}
 
int main(void)
{
    /* p 是函数指针 */
    int (* p)(int, int) = & max; // &可以省略
    int a, b, c, d;
 
    printf("请输入三个数字:");
    scanf("%d %d %d", & a, & b, & c);
 
    /* 与直接调用函数等价，d = max(max(a, b), c) */
    d = p(p(a, b), c); 
 
    printf("最大的数字是: %d\n", d);
 
    return 0;
}
~~~

另外函数指针也可以作为其他函数的形式参数，

~~~c
#include <stdlib.h>  
#include <stdio.h>
 
void populate_array(int *array, size_t arraySize, int (*getNextValue)(void))
{
    for (size_t i=0; i<arraySize; i++)
        array[i] = getNextValue();
}
 
// 获取随机值
int getNextRandomValue(void)
{
    return rand();
}
 
int main(void)
{
    int myarray[10];
    /* getNextRandomValue 不能加括号，否则无法编译，因为加上括号之后相当于传入此参数时传入了 int , 而不是函数指针*/
    populate_array(myarray, 10, getNextRandomValue);
    for(int i = 0; i < 10; i++) {
        printf("%d ", myarray[i]);
    }
    printf("\n");
    return 0;
}
~~~
用函数指针来调用函数就相当于是回调函数。

## 空指针
空指针是指不指向任何对象的指针
~~~c
 int *p = NULL;
~~~
或者是
~~~c
int *p = 0;
~~~

用空指针的好处是：
- 防止指针成为野指针（野指针是指不知道指针具体指向了哪里，没有进行初始化）
- 防止数据被意外篡改
在写程序的时候对于临时的指针在不用的时候要及时设为空指针，防止误用，不然调试的时候很难确定是野指针问题。
# 回调函数
回调函数就是一个通过函数指针调用的函数。
## 为什么要使用回调函数
解耦，在回调中，主程序把回调函数像参数一样传入库函数。这样一来，只要我们改变传进库函数的参数，就可以实现不同的功能，这样有没有觉得很灵活？并且丝毫不需要修改库函数的实现，这就是解耦。
我的直观意思就是，相比于把所有处理代码写在main函数里和分比写成不同个函数，直接在main函数里调用更复杂臃肿，所有都用普通函数来写调用比写一个函数库，然后用回调函数来调用更复杂臃肿一样，用回调函数更结构清晰，便于维护。


