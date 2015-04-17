---
layout: post
categories: style-guide
---

- Javascript 语言规范
  - var
  - 常量
  - 分号
  - 嵌套函数
  - 块内函数声明
  - Exceptions
  - Wrapper objects for primitive types
  - Multi- level prototype hierarchies多级原型链
  - 方法和属性的定义
  - delete
  - 闭包
  - eval()
  - with() {}
  - this
  - for- in 循环
  - 关联数组
  - Multiline string literals
  - 数组和对象字面量
  - 修改内建对象的原型
  - IE的条件注释 Conditional Comments
- Javascript 编码规范
  - 命名
  - Custom toString() methods
  - Deferred 初始化
  - 明确作用域
  - 代码格式化
  - Parentheses
  - 字符串
  - Visibility（私有域和保护域）
  - Javascript 类型
  - 注释
  - Providing Dependencies With goog.provide
  - 编译
  - Tips and Tricks

# Javascript 语言规范

## var

总是使用var声明变量

## 常量

常量值用 NAMES_LIKE_THIS 形式命名
使用 @const 来表示一个常量（不可覆盖）指针（变量或属性）
不要使用 const 关键字，IE下不支持
方法使用 @const 注释表明该方法在子类中不可覆盖
构造函数使用 @const 注释表明该类不能被继承（类似于 Java 中的 final）

## 分号

总是使用分号

函数表达式 vs. 函数声明
{% highlight javascript %}
var foo = function() {
  return true;
};  // semicolon here.

function foo() {
  return true;
}  // no semicolon here.
{% endhighlight %}

```javascript
var foo = function() {
  return true;
};  // semicolon here.

function foo() {
  return true;
}  // no semicolon here.
```

## 嵌套函数

可以使用嵌套函数，在创建 continuations 和隐藏帮助函数时非常有用

## 块内函数声明

不要这么声明

if (x) {
  function foo() {}
}

在块内定义函数的正确方法：使用一个被函数表达式初始化的变量

if (x) {
  var foo = function() {};
}


## Exceptions Yes

## 自定义 Exceptions Yes

## Standards features
尽量使用 standards features 而不是 non-standards features
例如 string.charAt(3) 而不是 string[3], element access with DOM functions instead of using an application-specific shorthand

## Wrapper objects for primitive types No

不要这么定义
var x = new Boolean(false);
if (x) {
  alert('hi');  // Shows 'hi'.
}


推荐强制类型转换（type casting）

var x = Boolean(0);
if (x) {
  alert('hi');  // This will never be alerted.
}
typeof Boolean(0) == 'boolean';
typeof new Boolean(0) == 'object';

将变量转换成 number，string  和 boolean 时非常有用

## 多级原型结构 不推荐
Javascript 使用多级原型结构实现继承。用户自定义类D使用用户自定义类B作为其原型，则出现多级结构。推荐使用库函数实现该功能。
function D() {
  goog.base(this)
}
goog.inherits(D, B);

D.prototype.method = function() {
  ...
};


## 方法和属性的定义

定义方法的推荐方式：
Foo.prototype.someMethod = function() {
  /* ... */
};
定义属性的推荐方式：在构造函数中初始化域
/** @constructor */
function Foo() {
  this.someProperty = value;
}

原因：Javascript 引擎优化基于 object 的 “shape”，向一个object添加一个属性（包括覆盖prototype中设置的值）改变了对象的形状，降低性能。
## delete

Foo.prototype.dispose = function() {
  this.property_ = null;
};
Instead of:

Foo.prototype.dispose = function() {
  delete this.property_;
};
原因：现代Javascript引擎中，改变一个对象的属性的数量比给属性重新赋值低得多。keyword在以下情况中使用：有必要删除对象的属性以改变对象迭代时的key

## 闭包 小心使用
One thing to keep in mind, however, is that a closure keeps a pointer to its enclosing scope. As a result, attaching a closure to a DOM element can create a circular reference and thus, a memory leak. For example, in the following code:

function foo(element, a, b) {
  element.onclick = function() { /* uses a and b */ };
}
the function closure keeps a reference to element, a, and b even if it never uses element. Since element also keeps a reference to the closure, we have a cycle that won't be cleaned up by garbage collection. In these situations, the code can be structured as follows:

function foo(element, a, b) {
  element.onclick = bar(a, b);
}

function bar(a, b) {
  return function() { /* uses a and b */ };
}






# Javascript 编码规范