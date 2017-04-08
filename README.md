#（一）基础知识与prototype继承方法
##基础知识
`原型`：每个对象的初始定义叫做原型，例如Object对象初始定义了prototype、tostring、isPrototypeOf等属性或方法，当实例化时会自动在内存中开辟空间，实现这些已经初始定义过的属性或方法
`实例`：程序使用原型对象时，生成的对象叫作实例（instance），生成对象的过程叫做实例化
Object中包含的properity，
properity中包含的__proto__，constructor

所有的原型都指向Object.properity，Object指向null

##原型链的实现

