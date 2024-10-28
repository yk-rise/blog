---
{"dg-publish":true,"permalink":"//blog/log/c/c/"}
---

[[日常/BLOG/学习log/C语言/C语言\|C语言]]
这里对正常定义和声明不做记录，记录一些在项目中经常会碰到的情况。更详细可以去看菜鸟教程。

## 传递数组给函数
### 传递一维数组
#### 更常用的方法
在函数外定义数组，然后用指针传入函数
~~~c
#include<stdio.h>
int* function(int* a){
	a[0] = 1;
	return a;
}
int main(){
	int a[10];
	int* b;
	b = function(a);
	}
~~~
#### 方式 1

形式参数是一个指针：
~~~c
void myFunction(int *param) 
{ 
. 
. 
. 
}
~~~
#### 方式 2

形式参数是一个已定义大小的数组：
~~~c
void myFunction(int param[10]) 
{ 
. 
. 
. 
}
~~~
#### 方式 3

形式参数是一个未定义大小的数组：
~~~c
void myFunction(int param[]) 
{ 
. 
. 
. 
}
~~~
形式参数又可以理解是函数的输入变量。

==PS==:数组作为形式参数的时候最好带上长度，可以用sizeof(arr)/arr(0),即用整个数组的字节长度除以一个数组元素的字节长度，可以得到这个数组有多少个元素。

### 传递二维数组

#### 方法1: 
第一维的长度可以不指定，但必须指定第二维的长度：
~~~c
void print_a(int a[][5])
~~~
#### 方法2: 
指向一个有5个元素一维数组的指针：
~~~c
void print_b(int (*a)[5])
~~~


> [!NOTE] 这里是怎么传二维数组的？（问题）
> Contents

#### 方法3: 
利用数组是顺序存储的特性,通过降维来访问原数组，==这里可以说，因为数组是顺序储存的，所以定义一个2x2的二维数组和定义一个1x4的一维数组占用的内存是一样的，
![Pasted image 20241012102934.png](/img/user/Pasted%20image%2020241012102934.png)

~~~c
void print_c(int *a)
~~~


如果知道二维数组的长度，当然选择第一或者第二种方式，但是长度不确定时，只能传入数组大小来遍历元素啦。


## 从函数返回数组
### 方式一-外部定义数组
可以通过返回数组的指针来返回得到经过函数处理后的数组。
在函数外定义数组，然后用指针传入函数
~~~c
#include<stdio.h>
int* function(int* a){
	a[0] = 1;
	return a;
}
int main(){
	int a[10];
	int* b;
	b = function(a);
}
~~~
### 方式二-动态内存分配
在函数里用动态内存分配，然后用指针指向这块内存，返回指针，然后在函数外定义一个指针保存函数返回的指针。
定义一个已经知道长度的数组的动态内存int* ans = malloc(sizeof(int) * length);这里是对int类型的数组的动态内存，所以sizeof(int) * length就是有length个int类型的内存，然后用int* ans来指向这块内存。
~~~c
int* function(int* nums){
	int* ans = (int*)malloc(sizeof(int) * length);
	return ans;
}
int main(){
    int* final = function(nums);
}

~~~
### 方式三-内部静态数组定义
在函数里创建一个静态数组，静态数组的生命周期贯穿整个程序，所以返回静态数组的地址在函数外仍然可以访问。在函数外定义一个指针，地址为函数返回值。
~~~c
int* function(){
	static int a[5];
	a[0] = 1;

	return a;
}
int main(){
	int* b;
	b = function();
} 

~~~
### 方式四-把数组包在结构体里，返回结构体
在结构体里定义数组，结构体成员用的时候深拷贝，所以能够保留数组。
#### 深拷贝与浅拷贝
浅拷贝只赋值了结构体里的值，不会复制指针所指的内存，而是指针的地址。所以如果指针所指的内存被修改了，在结构体里也会修改。
深拷贝则既复制了结构体里的值，还复制了指针所指的内存。是一个完全独立的内存。
~~~c
#include<stdio.h>
struct Witharray{
	int a[5];
};
struct Witharray function(){
	struct Witharray test1;
	test1.a[0] = 1;
	return test1;
}

int main(){
	struct Witharray test1 = function();
} 

~~~
## 指向数组的指针
~~~c
double *p;
double balance[10];

p = balance;
~~~
数组名的地址即为数组第一个元素的地址

## 静态数组与动态数组
静态数组特点：
- 内存分配：静态数组的内存通常分配在栈上，随着函数的调用和返回而自动管理。
- 大小固定：在定义时指定大小，且在程序运行过程中不能更改。
- 效率：由于在栈上分配内存，访问速度较快。
- 生命周期：静态数组的生命周期始于其定义时。如果在函数内部定义，生命周期与函数的调用相同；如果在全局范围定义，生命周期贯穿整个程序运行。

于静态数组，可以使用 sizeof 运算符来获取数组长度，例如：
~~~c
int array[] = {1, 2, 3, 4, 5};
int length = sizeof(array) / sizeof(array[0]);
~~~

动态数组特点：
- 内存分配：动态数组的内存空间在运行时通过动态内存分配函数手动分配，并存储在堆上。需要使用 `malloc`、`calloc` 等函数来申请内存，并使用 `free` 函数来释放内存。
- 大小可变：动态数组的大小在运行时可以根据需要进行调整。可以使用 `realloc` 函数来重新分配内存，并改变数组的大小。
- 生命周期：动态数组的生命周期由程序员控制。需要在使用完数组后手动释放内存，以避免内存泄漏。


~~~c
int size = 5; // 数组长度
int *array = malloc(size * sizeof(int));

// 使用数组

free(array); // 释放内存
~~~

==PS==:在不再使用该数组的时候，再释放内存。

## 结构体数组
结构体数组就是储存结构体的数组，每个数组元素都是一个结构体
### 定义方式
1. 静态定义
~~~c
typedef struct {
    int id;
    char name[50];
} Person;

Person people[5]; // 定义一个包含5个元素的结构体数组
~~~
2. 动态定义
~~~c
typedef struct {
    int id;
    char name[50];
} Person;

Person *people = malloc(5 * sizeof(Person)); // 动态分配一个包含5个元素的结构体数组
if (people == NULL) {
    // 处理内存分配失败
}

//记得不用在之后释放内存，避免内存泄漏
free(people);
~~~
### 访问方式
1. 用索引访问
~~~c

typedef struct {
    int id;
    char name[50];
} Person;

Person people[5] = {
    {1, "Alice"},
    {2, "Bob"},
    {3, "Charlie"},
    {4, "Diana"},
    {5, "Edward"}
};

// 访问第二个结构体的id成员
int id = people[1].id; // 注意数组索引从0开始，所以1代表第二个元素
~~~
2. 用指针访问
~~~c
Person *p = people;
for (int i = 0; i < sizeof(people) / sizeof(people[0]); i++) {
    printf("Person %d: ID = %d, Name = %s\n", i, p->id, p->name);
    p++; // 移动指针到下一个结构体
}
~~~

### 给函数传递结构体数组
1. 按值传递：按值传递会将整个结构体数组复制到函数中，这意味着函数中的任何修改都不会影响原始数组。这种方法适用于小型结构体或当不希望函数修改原始数据
~~~c
#include <stdio.h>

typedef struct {
    int id;
    char name[50];
} Person;

void printPeople(Person people[], int size) {
    for (int i = 0; i < size; i++) {
        printf("Person %d: ID = %d, Name = %s\n", i, people[i].id, people[i].name);
    }
}

int main() {
    Person people[] = {
        {1, "Alice"},
        {2, "Bob"},
        {3, "Charlie"}
    };
    int size = sizeof(people) / sizeof(people[0]);
    printPeople(people, size);
    return 0;
}

~~~
2. 按指针传递：按指针传递将数组的地址传递给函数，允许函数直接修改原始数组。这种方法适用于需要修改数组或数组较大的情况。
~~~c
#include <stdio.h>

typedef struct {
    int id;
    char name[50];
} Person;

void modifyPeople(Person *people, int size) {
    for (int i = 0; i < size; i++) {
        // 修改数组元素
        people[i].id += 100; // 例如，增加ID
        printf("Modified Person %d: ID = %d, Name = %s\n", i, people[i].id, people[i].name);
    }
}

int main() {
    Person people[] = {
        {1, "Alice"},
        {2, "Bob"},
        {3, "Charlie"}
    };
    int size = sizeof(people) / sizeof(people[0]);
    modifyPeople(people, size);
    return 0;
}

~~~


