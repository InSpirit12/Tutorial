# （一）基础知识与prototype继承方法
## 基础知识
`原型`：每个对象的初始定义叫做原型，例如Object对象初始定义了prototype、tostring、isPrototypeOf等属性或方法，当实例化时会自动在内存中开辟空间，实现这些已经初始定义过的属性或方法  
`实例`：程序使用原型对象时，生成的对象叫作实例（instance），生成对象的过程叫做实例化  
实例化常用的方法有new，对象字面量，Object.create()  

Object中包含prototype, prototype中又有constructor和__proto__  
prototype.__proto__属性是指针，指向父原型的构造函数  
prototype.constructor是构造函数，实例化时会调用该构造函数  

所有的原型都指向Object.prototype，Object.prototype.__proto__指向null  

##  原型链的实现  

放张图并描述  

`注意`：当使用prototype方法继承后，不能用对象字面量方法创建原型，因为对象字面量默认从Object继承，会重写原型链，切断原来的继承关系  

## 原型链的问题
一个子实例对父原型的属性进行修改后，修改的结果会被所有其他子实例共享  
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

