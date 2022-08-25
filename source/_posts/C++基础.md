# C++基础

## 函数的分文件编写

1. 创建后缀名为`.h`的头文件；
2. 创建后缀名为`.cpp`的源文件
3. 在头文件中写函数的声明，
4. 在源文件中写函数的定义。

## 指针

**1.占用内存：**在32为操作系统下（即x86）指针占4个字节空间；在64位操作系统下指针占8个字节空间，不管是什么数据类型。

**2.空指针：**指针变量指向内存中编号为0的空间；

```cpp
int* p = NULL;
cout << *p <<endl;
```

​	**用途：**初始化指针变量

​	**注意：**空指针的内存是不可以访问的；内存编号0~255为系统占用内存，不允许用户访问

**3.野指针:**指针变量指向非法内存空间

​	在程序中尽量避免出现野指针

```cpp
int* p = (int*)0x1100;
```

**总结：**空指针和野指针都不是我们申请的空间，因此不要访问

**4.`const`修饰指针**

- `const`修饰指针    ---**常量指针：指针的指向可以修改，但是指针指向的值不可以改**

  ```cpp
  const int* p = &a; //常量指针
  *p = 20;           //错误：指针指向的值不可以改
  p = &b;            //正确：指针指向可以改
  ```

- `const`修饰常量    ---**指针常量：指针的指向不可以改，指针指向的值可以改**

  ```cpp
  int* const p = &a; //指针常量
  *p = 20;           //正确：指向的值可以改
  p = &b;            //错误：指针指向不可以改
  ```

- `const`既修饰指针又修饰常量    ---**特点：指针的指向和指针指向的值都不可以改**

  ```cpp
  const int* const p = &a; //既修饰指针又修饰常量
  *p = 20;                 //错误：指向的值不可以改
  p = &b;                  //错误：指针指向不可以改
  ```

**5.指针和数组**

```cpp
int arr[] = {1,2,3,4,5,6,7,8,9,0};
int* p = arr;
cout << *p << endl;//访问数组中的第一个元素
p++;               //或者*p++
cout << *p << endl;//访问数组中的第二个元素
```

**6.指针和函数**

​	**注意：**普通函数使用时利用形参来进行函数运算，即只是将实参中的值赋予形参，即**值传递**；如果想改变实参中的值，则需要将实参的地址赋予函数，即进行**地址传递**，例如：

```cpp
void swap2(int* a, int* b)
{
	int temp = *a;
	*a = *b;
	*b = temp;
}
int main()
{
	int a = 10;
	int b = 20;
	swap2(&a, &b);
	cout << "a = " << a << endl;
	cout << "b = " << b << endl;
	return 0;
}
```

​	如果是**地址传递**，可以修饰实参。

**7.将指针综合在数组和函数中**

```cpp
//冒泡排序法函数
void BubbleSort(int* arr, int len)
{
	for (int i = 0; i < len - 1; ++i)
	{
		for (int j = 0; j < len - i - 1; ++j)
		{
			if (arr[j] > arr[j + 1])
			{
				int temp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = temp;
			}
		}
	}
}
//主函数
int main()
{
	int arr[] = { 4,3,6,9,1,2,10,8,7,5 };
	BubbleSort(arr, 10);                 //数组名就是数组的首地址！！！
	int* p = arr;
	for (int i = 0; i < 10; ++i)
	{
		cout << *p << endl;
		p++;
	}
	return 0;
}
```

**注意：数组名就是数组的首地址**

## 结构体

- ​	结构体属于用户**自定义的数据类型**，允许用户存储不同的数据类型

**1.结构体的定义和使用**

​	**语法：**`struct 结构体名 { 结构体成员列表 };`

​	通过结构体创建变量的方式有三种：

- ​	struct 结构体名 变量名

- ​	struct 结构体名 变量名 = { 成员1值 , 成员2值...}

- ​	定义结构体时顺便创建变量

  ```cpp
  //创建一个学生结构体
  struct Student
  {
  	string name;
  	int age;
  	float score;
  }s3;//方式3：定义结构体时顺便创建变量
  
  //主函数
  int main()
  {
  	struct Student s1;//方式1
  	s1.name = "张三";
  	s1.age = 18;
  	s1.score = 100;
  	struct Student s2 = { "李四",20,99.5 };//方式2
  	s3.name = "王五";
  	s3.age = 17;
  	s3.score = 59.4;
  	cout << " s1姓名：" << s1.name << " s1年龄：" << s1.age << " s1成绩：" << s1.score << endl;
  	cout << " s2姓名：" << s2.name << " s2年龄：" << s2.age << " s2成绩：" << s2.score << endl;
  	cout << " s3姓名：" << s3.name << " s3年龄：" << s3.age << " s3成绩：" << s3.score << endl;
  	return 0;
  }
  //注意：输出String类型时，必须在程序头加上`#include <string>`
  ```

​	**注意：**`struct`关键字在初始化时也可省略

**2.结构体数组**

- **作用：**将自定义的结构体放入数组中方便维护

- **语法：**`struct 结构体名 数组名[元素个数] = { {},{},...,{}}`

  ```cpp
  //主函数
  int main()
  {
  	Student arr[3] =
  	{
  		{"张三",20,100},
  		{"李四",18,99.5},
  		{"王五",17,59.4}
  	};
  	arr[0].name = "赵六";
  	for (int i = 0; i < sizeof(arr) / sizeof(arr[0]); ++i)
  	{
  		cout << " 姓名：" << arr[i].name << " 年龄：" << arr[i].age << " 成绩：" << arr[i].score << endl;
  	}
  	return 0;
  }
  ```

**3.结构体指针**

​	**作用：**通过指针访问结构体中的成员

- 利用操作符`->`可以通过结构体指针访问结构体属性

  ```cpp
  int main()
  {
  	Student s1 = { "张三",20,100 };
  	Student* p = &s1;//定义指针时也要用结构体定义!!!
  	cout << " 姓名：" << p->name << " 年龄：" << p->age << " 成绩：" << p->score << endl;
  }
  ```

**4.结构体嵌套结构体**

​	**作用：**结构体中的成员可以是另一个结构体

​	**例如：**每个老师教1个学生，一个老师的结构体中，记录一个学生的结构体

```cpp
//创建一个老师结构体
struct Teacher
{
	int id;
	string name;
	Student stu;
};

//主函数
int main()
{
	Student s1 = { "张三",20,100 };
	Teacher t1 = { 123456,"李老师",s1 };
	cout << " 教职工编号：" << t1.id << " 姓名：" << t1.name << endl;
	cout << " 所带学生姓名为：" << t1.stu.name << " 年龄为：" << t1.stu.age << " 成绩为：" << t1.stu.score <<endl;
	return 0;
}
```

**5.结构体做函数参数**

​	**作用：**将结构体作为参数向函数中传递

​	传递方式有两种：

- 值传递

- 地址传递

  **注意：**地址传递与值传递对实参本身的影响

```cpp
//值传递
void PrintStu1(struct Student s)
{
	s.name = "李四";//修改值后注意控制台变化
	cout << " 姓名：" << s.name << " 年龄：" << s.age << " s成绩：" << s.score << endl;
}
//地址传递
void PrintStu2(struct Student* p)
{
	p->name = "王五";
	cout << " 姓名：" << p->name << " 年龄：" << p->age << " s成绩：" << p->score << endl;
}
//主函数
int main()
{
	Student s1 = { "张三",20,100 };
	PrintStu1(s1);
	PrintStu2(&s1);
	cout << " 姓名：" << s1.name << " 年龄：" << s1.age << " s成绩：" << s1.score << endl;
	return 0;
}
```

**6.结构体中`const`的使用场景**

​	**作用：**用`const`防止误操作

```cpp
//将函数中的形参改为指针，可以减少内存使用，且不会复制新的副本出来
void PrintStu(const struct Student* p)
{
	//p->name = "王五";//操作失败，因为加入const
	cout << " 姓名：" << p->name << " 年龄：" << p->age << " s成绩：" << p->score << endl;
}
//主函数
int main()
{
	Student s1 = { "张三",20,100 };
	PrintStu(&s1);
	return 0;
}
```



# C++核心编程

​	本章主要针对C++**面向对象**编程技术做详细讲解，探讨C++的核心和精髓。

### 第一章 内存分区模型

C++程序在执行时，将内存大方向划分为**4个区域**

- **代码区：**存放函数体的二进制代码，由操作系统进行管理
- **全局区：**存放全局变量和静态变量以及常量
- **栈区：**由编译器自动分配释放，存放函数的参数值，局部变量等
- **堆区：**由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收

​	**内存四区存在的意义：**

​		不同区域存放的数据，赋予不同的生命周期，给我们更大的灵活编程

##### 1.1 程序运行前

​	在程序编译后生成了`exe`可执行程序，**未执行该程序前**分为两个区域：

​	**代码区：**

​		存放CPU执行的机器指令

​		代码区是**共享的**，共享的目的是对于频繁被执行的程序，只需要在内存中有一份代码即可

​		代码区是**只读的**，使其只读的原因是防止程序异外修改了它的指令

​	**全局区：**

​		全局变量和静态变量存放在此

​		全局区还包含了常量区，字符串常量和其他常量也存放在此

​		**该区域的数据在程序结束后由操作系统释放**

```cpp
#include <iostream>
using namespace std;

int p_a = 100;
int p_b = 200;
const int c_p_a = 10;

//主函数
int main()
{
	static int s_a = 1000;
	static int s_b = 2000;
	int a = 10;
	int b = 20;
	const int c_a = 10;
	cout << "局部变量a的地址：" << (int) & a << endl;
	cout << "局部变量b的地址：" << (int) & b << endl;
	cout << "全局变量p_a的地址：" << (int) & p_a << endl;
	cout << "全局变量p_b的地址：" << (int) & p_b << endl;
	cout << "静态变量s_a的地址：" << (int)&s_a << endl;
	cout << "静态变量s_b的地址：" << (int)&s_b << endl;
	cout << "字符串常量‘helloworld’的地址为：" << (int)&"helloworld" << endl;
	cout << "const修饰的全局变量(全局常量)c_p_a的地址为：" << (int)&c_p_a << endl;
	cout << "const修饰的局部变量(局部常量)c_a的地址为：" << (int)&c_a << endl;
	return 0;
}
```

​	总结：

- C++中在程序运行期前分为全局区和代码区
- 代码区特点是共享和只读
- 全局区中存放全局变量、静态变量、常量
- 常量区中存放`const`修饰的全局变量和字符串常量

##### 1.2 程序运行后

​	**栈区：**

​		由编译器自动分配释放，存放函数的参数值，局部变量等

​		**注意事项：**不要返回局部变量的地址，栈区开辟的数据由编译器自动释放

```cpp
#include <iostream>
using namespace std;

int* func()
{
	int a = 10;//局部变量 存放在栈区，栈区的数据在函数执行完后自动释放
	return &a;//返回局部变量的地址
}
//主函数
int main()
{
	int* p = func();
	cout << *p << endl;//第一次可以打印正确的数字，是因为编译器做了保留
	cout << *p << endl;//第二次这个数据就不再保留
	return 0;
}
//注意：一般项目运行在x86上，注意此时的x86和x64区别
```

​	**堆区：**

​		由程序员分配释放，若程序员不释放，程序结束时由操作系统回收

​		在C++中主要利用`new`在堆区中开辟内存

```cpp
int* func()
{
	//利用new关键字，可以将数据开辟至堆区
	//指针本质也是局部变量，放在栈上，指针保存的数据放在堆中
	int* p = new int(10);
	return p;
}
//主函数
int main()
{
	int* a = func();
	cout << * a << endl;
	cout << * a << endl;
	return 0;
}
```

##### 1.3 new操作符

​	C++中利用`new`操作符在堆区开辟数据

​	堆区开辟的数据，由程序员手动开辟，手动释放，释放利用操作符`delete`

​	语法：`new 数据类型`

​	利用`new`创建的数据，会返回该数据对应的类型的指针

```cpp
int* func()
{
	//在堆中创建整型数据
	//new返回的是该数据类型的指针
	int* p = new int(10);
	return p;
}
//利用new在堆区开辟数组
void test01()
{
	int* arr = new int[10];
	for (int i = 0; i < 10; ++i)
	{
		arr[i] = i;
	}
	for (int i = 0; i < 10; ++i)
	{
		cout << arr[i] << endl;
	}
	delete[] arr;//释放数组时，delete后应加[]
}
//主函数
int main()
{
	int* a = func();
	cout << *a << endl;
	cout << *a << endl;
	test01();
	//delete释放内存
	delete a;
	//cout << *a << endl;//内存以释放，非法操作
	return 0;
}
```

### 第二章 引用

##### 2.1 引用的基本使用

​	**作用：**给变量起别名

​	**语法：**`数据类型 &别名 = 原名`

```cpp
int main()
{
	int a = 10;
	int& b = a;
	cout << "a = " << a << endl;
	cout << "b = " << b << endl;
	b = 20;
	cout << "a = " << a << endl;
	cout << "b = " << b << endl;

	return 0;
}
```

##### 2.2 引用的注意事项

- 引用必须初始化
- 引用在初始化后，不可以改变

```cpp
#include <iostream>
using namespace std;

//主函数
int main()
{
	int a = 10;
	//int& b;//错误：引用必须初始化
	int& b = a;//正确
	int c = 20;
	b = c;//赋值操作，而不是更改引用
	cout << "a = " << a << endl;
	cout << "b = " << b << endl;
	cout << "c = " << c << endl;
	return 0;
}
```

##### 2.3 引用做函数参数

​	**作用：**函数传参时，可以利用引用的技术让形参修饰实参

​	**优点：**可以简化指针修改实参

```cpp
//1.值传递
void swap01(int a, int b)
{
	int temp = a;
	a = b;
	b = temp;
	return;
}
//2.地址传递
void swap02(int* a, int* b)
{
	int temp = *a;
	*a = *b;
	*b = temp;
}
//3.引用传递
void swap03(int& a, int& b)
{
	int temp = a;
	a = b;
	b = temp;
}

//主函数
int main()
{
	int a = 10;
	int b = 20;
	swap01(a, b);//值传递：形参不会修饰实参
	cout << a << " " << b << endl;
	swap02(&a, &b);//地址传递：形参会修饰实参
	cout << a << " " << b << endl;
	swap03(a, b);//引用传递：形参会修饰实参
	cout << a << " " << b << endl;
	return 0;
}
```

**总结：**通过引用参数产生的效果通地址传递是一样的。引用的语法更简单清楚。

##### 2.4 引用做函数返回值

​	作用：引用可以作为函数的返回值存在

​	**注意：不要返回局部变量引用**

​	用法：函数调用作为左值

```cpp
//引用做函数的返回值
//1.不要返回局部变量的引用
int& test01()
{
	int a = 10;//局部变量存放在四区中的 栈区
	return a;
}
//2.函数的调用可以作为左值
int& test02()
{
	static int b = 20;//静态变量，存放在全局区
	return b;
}

//主函数
int main()
{
	int& ref1 = test01();
	int& ref2 = test02();
	cout << ref1 << endl;//第一次编译器做了保留
	cout << ref1 << endl;//第二次错误，a的内存已经释放
	cout << ref2 << endl;
	cout << ref2 << endl;
	test02() = 1000;//如果函数的返回值是引用，那么函数的调用可以作为左值
	cout << ref2 << endl;
	cout << ref2 << endl;
	return 0;
}
```



##### 2.5 引用的本质

​	本质：**引用的本质在C++内部实现是一个指针常量**

```cpp
//发现引用，转换为 int& const ref = &a;
void func(int& ref)
{
	ref = 100;
	//ref是引用，内部转换为 *ref = 100;
}

//主函数
int main()
{
	int a = 10;
	int& ref = a;
	//自动转换为 int* const ref = &a;指针常量是指针指向不可改，也说明引用为什么不可以更改
	ref = 20;
	//内部发现 ref 是引用自动转换为 *ref = 20;
	cout << "a = " << a << endl;
	cout << "ref = " << ref << endl;
	func(a);
	cout << "a = " << a << endl;
	cout << "ref = " << ref << endl;
	return 0;
}
```

结论：C++推荐使用引用技术，因为语法方便，引用本质是指针常量，但是所有的指针操作编译器都帮我们做了

##### 2.6 常量引用

​	作用：常量引用主要用来修饰形参，防止误操作

​	在函数形参列表中，可以加 `const`修饰形参，防止形参改变实参

```cpp
void show(const int& val)
{
	//val = 100;//防止修改实参
	cout << "val = " << val << endl;
}
//主函数
int main()
{
	//常量引用
	//修饰形参，防止误操作
	//int& ref = 10;//错误：引用必须因一块合法的内存空间
	const int& ref = 10;
	//加上const之后 编译器将代码修改为 int temp = 10；int* ref = temp；
	//ref = 20；//错误：加入const之后变为只读，不可修改
	int a = 20;
	show(a);
	cout << "a = " << a << endl;
	return 0;
}
```



### 第三章 函数提高

##### 3.1 函数默认参数

​	在C++中，函数的形参列表中的形参是可以有默认值的

​	语法：`返回值类型 函数名 （参数 = 默认值）{}`

```cpp
//函数默认参数
//如果我们传入数据，就用自己的数据，否则就用默认值
int func(int a, int b = 20, int c = 30)
{
	return a + b + c;
}

//注意！：
//1、如果某个位置已经有了默认参数，那么从这个位置以后都要有默认参数
//int func1(int a = 10, int b, int c)//错误：a后面的b、c没有默认参数
//{
//	return a + b = c;
//}

// 2、如果函数声明有默认参数，函数实现就不能有默认参数
// 声明和实现只能有一个默认参数
//int func2(int a = 10,int b =10)//函数声明有默认参数
//int func2(int a = 20,int b =20)//错误：函数实现也有了默认参数
//{
//	return a + b;
//}


//主函数
int main()
{
	int a = 10;
	cout << func(a,30) << endl;
}
```



##### 3.2 函数占位参数

​	C++中函数的形参列表里可以有占位参数，用来做占位，调用函数时必须填补该位置

​	**语法：**`返回值类型 函数名 （数据类型）{}`

```cpp
//函数占位参数
//占位参数也可以有默认参数
void func(int a, int )
{
	cout << "This is a function!" << endl;
}

//主函数
int main()
{
	int a = 10;
	func(a,NULL);
	return 0;
}
```



##### 3.3 函数重载

###### 3.3.1 函数重载概述

​	**作用：**函数名可以相同，提高复用性

​	**函数重载满足条件**

- 同一个作用域下
- 函数名称相同
- 函数参数**类型不同** 或者**个数不同** 或**顺序不同**

​	**注意：**函数的返回值不可以作为函数重载的条件

```cpp
void func()
{
	cout << "函数func()的调用" << endl;
}
void func(int a)
{
	cout << "函数func(int a)的调用" << endl;
}
void func(int a, double b)
{
	cout << "函数func(int a,double b)的调用" << endl;
}
void func(double a, int b)
{
	cout << "函数func(int a,double b)的调用" << endl;
}
//函数的返回值不能作为函数重载的条件
//int func(double a, int b)
//{
//	cout << "函数func(int a,double b)的调用" << endl;
//}
//主函数
int main()
{
	func();
	func(10);
	func(10, 3.14);
	func(3.14, 10);
	return 0;
}
```

###### 3.3.2 函数重载的注意事项

- 引用作为重载条件
- 函数重载碰到函数默认参数

```cpp
//函数重载的注意事项
//1.引用作为重载条件
void func(int& a)//int& a = 10 不合法
{
	cout << "这是func(int& a)的调用" << endl;
}
void func(const int& a)//const int& a = 10 合法
{
	cout << "这是const func(int& a)的调用" << endl;
}

//2.函数重载碰到函数默认参数
void func2(int a, int b = 10)
{
	cout << "这是func2(int a, int b = 10)的调用" << endl;
}
void func2(int a)
{
	cout << "这是func2(int a)的调用" << endl;
}
//主函数
int main()
{
	int a = 10;
	func(a);
	func(10);
	//func2(a);//当函数重载碰到默认参数就会出现二义性，重载尽量不要写默认参数
	return 0;
}
```



### 第四章 类和对象

​	C++面向对象的三大特性为：**封装、继承、多态**

​	C++认为**万事万物皆为对象**，对象上有其属性和行为

​	**例如：**

​		人可以作为对象，属性有姓名、年龄、身高...，行为有走、跑、跳...

​		车可以作为对象，属性有轮胎、方向盘...,行为有载人、放音乐...

​		具有相同性质的**对象**，我们可以抽象为**类**，人属于人类，车属于车类

##### 4.1 封装

###### 4.1.1 封装的意义

​	封装是C++面对对象的三大特性之一

​	封装的意义：

- 将属性和行为作为一个整体，表现生活中的事物
- 将属性和行为加以权限控制

​	**封装意义一：**

​		在设计类的时候，属性和行为写在一起，表现事物

​	**语法：**`class 类名{ 访问权限： 属性 / 行为 };`  

```cpp
#include <iostream>
using namespace std;

const double pi = 3.14159;
//设计一个圆类，求圆的周长
class Circle
{
	//访问权限
	//公共权限
public:
	//属性
	double r;
	//行为
	//返回圆的周长
	double primeter()
	{
		return 2 * r * pi;
	}
	//返回圆的面积
	double area()
	{
		return r * r * pi;
	}
};

//主函数
int main()
{
	//通过圆类创建具体的圆（对象）
	//实例化：通过一个类创建一个对象的过程
	Circle c1;
	c1.r = 1;
	cout << "圆的周长为：" << c1.primeter() << endl;
	cout << "圆的面积为：" << c1.area() << endl;
	return 0;
}
```

​	**封装意义二：**

​		类在设计时，可以把属性和行为放在不同权限下，加以控制

​		访问权限有三种：

​			1.`public`        **公共权限：**类内可以访问  类外可以访问

​			2.`protected`  **保护权限：**类内可以访问  类外不可以访问  子类可以访问父类中的保护权限

​			3.`private`      **私有权限：**类内可以访问  类外不可以访问  子类不可以访问父类中的保护权限

```cpp
class person
{
public:
	string p_name;
protected:
	string p_car;
private:
	int p_password;
public:
	void set(string name, string car, int password)
	{
		p_name = name;
		p_car = car;
		p_password = password;
	}
};

//主函数
int main()
{
	person p1;
	p1.set("张三", "拖拉机", 123);
	cout << p1.p_name << endl;
	//cout << p1.p_car << endl;//保护权限类外不可以访问
	//cout << p1.p_password << endl;//私有权限类外不可以访问
	return 0;
}
```

###### 4.1.2 struct和class区别

​	在C++中`struct`和`class`唯一的**区别**就在于**默认的访问权限不同**

​	区别：

- `struct`默认权限为**公有**
- `class`  默认权限为**私有**

```cpp
class C1
{
	int a;//默认权限：私有
};

struct C2
{
	int b;//默认权限：公有
};
//主函数
int main()
{
	C1 c1;
	C2 c2;
	//c1.a = 10;//不可访问
	c2.b = 30;
	return 0;
}
```

###### 4.1.3 成员属性设置为私有

​	**优点1：**将所有成员属性设置为私有，可以自己控制读写权限

​	**优点2：**对于写权限，我们可以检测数据的有效性

```cpp
class person
{
private:
	string p_name;//设置为可读可写
	int p_age = 18;//设置为只读不写
	string p_lover;//设置为只写不读
public:
	//设置名字可写接口
	void setName(string name)
	{
		p_name = name;
	}
	//设置名字可读接口
	void showName()
	{
		cout << "姓名：" << p_name << endl;
	}
	//设置年龄只读接口
	void showAge()
	{
		cout << "年龄为：" << p_age << endl;
	}
	//设置爱人只写接口
	void setLover(string lover)
	{
		p_lover = lover;
	}
};

//主函数
int main()
{
	person p1;
	p1.setName("张三");
	p1.setLover("桃子女士");
	p1.showName();
	p1.showAge();
	return 0;
}
```

##### 4.2 对象的初始化和清理

- 生活中我们买的电子产品都基本会有出厂设置，在某一天我们不用的时候也会删除一些自己信息数据保证安全
- C++的面向对象中来源于生活，每个对象也都会有初始设置以及对象销毁前的清理数据的设置



###### 4.2.1 构造函数和析构函数

​	对象的**初始化和清理**也是两个非常重要的安全问题

​		一个对象或者变量没有初始状态，对其使用后果是未知

​		同样的使用完一个对象或变量，没有及时清理，也会造成一定的安全问题



​	C++利用了**构造函数**和**析构函数**解决上述问题，这两个函数将会被编译器自动调用，完成对象的初始化和清理。

​	对象的初始化和清理工作是编译器强制要我们做的事情，因此如果**我们不提供构造和析构，编译器会提供编译器提供的构造函数和析构函数，但其是空实现。**



- 构造函数：主要作用在于创建对象时为对象的成员属性赋值，构造函数有编译器自动调用，无需手动调用
- 析构函数：主要作用在于对象**销毁前**系统自动调用，执行一些清理工作



**构造函数语法：**`类名(){}`

1. 构造函数，没有返回值也不写`void`
2. 函数名称与类名相同
3. 构造函数可以有参数，因此可以发生重载
4. 程序在调用对象时会自动调用构造，无需手动调用，而且只会调用一次



**析构函数语法：**`~类名(){}`

1. 析构函数，没有返回值也不写`void`
2. 函数名称与类名相同，在名称前加上符号`~`
3. 析构函数不可以有参数，因此不可以发生重载
4. 程序在对象销毁前会自动调用析构，无需手动调用，而且只会调用一次

```cpp
class Person
{
public:
	//1 创建创建构函数
	Person()
	{
		cout << "这是Person构造函数的调用" << endl;
	}
	//2 创建析构函数
	~Person()
	{
		cout << "这是Person析构函数的调用" << endl;
	}
};



void test01()
{
	Person p1;//在栈上的数据，test01执行完毕后，会释放这个对象
}

//主函数
int main()
{
	test01();
	cout << "*********" << endl;
	Person p2;
	system("pause");////若不加pause，那么系统不会暂停，对象就会被释放
	return 0;
}
```



###### 4.2.2 构造函数的分类及调用

​	两种分类方式：

​		按参数分为：有参构造和无参构造

​		按类型分为：普通构造和构造拷贝

​	三种调用方式：

​		括号法

​		显示法

​		隐式转换法

```cpp
class Person
{
public:
	int age;
	Person()
	{
		cout << "Person的无参调用" << endl;
	}
	Person(int a)
	{
		age = a;
		cout << "Person的有参调用" << endl;
	}
	//拷贝构造参数
	Person(const Person& p)
	{
		cout << "Person的拷贝构造参数调用" << endl;
		//将传入的人身上所有的属性，拷贝到我身上
		age = p.age;
	}
	//创建析构函数
	~Person()
	{
		cout << "Person析构函数调用" << endl;
	}
};

//调用
void test01()
{
	//1.括号法
	Person p1;
	Person p2(10);
	Person p3(p2);
	cout << "p2的年龄：" << p2.age << endl;
	cout << "p3的年龄：" << p3.age << endl;
	//注意事项
	//调用默认构造函数时，不要加()
	//因为下面这行代码，编译器会认为是一个函数的声明,不会认为在创建对象
	//Person p1();

	//2.显示法
	Person p4;//无参构造
	Person p5 = Person(10);//有参构造
	Person p6 = Person(p5);//拷贝构造
	Person(10);//匿名对象 特点：当前行执行结束后，系统会立即回收掉匿名对象
	cout << "$$$" << endl;

	//注意事项2
	//不要利用拷贝构造函数初始化匿名对象,编译器会认为 Person(p3) === Person p3
	//Person(p3);

	//3.隐式转换法
	Person p7 = 10;//相当于 Person p7 = Person(10)
 }

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```

###### 4.2.3 拷贝构造函数调用时机

​	C++中拷贝构造函数调用时机通常由三种情况

- 使用一个已经创建完毕的对象来初始化一个新对象
- 值传递的方式给函数参数传值
- 以值方式返回局部对象 

```cpp
class Person
{
public:
	int p_age;
	Person()
	{
		cout << "Person默认构造函数调用" << endl;
	}
	~Person()
	{
		cout << "Person析构函数调用" << endl;
	}
	Person(int age)
	{
		cout << "Person有参构造函数" << endl;
		p_age = age;
	}
	Person(const Person& p)
	{
		cout << "Person拷贝构造函数" << endl;
		p_age = p.p_age;
	}
};


//拷贝构造函数调用时机
//1.使用一个已经创建完毕的对象来初始化一个新对象
void test01()
{
	Person p1(20);
	Person p2(p1);
	cout << "p2的年龄：" << p2.p_age << endl;
}
//2.值传递的方式给函数参数传值
void doWork(Person p)
{

}
void test02()
{
	Person p;
	doWork(p);
}
//3.值方式返回局部对象
Person doWork2()
{
	Person p1;
	return p1;
}
void test03()
{
	Person p = doWork2();
}



//主函数
int main()
{
	test01();
	cout << "***********" << endl;
	test02();
	cout << "***********" << endl;
	test03();
	system("pause");
	return 0;
}
```

###### 构造函数调用规则

​	默认情况下，C++编译器至少给一个类添加3个函数

1. 默认构造函数（无参，函数体为空）
2. 默认析构函数（无参，函数体为空）
3. 默认拷贝构造参数，对属性进行值拷贝



​	构造函数调用规则如下：

- 如果用户定义有参构造函数，C++不会提供默认无参构造，但是会提供默认拷贝构造
- 如果用户定义拷贝构造参数，C++不会再提供其他构造函数

```cpp
/构造函数的调用规则
//1、创建一个类，C++编译器会给每个类都添加至少三个函数
// 默认构造 （空实现）
// 析构函数 （空实现）
// 拷贝构造 （值拷贝）

//2、如果我们写了有参构造函数，编译器就不再提供默认构造，依然提供拷贝构造
//3、如果我们写了拷贝构造函数，编译器就不再提供其他构造函数
class Person
{
public:
	int p_age;
	/*Person()
	{
		cout << "Person默认构造函数调用" << endl;
	}*/
	/*Person(int age)
	{
		cout << "Person的有参构造函数调用" << endl;
		p_age = age;
	}*/
	Person(const Person& p)
	{
		cout << "Person拷贝构造函数调用" << endl;
		p_age = p.p_age;
	}
	~Person()
	{
		cout << "Person析构函数调用" << endl;
	}
};

//void test01()
//{
//	Person p;
//	p.p_age = 18;
//	Person p2(p);
//	cout << "p2的年龄：" << p2.p_age << endl;
//}
//void test02()
//{
//	Person p(28);
//	Person p2(p);
//}
void test03()
{
	//Person p;//只提供拷贝构造函时，编译器不会提供其他构造函数
}

//主函数
int main()
{
	//test01();
	//test02();
	system("pause");
	return 0;
}
```



###### 4.2.5 深拷贝与浅拷贝

​	深拷贝时面试经典问题，也是常见的一个坑

- 浅拷贝：简单的复制拷贝操作
- 深拷贝：在堆区重新申请空间，进行拷贝操作

```cpp
//深拷贝与浅拷贝
//浅拷贝带来的问题就是堆区的内存重复释放
class Person
{
public:
	int p_age;
	int* p_height;
	Person()
	{
		cout << "Person默认构造函数" << endl;
	}
	Person(int age,int height)
	{
		cout << "Person有参构造函数" << endl;
		p_age = age;
		p_height = new int(height);
	}
	//自己实现拷贝构造函数 解决浅拷贝带来的问题
	Person(const Person& p)
	{
		cout << "Person拷贝构造函数的调用" << endl;
		p_age = p.p_age;
		//p_height = p.p_height;//编译器默认实现就是这行代码
		//深拷贝操作
		p_height = new int(*p.p_height);
	}
	~Person()
	{
		//析构代码，将堆区开辟数据做释放操作
		if (p_height != NULL)
		{
			delete p_height;
			p_height = NULL;
		}
		cout << "Person析构函数" << endl;
	}
};

void test01()
{
	Person p1(18,160);
	cout << "p1的年龄：" << p1.p_age << " p1的身高：" << *p1.p_height << endl;
	Person p2(p1);
	cout << "p2的年龄：" << p2.p_age << " p2的身高：" << *p2.p_height << endl;
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



###### 4.2.6 初始化列表

​	**作用：**

​		C++提供了初始化列表语法，用来初始化操作

​	

​	**语法：**`构造函数():属性1(值1),属性2(值2)...{}`

```cpp
//初始化列表
class Person
{
public:
	int p_a;
	int p_b;
	int p_c;
	//传统初始化操作
	/*Person(int a, int b, int c)
	{
		p_a = a;
		p_b = b;
		p_c = c;
	}*/
	
	//初始化列表初始化属性
	Person(int a,int b,int c) :p_a(a), p_b(b), p_c(c)
	{

	}
};

void test01()
{
	Person p(10, 20, 30);
	cout << "p_a = " << p.p_a << endl;
	cout << "p_b = " << p.p_b << endl;
	cout << "p_c = " << p.p_c << endl;
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



###### 4.2.7 类对象作为类成员

​	C++类中的成员可以是另一个类的对象，我们称该成员为 **对象成员**

```cpp
class A{}
class B
{
    A a;
}
```

B类中有对象A作为成员，A为对象成员



那么当创建B对象时，A与B的构造和析构的顺序时谁先谁后？

```cpp
class Phone
{
public:
	Phone(string pName)
	{
		cout << "Phone的构造函数调用" << endl;
		p_PName = pName;
	}
	string p_PName;
	~Phone()
	{
		cout << "Phone析构函数调用" << endl;
	}
};

class Person
{
public:
	string p_Name;
	Phone p_Phone;
	Person(string name, string pName) :p_Name(name), p_Phone(pName)
	{
		cout << "Person的构造函数调用" << endl;
	}
	~Person()
	{
		cout << "Person析构函数调用" << endl;
	}
};

void test01()
{
	Person p("张三", "苹果MAX");
	cout << p.p_Name << "拿着" << p.p_Phone.p_PName << endl;
}
//当其他类对象作为本类成员，构造是先构造对象，再构造自身；析构的顺序相反
//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



###### 4.2.8 静态成员

​	静态成员就是在成员变量和成员函数前加上关键字`static`，称为静态成员

​	静态成员分类：

- 静态成员变量
  - 所有对象共享一份数据
  - 在编译阶段分配内存
  - 类内声明，类外初始化
- 静态成员函数
  - 所有对象共享一个函数
  - 静态成员函数只能访问静态成员变量



**示例1：**静态成员变量

```cpp
//静态成员变量
class Person
{
public:
	//1、所有对象都共享同一份数据
	//2、编译阶段就分配内存
	//3、类内声明，类外初始化操作
	static int p_A;
	//静态成员变量也是有访问权限的
private:
	static int p_B;
};
int Person::p_A = 100;//类外初始化
int Person::p_B = 300;

void test01()
{
	Person p;
	cout << p.p_A << endl;
	Person p2;
	p2.p_A = 200;
	cout << p.p_A << endl;
}
void test02()
{
	//静态成员变量 不属于某个对象，所有对象都共享同一份数据
	//因此静态成员有两种访问方式
	//1、通过对象进行访问
	Person p;
	cout << p.p_A << endl;
	//2、通过类名进行访问
	cout << Person::p_A << endl;
	//cout << Person::p_B << endl;//类外访问不到私有静态成员变量
}

//主函数
int main()
{
	//test01();
	test02();
	system("pause");
	return 0;
}
```

**示例2：**静态成员函数

```cpp
//静态成员函数
//所有对象共享一个函数
//静态成员函数只能访问静态成员变量

class Person
{
public:
	//静态成员函数
	static void func()
	{
		//p_B = 100;//报错：静态成员函数不可以访问非静态成员变量，无法区分到底是那个对象的p_B
		p_A = 100;//静态成员变量可以访问静态成员变量
		cout << "static void func调用" << endl;
	}
	static int p_A;//静态成员变量
	int p_B;
	//静态成员函数也是有访问权限的
private:
	static void func2()
	{
		cout << "static void func2调用" << endl;
	}
};
int Person::p_A;
void test01()
{
	//1、通过对象访问
	Person p;
	p.func();
	//2、通过类名访问
	Person::func();
	//Person::func2();//类外访问不到私有的静态成员函数
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```

##### 4.3 C++对象模型和this指针

###### 4.3.1 成员变量和成员函数分开储存

​	在C++中，类内的成员变量和成员函数分开储存

​	只有非静态成员变量才属于类的对象上

```cpp
//成员变量和成员函数分开储存
class Person01
{

};

class Person02
{
	int p_A;//非静态成员变量 属于类的对象
	static int p_B;//静态成员变量 不属于类的对象上
	void func(){}//非静态成员函数 不属于类的对象上
	static void func2(){}//静态成员函数 不属于类的对象上
};
int Person02::p_B;
void test01()
{
	Person01 p1;
	//空对象占用内存空间为：1
	//C++编译器会给每个空对象也分配一个字节空间，是为了区分空对象占内存的位置
	//每个空对象也应该有一个独一无二的内存地址
	cout << "size of p1 = " << sizeof(p1) << endl;
}
void test02()
{
	Person02 p2;
	cout << "size of p2 = " << sizeof(p2) << endl;
}

//主函数
int main()
{
	test01();
	test02();
	system("pause");
	return 0;
}
```

###### 4.3.2 this指针概念

​	通过4.3.1我们知道在C++中成员变量和成员函数是分开储存的

​	每一个非静态成员函数只会诞生一份函数实例，也就是说多个同类型的对象会共用一块代码

​	那么问题是：这一块代码是如何区分那个对象调用自己的呢？



​	C++通过提供特殊对象指针，`this`指针，解决上述问题。

​	**`this`指针指向被调用的成员函数所属的对象**



​	`this`指针是隐含每一个非静态成员函数内的一种指针

​	`this`指针不需要定义，直接使用即可



​	`this`指针的用途：

- 当形参和成员变量同名是，可用`this`指针来区分
- 在类的非静态成员函数中返回对象本身，可使用`return *this`

```cpp
class Person
{
public:
	int age;
	Person(int age)
	{
		//this指针指向的是 被调用的而成员函数所属的对象
		this->age = age;
	}
	Person& PeronAddAge(Person &p)
	{
		this->age += p.age;
		//this指向p2的指针，而*this指向的就是p2这个对象本体
		return *this;
	}
};
//1、解决名称冲突
void test01()
{
	Person p(18);
	cout << "p的年龄为：" << p.age << endl;
}
//2、返回对象本身用*this
void test02()
{
	Person p1(10);
	Person p2(20);
	//链式编程思想
	p2.PeronAddAge(p1).PeronAddAge(p1).PeronAddAge(p1);
	cout << "p2的年龄为：" << p2.age << endl;
}

//主函数
int main()
{
	test01();
	test02();
	system("pause");
	return 0;
}
```



###### 4.3.3 空指针访问成员函数

​	C++中空指针也是可以调用成员函数的，但是也要注意有没有用到`this`指针

​	

​	如果用到`this`指针，需要加以判断保证代码的健壮性

```cpp
//空指针调用成员函数
class Person
{
public:
	int p_age;
	void showClassName()
	{
		cout << "this is Person class" << endl;
	}
	void showPersonAge()
	{
		//报错原因是因为传入的指针是为NULL
		//添加判定条件，增加代码健壮性
		if (this == NULL)
		{
			return;
		}
		cout << "age = " << p_age << endl;
	}
};

void test01()
{
	Person* p = NULL;
	p->showClassName();
	p->showPersonAge();

}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



###### 4.3.4 const修饰成员函数



​	**常函数：**

- 成员函数后加const后我们称这个函数为**常函数**
- 常函数内不可以修改成员属性
- 成员属性声明时加关键字`mutable`后，在常函数中依然可以改



​	**常对象：**

- 声明对象前加`const`称该对象为常对象
- 常对象只能调用常函数

```cpp
//常函数
class Person
{
public:
	int p_A;
	//特殊变量，即使在常函数中，也可以修改这个值，加关键字mutable
	mutable int p_B;
	//this指针的本质 是指针常量 指针的指向是不可以修改的
	//const Person* const this
	//在成员函数后面加const，修饰的是this指向，让指针指向的值也不可以修改
	void showPerson() const
	{
		//this->p_A = 100;
		//this = NULL;//this指针不可以修改指针的指向
		this->p_B = 100;
	}
	void func(){}
};

void test01()
{
	Person p;
	p.showPerson();
}
//常对象
void test02()
{
	const Person p;//在对象前加const，变为常对象
	//p.p_A = 100;//报错
	p.p_B = 100;//特殊值，在常对象下也可以修改
	//常对象只能调用常函数
	p.showPerson();
	//常对象不可以调用普通成员函数，因为普通成员函数可以修改属性
	//p.func();
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



##### 4.4 友元

​	在程序里，有些私有属性 也想让类外一些函数或者类进行访问，就需要用到友元的技术



​	友元的目的就是让一个函数或者类 访问另一个类中私有成员

​	友元的关键字为 `friend`

​	友元的三种实现 

- 全局函数做友元
- 类做友元
- 成员函数做友元



###### 4.4.1 全局函数做友元

```cpp
//全局函数做友元
class Building
{
	//goodGuy全局函数是Building的好朋友，可以访问其中的私有成员
	friend void goodGuy(Building* building);
public:
	string m_LivingRoom;
	Building()
	{
		m_LivingRoom = "客厅";
		m_BedRoom = "卧室";
	}
private:
	string m_BedRoom;
};
//全局函数
void goodGuy(Building* building)
{
	cout << "好朋友全局函数 正在访问：" << building->m_LivingRoom << endl;
	cout << "好朋友全局函数 正在访问：" << building->m_BedRoom << endl;
}

void test01()
{
	Building building;
	goodGuy(&building);
}
//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



###### 4.4.2 类做友元

```cpp
//类做友元
class Building
{
	friend class GoodGuy;
public:
	string m_LivingRoom;
	Building();
private:
	string m_BedRoom;
};
class GoodGuy
{
public:
	Building* building;
	void visit();//参观函数 访问Building中的属性
	GoodGuy();
};
//类外写成员函数
Building::Building()
{
	m_LivingRoom = "客厅";
	m_BedRoom = "卧室";
}
GoodGuy::GoodGuy()
{
	//创建建筑物对象
	building = new Building;
}
void GoodGuy::visit()
{
	cout << "好朋友类正在访问：" << building->m_LivingRoom << endl;
	cout << "好朋友类正在访问：" << building->m_BedRoom << endl;
}
void test01()
{
	GoodGuy gg;
	gg.visit();
}
//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



###### 4.4.3 成员函数做友元

```cpp
//成员函数做友元
class Building;
class GoodGuy
{
public:
	GoodGuy();
	//让visit函数可以访问Building中私有成员
	void visit();
	//让visit2函数不可以访问Building中的私有成员
	void visit2();
	Building* building;

};
class Building
{
	friend void GoodGuy::visit();
public:
	string m_LivingRoom;
	Building();
private:
	string m_BedRoom;
};

//类外实现成员函数
Building::Building()
{
	m_LivingRoom = "客厅";
	m_BedRoom = "卧室";
}
GoodGuy::GoodGuy()
{
	building = new Building;
}
void GoodGuy::visit()
{
	cout << "visit函数正在访问：" << building->m_LivingRoom << endl;
	cout << "visit函数正在访问：" << building->m_BedRoom << endl;
}
void GoodGuy::visit2()
{
	cout << "visit函数正在访问：" << building->m_LivingRoom << endl;
}
void test01()
{
	GoodGuy gg;
	gg.visit();
	gg.visit2();
}
//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



##### 4.5 运算符重载

​	运算符重载概念：对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型

###### 4.5.1 加号运算符重载

​	作用：实现两个自定义数据类型相加的运算

```cpp
//加号运算符重载
class Person
{
public:
	int m_A;
	int m_B;
	//1、成员函数重载+号运算符
	/*Person operator+(Person& p)
	{
		Person temp;
		temp.m_A = this->m_A + p.m_A;
		temp.m_B = this->m_B + p.m_B;
		return temp;
	}*/
};
//2、全局函数重载+号
Person operator+(Person& p1, Person& p2)
{
	Person temp;
	temp.m_A = p1.m_A + p2.m_A;
	temp.m_B = p1.m_B + p2.m_B;
	return temp;
}
Person operator+(Person& p1, int num)
{
	Person temp;
	temp.m_A = p1.m_A + num;
	temp.m_B = p1.m_B + num;
	return temp;
}

void test01()
{
	Person p1;
	p1.m_A = 10;
	p1.m_B = 10;
	Person p2;
	p2.m_A = 10;
	p2.m_B = 10;
	//Person p3 = p1.operator+(p2);//成员函数重载本质调用
	//Person p3 = operatoe+(p1,p2);//全局函数重载本质调用
	Person p3 = p1 + p2;
	//运算符重载也可以发生函数重载
	Person p4 = p1 + 20;
	cout << "p3.m_A = " << p3.m_A << endl;
	cout << "p3.m_B = " << p3.m_B << endl;
	cout << "p4.m_A = " << p4.m_A << endl;
	cout << "p4.m_B = " << p4.m_B << endl;
}


//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```

> 总结1：对于内置的数据类型的表达式的运算符是不可能改变的
>
> 总结2：不要滥用运算符重载



###### 4.5.2 左移运算符重载

​	作用：可以输出自定义数据类型

```cpp
//左移运算符重载
class Person
{
public:
	int m_A;
	int m_B;
	//利用成员函数重载 左移运算符 p.operator<<(cout) 简化版本 p << cout
	//不会利用成员函数重载<<运算符，因为无法实现cout在左侧
};

//只能利用全局函数重载左移运算符
ostream& operator<<(ostream& out, Person& p)
{
	out << "m_A = " << p.m_A << " m_B = " << p.m_B;
	return out;
}

void test01()
{
	Person p;
	p.m_A = 10;
	p.m_B = 10;
	cout << p << endl;
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```

> 总结：重载左移运算符配合友元可以实现输出自定义数据类型



###### 4.5.3 递增运算符重载

​	作用：通过重载递增运算符，实现自己的整型数据

```cpp
//重载递增运算符
//自定义整型
class MyInteger
{
	friend ostream& operator<<(ostream& out, MyInteger& p);
public:
	MyInteger()
	{
		m_Num = 0;
	}
	//重载前置++运算符  返回引用为了一直对一个数据进行操作
	MyInteger& operator++()
	{
		m_Num++;
		return *this;
	}
	//重载后置++运算符
	//int代表占位参数，可以用于区分前置和后置递增
 	MyInteger& operator++(int)
	{
		//先 记录当时结果
		MyInteger temp = *this;
		//后 递增
		m_Num++;
		//最后将记录结果做返回
		return temp;
	}
private:
	int m_Num;
};
//重载<<运算符
ostream& operator<<(ostream& out, MyInteger& p)
{
	out << p.m_Num;
	return out;
}

void test01()
{
	MyInteger myint;
	cout << ++myint << endl;
	cout << myint++ << endl;
	cout << myint << endl;
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;

}
```

> 总结：前置递增返回引用，后置递增返回值



###### 4.5.4 赋值运算符重载

​	C++编译器至少给一个类添加4个函数

1. 默认构造函数（无参，函数体为空）
2. 默认析构函数（无参，函数体为空）
3. 默认拷贝构造函数，对属性进行拷贝
4. 赋值运算符`operator=`，对属性进行值拷贝



​	如果类中有属性指向堆区，做赋值操作也会出现深浅拷贝问题

```cpp
//赋值运算符重载
class Person
{
public:
	int* m_Age;
	Person(int age)
	{
		m_Age = new int(age);
	}
	~Person()
	{
		if (m_Age != NULL)
		{
			delete m_Age;
			m_Age = NULL;
		}
	}
	//重载赋值运算符
	Person& operator=(Person& p)
	{
		//编译器是提供浅拷贝
		//m_Age = p.m_Age
		//应该先判断是否有属性在堆区 如果有，先释放干净 然后再深拷贝
		if (m_Age != NULL)
		{
			delete m_Age;
			m_Age = NULL;
		}
		//深拷贝
		m_Age = new int(*p.m_Age);
		//返回对象本身
		return *this;
	}
};

void test01()
{
	Person p1(18);
	Person p2(20);
	Person p3(30);
	p3 = p2 = p1;
	cout << "p1的年龄为：" << *p1.m_Age << endl;
	cout << "p2的年龄为：" << *p2.m_Age << endl;
	cout << "p3的年龄为：" << *p3.m_Age << endl;
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



###### 4.5.5 关系运算符重载

​	**作用：**重载关系运算符，可以让两个自定义类型对象进行对比操作

```cpp
//关系运算符重载
class Person
{
public:
	string m_Name;
	int m_Age;
	Person(string name, int age)
	{
		m_Name = name;
		m_Age = age;
	}
	//重载==号
	bool operator==(Person& p)
	{
		if (m_Name == p.m_Name && m_Age == p.m_Age)
			return true;
		return false;
	}
};

void test01()
{
	Person p1("Tom", 18);
	Person p2("Jerry", 18);
	if (p1 == p2)
	{
		cout << "p1和p2相等" << endl;
	}
	else
	{
		cout << "p1和p2不相等" << endl;
	}
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



###### 4.5.6 函数调用运算符重



- 函数调用运算符`()`也可以重载
- 由于重载后使用的方式非常像函数的调用，因此称为仿函数
- 仿函数没有固定写法，非常灵活

```cpp
//函数调用运算符重载
//打印输出类
class MyPrint
{
public:
	//重载函数调用运算符
	void operator()(string test)
	{
		cout << test << endl;
	}
};
void MyPrint02(string test)
{
	cout << test << endl;
}
void test01()
{
	MyPrint myPrint;
	myPrint("hello world");//由于使用起来非常类似于函数 因此称为仿函数
	MyPrint02("hello");
}
//仿函数非常灵活，没有固定的写法
//加法类
class MyAdd
{
public:
	int operator()(int num1, int num2)
	{
		return num1 + num2;
	}
};

void test02()
{
	MyAdd myadd;
	cout << myadd(100, 200) << endl;
	//匿名函数对象
	cout << MyAdd()(100, 300) << endl;
}
//主函数
int main()
{
	test01();
	test02();
	system("pause");
	return 0;
}
```



##### 4.6 继承

​	**继承是面向对象三大特性之一**

​	有些类与类之间存在特殊关系，定义某些类时，下级别的成员除了拥有上一级的共性，还有自己的特性

​	这个时候我们就可以考虑利用继承的技术，减少重复代码



###### 4.6.1 继承的基本语法

​	**语法：**`class 子类 : 继承方式 父类`

- 子类 也称为 派生类
- 父类 也称为 基类

​	例如我们看到很多网站中，都有公共的头部，公共的的底部，甚至公共的左侧列表，只有中心内容不同

​	接下来我们分别利用普通写法和继承写法来实现网页中的内容，看一下继承存在的意义以及好处

```cpp
//继承实现
//公共页面
class BasePage
{
public:
	void header()
	{
		cout << "首页、公开课、登录、注册。。。（公共头部）" << endl;
	}
	void footer()
	{
		cout << "帮助中心、合作交流、站内地图。。。（公共底部）" << endl;
	}
	void left()
	{
		cout << "Java、Python、C++。。。（公共分类列表）" << endl;
	}
};
//继承公共页面
class Python:public BasePage
{
public:
	void content()
	{
		cout << "Python学科视频" << endl;
	}
};
//继承的好处：减少重复代码
//语法：class 子类 ： 继承方式 父类
//子类 也称为 派生类
//父类 也称为 基类
class Java :public BasePage
{
public:
	void content()
	{
		cout << "Java学科视频" << endl;
	}
};
void test01()
{
	cout << "Java页面" << endl;
	Java ja;
	ja.header();
	ja.footer();
	ja.left();
	ja.content();
	cout << "Python页面" << endl;
	Python py;
	py.header();
	py.footer();
	py.left();
	py.content();
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```

​	**派生类中的成员包含两大部分：**

​		一类是从基类继承过来的，一类是自己增加的成员

​	从基类继承过来的表现其共性，而新增的成员体现了其个性



###### 4.6.2 继承方式

​	**继承的语法：**`class 子类 : 继承方式 父类`

​	**继承方式一共有三种：**

- 公共继承
- 保护继承
- 私有继承

```cpp
//继承方式
class Base
{
public:
	int m_A;
protected:
	int m_B;
private:
	int m_C;
};
//公共继承
class Son1 :public Base
{
public:
	void func()
	{
		m_A = 10;//父类中的公共权限 到子类依然是公共权限
		m_B = 10;//父类中的保护权限 到子类依然是保护权限
		//m_C = 10;////父类中的私有权限 子类依然访问不到
	}
};
//保护继承
class Son2:protected Base
{
public:
	void func()
	{
		m_A = 100;//父类中公共成员 到子类中变为保护权限
		m_B = 100;//父类中保护成员 到子类中变为保护权限
		//m_C = 100;//父类中私有成员 子类访问不到
	}
};
//私有继承
class Son3 :private Base
{
public:
	void func()
	{
		m_A = 100;//父类中私有成员 到子类中变为私有权限
		m_B = 100;//父类中保护成员 到子类中变为私有权限
		//m_C = 100;//父类中私有成员 子类访问不到
	}
};
class GrandSon3 :public Son3
{
public:
	void func()
	{
		m_A = 1000;//到了Son3中m_A变为私有，即使是儿子，也访问不到
	}
};


void test01()
{
	Son1 s1;
	s1.m_A = 100;
	//s1.m_B = 100;//到Son1中 m_B 是保护权限 类外访问不到
}
void test2()
{
	Son2 s2;
	//s2.m_A = 1000;//在Son2中m_A变为保护权限，因此类外访问不到
}
void test03()
{
	Son3 s3;
	//s3.m_A = 1000;//到Son3中 变为 私有成员 类外访问不到
}

//主函数
int main()
{
	system("pause");
	return 0;
}
```



###### 4.6.3 继承中的对象模型

​	**问题：**从父类继承过来的成员，哪些属于子类对象中？



```cpp
//继承中的对象模型
class Base
{
public:
	int m_A;
protected:
	int m_B;
private:
	int m_C;
};

class Son :public Base
{
public:
	int m_B;
};

//利用开发人员命令提示工具查看对象模型
//跳转盘符
//跳转文件路径 cd 具体路劲下
//查看命名
// cl /dl reportSingleClassLayout类名 文件名

void test01()
{
	//16
	//父类中所有非静态成员属性都会被子类继承下去
	//父类中私有成员属性 是被编译器给隐藏了 因此访问不到 但确实被继承下去了
	cout << "size of Son = " << sizeof(Son) << endl;
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```

>利用开发人员命令提示工具查看对象模型：
>
>1. 跳转盘符
>2. 跳转文件路径 `cd` 具体路径下
>3. 查看命名
>4. `cl /dl reportSingleClassLayout类名 文件名`

> 结论：父类中私有成员也是被子类继承下去了，只是由编译器给隐藏后访问不到



###### 4.6.4 继承中的构造和析构顺序



​	子类继承父类后，当创建子类对象，也会调用父类的构造函数



​	**问题：**父类和子类的构造和析构顺序是谁先谁后？

```cpp
//继承中构造和析构顺序
class Base
{
public:
	Base()
	{
		cout << "Base构造函数" << endl;
	}
	~Base()
	{
		cout << "Base析构函数" << endl;
	}
};
class Son :public Base
{
public:
	Son()
	{
		cout << "Son构造函数" << endl;
	}
	~Son()
	{
		cout << "Son析构函数" << endl;
	}
};

void test01()
{
	//Base b;
	
	//继承中的构造和析构顺序如下：
	//先构造父类 再构造子类 析构的顺序与构造相反
	Son s;
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```

>继承中的构造和析构顺序如下：
>	先构造父类 再构造子类 析构的顺序与构造相反



###### 4.6.5 继承同名成员处理方式

​	**问题：**当子类与父类出现同名的成员，如何通过子类对象，访问到子类或父类中同名的数据呢？



- 访问子类同名成员 直接访问即可
- 访问父类同名成员 需要加作用域

```cpp
//继承中同名成员处理
class Base
{
public:
	Base()
	{
		m_A = 100;
	}
	void func()
	{
		cout << "Base func() 调用" << endl;
	}
	void func(int a)
	{
		cout << "Base func(int a) 调用" << endl;
	}
	int m_A;
};
class Son :public Base
{
public:
	Son()
	{
		m_A = 200;
	}
	void func()
	{
		cout << "Son func() 调用" << endl;
	}
	int m_A;
};

//继承同名成员处理
void test01()
{
	Son s;
	cout << "Son m_A = " << s.m_A << endl;
	//如果通过子类对象 访问到父类中同名成员 需要加作用域
	cout << "Base m_A = " << s.Base::m_A << endl;
}
//继承同名函数处理
void test02()
{
	Son s;
	s.func();//直接调用 调用的是子类中的同名成员
	s.Base::func();
	//如果子类中出现和父类同名的成员函数 子类的同名成员会隐藏掉父类中所有同名成员函数（重载）
	//若要访问 需要加作用域
	s.Base::func(100);
}

//主函数
int main()
{
	//test01();
	test02();
	system("pause");
	return 0;
}
```

> 总结：
>
> - 子类对象可以直接访问到子类中同名函数
> - 子类对象加作用域可以访问到父类同名成员
> - 当子类与父类拥有同名的成员函数，子类会隐藏父类中同名成员函数，加作用域可以访问到父类中同名函数



###### 4.6.6 同名静态成员处理方式



​	问题：继承中同名的静态成成员在子类对象上如何进行访问？



​	静态成员与非静态成员出现同名，处理方式一致



- 访问子类同名成员 直接访问即可
- 访问父类同名成员 需要加作用域



```cpp
//继承中的同名静态成员处理方式
class Base
{
public:
	static int m_A;

	static void func()
	{
		cout << "Base static void func 调用" << endl;
	}
	static void func(int a)
	{
		cout << "Base static void func(int a) 调用" << endl;
	}
};
int Base::m_A = 100;
class Son :public Base
{
public:
	static int m_A;

	static void func()
	{
		cout << "Son static void func 调用" << endl;
	}
};
int Son::m_A = 200;
//同名静态成员属性
void test01()
{
	//1、通过对象访问
	Son s;
	cout << "通过对象访问：" << endl;
	cout << "Son m_A = " << s.m_A << endl;
	cout << "Base m_A = " << s.Base::m_A << endl;
	//2、通过类名访问数据
	cout << "通过类名访问：" << endl;
	cout << "Son m_A = " << Son::m_A << endl;
	//第一个::代表通过类名的方式访问 第二个::代表访问父类作用域下
	cout << "Base m_A = " << Son::Base::m_A << endl;
}
//同名静态成员函数
void test02()
{
	//1、通过对象访问
	Son s;
	cout << "通过对象访问：" << endl;
	s.func();
	s.Base::func();
	//2、通过类名访问
	cout << "通过类名访问：" << endl;
	Son::func();
	Son::Base::func();
	//子类出现和父类同名静态成员函数，也会隐藏父类中所有同名成员函数
	//如果想访问父类中被隐藏的同名成员，需要加作用域
	Son::Base::func(100);
}

//主函数
int main()
{
	//test01();
	test02();
	system("pause");
	return 0;
}
```



###### 4.6.7 多继承语法

​	C++允许**一个类继承多个类**

​	

​	**语法：**`class 子类 : 继承方式 父类1 , 继承方式 父类2...`



​	多继承可能会引发父类中由同名成员出现，需要加作用域区分



​	**C++实际开发中不建议用多继承**



```cpp
//多继承语法
class Base1
{
public:
	int m_A;
	Base1()
	{
		m_A = 100;
	}
};
class Base2
{
public:
	int m_A;
	Base2()
	{
		m_A = 200;
	}
};
//子类 需要继承Base1和Base2
class Son :public Base1, public Base2
{
public:
	int m_C;
	int m_D;
	Son()
	{
		m_C = 300;
		m_D = 400;
	}
};

void test01()
{
	Son s;
	cout << "size of Son = " << sizeof(Son) << endl;
	//当父类中出现同名成员，需要加作用域区分
	cout << "Base1 m_A = " << s.Base1::m_A << endl;
	cout << "Base2 m_A = " << s.Base2::m_A << endl;
}
//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



###### 4.6.8 菱形继承

​	**菱形继承概念：**

​		两个派生类继承同一个基类

​		又有某个类同时继承这两个派生类

​		这种继承被称为菱形继承，或者钻石继承



​	**菱形继承问题：**

1. 羊继承了动物的数据，驼同样继承了动物的数据，当羊驼使用数据时，就会产生二义性
2. 羊驼继承自动物的数据继承了两份，其实我们应该清楚，这份数据我们只需要一份就可以



```cpp
//动物类
class Animal
{
public:
	int m_Age;
};
//利用虚继承 解决菱形继承的问题
//在继承之前 加上关键字 virtual 变为虚继承
// Animal类称之为 虚基类
//羊类
class Sheep :virtual public Animal
{

};
//驼类
class Tuo :virtual public Animal
{

};
//羊驼类
class SheepTuo :public Sheep, public Tuo
{

};

void test01()
{
	SheepTuo st;
	st.Sheep::m_Age = 18;
	st.Tuo::m_Age = 28;
	//当菱形继承，两个父类拥有相同数据，需要加以作用域区分
	cout << "st.Sheep::m_Age = " << st.Sheep::m_Age << endl;
	cout << "st.Tuo::m_Age = " << st.Tuo::m_Age << endl;
	cout << st.m_Age << endl;
	//这份数据我们知道 只有一份就可以 菱形继承导致数据有两份 资源浪费
}

int main()
{
	test01();
	system("pause");
	return 0;
}
```

总结：

- 菱形继承带来的主要问题是子类继承两份相同的数据，导致资源浪费以及毫无意义
- 利用虚继承可以解决菱形继承问题

> 虚继承简述：
>
> 1. 两个类在继承同一个基类时，在继承方法前加上`virtual`关键字
> 2. 这时第三级类继承的就是两个第二级类的`vbptr`，即`virtual base pointer`，虚基类指针
> 3. 虚基类指针指向第二级的两个类，两个类具有不同的偏移量，相同的数据将会合并



##### 4.7 多态

###### 4.7.1 多态的基本概念

​	**多态是C++面向对象三大特性之一**

​	

​	多态分为两类：

- 静态多态：函数重载 和 运算符重载属于静态多态，复用函数名
- 动态多态：派生类和虚函数实现运行时多态



​	静态多态和动态多态的区别：

- 静态多态的函数地址早绑定 - 编译阶段确定函数地址
- 动态多态的函数地址晚绑定 - 运行阶段确定函数地址



```cpp
//多态
//动物类
class Animal
{
public:
	//虚函数
	virtual void speak()
	{
		cout << "动物在说话" << endl;
	}
};
//猫类
class Cat :public Animal
{
public:
	void speak()
	{
		cout << "小猫在说话" << endl;
	}
};
//狗类
class Dog :public Animal
{
public:
	void speak()
	{
		cout << "小狗在说话" << endl;
	}
};

//执行说话的函数
//地址早绑定 在编译阶段确定函数地址
//如果想执行让猫说话 那么这个函数地址就不能提前绑定 需要在运行阶段进行绑定 地址晚绑定

//动态多态满足条件
//1、有继承关系
//2、子类重写父类的虚函数

//动态多态的使用
//父类的指针或者引用 指向子类对象

void doSpeak(Animal& animal)
{
	animal.speak();
}

void test01()
{
	Cat cat;
	doSpeak(cat);

	Dog dog;
	doSpeak(dog);
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```

总结：

**多态满足条件：**

- 有继承关系
- 子类重写父类中的虚函数

**多态使用条件：**

- 父类指针或者引用指向子类对象

重写：函数返回值类型 函数名 参数列表 完全一致称为重写



###### 4.7.2 多态案例一-计算器类

​	案例描述：

​		分别利用普通写法和多态技术，设计实现两个操作数进行运算的计算机类



​	多态的优点：

- 代码组织结构清晰
- 可读性强
- 利于前期和后期的扩展以及维护



```cpp
//分别利用普通写法和多态技术实现计算器

//普通写法
class Calculator
{
public:
	int m_Num1;
	int m_Num2;
	int getResult(string oper)
	{
		if (oper == "+")
		{
			return m_Num1 + m_Num2;
		}
		else if (oper == "-")
		{
			return m_Num1 - m_Num2;
		}
		else if (oper == "*")
		{
			return m_Num1 * m_Num2;
		}
		//如果想扩展新的功能 需要修改源码
		//在真实开发中 提倡 开闭原则
		//开闭原则：对扩展进行开放，对修改进行关闭
	}
};

void test01()
{
	Calculator c;
	c.m_Num1 = 10;
	c.m_Num2 = 10;

	cout << c.m_Num1 << " + " << c.m_Num2 << " = " << c.getResult("+") << endl;
	cout << c.m_Num1 << " - " << c.m_Num2 << " = " << c.getResult("-") << endl;
	cout << c.m_Num1 << " * " << c.m_Num2 << " = " << c.getResult("*") << endl;
}

//利用多态实现计算器
//多态好处：
// 1、组织结构清晰
// 2、可读性强
// 3、对于前期和后期扩展以及维护性高

//实现计算器抽象类
class AbstractCalculator
{
public:
	int m_Num1;
	int m_Num2;
	virtual int getResult()
	{
		return 0;
	}
};
//加法计算器类
class AddCalculator :public AbstractCalculator
{
public:
	int getResult()
	{
		return m_Num1 + m_Num2;
	}
};
//减法计算器类
class SubCalculator :public AbstractCalculator
{
public:
	int getResult()
	{
		return m_Num1 - m_Num2;
	}
};
//乘法计算器类
class MulCalculator :public AbstractCalculator
{
public:
	int getResult()
	{
		return m_Num1 * m_Num2;
	}
};

void test02()
{
	//多态使用条件
	//父类指针或者引用指向子类对象
	//利用指针
	//加法运算
	AbstractCalculator* abc = new AddCalculator;
	abc->m_Num1 = 10;
	abc->m_Num2 = 10;
	cout << abc->m_Num1 << " + " << abc->m_Num2 << " = " << abc->getResult() << endl;
	//用完后记得销毁
	delete abc;
	//减法运算
	//堆区内存释放了 但是父类指针类型没有变
	abc = new SubCalculator;
	abc->m_Num1 = 10;
	abc->m_Num2 = 10;
	cout << abc->m_Num1 << " - " << abc->m_Num2 << " = " << abc->getResult() << endl;
	delete abc;
	//乘法运算
	abc = new MulCalculator;
	abc->m_Num1 = 10;
	abc->m_Num2 = 10;
	cout << abc->m_Num1 << " * " << abc->m_Num2 << " = " << abc->getResult() << endl;
	delete abc;
}
//利用引用
int doCal(AbstractCalculator& a)
{
	return a.getResult();
}
void test03()
{
	AddCalculator a;
	a.m_Num1 = 20;
	a.m_Num2 = 20;
	cout << doCal(a) << endl;
}

//主函数
int main()
{
	//test01();
	//test02();
	test03();
	system("pause");
	return 0;
}
```



###### 4.7.3 纯虚函数和抽象类

​	在多态中，通常父类中虚函数的实现是毫无意义的，主要都是调用子类重写的内容



​	因此可以将虚函数改为**纯虚函数**



​	纯虚函数语法：`virtual 返回值类型 函数名 (参数列表) = 0;`



​	当类中有了纯虚函数，这个类也称为**抽象类**



​	**抽象类特点：**

- 无法实例化对象
- 子类必须重写抽象类中的纯虚函数，否则也属于抽象类 



```cpp
//纯虚函数和抽象类
class Base
{
public:
	//纯虚函数
	//只要有一个纯虚函数 这个类称为抽象类
	//抽象类特点：
	//1、无法实例化对象
	//2、抽象类的子类 必须要重写父类中的纯虚函数 否则也属于抽象类
	virtual void func() = 0;
};
class Son :public Base
{
public:
	virtual void func()
	{
		cout << "func函数调用" << endl;
	}
};

void test01()
{
	//Base b;  //抽象类无法实例化对象
	//new Base;//抽象类无法实例化对象
	//Son s;//子类必须要重写父类中的纯虚函数，否则无法实例化对象
	Base* base = new Son;
	base->func();
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



###### 4.7.4 多态案例二-制作饮品

​	**案例描述：**

​		制作饮品大大致流程为：煮水 - 冲泡 - 倒入杯中 - 加入辅料



​	利用多态技术实现本案例，提供抽象制作饮品基类，提供子类制作咖啡和茶叶



```cpp
//多态案例 制作饮品
class AbstractDrink
{
public:
	//煮水
	virtual void Boil() = 0;
	//冲泡
	virtual void Brew() = 0;
	//倒入杯中
	virtual void PourInCup() = 0;
	//加入辅料
	virtual void PutSomeThing() = 0;
	//制作饮品
	void makeDrink()
	{
		Boil();
		Brew();
		PourInCup();
		PutSomeThing();
	}
};
//制作咖啡
class Coffee :public AbstractDrink
{
public:
	//煮水
	virtual void Boil()
	{
		cout << "煮矿泉水" << endl;
	}
	//冲泡
	virtual void Brew()
	{
		cout << "冲泡咖啡" << endl;
	}
	//倒入杯中
	virtual void PourInCup()
	{
		cout << "倒入杯中" << endl;
	}
	//加入辅料
	virtual void PutSomeThing()
	{
		cout << "加入牛奶和糖" << endl;
	}
};
//制作茶
class Tea :public AbstractDrink
{
public:
	//煮水
	virtual void Boil()
	{
		cout << "煮山泉水" << endl;
	}
	//冲泡
	virtual void Brew()
	{
		cout << "冲泡茶叶" << endl;
	}
	//倒入杯中
	virtual void PourInCup()
	{
		cout << "倒入盏中" << endl;
	}
	//加入辅料
	virtual void PutSomeThing()
	{
		cout << "加入枸杞" << endl;
	}
};
//制作饮品
void doWork(AbstractDrink* drink)//AbstractDrink* drink = new Coffee
{
	drink->makeDrink();
	delete drink;//释放
}

void test01()
{
	doWork(new Coffee);
	cout << "----------------" << endl;
	doWork(new Tea);
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



###### 4.7.5 虚析构和纯虚析构

​	多态使用时，如果子类中有属性开辟到堆区，那么父类指针在释放时无法调用到子类的析构代码



​	解决方式：将父类中的析构函数改为**虚析构**或者**纯虚析构**



​	虚析构和纯虚析构共性：

- 可以解决父类指针释放子类对象
- 都需要具体的函数实现

​	虚析构和纯虚析构区别：

- 如果是纯虚析构，该类属于抽象类，无法实例化对象



​	虚析构语法：

​	`virtual ~类名(){}`

​	纯虚析构语法：

​	`virtual ~类名() = 0;`

​	`类名::类名(){}`



```cpp
//虚析构和纯虚析构
class Animal
{
public:
	//纯虚函数
	virtual void speak() = 0;
	Animal()
	{
		cout << "Animal构造函数调用" << endl;
	}
	//利用虚析构可以解决 父类指针释放子类对象时不干净的问题
	/*virtual ~Animal()
	{
		cout << "Animal析构函数调用" << endl;
	}*/

	//纯虚析构 需要声明也需要实现
	//有了纯虚析构 之后 这个类也属于抽象类 无法实例化对象
	virtual ~Animal() = 0;
};
Animal::~Animal()
{
	cout << "Animal纯析构函数调用" << endl;
 }

class Cat :public Animal
{
public:
	string* m_Name;
	virtual void speak()
	{
		cout << *m_Name << "小猫在说话" << endl;
	}
	Cat(string name)
	{
		cout << "Cat构造函数调用" << endl;
		m_Name = new string(name);
	}
	~Cat()
	{
		if (m_Name != NULL)
		{
			cout << "Cat析构函数调用" << endl;
			delete m_Name;
			m_Name = NULL;
		}
	}
};

void test01()
{
	Animal* animal = new Cat("Tom");
	animal->speak();
	//父类指针在析构函数时候 不会调用子类中析构函数，导致子类如果有堆区属性，出现内存泄漏
	delete animal;
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```

总结：

1. 虚析构或纯虚析构就是用来解决通过父类指针释放子类对象
2. 如果子类中没有堆区数据，可以不写为虚析构或纯虚析构
3. 拥有纯虚析构函数的类也属于抽象类



###### 4.7.6 多态案例三-电脑组装

​	案例描述：

​		电脑主要组成部件为CPU（用于计算），显卡（用于显示），内存条（用于存储）

​		将每个零件封装出抽象基类，并且提供不同的厂商生产不同的零件，例如Intel厂商和Lenovo

​		创建电脑类提供让电脑工作的函数，并且调用每个零件工作的接口

​		测试时组装三台不同的电脑进行工作



```cpp
//多态案例三 电脑组装

//抽象的CPU类
class CPU
{
public:
	virtual void calculate() = 0;
};
//抽象的显卡类
class GPU
{
public:
	virtual void show() = 0;
};
//抽象的内存条类
class RAM
{
public:
	virtual void storage() = 0;
};
//电脑类
class Computer
{
public:
	//构造函数中传入三个零件指针
	Computer(CPU* cpu,GPU* gpu,RAM* ram)
	{
		m_CPU = cpu;
		m_GPU = gpu;
		m_RAM = ram;
	}
	//工作函数
	void doWork()
	{
		m_CPU->calculate();
		m_GPU->show();
		m_RAM->storage();
	}
	//提供析构函数 释放3个电脑零件
	~Computer()
	{
		cout << "computer析构函数调用" << endl;
		if (m_CPU != NULL)
		{
			delete m_CPU;
			m_CPU = NULL;
		}
		if (m_GPU != NULL)
		{
			delete m_GPU;
			m_GPU = NULL;
		}
		if (m_RAM != NULL)
		{
			delete m_RAM;
			m_RAM = NULL;
		}
	}
private:
	CPU* m_CPU;
	GPU* m_GPU;
	RAM* m_RAM;
};

//具体零件厂商
//Lenovo
class LenovoCPU :public CPU
{
	void calculate()
	{
		cout << "Lenovo的CPU开始计算" << endl;
	}
};
class LenovoGPU :public GPU
{
	void show()
	{
		cout << "Lenovo的GPU开始显示" << endl;
	}
};
class LenovoRAM :public RAM
{
	void storage()
	{
		cout << "Lenovo的RAM开始存储" << endl;
	}
};
//Intel
class IntelCPU :public CPU
{
	void calculate()
	{
		cout << "Intel的CPU开始计算" << endl;
	}
};
class IntelGPU :public GPU
{
	void show()
	{
		cout << "Intel的GPU开始显示" << endl;
	}
};
class IntelRAM :public RAM
{
	void storage()
	{
		cout << "Intel的RAM开始存储" << endl;
	}
};

//测试案例01
void test01()
{
	CPU* cpu = new IntelCPU;
	GPU* gpu = new IntelGPU;
	RAM* ram = new IntelRAM;
	Computer* com = new Computer(cpu, gpu, ram);
	com->doWork();
	delete com;
}
//测试案例02
void test02()
{
	CPU* cpu = new LenovoCPU;
	GPU* gpu = new LenovoGPU;
	RAM* ram = new LenovoRAM;
	Computer* com = new Computer(cpu, gpu, ram);
	com->doWork();
	delete com;
}
//测试案例03
void test03()
{
	CPU* cpu = new IntelCPU;
	GPU* gpu = new IntelGPU;
	RAM* ram = new LenovoRAM;
	Computer* com = new Computer(cpu, gpu, ram);
	com->doWork();
	delete com;
}

//主函数
int main()
{
	test01();
	cout << "-------------" << endl;
	test02();
	cout << "-------------" << endl;
	test03();
	system("pause");
	return 0;
}
```

### 第五章 文件操作

​	程序运行时产生的数据都属于临时数据，程序一旦运行结束都会被释放

​	通过**文件可以将数据持久化**

​	C++中对文件操作需要包含头文件`< fstream >`



​	文件类型分为两种：

- 文本文件：文件以文本的**ASCII码**形式储存在计算机中
- 二进制文件：文件以文本的**二进制形式**存储在计算机就中，用户一般不能直接读懂他们

​	

​	操作文件的三大类：

- `ofstream`：写操作
- `ifstream`：读操作
- `fstream `  ：读写操作



##### 5.1 文本文件

###### 5.1.1 写文件

​	写文件步骤如下：
​	1、包含头文件

​		`#include <fstream>`

​	2、创建流对象

​		`ofstream ofs;`

​	3、打开文件

​		`ofs.open("文件路径",打开方式);`

​	4、写数据

​		`ofs << "写入的数据";`

​	5、关闭文件

​		`ofs.close();`



文件打开方式：

| **打开方式**  | **解释**                     |
| :------------ | ---------------------------- |
| `ios::in`     | 为读文件而打开文件           |
| `ios::out`    | 为写文件而打开文件           |
| `ios::ate`    | 初始位置：文件尾             |
| `ios::app`    | 追加方式写文件               |
| `ios::trunc`  | 如果文件存在，先删除，再创建 |
| `ios::binary` | 二进制方式                   |



**注意：**文件打开方式可以配合使用，利用`|`操作符

**例如：**用二进制方式写文件`ios::binary | ios::out`



```cpp
#include <iostream>
#include <fstream>
using namespace std;

//文本文件 写文件
void test01()
{
	//1、包含头文件 fstream

	//2、创建流对象
	ofstream ofs;
	//3、指定打开方式
	ofs.open("test.txt", ios::out);
	//4、写内容
	ofs << "姓名：张三" << endl;
	ofs << "性别：男" << endl;
	ofs << "年龄：18" << endl;
	//5、关闭文件
	ofs.close();
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```

总结：

- 文件操作必须包含头文件`fstream`
- 读文件可以利用`ofstream`或者`fstream`
- 打开文件时需要指定操作文件的路径，以及打开方式
- 利用`<<`可以向文件中写数据
- 操作完毕，要关闭文件



###### 5.1.2 读文件

​	读文件与写文件步骤相似，但是读取方式比较多



​	读文件步骤如下：

​		1、包含头文件

​			`#include <fstream>`

​		2、创建流对象

​			`ifstream ifs;`

​		3、打开文件并判断文件是否成功打开

​			`ifs.open("文件路径",打开方式);`

​		4、读数据

​			四种方式读取

​		5、关闭文件

​			`ifs.close();`



```cpp
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

//文本文件 读文件
void test01()
{
	ifstream ifs;
	ifs.open("test.txt", ios::in);
	if (!ifs.is_open())
	{
		cout << "文件打开失败" << endl;
		return;
	}
	//4、读数据
	//第一种
	char buf1[1024] = { 0 };
	while (ifs >> buf1)
	{
		cout << buf1 << endl;
		cout << "*" << endl;
	}
	ifs.close();
	//第二种
	ifs.open("test.txt", ios::in);
	char buf2[1024] = { 0 };
	while (ifs.getline(buf2, sizeof(buf2)))
	{
		cout << buf2 << endl;
		cout << "!" << endl;
	}
	ifs.close();
	//第三种
	ifs.open("test.txt", ios::in);
	string buf3;
	while (getline(ifs, buf3))
	{
		cout << buf3 << endl;
	}
	ifs.close();
	//第四种
	ifs.open("test.txt", ios::in);
	char c;
	while ((c = ifs.get()) != EOF)//EOF: end of file
	{
		cout << c;
	}
	ifs.close();
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



##### 5.2 二进制文件

​	以二进制的方式对文件进行读写操作

​	打开方式为`ios::binary`



###### 5.2.1 写文件

​	二进制方式写文件主要利用流对象调用成员函数`write`

​	函数原型：`ostream& write(const char* buffer,int len);`

​	参数解释：字符指针`buffer`指向内存中一段存储空间。`len`是读写的字节数



```cpp
#include <iostream>
#include <fstream>
using namespace std;

//二进制文件 写文件
class Person
{
public:
	char m_Name[64];
	int m_Age;
};

void test01()
{
	ofstream ofs;
	ofs.open("person.txt", ios::binary | ios::out);
	Person p = { "张三",18 };
	ofs.write((const char*)&p, sizeof(Person));
	ofs.close();
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



###### 5.2.2 读文件

​	二进制方式读文件主要利用流对象调用成员函数`read`

​	函数原型：`istream& read(char* buffer,int len);`

​	参数解释：字符指针`buffer`指向内存中一段存储空间。`len`是读写的字节数



```cpp
#include <iostream>
#include <fstream>
using namespace std;

//二进制文件 读文件
class Person
{
public:
	char m_Name[64];
	int m_Age;
};

void test01()
{
	ifstream ifs;
	ifs.open("person.txt", ios::binary | ios::in);
	if (!ifs.is_open())
	{
		cout << "文件打开失败" << endl;
		return;
	}
	Person p;
	ifs.read((char*)&p, sizeof(Person));
	cout << "姓名：" << p.m_Name << " 年龄：" << p.m_Age << endl;
	ifs.close();
}

//主函数
int main()
{
	test01();
	system("pause");
	return 0;
}
```



# C++提高编程

- 本阶段主要针对C++泛型编程和STL记住做详细讲解，探讨C++更深层次的使用

## 第一章 模板

### 1.1 模板的概念

​	模板就是建立通用的**模具**，大大**提高复用性**

模板的特点：

- 模板不可以直接使用，他只是一个框架
- 模板的通用并不是万能的



### 1.2 函数模板

- C++另一种编程思想称为泛型编程，主要利用的技术就是模板
- C++提供两种模板机制：**函数模板**和**类模板**

#### 1.2.1 函数模板语法

函数模板作用：

​	建立一个通用函数，其函数返回值类型和形参类型可以不具体制定，用一个**虚拟的类型**来代表

**语法：**

```cpp
template<typename T>
函数声明或定义
```

**解释：**

> template --- 声明创建模板
>
> typename --- 表面其后面的符号是一种数据类型，可以用class代替
>
> T --- 通用的数据类型，名称可以替换，通常为大写字母

**示例：**

```cpp
#include <iostream>
using namespace std;

//函数模板

//交换两个整型
void swapInt(int& a, int& b) {
	int temp = a;
	a = b;
	b = temp;
}

//交换两个浮点型
void swapDouble(double& a, double& b) {
	double temp = a;
	a = b;
	b = temp;
}

//函数模板
 //声明一个模板，告诉编译器后面代码紧跟着的T不要报错，是一个通用的数据类型
template<typename T>
void mySwap(T& a, T& b) {
	T temp = a;
	a = b;
	b = temp;
}

//测试
void test01() {
	int a = 10;
	int b = 20;
	swapInt(a, b);
	cout << "a = " << a << endl;
	cout << "b = " << b << endl;

	double c = 1.1;
	double d = 2.2;
	swapDouble(c, d);
	cout << "c = " << c << endl;
	cout << "d = " << d << endl;

	//利用函数模板交换
	//两种方式使用函数模板
	//1、自动类型推导
	mySwap(a, b);
	cout << "a = " << a << endl;
	cout << "b = " << b << endl;
	//2、显示指定类型
	mySwap<double>(c, d);
	cout << "c = " << c << endl;
	cout << "d = " << d << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

总结：

- 函数模板利用关键字 `template`
- 使用函数模板由两种方式：自动类型推导、显示指定类型
- 模板的是为了提高复用性，将类型参数化

#### 1.2.2 函数模板注意事项

注意事项：

- 自动类型推导，必须推导出一致的数据类型 T，才可以使用
- 模板必须要确定出T的数据类型，才可以使用

示例：

```cpp
#include<iostream>
using namespace std;

//函数模板注意事项

template<class T>
void mySwap(T& a, T& b) {
	T temp = a;
	a = b;
	b = temp;
}

//1、自动类型推导必须推导出一致的数据类型T才可以使用模板
void test01() {
	int a = 10;
	int b = 20;
	char c = 'c';
	mySwap(a, b);//正确
	//mySwap(a, c);//错误，推导出不一致的T类型
	cout << "a = " << a << endl;
	cout << "b = " << b << endl;
}

//2、模板必须要确定出T的数据类型，才可以使用
template<class T>
void func() {
	cout << "func调用" << endl;
}
void test02() {
	//func();没有确定T的数据类型
	func<int>();//正确
}

//主函数
int main() {
	test02();
	system("pause");
	return 0;
}
```



#### 1.2.3 函数模板案例

案例描述：

- 利用函数模板封装一个排序的函数，可以对不同的数据类型数组进行排序
- 排序规则从大到小，排序算法为选择排序
- 分别利用char数组和int数组进行测试

示例：

```cpp
#include <iostream>
using namespace std;

//实现通用 对数组进行排序的函数
//从大到小
//char 和 int

//交换函数模板
template<class T>
void mySwap(T& a, T& b) {
	T temp = a;
	a = b;
	b = temp;
}

//选择排序算法
template<class T>
void mySort(T arr[], int len) {
	for (int i = 0; i < len; i++) {
		int max = i;//认定最大值下标
		for (int j = i + 1; j < len; j++) {
			if (arr[max] < arr[j]) {
				max = j;//更新最大值
			}
		}
		if (max != i) {
			//交换max和i下标元素
			mySwap(arr[max], arr[i]);
		}
	}
}

//提供打印数组模板
template<class T>
void PrintArr(T arr[],int len) {
	for (int i = 0; i < len; i++) {
		cout << arr[i] << " " ;
	}
	cout << endl;
}

void test01() {
	//测试char数组
	char charArr[] = "badcfe";
	int len = sizeof(charArr) / sizeof(char);
	mySort(charArr, len);
	PrintArr(charArr, len);
}

void test02() {
	//测试int数组
	int intArr[] = { 3,6,5,2,4,1,8,7,9 };
	int len = sizeof(intArr) / sizeof(int);
	mySort(intArr, len);
	PrintArr(intArr, len);
}

//主函数
int main() {
	test01();
	test02();
	system("pause");
	return 0;
}
```



#### 1.2.4 普通函数与函数模板的区别

区别：

- 普通函数调用时可以发生自动类型转换（隐式类型转换）
- 函数模板调用时，如果利用自动类型推导，不会发生隐式类型转换
- 如果利用显示指定类型的方式，可以发生隐式类型转换

示例：

```cpp
#include<iostream>
using namespace std;

//普通函数与函数模板的区别

//普通函数
int myAdd01(int a, int b) {
	return a + b;
}

//函数模板
template<class T>
T myAdd02(T a, T b) {
	return a + b;
}


void test01() {
	int a = 10;
	int b = 20;
	char c = 'c';//c => 99,ASCII
	cout << myAdd01(a, c) << endl;
	//自动类型推导，不会发生隐式类型转换
	//cout << myAdd02(a, c) << endl;//无法推导出一致的数据类型
	//显式指定类型，会发生隐式类型转换
	cout << myAdd02<int>(a, c) << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 1.2.5 普通函数与函数模板的调用规则

​	调用规则如下：

1. 如果函数模板和普通函数都可以实现，优先调用普通函数
2. 可以通过空模板参数列表来强制调用函数模板
3. 函数模板也可以发生重载
4. 如果函数模板可以产生更好的匹配，优先调用函数模板

```cpp
//普通函数与函数模板调用规则
void myPrint(int a, int b) {
	cout << "调用的普通函数" << endl;
}

template<class T>
void myPrint(T a, T b) {
	cout << "调用的模板" << endl;
}

template<class T>
void myPrint(T a, T b,T c) {
	cout << "调用重载的模板" << endl;
}

void test01() {
	//如果函数模板和普通函数都可以调用，优先调用普通函数
	myPrint(2, 3);
	//通过空模板参数列表，强制调用函数模板
	myPrint<>(2, 3);
	//函数模板也可以重载
	myPrint(2, 3, 4);
	//如果能产生更好的匹配，优先调用模板
	myPrint(2.2, 3.3);
	
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：既然提供了函数模板，最好就不要提供普通函数，否则容易出现二义性



#### 1.2.6 模板的局限性

局限性：

- 模板的通用性并不是万能的

例如：

```cpp
template<class T>
void f(T a,T b){
    a = b;
}
```

​	在上述代码中提供的赋值操作，如果传入的a和b是一个数组，就无法实现了



再例如：

```cpp
template<class T>
void f(T a,T b){
    if(a > b){
        ...
    }
}
```

​	在上述代码中，如果T的数据类型传入的是像Person这样的自定义数据类型，也无法正常运行

​	因此C++为了解决这种问题，提供模板的重载，可以为这些**特定的类型**提供**具体化的模板**



示例：

```cpp
//模板的局限性
//模板并不是万能的，有些特定数据类型，需要用具体化方式做特殊实现

class Person {
public:
	string name;
	int age;
	Person(string name, int age) {
		this->name = name;
		this->age = age;
	}
};

//对比两个数据是否相等
template<class T>
bool myCompare(T& a, T& b) {
	if (a == b) {
		return true;
	}
	else {
		return false;
	}
}

//利用具体化Person的版本实现代码，具体有限调用
template<> bool myCompare(Person& p1, Person& p2) {
	if (p1.name == p2.name && p1.age == p2.age) {
		return true;
	}
	else {
		return false;
	}
}

void test01() {
	int a = 10;
	int b = 20;
	bool ret = myCompare(a, b);
	if (ret) {
		cout << "a == b" << endl;
	}
	else {
		cout << "a != b" << endl;
	}
}

void test02() {
	Person p1("Tom", 20);
	Person p2("Tom", 20);

	bool ret = myCompare(p1, p2);
	if (ret) {
		cout << "p1 == p2" << endl;
	}
	else {
		cout << "p1 != p2" << endl;
	}
}

//主函数
int main() {
	test01();
	test02();
	system("pause");
	return 0;
}
```

> 总结：
>
> - 利用具体化的模板，可以解决自定义类型的通用化
> - 学习模板并不是为了写模板，而是在STL 能够运用系统提供的模板



### 1.3 类模板

#### 1.3.1 类模板语法

​	类模板作用

- 建立一个通用类，类中的成员 数据类型可以不具体指定，用一个虚拟的类型来代表。

语法：

```cpp
template<typename T>
类
```

解释：

> template --- 声明创建模板
>
> typename --- 表面其后面的符号是一种数据类型，可以用class 代替
>
> T --- 通用的数据类型，名称可以替换，通常为大写字母

```cpp
//类模板
template<class NameType, class AgeType>
class Person {
public:
	NameType m_name;
	AgeType m_age;

	Person(NameType name, AgeType age) {
		this->m_name = name;
		this->m_age = age;
	}

	void showPerson() {
		cout << "name: " << m_name << endl;
		cout << "age = " << m_age << endl;
	}
};

void test01() {
	Person<string, int>p1("孙悟空", 999);
	p1.showPerson();
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：类模板和函数模板语法非常相似，在声明模板template后面加类，此类称为类模板



#### 1.3.2 类模板与函数模板的区别

​	区别：

- 类模板没有自动类型推导的使用方式
- 类模板在模板参数列表中可以有默认参数

```cpp
//类模板与函数模板的区别

template<class NameType,class AgeType = int>
class Person {
public:
	NameType m_name;
	AgeType m_age;
	Person(NameType name, AgeType age) {
		this->m_name = name;
		this->m_age = age;
	}
	void showPerson() {
		cout << "name: " << m_name <<endl;
		cout << "sge = " << m_age <<endl;
	}
};

//类模板没有自动类型推导
void test01() {
	//Person p1("孙悟空", 999);//错误，无法用自动类型推导
	Person<string, int> p1("孙悟空", 999);
	p1.showPerson();
}

//类模板在模板参数列表中可以有默认参数
void test02() {
	Person<string> p1("猪八戒", 9999);
	p1.showPerson();
}

//主函数
int main() {
	test01();
	test02();
	system("pause");
	return 0;
}
```



#### 1.3.3 类模板中成员函数创建时机

类模板中成员函数和普通类中成员函数创建时机是有区别的：

- 普通类中的成员函数一开始就可以创建
- 类模板中的成员函数在调用时才创建

```cpp
//类模板中成员函数的调用时机

class Person1 {
public:
	void showPerson1() {
		cout << "Person1 show!" << endl;
	}
};

class Person2 {
public:
	void showPerson2() {
		cout << "Person2 show!" << endl;
	}
};

template<class T>
class MyClass {
public:

	T obj;
	//类模板中的成员函数，并不是一开始就创建的，而是在模板调用时再生成
	void func1() {
		obj.showPerson1();
	}

	void func2() {
		obj.showPerson2();
	}
};

void test01() {
	MyClass<Person1>m;
	m.func1();
	//m.func2();//编译会出错，说明函数调用才会去创建成员函数
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：类模板中的成员函数并不是一开始就创建的，而是在调用时采取创建



#### 1.3.4 类模板对象做函数参数

学习目标

- 类模板实例化出的对象，像函数传参的方式

一共有三种传入方式：

1. 指定传入的类型  --- 直接显示对象的数据类型
2. 参数模板化         --- 将对象中的参数变为模板进行传递
3. 整个类模板化     --- 将这个对象类型 模板化进行传递

```cpp
//类模板对象做函数参数

template<class T1,class T2>
class Person {
public:

	T1 m_name;
	T2 m_age;

	Person(T1 name, T2 age) {
		this->m_name = name;
		this->m_age = age;
	}

	void showPerson() {
		cout << "name: " << m_name << endl;
		cout << "age = " << m_age << endl;
	}
};

//1、指定传入类型
void printPerson1(Person<string, int> &p) {
	p.showPerson();
}

void test01() {
	Person<string, int>p1("孙悟空", 999);
	printPerson1(p1);
}

//2、模板参数化
template<class T1,class T2>
void printPerson2(Person<T1, T2> &p) {
	p.showPerson();
	cout << "T1的类型为：" << typeid(T1).name() << endl;
	cout << "T2的类型为：" << typeid(T2).name() << endl;
}

void test02() {
	Person<string, int>p2("猪八戒", 9999);
	printPerson2(p2);
}

//3、将整个类模板化
template<class T>
void printPerson3(T& p) {
	p.showPerson();
	cout << "T的数据类型：" << typeid(T).name() << endl;
}

void test03() {
	Person<string, int>p("唐僧", 30);
	printPerson3(p);
}

//主函数
int main() {
	test01();
	test02();
	test03();
	system("pause");
	return 0;
}
```

> 总结：
>
> - 通过类模板创建的对象，可以有三种方式向函数中进行传参
> - 使用比较广泛的是第一种：指定传入的类型



#### 1.3.5 类模板与继承

当类模板

碰到继承时，需要注意以下几点：

- 当子类继承的父类是一个类模板时，子类再声明的时候，要指定出父类中T的类型
- 如果不指定，编译器无法给子类分配内存
- 如果想灵活指出父类中T的类型，子类也需变为类模板

```cpp
//类模板与继承

template<class T>
class Base {
	T m;
};

//class Son :public Base {}; //错误，必须要直到父类中的T类型，才能继承给子类
class Son :public Base<int> {

};

void test01() {
	Son s1;
}

//如果想灵活指定父类中T类型，子类也需要变类模板
template<class T1,class T2>
class Son2 :public Base<T2> {
public:
	T1 obj;

	Son2() {
		cout << "T1类型为：" << typeid(T1).name() << endl;
		cout << "T2类型为：" << typeid(T2).name() << endl;
	}
};

void test02() {
	Son2<int, char>S2;
}

//主函数
int main() {
	test01();
	test02();
	system("pause");
	return 0;
}
```



#### 1.3.6 类模板成员函数类外实现

​	类模板中成员函数类外现实时，需要加上模板参数列表

```cpp
//类模板成员函数类外实现
template<class T1,class T2>
class Person {
public:
	T1 m_name;
	T2 m_age;

	void showPerson();
	Person(T1 name, T2 age);
};

//构造函数类外实现
template<class T1,class T2>
Person<T1, T2>::Person(T1 name, T2 age){
		this->m_name = name;
		this->m_age = age;
}

//成员函数类外实现
template<class T1,class T2>
void Person<T1, T2>::showPerson() {
	cout << "name: " << m_name << endl;
	cout << "age = " << m_age << endl;
}

void test01() {
	Person<string, int>p("张三", 20);
	p.showPerson();
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
```



#### 1.3.7 类模板分文件编写

学习目标：

- 掌握类模板成员函数分文件编写产生的问题以及解决方式

问题：

- 类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到

解决：

- 方式1：直接包含`.cpp`源文件
- 方式2：将声明和实现写道同一个文件中，并更改后缀名为`.hpp`，`hpp`是约定的名称，并不是强制

`Person.hpp`代码：

```cpp
#pragma once
#include<iostream>
using namespace std;

template<class T1, class T2>
class Person {
public:
	T1 m_name;
	T2 m_age;
	Person(T1 name, T2 age);
	void showPerson();
};

template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age) {
	this->m_age = age;
	this->m_name = name;
}

template<class T1, class T2>
void Person < T1, T2>::showPerson() {
	cout << "姓名：" << m_name << endl;
	cout << "年龄：" << m_age << endl;
}
```

`源.cpp`代码：

```cpp
#include<iostream>
#include<string>
using namespace std;

//第一种解决方式
//#include "Person.cpp"

//第二种解决方式
//将 .h 和 .cpp 中的内容写到一起，将后缀改为 .hpp
#include "Person.hpp"

void test01() {
	Person<string, int>p("Jerry", 18);
	p.showPerson();
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：主流的解决方法是第二种，将类模板成员函数写到一起，并将后缀改为`.hpp`



#### 1.3.8 类模板与友元

学习目标：

- 掌握类模板配合友元函数的类内和类外实现



全局函数类内实现 - 直接在雷内声明友元即可

全局函数类外实现 - 需要提前让编译器直到全局函数的存在

```cpp
//通过全局函数 打印Person的信息

//提前让编译器直到Person类存在
template<class T1,class T2>
class Person;

//类外实现
template<class T1, class T2>
void printPerson2(Person<T1, T2>p) {
	cout << "类外实现-姓名：" << p.m_name << endl;
	cout << "类外实现-年龄：" << p.m_age << endl;
}

template<class T1,class T2>
class Person {
	//全局函数 类内实现
	friend void printPerson(Person<T1, T2>p) {
		cout << "类内实现-姓名：" << p.m_name << endl;
		cout << "类内实现-年龄：" << p.m_age << endl;
	}

	//全局函数 类外实现
	//加空模板参数列表
	//如果全局函数 是类外实现，需要让编译器提前知道这个函数的存在
	friend void printPerson2<>(Person<T1, T2>p);

private:
	T1 m_name;
	T2 m_age;
public:
	Person(T1 name, T2 age) {
		this->m_name = name;
		this->m_age = age;
	}
};

//1、全局函数在类内实现
void test01() {
	Person<string, int>p("Tom", 18);
	printPerson(p);
}

//2、全局函数类外实现
void test02() {
	Person<string, int>p("Jerry", 20);
	printPerson2(p);
}

//主函数
int main() {
	test01();
	test02();
	system("pause");
	return 0;
}
```

> 总结：建议全局函数做类内实现，用法简单，而且编译器可以直接识别



## 第二章 STL初识

### 2.1 STL 的诞生

- 长久以来，软件届一直希望建立一种可重复利用的东西
- C++的**面向对象**和**泛型编程**思想，目的就是**复用性的提升**
- 大多数情况下，数据结构和算法都未能有一套标准，导致被迫从事大量重复工作
- 为了建立数据结构和算法的一套标准，诞生了STL



### 2.2 STL 基本概念

- STL(Standard Template Library)标准模板库
- STL 从广义上分为：**容器(container)  算法(algorithm)  迭代器(iterator)**
- **容器**和**算法**之间通过**迭代器**进行无缝连接
- STL 几乎所有的代码都采用了模板或者模板函数



### 2.3 STL 六大组件

STL 大体分为六大组件：**容器**、**算法**、**迭代器**、**仿函数**、**适配器(配接器)**、**空间配置器**

1. 容器：各种数据结构，如`vector`、`list`、`deque`、`set`、`map`等，用来存放数据
2. 算法：各种常用的算法，如`sort`、`find`、`copy`、`for_each`等
3. 迭代器：扮演了容器与算法之间的胶合剂
4. 仿函数：行为类似函数，可作为算法的某种策略
5. 适配器：一种用来修饰容器或者仿函数或迭代器接口的东西
6. 空间适配器：负责空间的配置与管理



### 2.4 STL 中容器、算法、迭代器

容器：置物之所也

STL容器就是将应用最广泛的一些数据结构实现出来

常用的数据结构：数组、链表、树、栈、队列、映射表等

这些容器分为**序列式容器**和**关联式容器**两种

- 序列式容器：强调值得排序，序列式容器中的每个元素均有固定的位置
- 二叉树结构，各元素之间没有严格得物理上的顺序



算法：问题之解法也

有限的步骤，解决逻辑或数学上的问题，这一门学科我们叫做算法(Algorithm)

算法分为：**质变算法**和**非质变算法**

质变算法：是指运算过程中会更改区间内得元素的内容。例如拷贝、替换、删除等等

非质变算法：是指运算过程中不会更改区间内的元素内容。例如查找、计数、遍历、寻找极值等等



迭代器：容器和算法之间的粘合剂

提供一种方法，使之能够依序寻访某个容器所含的各个元素，而无需暴露该容器的内部表示方式

每个容器都有自己专属的迭代器

迭代器使用非常类似于指针，初学阶段我们可以先理解迭代器为指针

迭代器种类：

| 种类           | 功能                                                     | 支持运算                                 |
| -------------- | -------------------------------------------------------- | ---------------------------------------- |
| 输入迭代器     | 对数据的只读访问                                         | 只读，支持 ++、==、!=                    |
| 输出迭代器     | 对数据的只写访问                                         | 只写，支持 ++                            |
| 前向迭代器     | 读写操作、并能向前推进迭代器                             | 读写，支持 ++、==、!=                    |
| 双向迭代器     | 读写操作，并能向前和向后操作                             | 读写，支持 ++、--                        |
| 随机访问迭代器 | 读写操作，可以以条约的方式访问任意数据，功能最强的迭代器 | 读写，支持 ++、--，[n]、-n、<、<=、>、>= |

常用的容器中迭代器种类为双向迭代器，和随机访问迭代器



### 2.5 容器算法迭代器初识

STL 中最常用的容器为`vector`，可以理解为数组



#### 2.5.1 vector 存放内置数据结构

容器：`vector`

算法：`for_each`

迭代器：`vector<int>::iterator`

示例：

```cpp
#include<iostream>
#include<vector>
#include<algorithm>//标准算法头文件
using namespace std;

void myPrint(int val) {
	cout << val << endl;
}

void test01() {
	//创建一个vector容器，数组
	vector<int> v;

	//向容器中插入数据
	v.push_back(1);
	v.push_back(2);
	v.push_back(3);

	//通过迭代器访问容器中的数据
	//起始迭代器 指向容器中第一个元素
	vector<int>::iterator itBegin = v.begin();
	//结束迭代器 只想容器中最后一个元素的下一个元素
	vector<int>::iterator itEnd = v.end();

	//第一种遍历方式
	while (itBegin != itEnd) {
		cout << *itBegin << endl;
		itBegin++;
	}

	//第二种遍历方式
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << endl;
	}

	//第三种遍历方式
	for_each(v.begin(), v.end(), myPrint);
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 2.5.2 Vector 存放自定义数据类型

学习目标：vector 中存放自定义数据类型，并打印输出

示例：

```cpp
#include<iostream>
#include<vector>
#include<string>
using namespace std;

template<class T1,class T2>
class Person {
public:
	T1 m_name;
	T2 m_age;
	Person(T1 name, T2 age) {
		this->m_name = name;
		this->m_age = age;
	}
};

void test01() {
	vector<Person<string,int>>v;

	Person<string,int> p1("aaa", 10);
	Person<string, int> p2("bbb", 20);
	Person<string, int> p3("ccc", 30);
	Person<string, int> p4("ddd", 40);

	//向容器中添加数据
	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);

	//遍历数据
	for (vector<Person<string, int>>::iterator it = v.begin(); it != v.end(); it++) {
		//用解地址方式
		cout << "姓名：" << (*it).m_name << endl;
		//用箭头方式
		cout << "年龄：" << it->m_age << endl;
	}
}

//存放自定义数据类型 指针
void test02() {
	vector<Person<string, int>*>v;

	Person<string, int> p1("aaa", 10);
	Person<string, int> p2("bbb", 20);
	Person<string, int> p3("ccc", 30);
	Person<string, int> p4("ddd", 40);

	//向容器中添加数据
	v.push_back(&p1);
	v.push_back(&p2);
	v.push_back(&p3);
	v.push_back(&p4);

	//遍历容器
	for (vector<Person<string, int>*>::iterator it = v.begin(); it != v.end(); it++) {
		cout << "姓名：" << (*it)->m_name << endl;
		cout << "年龄：" << (*it)->m_age << endl;
	}
}

//主函数
int main() {
	test01();
	test02();
	system("pause");
	return 0;
}
```



#### 2.5.3 Vector 容器嵌套容器

学习目标：容器中嵌套容器，我们将所有的数据进行遍历输出

示例：

```cpp
#include<iostream>
#include<vector>
using namespace std;

//容器嵌套容器
void test01() {
	vector<vector<int>>v;

	//创建小容器
	vector<int>v1;
	vector<int>v2;
	vector<int>v3;
	vector<int>v4;

	//向小容器添加数据
	for (int i = 0; i < 4; i++) {
		v1.push_back(i+1);
		v2.push_back(i + 2);
		v3.push_back(i + 3);
		v4.push_back(i + 4);
	}

	//将小容器插入大容器
	v.push_back(v1);
	v.push_back(v2);
	v.push_back(v3);
	v.push_back(v4);

	//通过大容器，把所有数据遍历一遍
	for (vector<vector<int>>::iterator it = v.begin(); it != v.end(); it++) {
		//(*it) --- 容器 vector<int>
		for (vector<int>::iterator vit = (*it).begin(); vit != (*it).end(); vit++) {
			cout << *vit << " ";
		}
		cout << endl;
	}
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



## 第三章 STL - 常用容器

### 3.1 string 容器

#### 3.1.1 string 基本概念

**本质：**

- string 是C++ 风格的字符串，而string 本质上是一个类

**string 和 char* 区别：**

- `char*` 是一个指针
- string 是一个类，类内封装了 `char*`，管理这个字符串，是一个`char*` 型的容器

**特点：**

​	string 类内部封装了很多成员方法

​	例如：查找find，拷贝copy，删除delete，替换replace，插入insert

​	string 查找`char*`所分配的内存，不用担心复制越界和取值越界，由类内部进行负责



#### 3.1.2 string 构造函数

构造函数原型：

- `string();`                              //创建一个空的字符串 例如：string str；

  `string(const char* s)`;        //使用字符串s初始化

- `string(const string& str);` //使用一个string 对象初始化另一个string 对象

- `string(int n,char c);`          //使用n个字符c初始化

示例：

```cpp
#include<iostream>
#include<string>
using namespace std;

//string的构造函数

void test01() {
	string s1;//默认构造

	const char* str = "hello world !";
	string s2(str);
	cout << "s2 = " << s2 << endl;

	const string& str2 = "hello world !";
	string s3(str2);
	cout << "s3 = " << s3 << endl;

	string s4(10, 'a');
	cout << "s4 = " << s4 << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.1.3 string 赋值操作

功能描述：

- 给string 字符串进行赋值

赋值的函数原型:

- `string& operator=(const char* s);`     //char*类型字符串 赋值给当前的字符串
- `string& operator=(const string& s);` //把字符串s赋给当前的字符串
- `string& operator=(char c);`                 //字符赋值给当前的字符串
- `string& assign(const char* s);`              //把字符串s付给当前的字符串
- `string& assign(const char* s,int n);`    //把字符串s的前n个字符赋给当前的字符串
- `string& assign(const string& s);`           //把字符串s赋给当前字符串
- `string& assign (int n,char c);`               //用n个字符c赋给当前字符串

示例：

```cpp
#include<iostream>
#include<string>
using namespace std;

//string赋值操作
void test01() {
	//第一种
	string str1;
	str1 = "hello world";
	cout << "str1 = " << str1 << endl;
	//第二种
	string str2;
	str2 = str1;
	cout << "str2 = " << str2 << endl;
	//第三种
	string str3;
	str3 = 'a';
	cout << "str3 = " << str3 << endl;
	//第四种
	string str4;
	str4.assign("hello C++");
	cout << "str4 = " << str4 << endl;
	//第五种
	string str5;
	str5.assign("hello C++", 5);
	cout << "str5 = " << str5 << endl;
	//第六种
	string str6;
	str6.assign(str4);
	cout << "str6 = " << str6 << endl;
	//第七种
	string str7;
	str7.assign(5, 'a');
	cout << "str7 = " << str7 << endl;
	
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.1.4 string 字符串拼接

功能实现：

- 实现在字符串末尾拼接字符串

函数原型：

- `string& operator+=(const char* str);`//重载+=操作符
- `string& operator+=(const char c);`//重载+=操作符
- `string& operator+=(const string& str);`//重载+=操作符
- `string& append(const char* s);`//把字符串s连接到当前字符串结尾
- `string& append(const char* s,int n);`//把字符串s的前n个字符连接到当前字符串结尾
- `string& append(const string& s, int pos, int n);`//字符串s中从pos开始的n个字符连接到字符串结尾

示例：

```cpp
#include<iostream>
#include<string>
using namespace std;

//string字符串拼接
void test01() {
	string str = "hello ";
	
	//第一种
	str += "world ";
	cout << str << endl;
	//第二种
	str += '!';
	cout << str << endl;
	//第三种
	string str1 = " C++ ";
	str += str1;
	cout << str << endl;
	//第四种
	str.append(" Python ");
	cout << str << endl;
	//第五种
	str.append("abcdefg", 5);
	cout << str << endl;
	//第六种
	str.append("1234567", 2, 4);
	cout << str << endl;

}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.1.5 查找和替换

功能描述：

- 查找：查找指定字符串是否存在
- 替换：在指定位置替换字符串

函数原型：

- `int find(const string& str, int pos = 0) const;`         //查找str第一次出现的位置，从pos开始找
- `int find(const char* s, int pos = 0) const; `                //查找s第一次出现的位置，从pos开始查找
- `int find(const char* s, int pos, int n) const;`           //从pos位置查找s的前n个字符第一次位置
- `int find(const char c, int pos = 0) const;`                  //查找字符c第一次出现位置
- `int rfind(const string& str, int pos = npos) const;`   //查找str最后一次位置，从pos开始查找
- `int rfind(const char* s, int pos = npos) const;`          //查找s最后一次出现位置，从pos开始查找
- `int rfind(const char* s, int pos, int n)const;`            //从pos查找s的前n个字符最后一次位置
- `int rfind(const char c, int pos = 0) const;`                 //查找字符c最后一次出现位置
- `string& replace(int pos, int n, const string& str);`   //替换从pos开始n个字符为字符串str
- `string& replace(int pos, int n, const char* s);`          //替换从pos开始的n个字符为字符串s

示例：

```cpp
#include<iostream>
#include<string>
using namespace std;

//字符串的查找和替换

//1、查找
void test01() {
	//第一种
	string str1 = "abcdefghijklmnopqrstuvwxyz";
	int pos = str1.find("uvw");
	int pos1 = str1.find("de", 8);
	cout << "pos = " << pos << endl;
	cout << "pos1 = " << pos1 << endl;
	//第二种
	pos = str1.find('d');
	cout << "pos = " << pos << endl;
	
	//find 和 rfind区别：
	//rfind 从右往左查找 find 从左往右查找
}

//2、替换
void test02() {
	string str1 = "abcdefghijklmnopqrstuvwxyz";
	//从4号位置起，一个字符替换成6666
	string str = str1.replace(4, 1, "6666");
	cout << str << endl;
}

//主函数
int main() {
	test01();
	test02();
	system("pause");
	return 0;
}
```

> 总结：
>
> - find 查找是从左往右，rfind 是从右往左
> - find 找到字符串后返回查找的第一个字符位置，找不到返回-1
> - replace 在替换时，要制定从哪个位置起，多少个字符，替换成什么样的字符串



#### 3.1.6 string 字符串比较

功能描述：

- 字符串之间比较

比较方式：

- 字符串比较是按字符的ASCII码进行对比
- `=`返回`0`
- `>`返回`1`
- `<`返回`-1`

函数原型：

- `int compare(const string& s) const;`   //与字符串s比较
- `int compare(const char* s) const;`       //与字符串s比较

示例：

```cpp
#include<iostream>
#include<string>
using namespace std;

//string字符串比较
void test01() {
	string str1 = "iello";
	string str2 = "hello";
	if (str1.compare(str2) == 0) {
		cout << "str1 = str2" << endl;
	}
	else if (str1.compare(str2) > 0) {
		cout << "str1 > str2" << endl;
	}
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.1.7 string 字符串存取

​	string 中单个字符存取方式有两种

- `char& operator[](int n);`  //通过[]方式存取字符
- `char& at(int n);`                //通过at方法获取字符

示例：

```cpp
#include<iostream>
#include<string>
using namespace std;

//string字符存取
void test01() {
	string str = "hello";

	//1、通过[]访问单个字符
	for (int i = 0; i < str.length(); i++) {
		cout << str[i] << " ";
	}
	//2、通过at方法获取
	for (int i = 0; i < str.length(); i++) {
		cout << str.at(i) << " ";
	}
	//修改单个字符
	str[0] = 'x';
	str.at(1) = 'y';
	cout << str << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.1.8 string 插入和删除

功能描述：

- 对string字符串进行插入和删除字符操作

函数原型：

- `string& insert(int pos, const char* s);`            //插入字符串
- `string& insert(int pos, const string& str);`     //插入字符串
- `string& insert(int pos, int n, char c);`            //在指定位置插入n个字符c
- `string& erase(int pos, int n = npos);`                //删除从pos开始的n个字符

示例：

```cpp
#include<iostream>
#include<string>
using namespace std;

//string插入和删除
void test01() {
	//插入
	string str = "hello";
	str.insert(str.length(), "world");
	cout << str << endl;

	str.insert(5, 2, ' ');
	cout << str << endl;

	//删除
	str.erase(5, 1);
	cout << str << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.1.9 string 子串

功能描述：

- 从字符串中获取想要的子串

函数原型：

- `string substr(int pos = 0,int n =npos) const;`//返回由pos开始的n个字符串组成的字符串

示例：

```cpp
#include<iostream>
#include<string>
using namespace std;

//string 字串
void test01() {
	string str = "helloworld";
	string str1 = str.substr(5, 5);
	cout << str1 << endl;
}

//使用操作
void test02() {
	string email = "crisp077@163.com";
	int pos = email.find('@');
	string name = email.substr(0, pos);
	cout << name << endl;
}

//主函数
int main() {
	test01();
	test02();
	system("pause");
	return 0;
}
```

> 总结：灵活的运用求字串功能，可以在实际开发中获取有效的信息



### 3.2 vector 容器

#### 3.2.1 vector 基本概念

功能：

- vector 数据结构和数组非常相似，也称为单端数组

vector与普通数组区别：

- 不同之处在于数组是静态空间，而vector可以动态扩展

动态扩展：

- 并不是在原空间之后续接新的空间，而是找更大的内存空间，然后将原数据拷贝新空间，释放原空间
- vector 容器的迭代器是支持随机访问的迭代器



#### 3.2.2 vector 构造函数

功能描述：

- 创建vector容器

函数原型：

- `vector<T> v;`                            //采用模板实现类实现，默认构造函数
- `vector(v.begin(),v.end());`    //将v[begin(),end()]区间中的元素拷贝给本身
- `vector(n,elem);`                       //构造函数将n个elem拷贝给本身
- `vector(const vector& vec);`    //拷贝构造函数

示例：

```cpp
//vector构造函数
void test01() {
	//默认构造 无参构造
	vector<int>v1;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
	}
	printVector(v1);

	//通过区间方式进行构造
	vector<int>v2(v1.begin(), v1.end());
	printVector(v2);

	//n个elem方式构造
	vector<int>v3(10, 100);
	printVector(v3);

	//拷贝构造参数
	vector<int>v4(v3);
	printVector(v4);
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.2.3 vector 赋值操作

功能描述：

- 给vector 容器进行赋值

函数原型：

- `vector& operator=(const vector& vec);`//重载等号操作符
- `assign(beg,end);`//将[beg,end]区间中的数据拷贝赋值给本身
- `assign(n,elem);`//将n个elem拷贝赋值给本身

```cpp
#include<iostream>
#include<vector>
using namespace std;

void printVector(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//vector赋值操作
void test01() {
	vector<int>v1;
	for (int i = 0; i < 5; i++) {
		v1.push_back(i);
	}
	printVector(v1);

	vector<int>v2 = v1;
	printVector(v2);

	vector<int>v3;
	v3.assign(v1.begin(), v1.end());
	printVector(v3);

	vector<int>v4;
	v4.assign(5, 10);
	printVector(v4);
}


//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.2.4 vector 容量和大小

功能描述：

- 对vector 容器的容量和大小操作

函数原型：

- `empty();`                           //判断容器是否为空

- `capacity();`                      //容器的容量

- `size(); `                             //返回容器中元素的个数

- `resize(int num);`              //重新指定容器的长度为num，若容器变长，则以默认值填充新位置

  ​								            //如果容器变短，则末尾超出容器长度的元素被删除

- `resize(int num,elem);`     //重新指定容器的长度为num，若容器变长，则以elem值填充新位置

  ​											//如果容器变短，则末尾超出容器长度的元素被删除

示例：

```cpp
#include<iostream>
#include<vector>
using namespace std;

void PrintVec(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}


//vector容量和大小
void test01() {
	vector<int>v1;
	for (int i = 0; i < 7; i++) {
		v1.push_back(i);
	}
	PrintVec(v1);
	
	if (v1.empty()) {
		cout << "v1 为空" << endl;
	}
	else {
		cout << "v1 不为空" << endl;
	}
	cout << "v1的容量为：" << v1.capacity() << endl;

	v1.resize(10);
	PrintVec(v1);

	v1.resize(5);
	PrintVec(v1);

	v1.resize(10, 10);
	PrintVec(v1);

	cout << "v1大小为：" << v1.size() << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：
>
> - 判断是否为空 --- empty
> - 返回元素个数 --- size
> - 返回容器容量 --- capacity
> - 重新指定大小 --- resize



#### 3.2.5 vector 插入和删除

功能描述：

- 对vector容器进行插入、删除操作

函数原型：

- `push_back(ele);`                                                         //尾部插入元素ele
- `pop_back(); `                                                                //删除最后一个元素
- `insert(const_iterator pos, ele);`                            //迭代器指向位置pos插入元素ele
- `insert(const_iterator pos, int count, ele); `         //迭代器指向位置pos插入count个元素ele
- `erase(const_iterator pos); `                                      //删除迭代器指向的元素
- `erase(const_iterator start, const_iterator end);`//删除迭代器从start到end之间的元素
- `clear();`                                                                      //删除容器中所有元素

示例：

```cpp
#include<iostream>
#include<vector>
using namespace std;

void PrintVec(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//vector插入和删除
void test01() {
	vector<int>v1;
	for (int i = 0; i < 5; i++) {
		v1.push_back(i);
	}
	PrintVec(v1);

	v1.pop_back();
	PrintVec(v1);

	v1.insert(v1.begin(), -1);
	PrintVec(v1);

	v1.insert(v1.end(), 3, 6);
	PrintVec(v1);

	v1.erase(v1.begin());
	PrintVec(v1);

	v1.clear();
	PrintVec(v1);

}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：
>
> - 尾插 --- push_back
> - 尾删 --- pop_back
> - 插入 --- insert(位置迭代器)
> - 删除 --- erase(位置迭代器)
> - 清空 --- clear



#### 3.2.6 vector 数据存取

功能描述：

- 对vector 中的数据做存取操作

函数原型：

- `at(int idx);`//返回索引[idx]所指的数据
- `operator[idx];`//返回索引[idx]所指的数据
- `front();`//返回容器中第一个数据元素
- `back();`//返回容器中最后一个数据元素

```cpp
#include<iostream>
#include<vector>
using namespace std;

//vector数据存取
void test01() {
	vector<int>v;
	v.push_back(0);
	v.push_back(1);
	v.push_back(2);
	v.push_back(3);
	v.push_back(4);

	cout << "第四个元素是" << v.at(3) << endl;
	cout << "第三个元素是" << v[2] << endl;
	cout << "最前面的元素是" <<v.front() << endl;
	cout << "最后面的元素是" << v.back() << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.2.7 vector 互换容器

功能实现：

- 实现两个容器内元素进行互换

函数原型：

- `swap(vec);`//将vec与本身元素互换

示例：

```cpp
#include<iostream>
#include<vector>
using namespace std;

void PrintVec(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//vector互换容器

//1、基本使用
void test01() {
	vector<int>v1(5, 5);
	vector<int>v2(4, 4);
	cout << "v1 = ";
	PrintVec(v1);
	cout << "v2 = ";
	PrintVec(v2);
	v1.swap(v2);
	cout << "swap v1 = ";
	PrintVec(v1);
	cout << "swap v2 = ";
	PrintVec(v2);
}

//2、实际用途
///巧用swap可以收缩空间
void test02() {
	vector<int>v;
	for (int i = 0; i < 100000; i++) {
		v.push_back(i);
	}
	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;

	v.resize(3);
	cout << "重新指定大小后v的容量为：" << v.capacity() << endl;
	cout << "重新指定大小后v的大小为：" << v.size() << endl;

	//巧用swap收缩内存
	//vector<int>(v)//匿名对象
	// .swap(v)//容器交换
	vector<int>(v).swap(v);
	cout << "收缩内存后v的容量为：" << v.capacity() << endl;
	cout << "收缩内存后v的大小为：" << v.size() << endl;
}

//主函数
int main() {
	//test01();
	test02();
	system("pause");
	return 0;
}
```

> 总结：swap可以使两个容器互换，可以达到实用的收缩内存效果



#### 3.2.8 vector 预留空间

功能描述：

- 减少vector 在动态扩展容量时的扩展次数

函数原型：

- `reverse(int len);`//容器预留len个元素长度，预留位置不初始化，元素不可访问

示例：

```cpp
#include<iostream>
#include<vector>
using namespace std;

//vector预留空间
void test01() {
	vector<int>v1;
	
	int count1 = 0;//统计开辟次数
	int* p1 = NULL;
	for (int i = 0; i < 100000; i++) {
		v1.push_back(i);

		if (p1 != &v1[0]) {
			p1 = &v1[0];
			count1++;
		}
	}
	cout << "count = " << count1 << endl;

	//利用reserve预留空间
	vector<int>v2;
	v2.reserve(100000);
	int count2 = 0;//统计开辟次数
	int* p2 = NULL;
	for (int i = 0; i < 100000; i++) {
		v2.push_back(i);

		if (p1 != &v2[0]) {
			p1 = &v2[0];
			count2++;
		}
	}
	cout << "count = " << count2 << endl;
}


//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



### 3.3 deque 容器

#### 3.1.1 deque 容器基本概念

功能：

- 双端数组，可以对头端进行插入删除操作

deque 与vector 区别：

- vector 对于头部的插入删除效率低，数据量越大，效率越低
- deque 相对而言，对头部的插入删除速度会比vector 快
- vector 访问元素时的速度会比deque 快，这和两者内部实现有关

deque 内部工作原理：

- deque 内部有个**中控器**，维护每段缓冲区中的内容，缓冲区中存放真实数据
- 中控器维护的是每个缓冲区的地址，使得使用deque 时像一片连续的内存空间
- deque 容器的迭代器也是支持随机访问的



#### 3.3.2 deque 构造函数

功能描述：

- deque 容器构造

函数原型：

- `deque<T> deqT;`                   //默认构造形式
- `deque(beg, end);`                //构造函数将[beg,end]区间中的元素拷贝给本身
- `deque(n, elem);`                  //构造函数将n个elem拷贝给本身
- `deque(const deque& deq);`   //构造拷贝函数

示例：

```cpp
#include<iostream>
#include<deque>
using namespace std;

//deque构造函数

//为了防止篡改数据
//将打印设置为只读，加上const
void PrintDeque(const deque<int>& d) {
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		//*it = 100;//报错，容器中的数据只读，不可修改
		cout << *it << " ";
	}
	cout << endl;
}

void test01() {
	//默认
	deque<int>d1;
	for (int i = 0; i < 10; i++) {
		d1.push_back(i);
	}
	PrintDeque(d1);
	//区间元素拷贝
	deque<int> d2(d1.begin()+1, d1.end()-1);
	PrintDeque(d2);
	//拷贝构造
	deque<int> d3(d2);
	PrintDeque(d3);
	//n个elem
	deque<int> d4(4, 100);
	PrintDeque(d4);
}


//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.3.3 deque 赋值操作

功能描述：

- 给deque 容器进行赋值

容器原型：

- `deque& operator=(const deque& deq);`  //重载等号操作符
- `assign(beg,end);`                                   //将[beg,end]区间中的数据拷贝赋值给本身
- `assign(n,elem);`                                     //将n个elem拷贝赋值给本身

示例：

```cpp
#include<iostream>
#include<deque>
using namespace std;

void PriDeque(const deque<int>& d) {
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//deque赋值操作
void test01() {
	deque<int> d1;
	d1.push_back(0);
	d1.push_back(1);
	d1.push_back(2);
	d1.push_back(3);
	d1.push_back(4);
	PriDeque(d1);
	deque<int> d2;
	d2 = d1;
	PriDeque(d2);

	deque<int> d3;
	d3.assign(d1.begin()+1, d1.end()-1);
	PriDeque(d3);
	d3.assign(10,100);
	PriDeque(d3);
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.3.4 deque 大小操作

功能描述：

- 对deque 容器的大小进行操作

函数原型：

- `deque.empty();`                       //判断容器是否为空

- `deque.size();`                         //返回容器中元素的个数

- `deque.resize(num);`                //重新指定容器的长度为num，若容器变长，则以默认值填充新位置

  ​									              //如果容器变短，则末尾超出的容器长度的元素被删除

- `deque.resize(num,elem);`        //重新指定容器的长度为num，若容器变长，则以elem填充新位置

  ​												  //如果容器变短，则末尾超出的容器长度的元素被删除

示例：

```cpp
#include<iostream>
#include<deque>
using namespace std;

void PriDeque(const deque<int> d) {
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//deque大小操作
void test01() {
	deque<int> d1;
	for (int i = 0; i < 10; i++) {
		d1.push_back(i);
	}
	PriDeque(d1);

	if (d1.empty()) {
		cout << "容器为空" << endl;
	}
	else {
		cout << "容器不为空" << endl;
	}
	//deque容器没有容量操作
	cout << "d1的大小为：" << d1.size() << endl;
	d1.resize(5);
	PriDeque(d1);
	cout << "d1的大小为：" << d1.size() << endl;
	d1.resize(10, 10);
	PriDeque(d1);
	cout << "d1的大小为：" << d1.size() << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：
>
> - deque 没有容量的概念
> - 判断是否为空 --- empty
> - 返回元素个数 --- size
> - 重新指定个数 --- resize



#### 3.3.5 deque 插入和删除

功能描述：

- 向deque 容器中插入和删除数据

函数原型：

​	两端插入操作：

- `push_back(elem);`      //在容器尾部添加一个数据
- `push_front(elem);`    //在容器头部插入一个数据
- `pop_back();`               //删除容器最后一个数据
- `pop_front();`             //删除容器第一个数据

​	指定位置操作：

- `insert(pos,elem);`       //在pos位置插入一个elem元素的拷贝，返回新数据的位置
- `insert(pos,n,elem);`    //在pos位置插入n个elem的数据，无返回值
- `insert(pos,beg,end);`   //在pos位置插入[beg,end]区间的数据，无返回值
- `clear();`                        //清空容器内的所有数据
- `erase(beg,end);`            //删除[beg,end]区间的数据，返回下一个数据的位置
- `erase(pos);`                   //删除pos位置指定的数据，返回下一个数据的位置

示例：

```cpp
#include<iostream>
#include<deque>
using namespace std;

void PriDeque(const deque<int>& d) {
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//deque插入和删除
void test01() {
	//两端操作
	deque<int> d1;
	for (int i = 0; i < 10; i++) {
		d1.push_front(i);
	}
	PriDeque(d1);
	d1.pop_back();
	PriDeque(d1);
	d1.pop_front();
	PriDeque(d1);
	//指定位置操作
	d1.insert(d1.begin()+1, 10);
	PriDeque(d1);
	d1.insert(d1.begin(), 2, 10);
	PriDeque(d1);
	//按照区间插入
	deque<int> d2;
	d2.push_back(1);
	d2.push_back(2);
	d2.push_back(3);
	PriDeque(d2);
	d1.insert(d1.begin(), d2.begin(), d2.end());
	PriDeque(d1);
	//删除
	d1.erase(d1.begin() + 1);
	PriDeque(d1);
	d1.erase(d1.begin() + 1, d1.end() - 1);
	PriDeque(d1);
	d1.clear();
	PriDeque(d1);

}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：
>
> - 插入何删除提供的位置是迭代器



#### 3.3.6 deque 数据存取

功能描述：

- 对deque 中的数据做存取操作

函数原型：

- `at(int idx);`       //返回索引[idx]所指的数据
- `operator[idx];`    //返回索引[idx]所指的数据
- `front();`               //返回容器中第一个数据元素
- `back();`                 //返回容器中最后一个数据元素

```cpp
#include<iostream>
#include<deque>
using namespace std;

void PriDeque(const deque<int>& d) {
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//deque数据存取
void test01() {
	deque<int> d;
	for (int i = 0; i < 10; i++) {
		d.push_back(i);
	}
	PriDeque(d);
	cout << "d中第三个元素是：" << d.at(2) << endl;
	cout << "d中第二个元素是：" << d[1] << endl;
	cout << "d中第一个元素是：" << d.front() << endl;
	cout << "d中最后一个个元素是：" << d.back() << endl;

}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.3.7 deque 排序

功能描述：

- 利用算法实现对deque 容器进行排序

算法：

- `sort(iterator beg, iterator end)`   //对beg和end区间内元素进行排序

示例：

```cpp
#include<iostream>
#include<deque>
#include<algorithm>
using namespace std;

void PriDeque(const deque<int>& d) {
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//deque排序
void test01() {
	deque<int> d;
	d.push_back(5);
	d.push_back(2);
	d.push_back(1);
	d.push_back(3);
	d.push_back(4);
	PriDeque(d);
	//排序，默认从小到大
	//对于支持随机访问的迭代器的容器，都可以利用sort算法直接对其排序
	sort(d.begin(), d.end());
	PriDeque(d);

}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



### 3.5 stack 容器

#### 3.5.1 stack 基本概念

**概念：**stack 是一种先进后出(First In Last Out,FILO)的数据结构，他只有一个出口

栈中只有顶端的元素可以被外界使用，因此栈不允许有遍历行为

栈中进入数据称为 --- 入栈（push）

栈中弹出数据称为 --- 出栈（pop）



#### 3.5.2 stack 常用接口

功能描述：

- 栈容器常用的对外接口

构造函数：

- `stack<T> stk;`                      //stack 采用模板类实现，stack 对象的默认构造形式
- `stack(const stack& stk);`   //拷贝构造函数

赋值操作：

- `stack& operator=(const stack& stk);`    //重载等号操作符

数据存取：

- `push(elem);`    //向栈顶添加元素
- `pop();`             //从栈顶移除一个元素
- `top();`             //返回栈顶元素

大小操作：

- `empty();`         //判断堆栈是否为空
- `size();`           //返回栈的大小



示例：

```cpp
#include<iostream>
#include<stack>
using namespace std;

//stack常用接口
void test01() {
	stack<int> s;
	s.push(1);
	s.push(2);
	cout << "现在栈顶元素是：" << s.top() << endl;
	s.pop();
	cout << "现在栈顶元素是：" << s.top() << endl;
	if (s.empty()) {
		cout << "栈为空" << endl;
	}
	else {
		cout << "栈不为空" << endl;
	}
	s.push(10);
	s.push(100);
	cout << "现在栈的大小为：" << s.size() << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



### 3.6 queue 容器

#### 3.6.1 queue 基本概念

**概念：**Queue是一种**先进先出**(First In First Out,FIFO)的数据结构，他有两个出口



队列容器允许从一端新增元素，从另一端移除元素

队列中只有对头和对位才可以被外界使用，因此队列不允许有遍历行为

队列中进数据称为 --- 入队（push）

队列中出数据称为 --- 出队（pop）



#### 3.6.2 queue 常用接口

功能描述：

- 队列容器常用的对外接口

构造函数：

- `queue<T> q;`                      //queue 采用模板类实现，queue 对象的默认构造形式
- `queue(const queue& q);`   //拷贝构造函数

赋值操作：

- `queue& operator=(const queue& q);`    //重载等号操作符

数据存取：

- `push(elem);`    //向队尾添加元素
- `pop();`             //从队头移除一个元素
- `back();`            //返回最后一个元素
- `front();`           //返回第一个元素

大小操作：

- `empty();`         //判断队列是否为空
- `size();`           //返回队列的大小



示例：

```cpp
#include<iostream>
#include<queue>
#include<string>
using namespace std;

//queue常用接口

class Person {
public:
	string m_name;
	int m_age;
	Person(string name, int age) {
		this->m_name = name;
		this->m_age = age;
	}
	
};

//为了方便输出，重载“<<”
ostream& operator<<(ostream& out, Person& p) {
	out << "name:" << p.m_name << " age:" << p.m_age;
	return out;
}

void test01() {
	queue<Person> q;
	//准备数据
	Person p1("张三",10);
	Person p2("李四",20);
	Person p3("王五",30);
	Person p4("赵六",40);
	//入队
	q.push(p1);
	q.push(p2);
	q.push(p3);
	q.push(p4);
	//判断，只要队头不为空，查看队头，查看队尾，出队
	while (!q.empty()) {
		cout << "队头：" << q.front() << endl;
		cout << "队尾：" << q.back() << endl;
		cout << "队列大小为：" << q.size() << endl;
		cout << "————————————————————————————" << endl;
		q.pop();
	}


}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



###  3.7 List 容器

####  3.7.1 list 基本概念

​	**功能：**将数据进行链式存储

​	**链表**（list）是一种物理存储单元上非连续的存储结构，数据元素的逻辑顺序是通过链表中的指针链接实现的

​	链表的组成：链表由一系列**结点**组成

​	节点的组成：一个是存储数据元素的**数据域**，另一个是存储下一个结点地址的**指针域**

​	STL中的链表是一个双向循环链表



​	由于链表的存储方式并不是连续的内存空间，因此链表list 中的迭代器只支持前移和后移，属于**双向迭代器**



list优点：

- 采用动态存储分配，不会造成内存浪费和溢出
- 链表执行插入和删除操作十分方便，修改指针即可，不需要移动大量元素

list缺点：

- 链表灵活，但是空间（指针域）和时间（遍历）额外耗费较大



​	List有一个重要的性质，插入操作和删除操作都不会造成原有list迭代器的失效，这才vector时不成立的



#### 3.7.2 list构造函数

功能描述：

- 创建list容器

函数原型：

- `list<T> lst;`     		       //list 采用模板类实现，对象的默认构造形式
- `list(beg, end);`               //构造函数将[beg,end]区间中的元素拷贝给本身
- `list(n, elem);`                 //构造函数将n个elem拷贝给本身
- `list(const list& lst);`   //拷贝构造函数



示例：

```cpp
#include<iostream>
#include<list>
using namespace std;

void PriList(const list<int>& l) {
	for (list<int>::const_iterator it = l.begin(); it != l.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//list构造函数
void test01() {
	//默认构造
	list<int> l1;
	l1.push_back(1);
	l1.push_back(2);
	l1.push_back(3);
	l1.push_back(4);
	l1.push_back(5);
	PriList(l1);
	//区间传递
	list<int> l2(l1.begin(), l1.end());
	PriList(l2);
	//拷贝构造
	list<int> l3(l2);
	PriList(l3);
	//n个elem
	list<int> l4(5, 10);
	PriList(l4);
}



//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.7.3 list 赋值和交换

功能描述：

- 给list 容器进行赋值，以及交换list 容器

函数原型：

- `assign(beg, end);`                                //将[beg,end]区间中的数据拷贝赋值给本身
- `assign(n, elem);`                                  //将n个elem 拷贝赋值给本身
- `list& operator=(const list& list);`   //重载等号操作符
- `swap(lst);`                                             //将lst 域本身的元素互换



示例：

```cpp
#include<iostream>
#include<list>
using namespace std;

void PriList(const list<int>& l) {
	for (list<int>::const_iterator it = l.begin(); it != l.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//list赋值和交换
void test01() {
	list<int> l1;
	l1.push_back(1);
	l1.push_back(2);
	l1.push_back(3);
	l1.push_back(4);
	l1.push_back(5);
	PriList(l1);
	list<int> l2;
	list<int>::iterator it = l1.begin();
	it++;
	//assign[beg, end]
	l2.assign(it, l1.end());
	PriList(l2);
	//assign[n, elem]
	list<int> l3;
	l3.assign(5, 10);
	PriList(l3);
	
	list<int> l4 = l2;
	PriList(l4);

	cout << "将l1与l2交换" << endl;
	l1.swap(l2);
	cout << "l1:";
	PriList(l1);
	cout << "l2:";
	PriList(l2);

}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.7.4 list 大小操作

功能描述：

- 对list 容器的大小进行操作

函数原型：

- `size();`                  //返回容器中元素的个数

- `empty();`                 //判断容器是否为空

- `resize(num);`          //重新指定容器的长度为num，若容器变长，则以默认值填充新位置

  ​						          //如果容器变短，则末尾超出的容器长度的元素被删除

- `resize(n, elem);`    //重新指定容器的长度为num，若容器变长，则以elem填充新位置

  ​								  //如果容器变短，则末尾超出的容器长度的元素被删除



#### 3.7.5 list 插入和删除

功能描述：

- 对list 容器插入和删除

函数原型：

- `push_back(ele);`                                                         //尾部插入元素ele
- `pop_back(); `                                                                //删除最后一个元素
- `push_front(ele);`                                                       //在容器开头插入一个元素
- `pop_front();`                                                              //从容器开头移除一个元素
- `insert(pos, ele);`                                                     //在pos 位置插入ele元素的拷贝，返回新数据的位置
- `insert(pos, n, ele); `                                                //在pos 位置插入n个ele的数据，无返回值
- `insert(pos, beg, end);`                                             //在pos 位置插入[beg, end]区间的数据，无返回值
- `erase(pos); `                                                                //删除pos 位置的数据，返回下一个数据的位置
- `erase(beg, end);`                                                        //删除[beg, end]之间的元素，返回下一个数据的位置
- `clear();`                                                                      //删除容器中所有元素
- `remove(elem);`                                                             //删除容器中所有与elem值匹配的元素

示例：

```cpp
#include<iostream>
#include<list>
using namespace std;

void PriList(const list<int>& l) {
	for (list<int>::const_iterator it = l.begin(); it != l.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test01() {
	list<int> l;
	l.push_back(1);
	l.push_back(2);
	l.push_back(3);
	l.push_back(3);
	l.push_back(5);
	PriList(l);
	l.remove(3);
	PriList(l);
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.7.6 list 数据存取

功能描述：

- 对list 容器中数据进行存取

函数原型：

- `front();`       //返回第一个元素
- `back();`         //返回最后一个元素

注意：

- list 容器中不可以通过[]或者at方式访问数据



#### 3.7.7 list 反转和排序

功能描述：

- 将容器中的元素反转，以及将容器中的数据进行排序

函数原型：

- `reverse();`       //反转链表
- `sort();`            //链表排序

示例：

```cpp
#include<iostream>
#include<list>
using namespace std;

void PriList(const list<int>& l) {
	for (list<int>::const_iterator it = l.begin(); it != l.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test01() {
	list<int> l;
	l.push_back(1);
	l.push_back(2);
	l.push_back(3);
	l.push_back(3);
	l.push_back(5);
	PriList(l);
	l.reverse();
	PriList(l);
	//所有不支持随机访问迭代器的容器，不可以用标准算法库
	//sort(l.begim(), l.end())//报错
	//不支持随机访问迭代器的容器，内部会提供对应的一些算法
	l.sort();
	PriList(l);

}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：
>
> - sort是成员函数



### 3.8 set/multiset 容器

#### 3.8.1 set 基本概念

简介：

- 所有元素都会在插入时自动被排序

本质：

- set/multiset 属于**关联式容器**，底层结构是用**二叉树**实现的

set 和multiset 区别：

- set 不允许容器中有重复的元素
- multiset 允许容器有重复的元素



#### 3.8.2 set 构造和赋值

功能描述：

- 创建set 容器以及赋值

构造：

- `set<T> st;`                         //默认构造函数
- `set(const set& st);`          //拷贝构造函数

赋值：

- `set& operator=(const set& st);`         //重载等号操作符

示例：

```cpp
#include<iostream>
#include<set>
using namespace std;

//打印输出
void PriSet(const set<int> s) {
	for (set<int>::const_iterator it = s.begin(); it != s.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}


//set构造和赋值
void test01() {
	//默认构造
	set<int> s1;
	//set插入只有insert
	s1.insert(2);
	s1.insert(1);
	s1.insert(3);
	//不允许加入重复值
	s1.insert(2);
	//特点：插入元素自动排序
	PriSet(s1);
	//拷贝构造
	set<int> s2(s1);
	PriSet(s2);
	//赋值
	set<int> s3 = s2;
	PriSet(s3);
}


//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.8.3 set 大小和交换

功能描述：

- 统计set 容器大小以及交换set 容器

函数原型：

- `size();`          //返回容器中元素的数目
- `empty();`        //判断容器是否为空
- `swap(st);`      //交换两个集合容器

```cpp
#include<iostream>
#include<set>
using namespace std;

//打印输出
void PriSet(const set<int>& s) {
	for (set<int>::const_iterator it = s.begin(); it != s.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//set大小和交换
void test01() {
	set<int> s1;
	s1.insert(0);
	s1.insert(4);
	s1.insert(2);
	s1.insert(3);
	PriSet(s1);
	cout << "s1大小为：" << s1.size() << endl;
	if (s1.empty()) {
		cout << "s1容器为空" << endl;
	}
	else {
		cout << "s1容器不为空" << endl;
	}
	set<int> s2;
	s2.insert(0);
	PriSet(s2);
	s1.swap(s2);
	PriSet(s1);
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.8.4 set 插入和删除

功能描述：

- set 容器进行插入数据和删除数据

函数原型：

- `insert(elem);`             //在容器中插入元素
- `clear();`                      //清除所有元素
- `erase(pos);`                 //删除pos迭代器所指的元素，返回下一个元素的迭代器
- `erase(beg, end);`        //删除区间[beg, end]的所有元素，返回下一个元素的迭代器
- `erase(elem);`               //删除容器中值为elem的元素；

```cpp
#include<iostream>
#include<set>
using namespace std;

//打印
void PriSet(const set<int>& s) {
	for (set<int>::const_iterator it = s.begin(); it != s.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}


//set插入和删除
void test01() {
	set<int> s1;
	s1.insert(0);
	s1.insert(4);
	s1.insert(2);
	s1.insert(3);
	PriSet(s1);
	//删除操作
	s1.erase(3);
	PriSet(s1);
	set<int>::iterator it = s1.begin();
	it++;
	s1.erase(s1.begin(),it);
	PriSet(s1);
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.8.5 set 查找和统计

功能描述：

- 对set 容器进行查找数据以及统计数据

函数原型：

- `find(key);`     //查找key是否存在，若存在，返回该键的元素迭代器；若不存在，返回set.end();
- `count(key);`    //统计key元素的个数

示例：

```cpp
#include<iostream>
#include<set>
using namespace std;

//打印
void PriSet(const multiset<int>& s) {
	for (multiset<int>::const_iterator it = s.begin(); it != s.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//set查找和统计
void test01() {
	multiset<int> s1;
	for (int i = 0; i < 10; i++) {
		s1.insert(i);
		s1.insert(i + 1);
	}
	PriSet(s1);
	//查找 find返回的是迭代器
	s1.erase(s1.find(9));
	PriSet(s1);
	//统计
	cout << "s1中6的个数为：" << s1.count(6) << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：
>
> - 查找 --- find（返回的是迭代器）
> - 统计 --- count（对于set，结果只有0 1两种）



#### 3.8.6 set 和 multiset区别

区别：

- set 不可以插入重复数据，而multiset可以
- set 插入数据的同时会返回插入结果，表示插入成功
- multiset 不会检测数据，因此可以插入重复数据

```cpp
#include<iostream>
#include<set>
using namespace std;

//set 和 multiset 区别
void test01() {
	//对于set
	set<int> s;
	//第一次插入 10
	pair<set<int>::iterator, bool> ret = s.insert(10);
	if (ret.second) {
		cout << "第一次插入成功" << endl;
	}
	else {
		cout << "第一次插入失败" << endl;
	}
	//第二次插入 10
	ret = s.insert(10);
	if (ret.second) {
		cout << "第二次插入成功" << endl;
	}
	else {
		cout << "第二次插入失败" << endl;
	}

	//对于multiset
	multiset<int> ms;
	ms.insert(10);
	ms.insert(10);
	for (multiset<int>::iterator it = ms.begin(); it != ms.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;

}


//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：
>
> - 如果不允许插入重复数据可以利用set
> - 如果需要插入重复数据利用multiset



#### 3.8.7 pair 对组创建

功能描述：

- 成对出现的数据，利用队组可以返回两个数据

两种创建方式：

- `pair<type, type> p (value1, value2);`
- `pair<type, type> p = make_pair(value1, value2);`

示例：

```cpp
#include<iostream>
#include<string>
using namespace std;

//对组创建
void test01() {
	//第一种方式
	pair<string, int> p1("tom", 20);
	cout << "姓名：" << p1.first << " 年龄：" << p1.second << endl;
	//第二种方式
	pair<string, int> p2 = make_pair("jerry", 10);
	cout << "姓名：" << p2.first << " 年龄：" << p2.second << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.8.8 set 容器排序

学习目标：

- set 容器默认排序规则为从小到大，掌握如何改变排序规则

主要技术点：

- 利用仿函数，可以改变排序规则

示例1：set存放内置数据类型

```cpp
#include<iostream>
#include<set>
using namespace std;

class MyCompare {
public:
	bool operator()(const int& v1,const int& v2)const {
		return v1 > v2;
	}
};

//打印
void PriSet(const set<int>& s) {
	for (set<int>::const_iterator it = s.begin(); it != s.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//set容器排序
void test01() {
	set<int>s1;
	s1.insert(10);
	s1.insert(50);
	s1.insert(30);
	s1.insert(20);
	//默认顺序为从小到大
	PriSet(s1);
	//指定顺序从大到小
	set<int, MyCompare>s2;
	s2.insert(10);
	s2.insert(90);
	s2.insert(30);
	s2.insert(50);
	for (set<int, MyCompare>::iterator it = s2.begin(); it != s2.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

示例2：set存放自定义数据类型

```cpp
#include<iostream>
#include<string>
#include<set>
using namespace std;

//set 排序 自定义数据类型

class Person {
public:
	string m_Name;
	int m_Age;
	Person(string name, int age) {
		this->m_Name = name;
		this->m_Age = age;
	}
};

class ComparePerson {
public:
	bool operator()(const Person& p1, const Person& p2)const {
		//按照年龄降序
		return p1.m_Age > p2.m_Age;
	}
};

void test01() {
	//自定义数据类型 都会指定排序规则
	set<Person,ComparePerson> s;

	//创建Person对象
	Person p1("张三", 10);
	Person p2("李四", 20);
	Person p3("王五", 30);
	Person p4("赵六", 40);
	s.insert(p1);
	s.insert(p2);
	s.insert(p3);
	s.insert(p4);

	//遍历
	for (set<Person,ComparePerson>::iterator it = s.begin(); it != s.end(); it++) {
		cout << "姓名：" << it->m_Name << " 年龄：" << it->m_Age << endl;
	}
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



### 3.9 map/ multimap 容器

#### 3.9.1 map基本概念

简介：

- map 中所有元素都是pair
- pair 中第一个元素为key（键值），起到索引作用，第二个元素为value（实值）
- 所有元素都会根据元素的键值自动排序

本质：

- map/multimap 属于**关联式容器**，底层结构是用二叉树实现

优点：

- 可以根据key 值快速找到value 值

map 和multimap 区别：

- map不允许容器中有重复key 值元素
- multimap 允许容器中有重复key 值元素



#### 3.9.2 map 构造和赋值

函数原型：

- `map<T1, T2> mp;`                //map默认构造函数
- `map(const map& mp);`         //拷贝构造函数

赋值：

- `map& operator=(const map& mp);`             //重载等号操作符

示例：

```cpp
#include<iostream>
#include<string>
#include<map>
using namespace std;

//打印
void PrintMap(const map<int, int>& mp) {
	for (map<int, int>::const_iterator it = mp.begin(); it != mp.end(); it++) {
		cout << "key = " << (*it).first << " value = " << (*it).second << endl;
	}
}

//map构造和赋值
void test01() {
	map<int, int> mp;
	mp.insert(pair<int, int>(1, 10));
	mp.insert(pair<int, int>(3, 30));
	mp.insert(pair<int, int>(4, 40));
	mp.insert(pair<int, int>(2, 20));
	PrintMap(mp);
	cout << endl;
	//拷贝构造
	map<int, int> mp2(mp);
	PrintMap(mp2);
	cout << endl;
	//赋值
	map<int, int> mp3;
	mp3 = mp2;
	PrintMap(mp3);
}


//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：map 中所有元素都是成对出现，插入数据时候要使用对组



#### 3.9.3 map 大小和交换

功能描述：

- 统计map 容器大小以及交换map 容器

函数原型：

- `size();`              //返回容器中元素的数目
- `empty();`            //判断容器是否为空
- `swap(mp);`           //交换两个map容器



#### 3.9.4 map 插入和删除

函数原型：

- `insert(elem);`              //在容器中插入元素
- `clear();`                       //清除所有元素
- `erase(pos);`                  //删除pos 迭代器所指的元素，返回下一个元素的迭代器
- `erase(beg, end);`         //删除区间[beg, end]的所有元素，返回下一个元素的迭代器
- `erase(key);`                  //删除容器中值为key 的元素

```cpp
#include<iostream>
#include<map>
using namespace std;

//打印
void PrintMap(const map<int, int>& m) {
	for (map<int, int>::const_iterator it = m.begin(); it != m.end(); it++) {
		cout << "key = " << (*it).first << " value = " << (*it).second << endl;
	}
}

//map插入和删除
void test01() {
	map<int, int>m;

	//插入
	//第一种
	m.insert(pair<int, int>(1, 10));
	//第二种
	m.insert(make_pair(3, 30));
	//第三种
	m.insert(map<int, int>::value_type(2, 20));
	//第四种
	m[0] = 0;
	//[]不建议插入，用途 可以利用key访问value
	cout << m[5] << endl;

	PrintMap(m);
	cout << endl;

	//删除
	//第一种
	m.erase(m.begin());
	PrintMap(m);
	cout << endl;
	//第二种
	m.erase(3);
	PrintMap(m);
	//清空
	m.clear();
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.9.5 map 查找和统计

函数原型：

- `find(key);`      //查找key 是否存在，若存在。返回该键的元素的迭代器；若不存在，返回map.end()
- `count(key);`     //统计key 的元素个数

```cpp
#include<iostream>
#include<map>
using namespace std;

//查找和统计
void test01() {
	map<int, int> m;
	m.insert(pair<int, int>(1, 10));
	m.insert(pair<int, int>(3, 30));
	m.insert(pair<int, int>(4, 40));
	m.insert(pair<int, int>(2, 20));

	//查找
	map<int, int>::iterator pos = m.find(3);

	if (pos != m.end()) {
		cout << "查到了元素key = " << (*pos).first << " value = " << (*pos).second << endl;
	}
	else {
		cout << "没有查找到元素" << endl;
	}

	//统计
	//map不允许重复插入
	cout << "num = " << m.count(3) << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 3.9.6 map 容器排序

主要技术点：

- 利用仿函数，可以改变排序规则

```cpp
#include<iostream>
#include<map>
using namespace std;

class MyCompare {
public:
	bool operator()(const int& v1,const int& v2)const {
		return v1 > v2;
	}
};

//打印(利用仿函数)
void PrintMap(const map<int, int, MyCompare>& m) {
	for (map<int, int>::const_iterator it = m.begin(); it != m.end(); it++) {
		cout << "key = " << (*it).first << " value = " << (*it).second << endl;
	}
}

//测试
void test01() {
	map<int, int, MyCompare>m;
	m.insert(make_pair(1, 10));
	m.insert(make_pair(4, 40));
	m.insert(make_pair(2, 20));
	m.insert(make_pair(3, 30));
	PrintMap(m);
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：
>
> - 利用仿函数可以指定map 容器的排序规则
> - 对于自定义数据类型，map必须要指定排序规则，同 set容器



## 4 STL-函数对象

### 4.1 函数对象

#### 4.1.1 函数对象概念

概念：

- 重载**函数调用操作符**的类，其对象常称为**函数对象**
- **函数对象**使用重载的`()`时，行为类似函数调用，也叫仿函数

本质：

- 函数对象（仿函数）是一个类，不是一个函数



#### 4.1.2 函数对象使用

特点：

- 函数对象在使用时，可以像普通函数那样调用，可以有参数，可以有返回值
- 函数对象超出普通函数的概念，函数对象可以有自己的状态
- 函数对象可以作为参数传递

```cpp
#include<iostream>
#include<string>
using namespace std;

//函数对象基本使用

class Myadd {
public:
	int operator()(int v1,int v2) {
		return v1 + v2;
	}
};

//1、函数对象在使用时，可以像普通函数那样调用，可以有参数，可以有返回值
void test01() {
	Myadd myadd;
	cout << myadd(10, 20) << endl;
}

//2、函数对象超出普通函数的概念，函数对象可以有自己的状态
class MyPrint {
public:
	int count;

	MyPrint() {
		this->count = 0;
	}
	void operator()(string test) {
		cout << test << endl;
		this->count++;
	}
};
void test02() {
	MyPrint myprint;
	myprint("hello c++");
	myprint("hello c++");
	myprint("hello c++");
	myprint("hello c++");

	cout << "MyPrint调用次数为：" << myprint.count << endl;
}

//3、函数对象可以作为参数传递

void doPrint(MyPrint& mp, string test) {
	mp(test);
}

void test03() {
	MyPrint myprint;
	doPrint(myprint, "hello world!");
}

//主函数
int main() {
	test01();
	test02();
	test03();
	system("pause");
	return 0;
}
```

> 总结：仿函数写法非常灵活，可以作为参数进行传递



### 4.2 谓词

#### 4.2.1 谓词概念

概念：

- 返回bool 类型的仿函数称为**谓词**
- 如果operator() 接受一个参数，那么叫做一元谓词
- 如果operator() 接受两个参数，那么叫做二元谓词



#### 4.2.2 一元谓词

```cpp
#include<iostream>
#include<vector>
using namespace std;

class GreatFive {
public:
	bool operator()(int val) {
		return val > 5;
	}
};

//一元谓词
void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	//查找容器中 有没有大于5的数字
	vector<int>::iterator it = find_if(v.begin(), v.end(), GreatFive());
	if (it == v.end()) {
		cout << "没有找到" << endl;
	}
	else {
		cout << "找到了大于5的数字为：" << *it << endl;
	}
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

#### 4.2.3 二元谓词

```cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

//二元谓词
class MyCompare {
public:
	bool operator()(int v1, int v2) {
		return v1 > v2;
	}
};


void test01() {
	vector<int> v;
	v.push_back(10);
	v.push_back(40);
	v.push_back(50);
	v.push_back(30);
	v.push_back(20);
	sort(v.begin(), v.end());
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;

	//使用函数对象 改变算法策略 变为排序规则为从大到小
	sort(v.begin(), v.end(), MyCompare());
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



### 4.3 内建函数对象

#### 4.3.1 内建函数对象意义

概念：

- STL 内建了一些函数对象

分类：

- 算术仿函数
- 关系仿函数
- 逻辑仿函数

用法：

- 这些仿函数所产生的对象，用法和一般函数完全相同
- 使用内建函数对象，需要引入头文件`#include<functional>`



#### 4.3.2 算术仿函数

功能描述：

- 实现四则运算
- 其中negate 是一元运算，其他都是二元运算

仿函数原型：

- `template<class T> T plus<T>;`                  //加法仿函数
- `template<class T> T minus<T>;`                //减法仿函数
- `template<class T> T multiplies<T>;`        //乘法仿函数
- `template<class T> T divides<T>;`             //除法仿函数
- `template<class T> T modulus<T>;`             //取模仿函数
- `template<class T> T negate<t>;`               //取反仿函数

示例：

```cpp
#include<iostream>
#include<functional>
using namespace std;

//内建函数对象 算术仿函数

//negate 一元仿函数 取反仿函数
void test01() {
	negate<int> n;
	cout << n(50) << endl;
}

//plus 二元仿函数 加法
void test02() {
	plus<int> n;
	cout << n(20, 30) << endl;
}

//主函数
int main() {
	test01();
	test02();
	system("pause");
	return 0;
}
```



#### 4.3.3 关系仿函数

函数功能：

- 实现关系对比

仿函数原型：

- `template<class T> bool equal_to<T>;`              //等于
- `template<class T> bool not_equal_to<T>;`       //不等于
- `template<class T> bool greater<T>;`                //大于
- `template<class T> bool greater_equal<T>;`      //大于等于
- `template<class T> bool less<T>;`                      //小于
- `template<class T> bool less_equal<T>;`            //小于等于

示例：

```cpp
#include<iostream>
#include<vector>
#include<functional>
#include<algorithm>
using namespace std;

//内建函数对象 关系仿函数
void test01() {
	vector<int>v;
	v.push_back(10);
	v.push_back(50);
	v.push_back(40);
	v.push_back(30);
	greater<int>n;
	sort(v.begin(), v.end(), n);
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 4.3.4 逻辑仿函数

功能描述：

- 实现逻辑运算

函数原型：

- `template<class T> bool logical_and<T>;`     //逻辑与
- `template<class T> bool logical_or<T>;`       //逻辑或
- `template<class T> bool logical_not<T>;`      //逻辑非

示例：

```cpp
#include<iostream>
#include<vector>
#include<functional>
#include<algorithm>
using namespace std;

//内建函数对象 逻辑仿函数
void test01() {
	vector<bool> v;
	v.push_back(true);
	v.push_back(false);
	v.push_back(false);
	v.push_back(true);
	for (vector<bool>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;

	//利用逻辑非 将容器v搬运到v2中，并执行取反
	vector<bool>v2;
    //开辟空间
	v2.resize(v.size());

	transform(v.begin(), v.end(), v2.begin(), logical_not<bool>());

	for (vector<bool>::iterator it = v2.begin(); it != v2.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}


//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

逻辑仿函数实际应用较少，了解即可



## 5 STL-常用算法

概述：

- 算法主要是由头文件`<algorithm>``<functional>``<numeric>`组成
- `<algorithm>`是所有STL 头文件中最大的一个，范围涉及到比较、交换、查找、遍历操作、复制、修改等等
- `<numeric>`体积很小，只包括几个在序列上面进行简单数学运算的模板函数
- `<functional>`定义了一些模板类，用以声明函数对象



### 5.1 常用遍历算法

算法简介：

- `for_each`         //遍历容器
- `transform`       //搬运容器到另一个容器中



#### 5.1.1 for_each

功能描述：

- 实现遍历容器

函数原型：

- `for_each(iterator beg, iterator end, _func);`
  - 遍历算法 遍历容器元素
  - beg 开始迭代器
  - end 结束迭代器
  - _func 函数或者函数对象

示例：

```cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

//遍历 for_each

//普通函数
void print01(int val) {
	cout << val << " ";
}

//仿函数
class print02 {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};

void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}

	for_each(v.begin(), v.end(),print01);
	cout << endl;

	for_each(v.begin(), v.end(), print02());
	cout << endl;

}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：for_each在实际开发中是最常用遍历算法，需要熟练掌握



#### 5.1.2 transform

功能描述：

- 搬运容器到另一个容器

函数原型：

- `transfrom(iterator beg1, iterator end1, iterator beg2, _func)`
  - beg1 原容器开始迭代器
  - end1 原容器结束迭代器
  - beg2 目标容器开始迭代器
  - _func 函数或者函数对象

示例：

```cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

//遍历算法 transform


class Transform {
public:
	int operator()(int val) {
		return val + 1000;
	}
};

class print {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};

void test01() {
	vector<int> v1;
	v1.push_back(10);
	v1.push_back(50);
	v1.push_back(30);
	v1.push_back(40);

	vector<int> v2;
	//目标容器需要提前开辟空间
	v2.resize(v1.size());
	transform(v1.begin(), v1.end(), v2.begin(), Transform());
	for_each(v2.begin(), v2.end(), print());
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 注意：搬运的目标容器必须要提前开辟空间，否则无法正常搬运



### 5.2 常用查找算法

学习目标：

- 掌握常用的查找算法

算法简介：

- `find`                      //查找元素
- `find_if`                 //按条件查找元素
- `adjacent_find`       //查找相邻重复元素
- `binary_search`       //二分查找法
- `count`                     //统计元素个数
- `count_if`                //按条件统计元素个数



#### 5.2.1 find

功能描述：

- 查找指定元素，找到返回指定元素的迭代器，找不到返回结束迭代器

函数原型：

- `find(iterator beg, iterator end, value);`
  - 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置
  - beg 开始迭代器
  - end 结束迭代器
  - value 查找的元素

示例：

```cpp
#include<iostream>
#include<vector>
#include<string>
#include<algorithm>
using namespace std;

//查找 find

class Person {
public:
	string m_name;
	int m_age;

	Person(string name, int age) {
		this->m_name = name;
		this->m_age = age;
	}
	
	//重写==操作符，为了底层 find 如何对比
	//可以写入一个元素查找，或者直接输入自定义类型查找
	//看个人需求
	bool operator==(const string name) {
		return this->m_name == name;
	}

};

//内置数据类型查找
void test01() {
	vector<int> v;
	v.push_back(10);
	v.push_back(20);
	v.push_back(30);
	v.push_back(40);
	v.push_back(50);
	
	//利用迭代器接收查找到的数据
	vector<int>::iterator it = find(v.begin(), v.end(), 40);

	if (it != v.end()) {
		cout << "找到了：" << *it << endl;
	}
	else {
		cout << "没有找到" << endl;
	}
}

//自定义数据类型查找
void test02() {
	vector<Person> v;

	//创建数据
	Person p1("张三", 10);
	Person p2("李四", 20);
	Person p3("王五", 30);
	Person p4("赵六", 10);

	//加入容器
	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);

	//查找
	//若不重写，find 不知道比较哪一个元素
	vector<Person>::iterator it = find(v.begin(), v.end(), "李四");

	if (it != v.end()) {
		cout << "找到了，姓名：" << (*it).m_name << " 年龄：" << (*it).m_age << endl;
	}
	else {
		cout << "没找到" << endl;
	}
}


//主函数
int main() {
	test01();
	test02();
	system("pause");
	return 0;
}
```

> 总结：利用find可以在容器中找到指定的元素，返回值是**迭代器**



#### 5.2.2 find_if

功能描述：

- 按条件查找元素

函数原型：

- `find_if(iterator beg, iterator end, _Pred);`
  - 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置
  - beg 开始迭代器
  - end 结束迭代器
  - _Pred 函数或者谓词（返回bool 类型的仿函数）

示例：

```cpp
#include<iostream>
#include<vector>
#include<string>
#include<algorithm>
using namespace std;

//按条件查找元素 find_if

//自定义数据类型
class Person {
public:
	string m_name;
	int m_age;
	Person(string name, int age) {
		this->m_name = name;
		this->m_age = age;
	}
};

//写一个谓词（内置数据类型判断）
class GreatThirty {
public:
	bool operator()(int val) {
		return val > 30;
	}
};

//换一种写法，用函数（自定义数据类型判断）
bool DIY_GreatTwenty(const Person& p) {
	return p.m_age > 20;
}


//内置数据类型
void test01() {
	vector<int> v;
	v.push_back(10);
	v.push_back(50);
	v.push_back(40);
	v.push_back(30);

	vector<int>::iterator it = find_if(v.begin(), v.end(), GreatThirty());

	if (it != v.end()) {
		cout << "找到了 大于30 的元素为：" << (*it) << endl;
	}
	else {
		cout << "没有找到" << endl;
	}
}

//自定义数据类型
void test02() {
	vector<Person> v;

	Person p1("张三", 10);
	Person p2("李四", 20);
	Person p3("王五", 30);
	Person p4("赵六", 40);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);

	vector<Person>::iterator it = find_if(v.begin(), v.end(), DIY_GreatTwenty);

	if (it != v.end()) {
		cout << "找到了 大于20 的元素,姓名为：" << (*it).m_name << " 年龄为：" << (*it).m_age << endl;
	}
	else {
		cout << "没有找到" << endl;
	}
}

//主函数
int main() {
	test01();
	test02();
	system("pause");
	return 0;
}
```



#### 5.2.3 adjacent_find

功能描述：

- 查找相邻重复元素

函数原型：

- `adjacent_find(iterator beg, iterator end);`
  - 查找相邻重复元素，返回相邻元素的第一个位置的迭代器
  - beg 开始迭代器
  - end 结束迭代器

示例：

```cpp
#include<iostream>
#include<vector>
#include<string>
#include<algorithm>
using namespace std;

//查找相邻重复元素 adjacent_find
void test01() {
	vector<int> v;
	v.push_back(10);
	v.push_back(20);
	v.push_back(10);
	v.push_back(30);
	v.push_back(30);
	v.push_back(40);

	vector<int>::iterator it = adjacent_find(v.begin(), v.end());

	if (it != v.end()) {
		cout << "找到了相邻重复元素：" << (*it) << endl;
	}
	else {
		cout << "没有找到" << endl;
	}
}


//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 5.2.4 binary_search

功能描述：

- 查找指定元素是否存在

函数原型：

- `bool binary_search(iterator beg, iterator end, value);`
  - 查找指定元素，查到 返回true，否则false
  - 注意：在**无序序列中不可用！**，而且只能用于升序序列例如`set`容器
  - beg 开始迭代器
  - end 结束迭代器
  - value 查找的元素

示例：

```cpp
#include<iostream>
#include<set>
#include<vector>
#include<algorithm>
using namespace std;

//二分查找指定元素是否存在 binary_search
void test01() {
	set<int> s;
	s.insert(20);
	s.insert(50);
	s.insert(30);
	s.insert(10);
	s.insert(40);

	if (binary_search(s.begin(), s.end(),50)) {
		cout << "在 set 容器中找到了50" << endl;
	}
	else {
		cout << "在 set 容器中没有找到" << endl;
	}

	//试用 binary_search 查找无序容器
	vector<int> v;
	v.push_back(50);
	v.push_back(40);
	v.push_back(30);
	v.push_back(20);

	if (binary_search(v.begin(), v.end(), 50)) {
		cout << "在 vector 容器中找到了50" << endl;
	}
	else {
		cout << "在 vector 容器中没有找到" << endl;
	}
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：二分查找法效率很高，值得注意的是查找的容器中元素必须是有序序列



#### 5.2.5 count

功能描述：

- 统计元素个数

函数原型：

- `count(iterator beg, iterator end, value);`
  - 统计元素出现次数
  - beg 开始迭代器
  - end 结束迭代器
  - value 查找的元素

示例：

```cpp
#include<iostream>
#include<vector>
#include<string>
#include<algorithm>
using namespace std;

//统计元素个数 count

//自定义数据类型
class Person {
public:
	string m_name;
	int m_age;
	Person(string name, int age) {
		this->m_name = name;
		this->m_age = age;
	}

	//重载==符号，方便元素查找
	bool operator==(const int val) {
		return this->m_age == val;
	}
};

//内置数据类型
void test01() {
	vector<int> v;
	v.push_back(10);
	v.push_back(20);
	v.push_back(10);
	v.push_back(40);
	v.push_back(10);

	int sum = count(v.begin(), v.end(), 10);
	cout << "容器中10的个数为：" << sum << endl;
}

//自定义数据类型
void test02() {
	vector<Person> v;

	Person p1("张三", 10);
	Person p2("李四", 20);
	Person p3("王五", 30);
	Person p4("赵六", 20);
	Person p5("贾七", 20);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	v.push_back(p5);

	cout << "年龄是20岁的人的个数是：" << count(v.begin(), v.end(), 20) << endl;
}

//主函数
int main() {
	test01();
	test02();
	system("pause");
	return 0;
}
```

> 总结：统计自定义数据类型时候，需要配合`operator==`



#### 5.2.6 count_if

功能描述：

- 按条件统计元素个数

函数原型：

- `count_if(iterator beg, iterator end, _Pred);`
  - 按条件统计元素出现次数
  - beg 开始迭代器
  - end 结束迭代器
  - _Pred 谓词

示例：

```cpp
#include<iostream>
#include<vector>
#include<string>
#include<algorithm>
using namespace std;

//按条件统计元素个数

//谓词（寻找小于30）
class LessThirty {
public:
	bool operator()(int val) {
		return val < 30;
	}
};

//统计内置数据类型
void test01() {
	vector<int> v;
	v.push_back(10);
	v.push_back(20);
	v.push_back(20);
	v.push_back(30);
	v.push_back(40);

	int sum = count_if(v.begin(), v.end(), LessThirty());

	cout << "小于30的数有 " << sum << " 个" << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



### 5.3 常用排序算法

算法简介：

- `sort`                           //对容器内元素进行排序
- `random_suffle`           //洗牌 指定范围内的元素随机体哦阿正次序
- `merge`                         //容器元素合并，并存储到另一容器中
- `reverse`                      //反转指定范围的元素



#### 5.3.1 sort

功能描述：

- 对容器内元素进行排序

函数原型：

- `sort(iterator beg, iterator end, _Pred);`
  - 对容器内元素进行排序
  - beg 开始迭代器
  - end 结束迭代器
  - _Pred 谓词

示例：

```cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

//排序 sort

//打印输出
void PriVec(int val) {
	cout << val << " ";
}

//创建二元谓词来降序排列
class Descending {
public:
	bool operator()(int val1, int val2) {
		return val1 > val2;
	}
};

void test01() {
	vector<int> v;
	v.push_back(1);
	v.push_back(4);
	v.push_back(5);
	v.push_back(2);
	v.push_back(3);

	//常规排序
	sort(v.begin(), v.end());
	for_each(v.begin(), v.end(), PriVec);
	cout << endl;

	//降序排列
	sort(v.begin(), v.end(), Descending());
	for_each(v.begin(), v.end(), PriVec);
	cout << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 5.3.2 random_shuffle

功能描述：

- 洗牌 知道那个范围内的元素随机调整次序

函数原型：

- `random_shuffle(iterator beg, iterator end);`
  - 指定范围内的元素随机调整次序
  - beg 开始迭代器
  - end 结束迭代器

示例：

```cpp
#include<iostream>
#include<vector>
#include<algorithm>
#include<ctime>
using namespace std;

//指定范围内的元素随机调整次序

//打印输出
class PriVec {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};

void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}

	cout << "原顺序：";
	for_each(v.begin(), v.end(), PriVec());
	cout << endl;

	//注意：虽然打乱了，但是每次运行的相同次都是一样的
	//可以加一个随机数种子完成真实打乱
	cout << "第一次洗牌：";
	random_shuffle(v.begin(), v.end());
	for_each(v.begin(), v.end(), PriVec());
	cout << endl;
	cout << "第二次洗牌：";
	random_shuffle(v.begin(), v.end());
	for_each(v.begin(), v.end(), PriVec());
	cout << endl;

}


//主函数
int main() {
	//随机数种子
	srand((unsigned int)time(NULL));
	test01();
	system("pause");
	return 0;
}
```

> 总结：random_shuffle洗牌算法比较实用，使用时记得加随机数种子



#### 5.3.3 merge

功能描述：

- 两个元素合并，并存储到零一容器中

函数原型：

- `merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`
  - 容器元素合并，并存储到另一种容器中
  - 注意：两个容器必须是有序的
  - beg1 容器1开始迭代器
  - end1 容器1结束迭代器
  - beg2 容器2开始迭代器
  - end2 容器2结束迭代器
  - dest 目标容器开始迭代器

示例：

```cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

//两个容器元素合并，并存储到零一容器中 merge

//打印
class Print {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};

void test01() {

	vector<int> v1;
	v1.push_back(2);
	v1.push_back(3);
	v1.push_back(4);
	v1.push_back(5);

	vector<int> v2;
	v2.push_back(1);
	v2.push_back(7);
	v2.push_back(8);

	vector<int> vTarget;
	//扩容
	vTarget.resize(v1.size() + v2.size());
	
	merge(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());
	for_each(vTarget.begin(), vTarget.end(), Print());
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 5.3.4 reverse

功能描述：

- 将容器内元素进行反转

函数原型：

- `reverse(iterator beg, iterator end);`
  - 反转反转范围的元素
  - beg 开始迭代器
  - end 结束迭代器

示例：

```cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

//将容器内元素进行反转 reverse

//打印
class myPrint {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};

void test01() {
	vector<int> v;
	v.push_back(1);
	v.push_back(2);
	v.push_back(3);
	v.push_back(4);

	cout << "原容器：";
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
	cout << "反转后的容器：";
	reverse(v.begin(), v.end());
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}


//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



### 5.4 常用拷贝和替换算法

算法简介：

- `copy`                   //容器内指定元素拷贝到另一容器中
- `replace`              //将容器内指定范围的就元素修改为新元素
- `replace_if`         //容器内指定范围满足条件的元素替换为新元素
- `swap`                    //互换两个容器的元素



#### 5.4.1 copy

功能描述：

- 容器内指定元素拷贝到另一容器中

函数原型：

- `copy(iterator beg, iterator end, iterator dest);`
  - beg 开始迭代器
  - end 结束迭代器
  - dest 目标迭代器

示例：

```cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

//容器内指定范围的元素拷贝到另一容器中

//打印
class MyPrint {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};

void test01() {
	vector<int> v1;
	v1.push_back(1);
	v1.push_back(2);
	v1.push_back(3);
	v1.push_back(4);

	vector<int> v2;
	v2.resize(v1.size());

	copy(v1.begin(), v1.end(), v2.begin());

	for_each(v2.begin(), v2.end(), MyPrint());

	//深拷贝
	v1.clear();

	for_each(v2.begin(), v2.end(), MyPrint());

}


//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：利用copy算法在拷贝时，目标容器记得提前开辟空间



#### 5.4.2 replace

功能描述：

- 将容器内只当范围的就元素修改为新元素

函数原型：

- `replace(iterator beg, iterator end, oldvalue, newvalue);`
  - 将区间内旧元素 替换成 新元素
  - beg 开始迭代器
  - end 结束迭代器
  - oldvalue 旧元素
  - newvalue 新元素

示例：

```cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

//将容器内只当范围的就元素修改为新元素 replace

//打印输出
void myPrint(int val) {
	cout << val << " ";
}

void test01() {
	vector<int> v;
	v.push_back(10);
	v.push_back(20);
	v.push_back(10);
	v.push_back(30);
	v.push_back(10);

	for_each(v.begin(), v.end(), myPrint);
	cout << endl;

	//注意：将所有的指定值都替换
	replace(v.begin(), v.end(), 10, 50);

	for_each(v.begin(), v.end(), myPrint);
	cout << endl;
}


//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：replace 会替换区间内满足条件的元素



#### 5.4.3 replace_if

功能描述：

- 将区间内满足条件的元素，替换成指定元素

函数原型：

- `replace_if(iterator beg, iterator end, _Pred, newvalue);`
  - 按条件替换元素，满足条件的替换成指定元素
  - beg 开始迭代器
  - end 结束迭代器
  - _Pred 谓词
  - newvalue 新元素

示例：

```cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

//将区间内满足条件的元素，替换成指定元素 replace_if

//谓词（小于6）
class LessSix {
public:
	bool operator()(int val) {
		return val < 6;
	}
};

//打印
void myPrint(int val) {
	cout << val << " ";
}

void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	for_each(v.begin(), v.end(), myPrint);
	cout << endl;

	//将小于6的数全部替换为100
	replace_if(v.begin(), v.end(), LessSix(), 100);

	for_each(v.begin(), v.end(), myPrint);
	cout << endl;

}


//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



#### 5.4.4 swap

功能描述：

- 互换两个容器的元素

函数原型：

- `swap(container c1, container c2);`
  - 互换两个容器
  - c1 容器1
  - c2 容器2

示例：

```cpp
#include<iostream>
#include<stdio.h>
#include<vector>
#include<algorithm>
using namespace std;

//互换两个容器的元素 swap

//打印
class myPrint {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};

void test01() {

	vector<int> v1;
	for (int i = 0; i < 10; ++i) {
		v1.push_back(i);
	}
	cout << "v1:";
	for_each(v1.begin(), v1.end(), myPrint());
	cout << endl;
	vector<int> v2;
	for (int i = 5; i > 0; --i) {
		v2.push_back(i);
	}
	cout << "v2:";
	for_each(v2.begin(), v2.end(), myPrint());
	cout << endl;

	//互换容器
	swap(v1, v2);

	cout << "互换容器后：" << endl;

	cout << "v1:";
	for_each(v1.begin(), v1.end(), myPrint());
	cout << endl;

	cout << "v2:";
	for_each(v2.begin(), v2.end(), myPrint());
	cout << endl;
}


//主函数、
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：同种容器类型才可交换



### 5.5 常用算术生成算法

注意：

- 算术生成算法属于小型算法，使用时包含的头文件为`#include<numeric>`

算法简介：

- `accumulate`        //计算容器元素累计总和
- `fill`                   //向容器中添加元素



#### 5.5.1 accumulate

功能描述：

- 计算区间内 容器元素累积总和

函数原型：

- `accumulate(iterator beg, iterator end, value);`
  - 计算容器元素累积总和
  - beg 开始迭代器
  - end 结束迭代器
  - value 起始值

示例：

```cpp
#include<iostream>
#include<vector>
#include<numeric>
using namespace std;

//计算容器元素累积总和 accumulate

void test01() {
	vector<int> v;
	for (int i = 1; i <= 100; ++i) {
		v.push_back(i);
	}
    //参数3 是一个起始的累加值
	int sum = accumulate(v.begin(), v.end(), 10000);
	cout << "v中元素累加为: " << sum << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```

> 总结：accumulate使用时头文件注意是numeric



#### 5.5.2 fill

功能描述：

- 向容器中填充指定的元素

函数原型：

- `fill(iterator beg, iterator end, value);`
  - 向容器中填充元素
  - beg 开始迭代器
  - end 结束迭代器
  - value 填充的值

示例：

```cpp
#include<iostream>
#include<vector>
#include<algorithm>
#include<numeric>
using namespace std;

//向容器中填充元素

//打印输出
class myPrint {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};

void test01() {
	vector<int> v;
	v.resize(10);
	fill(v.begin(), v.end(), 10);
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;

	fill(v.begin() + 1, v.end() - 1, 6);
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



### 5.6 常用集合算法

算法简介：

- `set_intersection`        //求两个容器的交集
- `set_union`                    //求两个容器的并集
- `set_difference`            //求两个容器的差集



#### 5.6.1 set_intersection

功能描述：

- 求两个容器的交集

函数原型：

- `set_intersection(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`
  - 求两个集合的交集
  - 注意：**两个集合必须是有序序列**
  - beg1 容器1开始迭代器
  - end1 容器1结束迭代器
  - beg2 容器2开始迭代器
  - end2 容器2结束迭代器
  - dest 目标容器开始迭代器

#### 5.6.2 set_union

功能描述：

- 求两个集合的并集

函数原型：

- `set-union(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest)`
  - 求两个集合的并集
  - 注意：**两个集合必须是有序序列**
  - beg1 容器1开始迭代器
  - end1 容器1结束迭代器
  - beg2 容器2开始迭代器
  - end2 容器2结束迭代器
  - dest 目标容器开始迭代器

#### 5.6.3 set_difference

功能描述：

- 求两个集合的差集

函数原型：

- `set-fidderence(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest)`
  - 求两个集合的差集
  - 注意：**两个集合必须是有序序列**
  - beg1 容器1开始迭代器
  - end1 容器1结束迭代器
  - beg2 容器2开始迭代器
  - end2 容器2结束迭代器
  - dest 目标容器开始迭代器



#### 5.6.4 示例

```cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

//打印输出
class myPrint {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};

//求两个容器的交集

void test01() {
	vector<int> v1;
	v1.push_back(1);
	v1.push_back(2);
	v1.push_back(3);
	v1.push_back(4);

	vector<int> v2;
	v2.push_back(3);
	v2.push_back(4);
	v2.push_back(5);
	v2.push_back(6);

	cout << "v1: ";
	for_each(v1.begin(), v1.end(), myPrint());
	cout << endl;

	cout << "v2: ";
	for_each(v2.begin(), v2.end(), myPrint());
	cout << endl;

	//目标容器提前开辟空间
	vector<int> vTarget1;
	//最特殊的情况是是大容器包含小容器
	vTarget1.resize(min(v1.size(), v2.size()));

	//获取交集
	vector<int>::iterator itEnd1 =  set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget1.begin());
	cout << "v1与v2的交集：";
	for_each(vTarget1.begin(), itEnd1, myPrint());
	cout << endl;

	//获取并集
	vector<int> vTarget2;
	//最特殊的情况是每个容器的值都不相同
	vTarget2.resize(v1.size() + v2.size());
	vector<int>::iterator itEnd2 = set_union(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget2.begin());
	cout << "v1与v2的并集：";
	for_each(vTarget2.begin(), itEnd2, myPrint());
	cout << endl;
	
	//获取差集
	vector<int> vTarget3;
	//最特殊的情况是每个容器的值都不相同
	vTarget3.resize(max(v1.size(), v2.size()));
	vector<int>::iterator itEnd3 = set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget3.begin());
	cout << "v1与v2的差集：";
	for_each(vTarget3.begin(), itEnd3, myPrint());
	cout << endl;

}

//主函数
int main() {
	test01();
	system("pause");
	return 0;
}
```



