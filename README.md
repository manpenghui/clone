# clone
js的拷贝

## 引子
### 一般来说在JavaScript中有两种数据类型：Primitive（String、Number、Boolean、null、undefinded）和Object（Reference）。在JavaScript中的Object数据是Mutable（可以变的），由于是使用Reference（引用）的方式，所以当修改复制的值也会修改原始值。例如下面的map2值是指到map1，所以当map1值一改，map2的值也会受影响。
```javascript
  var map1 = { a :  1 }; 
  var map2 = map1; 
  map2 . a  =  2
   map1.a  //2
 ```
 ### 那么如何在引用引用对象时，不会出现上面的这种结果呢，这就涉及到js的浅度和深度拷贝的问题；
 
 ##  产生的原因
 ### 首先深复制和浅复制只针对像 Object, Array 这样的复杂对象的。简单来说，浅复制只复制一层对象的属性，而深复制则递归复制了所有层级。因为浅复制只会将对象的各个属性进行依次复制，并不会进行递归复制，而 JavaScript 存储对象都是存地址的，所以浅复制会导致 map1.a 和 map2.a 指向同一块内存地址。
 ### 而深复制则不同，它不仅将原对象的各个属性逐个复制出去，而且将原对象各个属性所包含的对象也依次采用深复制的方法递归复制到新对象上。这就不会存在上面 map1 和 map2 的 a 属性指向同一个对象的问题。
 
 ## 如何解决
 1.源码实现，逐层检测，递归原型链，实现深度拷贝
```javascript
  var cloneObj = function(obj){
    var str, newobj = obj.constructor === Array ? [] : {};
    if(typeof obj !== 'object'){
        return;
    } else if(window.JSON){
        str = JSON.stringify(obj), //系列化对象
        newobj = JSON.parse(str); //还原
    } else {
        for(var i in obj){
            newobj[i] = typeof obj[i] === 'object' ? 
            cloneObj(obj[i]) : obj[i]; 
        }
    }
    return newobj;
};
```
2.使用jquery的extend()方法实现深度和浅度拷贝(第一个参数用来确定是否进行深度拷贝)
```javascript
jQuery.extend(true, { a : { a : "a" } }, { a : { b : "b" } } );
jQuery.extend( { a : { a : "a" } }, { a : { b : "b" } } );
```
## 问题
### 需要注意的是，如果对象比较大，层级也比较多，深复制会带来性能上的问题。在遇到需要采用深复制的场景时，可以考虑有没有其他替代的方案。在实际的应用场景中，也是浅复制更为常用。

## 其他解决方案
#### facebook在解决react 的 redux 时打造了 ImmutableJS，优化了深度拷贝带来的效能问题






