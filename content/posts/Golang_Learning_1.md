---
title: "Golang 语言笔记(1)"
date: 2024-03-10T16:04:04+08:00
draft: true
taps: []
categories: []
author: ["Yeelight"]
showtoc: true
weight:
---

## 基本类型 ：
- Int（int8，int16，int32，int64）
- Uint（uint 8，uint 16，uint 32，uint 64）
- Bool
- String
- Byte (uint8 的别名)
- Rune（int32 的别名一个 unicode）
- Float 32，float 64
- Complex 32，complex 64 （虚数）
## 变量

在Go语言中，变量使用 `var` 关键字进行声明。变量可以在声明时进行初始化，也可以单独声明后再进行赋值。另外，Go语言还支持使用 `:=` 运算符进行短变量声明，用于在函数内部快速声明并初始化变量。
示例：
```go
var name type
var name = 1
var (
	a int
	b bool
	c string
)
i := 1 //只能用在局部函数
```

### 匿名变量
在 Go 语言中，匿名变量用下划线 `_` 表示。它通常用于占位，表示一个不需要的值。在函数返回多个值时，如果你只关心其中的某些值，可以使用匿名变量来忽略其他值。

示例：
```go
_, err := SomeFunction() // 忽略第一个返回值
```

## 常量

在 Go 语言中，常量使用 `const` 关键字进行声明。常量在声明时必须进行初始化，并且一旦被赋值后就不能再次修改。常量的命名规则与变量相同，使用驼峰命名法或全大写命名法。

示例：
```go
const Pi = 3.14159
const （
	a := 10
	b := 20
）
```

## ASCLL 码
- 用 `\x` 表示十六进制
- 用 `\` 表示八进制
- 用 `\u` 表示 unicode

## Go 语言中的数据结构
### 基础数据结构

> 1. **[[数组（Array）]]** - 由固定长度的相同类型元素组成的数据结构。
> 2. **切片（Slice）** - 由数组构成的动态长度序列，提供了更灵活的操作方式。
> 3. **映射（Map）** - 存储键值对的集合，用于快速检索数据。
> 4. **结构体（Struct）** - 可以包含不同类型字段的复合数据类型。

#### 数组（Array）
#### 切片 （Slice）
#### 映射 （Map）
#### 结构体 （Struct）


### 其他数据结构和类型

> 1. **通道（Channel）** - 用于在 Go 协程之间进行通信的类型。
> 2. **接口（Interface）** - 定义对象的行为，是一种抽象类型。
> 3. **指针（Pointer）** - 存储变量的内存地址，用于直接访问内存中的值。

#### 通道（Channel）
#### 接口（Interface）
#### 指针（Pointer）


## 命名规范

注意：
> **首字母大小写：** 以大写字母开头的标识符是 **public** 的（可导出的），可以被其他包访问。以小写字母开头的标识符是私有的，只能在当前包内访问。

在 Go 语言中，有一些命名规范适用于不同的命名情况。以下是一些常见的命名规范：

1. **包名**：包名应该使用单数形式，且应该是小写的，例如 `utils`。
2. **文件名**：文件名应该全部使用小写字母，可以包含下划线 `_`，例如 `my_file.go`。
3. **变量**：变量名使用驼峰命名法，例如 `myVariable`。私有变量的命名应该以小写字母开头，公共变量则以大写字母开头。
4. **常量**：常量的命名应该全部使用大写字母，可以包含下划线 `_`，例如 `MAX_SIZE`。
5. **函数**：函数名同样使用驼峰命名法，例如 `calculateTotal`。
6. **结构体**：结构体的命名同样使用驼峰命名法，例如 `type MyStruct struct`。
7. **接口**：接口的命名同样使用驼峰命名法，例如 `type MyInterface interface`。
8. **枚举**：枚举的命名同样使用驼峰命名法，例如 `type Color int`。


在 Go 语言中，命名变量时有一些常见的命名规范，这些规范有助于编写清晰、易读的代码。

1. 使用驼峰命名法：变量名应该使用驼峰命名法，即除第一个单词外，其余单词的首字母大写，例如 `myVariable`。
2. 使用有意义的名称：变量名应该具有描述性，能够清晰地表达变量的用途和含义。
3. 避免使用单个字符作为变量名：除非是临时变量或者循环变量，否则应该避免使用单个字符作为变量名，以提高代码的可读性。
4. 使用短小的名称：变量名应该尽量简洁，但又能清晰表达变量的含义。
5. 使用全大写命名的常量：在 Go 语言中，全大写的变量名通常用于表示常量。
6. 遵循约定俗成的命名规范：Go 语言社区有一些常见的命名约定，比如用 `i` 表示循环变量，用 `err` 表示错误变量等，建议遵循这些约定以保持代码风格的一致性。
