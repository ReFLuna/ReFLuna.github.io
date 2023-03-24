

# 数组



array数组步骤

1. 数组定义；
   1. 类型 名字 大小

2. 数组初始化；
   1. 数组 集成初始化
   2. 数组不能对数组初始化 只能遍历

3. 数组运算；

4. 数组输出；



二维数组的遍历初始化

```c
for (i=0; i<r; i++){
    for (j=0; j<c; j++){
        a[i][j] = X;
    }
}
```

集成初始化

```c
int a[][] = {
    {...},
    {...},
    ...
}
```





# 指针（pointer）

**保存地址的变量**



```c
int* p,q;
//两个指针是等价的 不管 * 号靠近 int 还是靠近 p
// C语言中 没有 int* 这种类型
int *p,q;

* 是一个单目运算符 用来访问指针的值所表示的地址上的变量
    可以做右值 也可做左值

```

## 指针的使用

### 1 交换变量的值

- 函数需要返回多个，其值就只能通过指针返回（函数返回值 只能是一个）
  - 传入的 参数实际上是 需要保存带回的结果变量；



```c
#include <stdio.h>

// the function defintes two pointers   
void minmax(int a[], int len, int *min, int *max );		//函数需要返回多个，其值就只能通过指针返回（函数返回值 只能是一个）

int main(void)
{
	int a[] = {1,2,3,4,5,6,7,8,9,10};
	int min,max;
	minmax(a, sizeof(a)/sizeof(a[0]), &min, &max);		//取到了min max 的地址给函数
	printf("min=%d, max=%d\n", min ,max);

	return 0;
}

void minmax(int a[], int len, int *min, int *max )		//应注意 变量还是 min max 加星号 “ * ” 只是做了运算处理 
{
	int i;
	*min = *max = a[0];
	for (i=1;i<len;i++)
	{
		if(a[i] < *min)
		{
			*min = a[i];
		}
		if(a[i] > *max)
		{
			*max = a[i];
		}
	}
}
```



### 2 situation for pointer

- 函数返回运算的状态，结果通过指针返回
- 常见处理方法是让函数返回特殊的不属于有效范围内的值来表示出错：
  - -1 或0
- 但是当任何数值都可能是有效结果，就得分开返回



## the qustion of pointer for programmmer



### pointer innitialzation

- ti is must  to be done. If you are not , it will be bug.
- especially if  ` *p = 0`  void pointer  cant be assigned





### 数组变量是特殊的指针

指针也可以看成 一个单元的数组 `*p <==> a[1]`



传入函数的数组成了什么？

```c
int isPrime(int x,int knwnPrinmes[], int number0fPrimes)
```



- 函数参数表中的数组实际上是指针

  - sizeof(a) == sizeof(int*)

  - 但是可以用数组的运算符 [ ] 进行运算

    - 四种等价的

      ```c
      int sum(int *ar, int  n);
      int sum(int *, int);
      int sum(int ar[],int n);
      int sum(int [],int);
      ```

- 数组变量本身表达地址，所以

  - `int a[10]; int *p=a;` 无需用&取地址
  - 但是数组的单元表达的是变量，需要用&取地址
  - a==&a[0]
  - `int *p=&min //取地址给指针`

- [] 运算符可以对数组做，也可以对指针做；

  - p[0] <==> a[0]

- “ * ”号运算符可以对指针做，也可以对数组做：

  - *a = 25

- 数组变量是 const的指针，所以不能被赋值

  - 数组变量间也不可以相互赋值

  - 对于数组变量来说

    - ```c
      int b[] <=等价于=> int *const b;
      
      int a[] <==>int *const a=...
      ```

- 指针是 const

  - 表示 一旦得到某个变量的地址，不能再指向其他变量

    - ```c
      int *const q = &i; //q is const
      *q = 26; //Ok
      q++; // error
      ```

  - 表示不能通过这个指针区修改那个变量（并不能使得那个变量成为const）

  - ```c
    const int *p = &i;
    *p = 26; //error!(*p)是 const
    i = 26; //ok
    p = &j; //ok
    
    p和i都可以变化
        但是不可以通过p去做赋值（ *p = 26 这个做不了 只能读不能写)
    
        
    int i;
    const int* p1 = &i; (1)
    int const* p2 = &i; (2)
    int *const p3 = &i; (3)
        //只有两种意思
    指针不可以修改，或者说 通过指针不可以修改
        判断那个可以被 const 了标志是 const 在 * 的前面还是后面
        const 在 * 的前面 表示 它所指的东西不可以被修改 (1)(2) 是同一类的 即 不能通过赋值指针改变值
        const 在 * 的后面 表示 指针（地址 / p）不能被修改
        
    ```



### const 数组

const int a[] = {1,2,3,4,5,6,7};

数组变量  a 本身相当于一个 const 的指针，不能再指向别的数组了

再加 const 表明  数组的每个单元都是 const int  均不可改变

所以必须通过初始化进行赋值



### 保护数组值

- 因为把数组传入函数时传递的是地址，所以那个函数内部可以修改数组的值
- 为了保护数组不被函数破坏，可以设置参数为 const 
  - int sum (const int a[], int lenth);



## 指针运算



指针加一实际上是加的  类型的sizeof()

“ * ”是单目运算符

*(p+n)  <=等价于=> a[n] 



指针计算

- 这些算数运算可以对指针做：
  - 给指针 加 减一个整数（+，+=，- ，-=）
  - 递增递减（++/-）
  - 两指针相减（得到的是声明数据类型长度）
- *p++
  - 取出p所指的那个数据来，完事之后顺便把p移动到下一个位置去
  - “ * ”的优先级虽然够高，但是没有 ++ 高
  - 常用于数组类的连续空间操作
  - 在某些CPU上，这可以直接被翻译成一条汇编指令

- 指针比较
  - <, <=,==,>,>=,!= 都可以对指针做
  - 比较他们在内存中的地址
  - 数组中的单元的地址肯定是线性递增的



零地址 （0地址）

- 当然你的内存中有 0 地址，但是 0 地址通常是个不能随便碰的地址

  - 要求很高 都或者写 都可能不被允许

- 所以你的指针不应该具有 0 值

- 因此可以用 0 地址来表示特殊的事情：

  - 返回的指针是无效的
  - 指针没有被真正地初始化（先初始化为 0）

- NULL是一个预定定义的符号，表示 0 地址

  - 有的编译器不愿意你用 0 来表示 0 地址，可以用 NULL

- 任何程序都有零地址

  - 运行一个程序 计算机会分配一个虚拟内存 内存中有零0地址

  

指针的类型

- 无论指向什么类型，所有的指针的大小都是一样的，因为都是地址
- 但是指向不同类型的指针是不能相互赋值的
  - 主要是为了避免用错指针



指针的类型转换



输入数据

- 如果输入数据时 定义

- ```c
  #include <stdlib.h>
  int *a = (int*)malloc(n*sizeof(int));
  //molloc 函数 给个参数 说 需要多少内存
  n*sizeof(int) 所需要的空间
  （int*） 数据类型
  ...
  free(a) //空间释放
      
  //----------------------//
  使用
  #include <stdlib.h>
  void* malloc(sieze_t size);
  
      
  ```

- malloc 使用

  - 要包含库函数
  - 向 malloc 申请的空间的大小是以字节为单位的
  - 返回的结果是 void* ,需要类型转换换为自己所需要的类型
  - `（ int* ）malloc(n*sizeof(int))`

- 没空间了？

  - 如果申请失败则返回0，或者叫做NULL
  - 你的系统能给你多大空间？

  ```c
  #include <stdio.h>
  #include <stdlib>
  
  int main(void)
  {
      void *p;
      int cnt = 0;
      while( (p = malloc(100*1024*1024)))
      {
          cnt++;
      }
      printf("分配了%d00MMB的空\n",cnt);
      
      return 0;
  }
  
  ```

  

- free() 空间释放函数

  - 将申请的空间还给系统
  - 申请过的空间，最终都应该要还
  - 还的都应该是申请的地址
  - 只能还申请来的空间的首地址 #important

- 常见问题

  - 申请了没有free 长时间运行内存逐渐下降
  - free 过了再free
  - 地址变过了，直接去free 会失败
  - 解决方法 
    - 1 对于每个 molloc 要有一个free 与之对应 
    - 2 程序 架构 要有足够的清晰的认识
    - 3 经验



# 10 字符串

## 字符数组

例如

char word [] = {'H', 'e', 'i', 'i', 'o', '!', '\0'};

一共7位

因为 最后有个零所以可以



## 字符串

- 以0（整数 0 ）结尾的一串字符
  - 0或 '\0' 时一样的，但是和 '0' 不同
  - 整数 0 表达的是 int 类型 4个字节
  - '\0'是一个字节的东西 这个是字符串里面更想要的
  - '0' 是一个字符 是 ascll 的 0  是可以读到得零
- 0 标识字符串的结束，但它不是字符串的一部分
  - 计算字符串长度的时候不包含这个 0
- 字符串以数组的形式存在，可以数组或指针的形式访问
  - 更多的是以指针形形式
- <string.h> 字符串 标准库 里面有很多处理字符串的函数
- C语言的字符串是以字符数组的形态存在
  - 不能用运算符对字符串做运算
  - 通过 数组的方式可以遍历字符串
- 唯一特殊的是 字符串的字面量（"字符串"）可以用来初始化字符数组

## 字符串变量

- char *str = "Hello";
  - char 类型的指针 str 它指向 一个字符数组 它里面放的内容是 Hello
  - str是指针,初始化为指向一个字符串常量
    - 由于这个常量所在的地方，所以实际上 s 是 const char* s ,但是历史原因，编译器接受不带 const 的写法
    - 所以试图对 str  所指的字符串做写入会导致严重的后果（程序崩溃掉）
    - "Hello" 存放位置 是代码段
- char word[] = "Hello";
  - 定义 char类型的空数组  初始化为字符数组
- char line[10] = "Hello";
  - 10个字节的数组 line 
- "hello" 会被编译器变成一个字符数组放在某处，这个数组长度 会加1

## 字符串常量



### 指针还是数组

是定义为指针类型，还是数组模式

char *str = "Hello";

- char word[] = "Hello";
  - 数组： 这个字符串在 本地变量这里
    - 作为本地变量空间自动化收回
  - 指针：这个字符串不知道具体在哪里
    - 处理参数
    - 动态分配空间
    - 存放于代码段
    - 表达字符串，只读不写



#### char* 是字符串？

- 字符串可以表达char* 的形式
- char* 不一定是字符串
  - 本意是指向字符的指针，可能指向的是字符的数组（就像int*一样）
  - 只有当它所指的字符串数组有**结尾的 0**，才能说它所指的是字符串



## 字符串的输入输出

### 字符串赋值

- char *t = "title";
- char *s;
- s = t;
- 并没有产生新的字符串，只是让指针s指向了t所指的字符串，对s的任何操作就是对t做的

#### 字符串的输入输出

- char string[8];
- %s 读的是一个单词
- scanf("%s",string );
- printf("%s",string);



- scanf读入一个单词（到 空格、tab 或者 enter 为止）
- sacnf是不安全的，因为不知道要读入的内容长度
- scanf("%7s",string);
- 在%和s之间的数字表示最多允许读入的字符的数量，这个数字应该比数组的大小小1，( 有一个  '\0' )
  - 下一次scanf 从哪里开始

#### 常见错误

- 以为char* 是字符串类型，定有了一个字符串类型的变量string 就可以使用了
  - 由于没有对string初始化为 0 ，所以不一定1每次运行都出错
- char buffer[] = "";
  - 这个数组的长度只有1；



#### 字符串数组

用一个数组表达多个字符串

- char **a
  - a是一个指针，指向另一个指针，那个指针指向另一个字符（串）
- `char a[][n]` n 必须要有
  - 第二位应当 有大小 具体

```c
#include <stdio.h>

int main(void)
{
    char *a[]={
        "Hello",
        "World",
        "fesafsafaffef",
    }; 
    printf("%s",a[2]);
    return 0;
}
// a[0] --> char*
```



#### 二维数组 与 指针数组

![数组指针与二维数组](attac\数组指针与二维数组.png)



#### 数组指针与指针数组

 [(1条消息) 指针数组和数组指针（非常易懂）_蔡泽基✔℡的博客-CSDN博客](https://blog.csdn.net/itszok/article/details/121198169) 





### 字符串数组应用

应用于 main函数的参数

- int main(int argc, char const *argv[])
- argv[0] 是命令本身
  - 当使用 Unix 的符号链接时，反映 符号链接的名字



#### putchar

- int putchar(int c);
  - 虽然 是 int 但 一次只能接受一个字符
  - 返回也是int 表示这次写入几个字符
- 向标准输出写一个字符
- 返回写了几个字符，EOF（-1）表示写失败



#### getchar

- int getchar(void);
- 从标准输入读入一个字符
- 返回类型是 int 是为了返回EOF（-1）
  - windows -->Ctrl+Z
  - Unix --> Ctrl+D

```c
#include <stdio.h>

int main(int argc, char const *agrv[] )
{
	int ch;
	while ( (ch = getchar() ) != EOF)
	{
		putchar(ch);
	}

	printf("EOF\n");
	return 0;
}
```





## string.h 标准库

- strlen
- strcmp
- strcpy
- strcat
- strchr
- strstr

### strlen

```c
#include <stdio.h>
#include <string.h>

int main(int argc, char const *argv[]) //const * 在星前 不能弄通过指针修改值
{
    char line[] = "Hello";
    prinf("strlen=%lu\n", strlen(line));
    printf("sizeof=%lu\n",sizeof(line));
    
    return 0;
}
```

```c
#include <stdio.h>
#include <string.h>

int mylen(const char* s)    //定义了 字符指针 const 在前 不能通过赋值指针改变值 
{
    int index   = 0;
    while ( s[index] != '\0') {
        index++ ;
    }
    return index;
}


int main(int argc, char const *argv[])
{
    char line[] = "Hello";
    printf("strlen=%lu\n", mylen(line));
    printf("sizeof=%lu\n",sizeof(line));
    
    return 0;
}
```

写函数 mylen



**知道 有多大 就用 for  不知道就用 while**



### strcmp

- int strcmp(const char *s1,const char *s2);
- 比较两个字符串，返回：
  - 0: s1 == s2
  - 1: s1 > s2
  - -1: s1 < s2
  - 返回差值(ascll  差值)

### strcpy

- char *strcpy(char *restrict dst, const char *restrict src);
- 把src 的字符串拷贝到dst
  - restrict 表明src 和dst 不重叠
- 返回 dst
  - 为了能链起代码来

#### 复制一个字符串

char *dst = (char*)malloc(strlen(src)+1);

// + 1 保证了空间足够 加上'\0' 的 余量

strcpy(dst,src);



### 字符串搜索函数

#### 字符串中找字符

- char *strchr(const char *s, int c);
  - 字符串中找字符
  - 在 *s 指向的字符串 中寻找 字符串中 c
  - 从左边往右数
  - 返回的是指针
- char *strrchr(const char *s, int c);
  - 表示从右边找
- 返回NUll 没有找到



- 如何寻找第二个？





#### 字符串中找字符串

- char *strstr(const char *s1, const char *s2)
- char *strcasestr(const char *s1, const char *s2);





# 11

## 枚举

### 符号常量化

用符号而不是具体的数字来表示程序中的数字

```c
const int red = 0;
const int yellow = 1;
const int green = 2;
```



用枚举而不是定义独立的const int 变量



```c
enum COLOR {RED, YELLOW, GREEN}
```



枚举是用户定义的数据类型，它用关键字 enum 以如下语法来声明：

```c
enum 枚举类型名字 {名字0, ..., 名字n};

枚举类型 名字 通常并不真的使用，常用的是在打括号里面的名字，
    他们就是常量符号，他们的数据类型是 int, 依次从 0 到 n 如 ：
    enum colors{red, yellow, green};

**创建了 三个常量 red 值是0，
** yellow 是1，
    而green 是2，
    
```

- 当 需要一些可以排列起来的常量值时，定义枚举的意义就是给这些常量值名字。



## 结构体

- 一种复合的数据类型
- 各种类型的成员 来表达



```c
#include <stdio.h>

int main(int argc, char const *agrv[] )
{
	struct date
	{
		int month;
		int day;
		int year;
	};  **分号最容易漏掉


	struct date today;

	today.month = 07;
	today.day = 31;
	today.year = 2019;


	printf("Today's date is %i-%i-%i.\n", today.year, today.month, today.day);

	return 0;

}
```



#### 容易出错的地方

- 漏掉结构体的分号

- 和本地变量一样，在函数内部声明的结构体类型只能在函数内部使用
- 所以通常在函数声明结构体类型，这样就可以被多个函数所使用了


```c
#include <stdio.h>
struct date
	{
		int month;
		int day;
		int year;
	};
	
int main(int argc, char const *agrv[] )
{
	struct date today;
	today.month = 07;
	today.day = 31;
	today.year = 2019;

	printf("Today's date is %i-%i-%i.\n", today.year, today.month, today.day);

	return 0;

}
```



#### struct 声明形式

```c
声明结构类型
struct point{
    int x;
    int y;
}
声明结构变量
struct point p1, p2;
	p1 和 p2 都是point里面有x和y的值
结构类型 结构变量不是一个东西
    
    ---
  
struct {
    int x;
    int y;
} p1, p2;
p1 和 p2 都是一种无名结构，里面有x和y
    
    
    ---
    
struct point{
    int x;
    int y;
} p1, p2;
p1 和 p2 都是point，里面有x和y的值t
    
    一 三 种都声明了结构point。
    第二种没有声明 point,只定义了两个变量

```



放在函数内部的变量叫本地变量 本地变量没有默认的初始值

给结构变量赋值初始值



#### 结构成员

- 结构和数组有点像
- 数组用 [] 运算符和下标访问其成员
  - a[0] = 10;
- 结构用 . 运算符和名字访问其成员
  - today.day
  - pl.x





#### 结构运算

要访问整个结构，直接用结构变量的名字

- 对于整个结构，可以做赋值、取地址，也可以传递给函数参数
- p1 = (struct point){5,10};//相当于p1.x = 5；p1.y = 10; 前面的“（）”相当于强制类型转换
- p1 = p2; //相当p1.x = p2.x; p1.y = p2.y; 数组无法做这两种运算
- 



#### 结构指针

- 和数组不同，结构变量的名字并不是结构变量的地址，必须使用&运算符
- struct date *pDate = &today;
- 

#### 输入结构

- 没有直接的方式可以一次scanf 一个结构
- 写一个函数来读入结构
  - ->
- 读入的结构如何送出来





- 记住C在函数调用时是传值的
  - 所以函数中的p与main 中的y是不同的
  - 在函数读入了p的数值之后，还没有任何东西

```c
#include <stdio.h>

struct point {
	int x;
	int y;
};


void getStruct(struct point);
void output(struct point);

void main(){
	struct point y = {0,0};
	getStruct(y);
	output(y);
}


void getStruct(struct point p){
	scanf("%d",&p.x);
	scanf("%d",&p.y);
	printf("%d,%d",p.x, p.y);
}


void output(struct point p){
	printf("%d,%d",p.x, p.y);
}
```



#### 解决方案

- 之前是把结构体传入了函数，然后在函数中操作，但是没有返回回去
  - 问题在于传入函数的是外面那个结构的克隆体，而不是指针
    - 传入结构和传入数组是不同的
- 在这个输入函数中，完全可以创建一个临时的结构变量，然后把这个结构返回给调用者

### 结构指针作为参数

### 指向结构的指针

```c
struct date{
    int month;
    int day;
    int year;
} myday;

struct date *p = &myday; //p 是date 结构指针
(*p).month = 12;
p->month = 12;
```

- 用-> （指向于）表示指针所指的结构变量中的成员

### 结构指针参数

```c
void main(){
    struct point y = {0,0};
    inputPoint(&y);
    output(y);
}

struct point* inputPoint(struct point *p)
{
    scanf("%d",&(p->x));
    scanf("%d".&(p->y));
    return p;
}
```




```c
#include <stdio.h>

struct point {
	int x;
	int y;
};

struct point* getStruct(struct point *);
void output(struct point );
void print(const struct point *p);


int  main(int argc, char const *agrv[])
{
	struct point y = {0,0};
	getStruct(&y);							//传入的是 y结构体 的
	output(*getStruct(&y));
	print(getStruct(&y));
}


struct point* getStruct(struct point *p)56
{
	scanf("%d",&p->x);			// -> 优先级高于 &
	scanf("%d",&p->y);
	printf("%2d,%2d",p->x, p->y);
	return p;
}


void output(struct point p){
	printf("%d,%d",p.x, p.y);
}

void print(const struct point *p){
	printf("%d,%d",p->x, p->y);
}
```



#### 结构数组



struct date dates[100]; //由100个 date 组成的数组 每一个date 是一个结构变量

struct date dates[] = {{4,5,2005}, {2,4,2005}} //最外边的大括号说明是在初始化一个数组，里面每一层大括号是一个数组元素



```c
#include <stdio.h>

struct time {
	int hour;
	int minutes;
	int seconds;
};

struct time timeUpdate(struct time new);

int  main(void)
{
	struct time testTimes[5] = {
		{11,59,59},{12,0,0},{1,29,59},{23,59,59},{19,12,27}
	};

	int i;

	for ( i=0; i<5; ++i){
		printf("Time is %.2i:%.2i:%.2i\n",testTimes[i].hour, testTimes[i].minutes, testTimes[i].seconds);
	
		testTimes[i] = timeUpdate(testTimes[i]);
		printf("...one second later it's %.2i:%.2i:%.2i\n",testTimes[i].hour, testTimes[i].minutes, testTimes[i].seconds);
	};
	return 0;
}



// 结构体函数
//传的参数：为结构体类型
struct time timeUpdate(struct time now)
{
	// printf("Time is %.2i:%.2i:%.2i\n",now.hour, now.minutes, now.seconds);
	++now.seconds;
	// printf("Time is %.2i:%.2i:%.2i\n",now.hour, now.minutes, now.seconds);
	if(now.seconds == 60){
		now.seconds = 0;
		++now.minutes; 
		// printf("Time is %.2i:%.2i:%.2i\n",now.hour, now.minutes, now.seconds);
		if(now.minutes == 60){
			now.minutes = 0;
			++now.hour;
			// printf("Time is %.2i:%.2i:%.2i\n",now.hour, now.minutes, now.seconds);
			if(now.hour == 24){
				now.hour = 0;
				// printf("Time is %.2i:%.2i:%.2i\n",now.hour, now.minutes, now.seconds);
			}
		}
	}
}
```



#### 结构中的结构

结构的嵌套



```c
struct point{
    int x;
    int y;
};

struct rectangle{
    struct point pt1;
    struct point pt2;
};
如果有变量
struct rectangle r;
就可以有；
    r.pt1.x, r.pt1.y;
	r.pt2.x, r.pt2.y;
	

如果有变量定义：
    struct rectangle r, *rp;
	rp = &r;
那么下面的四种形式是等价的：
    r.pt1.x
    rp->pt1.x
    (r.pt1).x
    (rp->pt1).x
但是没有rp->ptl->x (因为 pt1 不是指针)

    
    
    
    
  
    
    
    
    
    
    
```









## 定义数据类型

### typedef

声明新的类型名字

新的名字是某种类型的名字

- 新的名字 是某种类型的别名 更清晰具有可以移植的特性
- 改善了程序的可读性
- 载入已有的类型的名字
- 简化复杂的名字
- 





```c
#include < >

typedef int Length;	//length 就等价于 int  类型


typedef long int64_t;	
typedef struct ADate{	//第一个是原来的类型
    int month;
    int day;
    int year;
} Date;					// 第二个是定义的 简化复杂的名字 这个才是关键性的名字

int64_t i= 10000000000;
Date d = {9,1,2005};
```



![](attac\typedef.png)





## 联合

info 它与struct 很相似



联合 大家联合起来使用同一个空间



```c
union AnElt{
    int i;
    char;
} elt1,elt2;


```



### 联合的特点

- 存储
  - 所有的成员共享一个空间
  - 同一时间只有一个成员是有效的
  - union 的大小是其最大的成员
- 初始化
  - 对第一成员做初始化



*小端的计算机 低位在前 x86类型的计算机*

```c
#include <stdio.h>

typedef union{
	int i;
	char ch[sizeof(int)];
} CHI;


int main (int argc, char const *argv[])
{
	CHI chi;
	int i;
	chi.i = 1234;
	for (i = 0; i < sizeof(int); i++)
	{
		printf("%02hhX",chi.ch[i]);
	}
	printf("\n");

	return 0;
}
```

# 12



## 全局变量



- 本地变量：函数内部定义的变量
  - 不做初始化 初值为不确定值

- 全局变量：定义在函数外面的变量是全局变量
- 全局变量具有全局的生存期和作用域
  - 他们与任何函数都无关
  - 在任何函数内部都可以使用他们,改变他们

### 全局变量初始化

- 没有做初始化的全局变量会得到默认的 0 值
  - 指针会得到NULL值
- 他们的初始化发生在main函数之前

```c
#include <stdio.h>

int f(void);
int gAll ;//全局变量不做初始化 默认为 0 

int main(int argc, char const *argv[])
{

	printf("in %s gAll = %d\n", __func__ , gAll);
	f();
	printf("agn in %s gAll = %d\n", __func__ , gAll);
	return 0;
}

int f(void)
{
	printf("in %s gAll = %d\n", __func__ , gAll);
	gAll += 2;
	printf("agn in %s gAll = %d\n", __func__, gAll);
	return gAll;
}

//-------------
in main gAll = 0 //初值默认为零
in f gAll = 0
agn in f gAll = 2
agn in main gAll = 2
请按任意键继续. . .
```

- 只能用编译时刻已知的值来初始化全局变量（不能使用函数，函数返回值，变量）
  - 避免全局变量间相互初始化(虽然下面程序可以运行 但不推荐)



```c
#include <stdio.h>

int f(void);
const int gAll = 12;
int g2 = gAll ;

int main(int argc, char const *argv[])
{

	printf("in %s gAll = %d\n", __func__ , gAll);
	f();
	printf("agn in %s gAll = %d\n", __func__ , gAll);
	return 0;
}

int f(void)
{
	printf("in %s gAll = %d\n", __func__ , gAll);
	printf("agn in %s gAll = %d\n", __func__, gAll);
	return gAll;
}
```



- 被隐藏的全局变量
  - 如果函数内部存在与全局变量同名的变量，则全局变量被隐藏

```c
#include <stdio.h>

int f(void);
int gAll = 12;


int main(int argc, char const *argv[])
{

	printf("in %s gAll = %d\n", __func__ , gAll);
	f();
	printf("agn in %s gAll = %d\n", __func__ , gAll);
	return 0;
}

int f(void)
{
	int gAll = 1;
	printf("in %s gAll = %d\n", __func__ , gAll);
	gAll += 2;
	printf("agn in %s gAll = %d\n", __func__, gAll);
	return gAll;
}

//----------------
//
in main gAll = 12
in f gAll = 1
agn in f gAll = 3
agn in main gAll = 12
请按任意键继续. . .
```



### 全局变量注意的问题汇总

- 避免全局变量间相互初始化

- 被隐藏的全局变量
  - 如果函数内部存在与全局变量同名的变量，则全局变量被隐藏



```c
#include <stdio.h>

int f(void);
int gAll = 12;

int main(int argc, char const *argv[])
{

	printf("in %s gAll = %d\n", __func__ , gAll);//in main gAll = 12
	f();
	printf("agn in %s gAll = %d\n", __func__ , gAll);//agn in main gAll = 14
	return 0;
}

int f(void)
{
	printf("in %s gAll = %d\n", __func__ , gAll);//in f gAll = 12
	gAll += 2;
	printf("agn in %s gAll = %d\n", __func__, gAll);//agn in f gAll = 14
	return gAll;
}

// ----------------
//result
in main gAll = 12
in f gAll = 12
agn in f gAll = 14
agn in main gAll = 14
请按任意键继续. . .




```





```markdown

code: __func__
means: __func__是预定义的宏，用于获取当前函数的名称。 此宏已添加到C99中

info: https://blog.csdn.net/cumt30111/article/details/107801977

```













## 静态本地变量

- 本地变量定义时加上 static修饰符就成为静态本地变量
- 当函数离开的时候，静态本地变量会继续存在并保持其值
- 静态本地变量的初始化只会在第一次进入这个函数时做，以后进入函数时会保持上次离开时的值。他的初始化只做一次，第二次就再执行语句了。





```c
#include <stdio.h>

int f(void);
int gAll = 12;

int main(int argc, char const *argv[])
{
	f();
	f();
	f();
	return 0;
}

int f(void)
{
	static int all = 1; //初始化只做一次
	printf("in %s all = %d\n", __func__, all);
	all += 2;
	printf("agn in %s all = %d\n", __func__,all);
	return all;
}
//------
	//f()
in f all = 1   // 只做一次初始化
agn in f all = 3
    //f()
in f all = 3
agn in f all = 5
    //f()
in f all = 5
agn in f all = 7
请按任意键继续. . .

```



- 静态本地变量实际上是特殊的全局变量（他们放在一起的）
  - 他们位于相同的内存区域
- 静态本地变量具有全局的生存期，，他被放在函数中，函数内部的局部作用域
- static 在这里的意思是局部作用域(本地可访问)



```c
#include <stdio.h>

int f(void);
int gAll = 12;

int main(int argc, char const *argv[])
{
	f();
	return 0;
}

int f(void)
{
	int k = 0;							//普通本地变量
	static int all = 1;					//静态本地变量
	printf("&gAll = %p\n", &gAll);
	printf("&all = %p\n",&all);
	printf("&k = %p\n", &k);
	printf("in %s all = %d\n", __func__, all);
	all += 2;
	printf("agn in %s all = %d\n", __func__,all);
	return all;
}

//----
&gAll = 0000000000403010
&all  = 0000000000403014
    //两者相差 4 一个 int 字节长度 
    
&k = 000000000061FDEC
    //本地变量
in f all = 1
agn in f all = 3
请按任意键继续. . .


```



## 全局变量的注意事项

### *返回指针的函数

- 返回本地变量的地址是危险的
  - 因为一旦离开函数本地变量就不存在了（不能确定了）
- 返回全局变量或静态本地变量的地址是安全的
- 返回在函数内malloc的内存是安全的，但是容易造成问题
- 最好的做法是传入的指针



```c
#include <stdio.h>

int * f(void);
void g(void);

int main(int argc, char const *argv[])
{
	int *p = f();
	printf("*p = %d\n",*p);
	g();
	printf("*p = %d\n",*p);

	return 0;
}


int* f(void)
{
	int i = 12;
    printf("%p\n",&i);
	return &i;
}

void g(void)
{
	int k = 24;
	printf("k = %d\n", k);
}


//------x64
*p = 12
k = 24
*p = 24

F:\Design\C\12.1.3\x64\Debug\12.1.3.exe (进程 13172)已退出，代码为 0。
要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口. . .
```



- 本地变量i k 保存在同一个地址上
- 本地变量在函数中结束后 地址又会被重新分配给其他变量



```c
#include <stdio.h>

int* f(void);
void g(void);

int main(int argc, char const* argv[])
{
	int* p = f();
	printf("*p = %d\n", *p);
	g();
	printf("*p = %d\n", *p);

	return 0;
}


int* f(void)
{
	int i = 12;
	printf("%p\n", &i);
	return &i;
}

void g(void)
{
	int k = 24;
	printf("%p\n", &k);
	printf("k = %d\n", k);
}
//---------------------
0000002CA82FF514	//i 所在的地址
*p = 12
0000002CA82FF514	//k 所在的地址
k = 24
*p = 24

F:\Design\C\12.1.3\x64\Debug\12.1.3.exe (进程 5780)已退出，代码为 0。
按任意键关闭此窗口. . .

```



### 关于全局变量的贴士 tips

- 不要使用全局变量来在函数间传递参数和结果
- 尽量避免使用全局变量
  - 丰田汽车案子
    - 大量使用全局变量 有问题
- *使用全局变量和静态本地变量的函数线程不安全

- 本地变量离开函数后就不受控制了







## 12.2 宏定义



编译预处理指令

- #开头的是编译预处理指令
- 他们不是C语言的成分，但是C语言离不开他们
- #define 用来定义一个宏

```c
#include <stdio.h>

//const double PI = 3.14159;

#define PI 3.14159

int main (int argc, char const * argv[])
{
    printf("%f\n", 2*PI*3.0);
    //printf("%f\n",2.3.14159*3.0);
    return 0;
}

```



#### 编译与链接

```bash
$ ls -l
x.c

$ gcc x.c --save-temps

$ ls -l

x.c -(1)-> x.i -(2)-> x.s -(3)-> x.o -(4)-> a.out

$ tail x.i

```

- (1) 执行：编译预处理
  - 把编译预处理指令都执行完 （还是C语言文件）
  - 做的是简单的工作--替换 ，替换掉文件中该替换的
    - 比如 #define 的宏
    - 比如 全局变量
    - 编译器不会管 字符串

- (2) 执行：真正由C编译器进行编译形成汇编代码文件
- (3)执行：汇编代码文件汇编形成目标代码文件
- (4)执行：汇编代码文件经过链接（链接其他文件）



#### define

- #define <名字> <值>
- 注意没有结尾的分号，因为不是 C 的语句
- 名字必须是一个单词，值可以是任何东西
- 在C语言的编译器开始编译前，编译预处理程序(cpp)会把程序中的名字换成值
  - 完全的文本替换
- gcc --save-temps





### 宏

- 如果一个宏的值中有其他的宏的名字，也是会被替换的

- 如果一个宏的值超过一行，最后一行之前的行末需要加 \

- 宏的值后面出现的注释不会被当作宏的值的一部分

- ```c
  #include <stdio.h>
  
  //const double PI = 3.14159;
  
  #define PI 3.14159
  #define FORMAT "%f\n"
  #define PI2 2*PI //pi * 2
  #define PRT printf("%f\n",PI); \ //反斜杠表示连接 连接下一部分也是宏定义的一部分 
              printf("%f\n",PI2);
  
  
  int main (int argc, char const * argv[])
  {
      // printf(FORMAT, 2*PI*3.0);
      //printf("%f\n",2.3.14159*3.0);
      PRT;
      return 0;
  }
  
  ```



#### 没有值的宏

- #define _DEBUG

- 这类宏是用于条件编译的，后面有其他的编译预处理指令来检擦这个宏是否已经被定义过了



#### 予定义的宏

```c
__LINE__
__FILE__
__DATE__
__TIME__
__STDC__
```



程序: 12.2.1.3.c





### 12.2.2 带参数的宏

#### 像函数的宏

- `#define cube(x) ((x)*(x)*(x))`
- 宏可以带参数
  - cube(立方)
  - cube 名字 变量 x
  - 最后面 式子

#### 错误定义的宏

- `#defineRADTODEG(x)(x*57.29578)`

- `#define RADTODEG(x)(x)*57.29578`

- ```c
  #include <stdio.h>
  
  #define RADTODEG1(x)(x*57.29578)
  #define RADTODEG2(x)(x)*57.29578
  
  int main(int argc, char const *argv[])
  {
  	// int i;
  	// scanf("%d",&i);
  	// printf("%d\n",cube(i));
  	printf("%f\n", RADTODEG1(5+2));
  	printf("%f\n", 180/RADTODEG2(1));
  	return 0;
  }
  //---------------
  119.591560
  10313.240400
  请按任意键继续. . .
  ```



- 查看过程

- ```bash
  cyan@orange MINGW64 /f/FPGA2/jisuan/c/12.2.2.2
  $ gcc 12.2.2.2.c --save-temps
  
  cyan@orange MINGW64 /f/FPGA2/jisuan/c/12.2.2.2
  $ ls -l
  total 177
  -rwxr-xr-x 1 cyan 197121 54024 Mar 21 14:07 12.2.2.2*
  -rw-r--r-- 1 cyan 197121   282 Mar 21 14:10 12.2.2.2.c
  -rw-r--r-- 1 cyan 197121 53913 Mar 21 14:11 12.2.2.2.i
  -rw-r--r-- 1 cyan 197121  1002 Mar 21 14:11 12.2.2.2.o
  -rw-r--r-- 1 cyan 197121   940 Mar 21 14:11 12.2.2.2.s
  -rwxr-xr-x 1 cyan 197121 54024 Mar 21 14:11 a.exe*
  
  cyan@orange MINGW64 /f/FPGA2/jisuan/c/12.2.2.2
  $ tail 12.2.2.2.c
  
  int main(int argc, char const *argv[])
  {
          // int i;
          // scanf("%d",&i);
          // printf("%d\n",cube(i));
          printf("%f\n", RADTODEG1(5+2));
          printf("%f\n", 180/RADTODEG2(1));
          return 0;
  }
  cyan@orange MINGW64 /f/FPGA2/jisuan/c/12.2.2.2
  $ tail 12.2.2.2.i
  # 7 "12.2.2.2.c"
  int main(int argc, char const *argv[])
  {
  
  
  
   printf("%f\n", (5+2*57.29578));
   printf("%f\n", 180/(1)*57.29578);
   return 0;
  }
  
  ```



#### 带参数的宏的原则

- 一切都要括号
  - 整个值要括号
  - 参数出现的每个地方都要括号
- #define RADTOEDG (x) ((x)*57.29578)



#### 带参数的宏

- 可以多带个参数
  - `#define MIN(a,b) ((a)>(b)?(b)>(a))`
- 也可以组合（嵌套）使用其他宏



#### 分号？

- 在定义宏语句时不要在结尾使用分号

- 原因
  - 他不是编译预处理指令，不是严格意义上的C指令



#### 带参数的宏 的使用意义

- 在大型代码中使用非常普遍
- 可以非常复杂，比如“产生”函数（他会比调用函数效率高，但是代码展开使得代码变长）
  - 在#和## 这两个运算符的帮助下可以，定义产生函数
- 存在中西方文化差异
- 宏在使用时有个很大的缺陷，它是没有任何类型检查的（定义时 也不具有类型）
  - 部分宏（带参数的宏）会被inline函数替代，用于做参数类型的检查



#### 其他编译预处理指令

- 条件编译
- error
- 。。。



## 12.3 大程序



#### 多个.c文件

- main()里的代码太长了适合分成几个函数
- 一个源代码文件太长了适合分成几个文件
- 两个独立的源代码文件不能编译形成可执行文件

```c
#include <stdio.h>
#include "max.c"

int max(int a,int b); //原型声明

int main(void)
{
	int a = 5;
	int b = 6;
	printf("%d\n",max(a,b));
}
//--------------
int max(int a, int b)
{
	return a>b?a:b;
}
```









### 项目

C语言文件处理

大致分为

- 对单个源代码文件编译 形成  “ .o” 文件
- 对整个项目做链接 形成  “ a.exe”  可执行文件



#### 编译单元

- 一个 .c 文件是一个编译单元
- 编译器每次编译只处理一个编译单元







## 头文件

- 把函数原型放在一个文件（以 .h 结尾）中，在需要调用这个函数的源代码文件（ .c 文件） 中 #include 这个头文件， 就能让编译器在编译的时候知道函数的原型
  - 函数的原型？？？？？？
- xxx.h 的头文件 形成了一个桥梁 或者说是一份合同
  - xxx.h 里面的 原型声明 是同函数一致的的
    - 它承诺说我的函数输入输出长这个样子
    - 假如你使用我的函数 就要包含我(xxx.h)

 #### # include

- 编译预处理指令，和宏一样，在编译之前就处理了
- 他把那个文件的全部内容文本原封不动的插入到它所在的地方
  - 所以也不是一定要在  .c 文件的最前面



```bash
$ gcc --save-temps main.c -c  //表示只做编译，不做链接

cyan@orange MINGW64 /f/FPGA2/jisuan/c/12.3
$ ls -l
total 123
-rwxr-xr-x 1 cyan 197121 54312 Mar 21 16:26 a.exe*
-rw-r--r-- 1 cyan 197121   142 Mar 21 16:26 main.c
-rw-r--r-- 1 cyan 197121 53897 Mar 21 16:30 main.i
-rw-r--r-- 1 cyan 197121   974 Mar 21 16:30 main.o
-rw-r--r-- 1 cyan 197121   778 Mar 21 16:30 main.s
-rw-r--r-- 1 cyan 197121    74 Mar 21 16:24 max.c
-rw-r--r-- 1 cyan 197121    30 Mar 21 16:24 max.h

cyan@orange MINGW64 /f/FPGA2/jisuan/c/12.3
$ tail -n 20 main.i
  __attribute__ ((__dllimport__)) size_t __attribute__((__cdecl__)) _fread_nolock_s(void *_DstBuf,size_t _DstSize,size_t _ElementSize,size_t _Count,FILE *_File);
# 1398 "C:/CandCpp/mingw64/x86_64-w64-mingw32/include/stdio.h" 2 3

# 1 "C:/CandCpp/mingw64/x86_64-w64-mingw32/include/_mingw_print_pop.h" 1 3
# 1400 "C:/CandCpp/mingw64/x86_64-w64-mingw32/include/stdio.h" 2 3
# 2 "main.c" 2
# 1 "max.h" 1

# 1 "max.h"
double max(double a,double b);
# 3 "main.c" 2



int main(void)
{
 int a = 5;
 int b = 6;
 printf("%f\n", max(a,b));
}



```



####  `include " "`还是 `< >`

- include 有两种形式插入文件
  - “ ” 要求编译器首先在当前目录（ .c 文件所在的目录）寻找这个文件，如果没有，到编译器指定位置的目录去寻找
  - < > 让编译器自己知道自己的标准库的头文件在哪里
  - 环境变量和编译器命令行参数也可以指定寻找头文件目录



#### #include的误区

- #include 不是用来引入库的
  - #include <> 只是把 后面的文件原封不动的插入这一行中 就做着一件事情
  - stdio.h 里只有printf的原型，printf的代码在另外的地方，某个 .lib(windows)或 .a(Unix)中
  - 现在的C语言默认会引入所有的标准库
  - #include <stdio.h> 只是为了让编译器知道printf函数的原型，保证你调用是给出的参数值是正确的类型



#### 头文件

- 在使用和定义这个函数的地方都应该 #include 这个头文件
  - 使用的地方 编译器能帮忙检查对函数的调用是否是对的，参数是否正确
- 一般的做法就是任何 .c 都有对应的同名的 .h，把所有对外公开的函数的原型和全局变量的声明都放进去
  - 全局变量可以在多个 .c 文件间共享



#### 不对外公开函数

- 在函数前面加上  static 就使得它成为只能在所在的编译单元中使用的函数
- 在全局变量前面加上static就使得它成为只能在所在的编译单元中使用的全局变量



### 12.3.3 声明



#### 变量的声明

- `int i;`是变量的定义
- `extern int i;`是变量的声明
- 定义和声明 在C语言中是两种不同的东西
  - 函数的定义 是定义
  - 函数的原型是声明
  - 变量的定义是定义
  - extern 的变量的声明是声明
    - 作为声明 不能初始化

#### 声明与定义

- 声明是不产生代码（只是记下来）
  - 函数原型
  - 变量声明
  - 结构声明
  - 宏声明
  - 枚举声明
  - 类型声明
  - inline函数
- 定义是产生代码的东西
  - 函数
  - 全局变量



#### 头文件

- 只有声明可以被放在头文件中
  
  - 是规则不是法律
- 否者会造成一个项目中多个编译单元里有重名的实体
  - *某些编译器允许几个编译单元中存在同名函数，或者用weak修饰符来强调这种存在

	#### 重复声明

- 同一个编译单元里，同名的结构不能被重复声明

- 如果你的头文件里有结构的声明，很难这个头文件不会在一个编译单元里被`#include`多次

- 避免重复引用 所以需要 “标准头文件结构”--条件编译

  - ```c
    #ifndef __MAX_H__ //条件编译指令
    #define __MAX_H__
    
    double max(double a, double b);
    extern int gall;
    
    struct Node{
        int value;
        char* name;
    };				// 结构体的结尾需要
    
    #endif
    
    ////-注释---------------
    #ifndef  __XXXX__//条件编译指令 也是编译预处理指令
    
    
    ```

  - `#ifndef  __XXXX__`//条件编译指令 也是编译预处理指令;一般双下划线

  - 运用条件编译和宏，保证这个头文件在一个编译单元中只会被`#include` 一次

  - `#pragma once`也能起到相同作用，但是不是所有的编译器都支持











# 13

#### 格式化的输入输出

```c
printf(%[flags][width][.prec][hlL]type)
scanf(%[flags]type)
```

- flag：含义
  - ` -` : 左对齐
  - +：在前面放+或-
    - `+` 表示强制输出那个加号
    - `-`表示左对齐
  - (space):整数留空
  - 0：零填充
- width 或 .prec
  - number: 最小字符数(整个字符个数)
  - `*` 下一个参数是字符数 （把放在前面字符串中的参数 放在后面，可以有更多用途，参数调用等 有更高的格式上的灵活性）
  - 。number：小数点后的位数
  - ` .* `下一个参数是小数点后的位数
- hiL 修饰符号
  - hh:单个字节
  - h:short
  - l:long
  - ll:long long
  - L:long double
- type
  - i 或者d ：int
  - i:读整数，可能是十六进制或八进制
  - f: float
  - p: 指针
  - s:字符串
  - c:char
  - g或者G:float
  - [...]：所有允许的字符



#### printf和scanf的返回值

- 读入的项目数
- 输出的字符数



- 在要求严格的程序中，应该判断每次调用的scanf 或printf的返回值，从而了解程序运行中是否存在问题





## 文件输入输出

- 13.1.2.1.c

```c
#include <stdio.h>

int main(int argc, char const *argv[])
{
	int num;
	int i1 = scanf("%i",&num);
	int i2 = printf("%d\n",num);

	printf("%d:%d\n", i1, i2);

	return 0;
}
```



- 命令行运行

```bash
cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$ gcc 13.1.2.1.c -o test  //更改链接后程序名字

cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$ ./test.exe				//程序启动
1234
1234
1:5

cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$ gcc 13.1.2.1.c -o test

cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$ ./test.exe
1234
1234
1:5

cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$ ./test.exe > 12.out		//程序重定向写入
123456

cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$ more 12.out		//	没有more查看指令
bash: more: command not found

cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$ tail 12.out		// 采用tail 或者 cat 查看
123456
1:7

cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$ cat > 12.in		//cat 的 写入命令
123456
123
eixt()


cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$ cat 12.in
123456
123
eixt()

cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$ cat >12.in	//再次直接写入
156456

cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$ cat 12.in		//这样的直接写入会是覆盖
156456

cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$ ./test.exe < 12.in
156456
1:7

cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$ ./test.exe <12.in > 12_2.out //重定向输入输出

cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$ cat 12_2.out			//查看
156456
1:7

cyan@orange MINGW64 /f/FPGA2/jisuan/c/13.1.2
$

///----------
cat
ctrl + d
ctrl + c
```





- 已经写好了程序 它是用标准的输入输出（也就是用printf 和scanf 来做输入输出的）

- 用>和< 做重定向





#### FILE

- 在`stdio.h`中 已经声明了`FILE*`这种指针

```c

FILE* fopen(const char* restrict path,const char* restrict mode);	//定义指针，打开文件

int fclose(FILE *stream);

// 文件的读和写函数
fscanf(FILE*,...)
fprintf(FILE*,...)
```



#### 打开文件标准代码

```c
FILE* fp = fopen("file", "r");
	if(fp){
        fscanf(fp,...);
        fclose(fp);
    }else{
        ...
    }
```



- fopen
  - r 只读
  - r+打开读写，从头开始
  - w 打开只写。如果不存在则新建，如果存在则清空
  - w+ 打开读写。如果不存在则新建，如果存在则清空
  - a 打开追加。如果不存在则新建，如果存在则从文件尾开始
  - ..x 只新建，如果文件已存在则不能打开
  - 





 ### 13.1.3 二进制文件



- 其实 所有文件最终都是二进制的
- 文本文件无非是最简单的方式可以读写的文件
  - more tail /读
  - cat + 重定向 < 、>  /简单读写
  - vi
- 二进制文件是需要专门的程序来读写的文件
- 文本文件的输入输出是格式化，可能经过转码



#### 文本VS二进制

- Unix喜欢用文本文件来做数据存储和程序配置
  - 交互式终端出现使得人与计算机沟通增强
  - Unix和shell 提供读写文本的小程序
- window喜欢用二进制文件
  - DOS 并不继承和熟悉Unix
  - 二进制更接近底层





- 优缺点
  - 文本优势 方便交互，跨平台
  - 文本缺点 程序输入输出要经过格式化，开销大
  - 二进制的缺点是人类读写困难，且不跨平台
    - int 的大小不一致，大小端问题。。。
  - 二进制优点是程序读写快





#### 程序为什么要文件

有三种原因

- 配置信息
  - Unix用文本
    - vi vim 即可编辑
  - window用注册表
    - 整个是一个二进制文件
    - 特定工具编辑
- 数据
  - 稍微大的数据放数据库
- 媒体
  - 这个只能是二进制的
  - 现在是，程序通过第三方库来读写文件，很少直接读写二进制文件



### 按位运算

- 把整数当作二进制的东西做运算







#### 逻辑预算与按位运算

- 对于逻辑运算，它只看到两个值：0 和 1 ；
- 可以认为逻辑运算相当于把所的**非 0 **值都变成1，然后做按位运算
- 在计算机内部只有按位运算
- 5&4-->4 
  - 而 5&&4 -->1&1 -->1
- 5|4 -->5  
  - 而5||4 --> 1|1 --> 1
- ~4 --> 3 
  - 而  !4 --> !1 --> 0



#### 按位异或^

#### 左移 右移

### 位运算例子

#### 输出一个数的二进制





### 位段

对于控制多个位 C语言提供了位段这一方法

一般应用于 一些比较底层的 直接操作硬件的场合

- 把一个 int 的若干位组合成一个结构

  

```c
struct{
    unsigned int leading :3;
    unsigned int FLAG1 : 1;
    unsigned int FLAG2 : 1;
    int trailing : 11;
};
// : 后面的数字 表示所占 bit 数目
```



- 可以直接用位段的成员来访问
  - 比移位 、与、 或 还方便
- 编译器会安排其中的位的排列，不具有可移植性
- 当所需的位超过一个 int 时 会采用多个 int 





## 14 

### 14.1.1可变数组

### 14.2.2 链表

- 假象：一个数组 最后 指向 下一个数组 最后一个元素 指向下一个数组
- 链表（可能存在多个节点）
  - 一个节点分为两部分
    - 一部分是数据
    - 一部分是指向下一个节点的指针
      - 指针可以指向下一个相同的单元
      - 开始有个头指针指向第一个节点
      - 最后一个节点 的地址有 表示结束的





![1679490680571](attac\链表示意图.png)





























