---
{"dg-publish":true,"permalink":"//blog/log/c/c/"}
---

[[日常/BLOG/学习log/C语言/C语言\|C语言]]
结构体是一种数据类型，可以储存不同类型的数据。

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="//blog/log/c//" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">




[[实验室/自我提升/C语言/C语言结构体\|C语言结构体]]
## 结构体作为函数参数
~~~c
#include <stdio.h>
#include <string.h>
 
struct Books
{
   char  title[50];
   char  author[50];
   char  subject[100];
   int   book_id;
};

/* 函数声明 */
void printBook( struct Books book );
int main( )
{
   struct Books Book1;        /* 声明 Book1，类型为 Books */
   struct Books Book2;        /* 声明 Book2，类型为 Books */
 
   /* Book1 详述 */
   strcpy( Book1.title, "C Programming");
   strcpy( Book1.author, "Nuha Ali"); 
   strcpy( Book1.subject, "C Programming Tutorial");
   Book1.book_id = 6495407;

   /* Book2 详述 */
   strcpy( Book2.title, "Telecom Billing");
   strcpy( Book2.author, "Zara Ali");
   strcpy( Book2.subject, "Telecom Billing Tutorial");
   Book2.book_id = 6495700;
 
   /* 输出 Book1 信息 */
   printBook( Book1 );

   /* 输出 Book2 信息 */
   printBook( Book2 );

   return 0;
}
void printBook( struct Books book )
{
   printf( "Book title : %s\n", book.title);
   printf( "Book author : %s\n", book.author);
   printf( "Book subject : %s\n", book.subject);
   printf( "Book book_id : %d\n", book.book_id);
}
~~~

</div></div>




<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="//blog/log/c//" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">




[[实验室/自我提升/C语言/C语言结构体\|C语言结构体]]
## 指向结构的指针

您可以定义指向结构的指针，方式与定义指向其他类型变量的指针相似，如下所示：
~~~c
struct Books *struct_pointer;
~~~
现在，您可以在上述定义的指针变量中存储结构变量的地址。为了查找结构变量的地址，请把 & 运算符放在结构名称的前面，如下所示：
~~~c
struct_pointer = &Book1;
~~~
为了使用指向该结构的指针访问结构的成员，您必须使用 -> 运算符，如下所示：
~~~c
struct_pointer->title;
~~~

~~~c
#include <stdio.h>
#include <string.h>
 
struct Books
{
   char  title[50];
   char  author[50];
   char  subject[100];
   int   book_id;
};

/* 函数声明 */
void printBook( struct Books *book );
int main( )
{
   struct Books Book1;        /* 声明 Book1，类型为 Books */
   struct Books Book2;        /* 声明 Book2，类型为 Books */
 
   /* Book1 详述 */
   strcpy( Book1.title, "C Programming");
   strcpy( Book1.author, "Nuha Ali"); 
   strcpy( Book1.subject, "C Programming Tutorial");
   Book1.book_id = 6495407;

   /* Book2 详述 */
   strcpy( Book2.title, "Telecom Billing");
   strcpy( Book2.author, "Zara Ali");
   strcpy( Book2.subject, "Telecom Billing Tutorial");
   Book2.book_id = 6495700;
 
   /* 通过传 Book1 的地址来输出 Book1 信息 */
   printBook( &Book1 );

   /* 通过传 Book2 的地址来输出 Book2 信息 */
   printBook( &Book2 );

   return 0;
}
void printBook( struct Books *book )
{
   printf( "Book title : %s\n", book->title);
   printf( "Book author : %s\n", book->author);
   printf( "Book subject : %s\n", book->subject);
   printf( "Book book_id : %d\n", book->book_id);
}
~~~

</div></div>




<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="//blog/log/c//" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">




[[实验室/自我提升/C语言/C语言结构体\|C语言结构体]]
## 动态可变长的结构体：
~~~c
typedef struct
{
  int id;
  char name[0];
}stu_t;

定义该结构体，只占用4字节的内存，name不占用内存。

stu_t *s = NULL;    //定义一个结构体指针
s = malloc(sizeof(*s) + 100);//sizeof(*s)获取的是4，但加上了100，4字节给id成员使用，100字节是属于name成员的
s->id = 1010;
strcpy(s->name,"hello");
~~~
注意：一个结构体中只能有一个可变长的成员，并且该成员必须是最后一个成员。

</div></div>



<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="//blog/log/c//" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">




[[实验室/自我提升/C语言/C语言结构体\|C语言结构体]]
## 包含结构体数组的结构体
例如：
![Pasted image 20241019160257.png](/img/user/Pasted%20image%2020241019160257.png)
其中cfar2d_result_t的结构体就是一个包含了cfar2d_point_t结构体数组的结构体，其中注意在cfar2d_result_t结构体定义里是定义的cfar2d_point_t结构体数组的指针，所以初始化的时候可以先定义一个结构体数组来初始化，然后把这个结构体数组的地址赋给指针。==（注意：数组的名称就是数组第一个元素的地址。再注意：数组的第一个元素按数组类型定，如果是结构体数组，那么第一个元素就是结构体）==。
![Pasted image 20241019160951.png](/img/user/Pasted%20image%2020241019160951.png)
### 给函数传递结构体（数组）
把结构体的地址传进变量的指针即可
函数定义：
![Pasted image 20241019161059.png](/img/user/Pasted%20image%2020241019161059.png)

使用函数
![Pasted image 20241019161113.png](/img/user/Pasted%20image%2020241019161113.png)
之后如果是结构体，可以用‘.’来访问结构体元素
![Pasted image 20241019161244.png](/img/user/Pasted%20image%2020241019161244.png)
如果是结构体数组，则如数组一样用‘[i]’来访问不同结构体
![Pasted image 20241019161443.png](/img/user/Pasted%20image%2020241019161443.png)
如果是结构体中的结构体数组，则用‘->’访问结构体数组
![Pasted image 20241019161338.png](/img/user/Pasted%20image%2020241019161338.png)



</div></div>
