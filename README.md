# macro-const-static-extern

## macro 和 const 区别

```
// macro
#define OBAccount @"account"
// const
NSString * const OBPassword = @"password";
```

字符串常量，推荐使用 `const`，不建议使用 `macro`。
每使用 `macro` 都会消耗内存？错！`macro` 定义的是常量，只会生成一份内存。

- 编译时刻：`macro` 是预编译（`#` 编译之前处理），`const` 是编译阶段。
- 编译检查：`macro` 不做检查，不会报编译错误，只是替换，`const` 会编译检查，会报编译错误。
- 宏的好处：`macro` 可以定义一些函数、方法，`const` 不可以。
- 宏的坏处：`macro` 大量使用，容易造成编译时间过久，每次都需要重新替换，而且非常难管理。

## const 作用

- 仅仅是用来修饰右边的变量（基本数据类型 `b`，指针类型 `*p`）。
- `const` 修饰的变量是 readonly。

### `const` 修饰基本数据类型

```
const int b = 20; // b：只读变量，不允许修改值。
int const b = 20; // b：只读变量，不允许修改值。
```

### `const` 修饰指针类型

```
int * const p = &a;       // p:只读 *p:可变
const int * p = &a;       // *p:只读 p:可变
int const * p = &a;       // *p:只读 p:可变
int const * const p = &a; // *p:只读 p:只读
const int * const p = &a; // *p:只读 p:只读
```

### const 使用场景

- 定义一个只读的全局变量
- 方法中定义只读参数

```
// 定义只读全局常量
NSString * const string = @"hello world";

// 当一个方法的参数，只读
- (void)test:(NSString * const)a
{

}

// 基本数据类型，只读
- (void)test2:(int const)a
{

}

// 指针类型，只读，不能通过指针修改值
- (void)test1:(int const *)a
{
    // *a = 10;
}
```

## static 作用

`static` 修饰局部变量

- 延长这个局部变量的生命周期，只要成员允许，局部变量就会一直存在。
- 局部变量只会分配一次内存，只会在程序运行时执行一遍。

`static` 修饰全局变量

- 只会修改全局变量的作用域，表示只能是当前文件内使用。


## extern 作用

- 声明一个变量，不能定义变量。
- 修饰的变量不能初始化，一般用于声明全局变量。

## static 和 const 联合使用

`static` 修饰全局变量，只改变作用域。为什么要改变全局变量作用域，防止重复声明全局变量。

```
// 声明一个静态的全局只读常量
static const int a = 20;
```

`staic` 和 `const` 联合的作用：声明一个静态的全局只读常量。

```
// 开发中经常拿到 key 修改值，因此用 const 修饰 key，表示 key 只读，不允许修改，且只能在本文件中使用。
static  NSString * const key = @"name";

// 如果 const修饰 *key1，表示 *key1 只读，key1 还是能改变。
static  NSString const *key1 = @"name";
```

## extern 和 const 联合使用

**多个文件**经常使用同一个字符串常量，可以使用 `extern` 和 `const` 组合。
需要多个文件共享，使用全局，不需要共享使用 `static`。

- .h

```
// 对外声明全局只读变量
extern NSString * const nameKey = @"name";
```

- .m

```
// 全局只读变量
NSString * const nameKey = @"name";
```


