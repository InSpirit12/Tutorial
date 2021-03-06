# function对象

## function基础
function是一个object对象,所以继承了Object中的属性与方法  
函数内部有arguments和this两个对象，arguments是个类数组对象（不是array的实例但功能相似），存储了函数的所有参数，this指向调用该function的作用域  
function没有重载，后定义的函数会覆盖先定义的函数，但可以通过判断arguments.length，typeof（参数）的方法来模拟重载，不过也没什么必要  

## 传递参数
两种都存储在栈内存，  区别是：
`基本类型`存储的是值，在栈内存中存储  
`引用类型`存储的是对象实际存储内存的地址，在栈内存中存储，而对象实际存储内存在堆内存中  
堆的优势是可以动态分配内存大小，生存期也不必事先告诉编译器，因为它是在运行时动态分配内存的。缺点就是要在运行时动态分配内存，存取速度较慢  
栈的优势是，存取速度比堆要快，仅次于直接位于CPU中的寄存器。另外，栈数据可以共享。但缺点是，存在栈中的数据大小与生存期必须是确定的，缺乏灵活性  
函数的参数基本类型，也可以是对象，而且`传递参数（arguments）都是按值传递的`，也就是说基本类型按值传递，对象也是按值传递，也就是说会把对象在内存中的地址赋值给arguments的其中一项，所以当该地址对应的内存存储内容变化后，函数对应的参数值也会随着变化  
从此可以引申出，`引用传递其实是值传递的特例`  

自动更新
arguments 对象为其内部属性以及函数形式参数创建 getter 和 setter 方法，因此，改变形参的值会影响到 arguments 对象的值，反之亦然  
```
function foo(a, b, c) {
  arguments[0] = 2;
  a; // 2                                                           

  b = 4;
  arguments[1]; // 4

  var d = c;
  d = 9;
  c; // 3
}
foo(1, 2, 3);
```

## 函数声明与函数表达式
函数声明：  
解析器会优先解析函数声明，所以函数声明会被提升，可以在任何文件定义和引用而不必担心引用失败报错  
```
console.log(sum); //不会报错
function sum () {
  return 1;
}
```

函数表达式：  

```
console.log(sum); //会报错
var sum = function () {
  return 1;
}
console.log(sum); //不会报错
```
## 函数属性与方法
非继承方法call，apply，设置this对象的指向  
区别：  
```
call(this, num1, num2...);
apply(this, [num1, num2...]);
```
## 闭包
`官方定义`：闭包(Closure)是词法闭包(Lexical Closure)的简称，是引用了自由变量的函数。这个被引用的自由变量将和这个函数一同存在，即使已经离开了创造它的环境也不例外  
闭包其实就是作用域的扩展与对象销毁的知识点，当一个对象被调用时会优先在自身作用域搜索，如果没找到再去上层作用域搜索，直到搜索到全局，若还没找到则返回null  
当一个函数调用了外部对象，且外部对象执行完毕后，理论上外部对象的属性和方法都应该被回收，但如果有其它函数引用了属性和方法，该变量所占内存将不会被释放  
`注意1`：如果闭包调用的是基本类型，按照按值传递的方法，该属性会复制一份到闭包中，如果调用的是对象，按照指针引用方法，内存空间始终是运行时开辟的那块   
`注意2`：过度使用闭包会占用更多内存，故意使用可以使内存溢出  

## this的使用（还需深化）
this的指向可以分为几种情况：  
### 函数调用

### 作为对象方法调用
this指向它的上级对象
### 作为构造函数调用
this指向新对象
### apply调用
this指向第一个参数
