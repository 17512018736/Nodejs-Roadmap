# 对象引用

* 对象属于引用类型是属性和方法的集合。引用类型可以拥有属性和方法，属性也可以是基本类型和引用类型。

* javascript不允许直接访问内存中的位置，不能直接操作对象的内存空间。实际上操作的是对象的引用，所以引用类型的值是按引用访问的。准确说引用类型的存储需要内存的栈区和堆区(堆内存)共同完成，栈区内保存变量标识符和指向堆内存中该对象的指针(也可以说该对象在堆内存中的地址)。

### 引用类型比较1

引用类型是按照引用访问的，因此对象(引用类型)比较的是堆内存中的地址是否一致，很明显a和b在内存中的地址是不一样的。

```javascript
const a = {};
const b = {};

a == b //false

```

### 引用类型比较2

下面对象d是对象c的引用，这个值d的副本实际上是一个指针，而这个指针指向堆内存中的一个对。因此赋值操作后两个变量指向了同一个对象地址，只要改变同一个对象的值另外一个也会发生改变。

```javascript
const c = {};
const d = c;

c == d //true

c.name = 'zhangsan';
d.age = 24;

console.log(c); //{name: "zhangsan", age: 24}
console.log(d); //{name: "zhangsan", age: 24}
```

### 数组对象深度拷贝

> 对于下面这样一个复杂的数组对象，要做到深度拷贝(采用递归的方式)，在每次遍历之前创建一个新的对象或者数组，从而开辟一个新的存储地址，这样就切断了引用对象的指针联系。

```javascript
const a = {
    name: 'zhangsan',
    school: {
        university: 'shanghai',
    },
    hobby: ['篮球', '足球'],
    classmates: [
        {
            name: 'lisi',
            age: 22,
        },
        {
            name: 'wangwu',
            age: 21,
        }
    ]
};
```

```javascript
function copy(elments){
    //根据传入的元素判断是数组还是对象
    let newElments = elments instanceof Array ? [] : {};

    for(let key in elments){
        //注意数组也是对象类型，如果遍历的元素是对象，进行深度拷贝
        newElments[key] = typeof elments[key] === 'object' ? copy(elments[key]) : elments[key];
    }

    return newElments;
}
```

```javascript
const b = copy(a);

b.age = 24;
b.school.highSchool = 'jiangsu';
b.hobby.push('🏃');
b.classmates[0].age = 25;

console.log(JSON.stringify(a)); 
//{"name":"zhangsan","school":{"university":"shanghai"},"hobby":["篮球","足球"],"classmates":[{"name":"lisi","age":22},{"name":"wangwu","age":21}]}
console.log(JSON.stringify(b));
//{"name":"zhangsan","school":{"university":"shanghai","highSchool":"jiangsu"},"hobby":["篮球","足球","🏃"],"classmates":[{"name":"lisi","age":25},{"name":"wangwu","age":21}],"age":24}
```

### exports与module.exports的区别

exports相当于module.exports 的快捷方式如下所示:

```javascript
const exports = modules.exports;
```

但是要注意不能改变exports的指向，我们可以通过 ``` exports.test = 'a' ``` 这样来导出一个对象, 但是不能向下面示例直接赋值，这样会改变exports的指向

```javascript
//错误的写法 将会得到undefined
exports = {
  'a': 1,
  'b': 2
}

//正确的写法
modules.exports = {
  'a': 1,
  'b': 2
}

```