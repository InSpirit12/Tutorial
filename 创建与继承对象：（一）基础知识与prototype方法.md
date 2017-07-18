# 创建与继承对象：（一）基础知识与prototype方法
## 基础知识
`原型对象（Object Prototype）`：每个对象的初始定义叫做原型，例如Object对象初始定义了prototype、tostring、isPrototypeOf等属性或方法，当实例化时会自动在内存中开辟空间，实现这些已经初始定义过的属性或方法  
原型对象包含了constructor属性，指向构造函数（Object）  
`实例（o）`：程序使用原型对象时，生成的对象叫作实例（instance），生成对象的过程叫做实例化，实例里面有一个__proto__属性是指针,指向原型对象（Object Prototype）  
实例化常用的方法有new，对象字面量，Object.create()   

`构造函数（Object）`：构造函数中有一个prototype属性是指针，指向原型对象（Object Prototype）  

所有的原型最终都指向Object原型，Object原型中的constructor指针属性指向null  

## 创建对象
### 工厂模式
通过工厂模式写原型，通过对象字面量方式实例化对象   
```
function test (name, age) {
    var o = new Object ();
    o.name = name;
    o,age = age;
    return o;
}
var person1 = test('john', 20);
```

### 构造函数
构造函数其实也是普通函数，可以被普通使用，只有当被new方法调用时，才成为了构造函数，在js中构造函数是引用，而非像类中那样复制  
Js中预先定义了一些原型，例如Object、Array,string等  
通过new关键字来调用构造函数，实例化对象，分为如下四步：  
（1）	创建一个新对象  
（2）	将构造函数的作用域赋给新对象  
（3）	执行构造函数中的代码  
（4）	返回新对象  
实例化完毕后，新对象内部会有一个o.constructor（构造函数）属性，指向自身的构造函数  
```
function Person (name, age) {
	this.name = name;
	this.age = age;
	this.sayName = function () {
		console.log(this.name);
	}
}
var person1 = new Person('wang', 29);
```
`缺点`：  
无法复用，每次new都会占用空间新建一套  

### 原型
```
function Person () {
	Person.prototype.name = 'wang';
	Person.prototype.age = 29;
	Person.prototype.sayName = function () {
		console.log(this.name);
	}
}
var person1 = new Person();
```
`缺点`：  
（1）原型方法无法传递参数   
（2）一个子实例对父原型的属性进行修改后，修改的结果会被所有其他继承该原型的子实例共享  
例如：   
```
javascript
function SuperType () {
  this.colors = ['red', 'blue', 'yellow'];
}

function SubType () {
}

SubType.prototype = new SuperType();

var instance1 = new SubType;
var instance2 = new SubType;
instance1.colors.push('black');

console.log(instance1.colors);  //red,blue,yellow,black
console.log(instance2.colors);  //red,blue,yellow,black
```

简单的原型语法，结合对象字面量方法创建 
`注意`：当使用prototype方法继承后，不能用对象字面量方法创建原型，因为对象字面量默认从Object继承，会重写原型链，切断原来的继承关系  
```
Person.prototype = {
	name: 'wang',
	age: 29,
	sayName: function() {
		console.log(this.name);
	}
}
```
该方法会使constructor不再指向构造函数Person  
如果还需使用constructor，在对象字面量里面加上  
```
Person.prototype = {
  constructor: Person,
	name: 'wang',
	age: 29,
	sayName: function() {
		console.log(this.name);
	}
}
```

### 组合使用构造函数与原型
`目前最主流的方式`  
将`定义的方法`与`需要共享的属性`放到原型中，将`实例属性`放到构造函数中  
```
function Person (name, age) {
	this.name = name;
	this.age = age;
	Person.prototype.sayName = function () {
		console.log(this.name);
	}
}
var person1 = new Person('wang', 29);
```
### 动态原型模式

### 寄生构造函数模式
和工厂函数基本一样  
### 稳妥构造函数模式

## 继承
### 原型链
同上，有问题1、2  
### 借用构造函数
问题1的解决：通过call和apply传递参数  
```
function Father (name) {
	this.name = name;
}
function Son () {
	Father.call(this, 'wang');
}
var instance1 = new Son();
console.log(instance1.name);
```
问题2的解决：在子类的构造函数中调用父类的构造函数，可以避免原型链中子实例共享的情况  
```
function Father () {
	this.colors = ['red', 'blue', 'yellow'];
}
function Son () {
	Father.call(this);
}
var instance1 = new Son();
var instance2 = new Son();
instance1.colors.push('black');
console.log(instance1.colors);	//'red', 'blue', 'yellow', 'black'
console.log(instance2.colors);	//'red', 'blue', 'yellow'
```
### 组合继承
原型链和借用构造函数的组合，通过原型链实现对`原型属性和方法`的继承，通过借用构造函数实现对`实例属性和方法`的继承  

### 原型式继承

### 寄生式继承

### 寄生组合式继承





