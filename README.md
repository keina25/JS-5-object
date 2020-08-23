## JS 对象基本用法

---

### 声明对象的两种语法

---

- 对象（Object):对象就是一组"键值对"（key-value）,是一种无序的复合数据集合

```Javascript
var obj={
    foo:'hello',
    bar:'world'
}
```

- 上面代码中，大括号就定义了一个对象，它被赋值给变量 obj，所以变量 obj 就指向一个对象。
- 该对象内部包含两个键值对，第一个键值对是 foo: 'Hello'，其中 foo 是“键名”（成员的名称），字符串 Hello 是“键值”（成员的值）。键名与键值之间用冒号分隔。第二个键值对是 bar: 'World'，bar 是键名，World 是键值。两个键值对之间用逗号分隔。
- 写法：

1. 简单写法

```Javascript
let obj={
    'name':'frank','age':18
}
```

2. 正规写法：

```javascript
let obj = new Object({
  name: "frank",
});
```

3. 匿名写法：

```javascript
console.log({ name: "frank", age: 18 });
```

- 细节注意：

1. 键名是字符串，不是标识符，可以包含任意字符
2. 引号可以省略，省略之后就只能写标识符
3. 就算引号省略，键名也还是字符串

- 键名：对象的所有键名都是字符串

```javascript
let obj = {
  1: "a",
  3.2: "b",
  1e2: true, //e为100
  1e-2: true,
  0.234: true,
  0xff: true, //16进制(255:10进制)
};
```

- 重点：

1. 可以通过 Object.keys(obj)得到所有 obj 的所有 key
2. 如果键名不符合标识名的条件（比如第一个字符为数字，或者含有空格或运算符），且也不是数字，则必须加上引号，否则会报错。

- 对象的每一个键名又称为“属性”（property），它的“键值”可以是任何数据类型。如果一个属性的值为函数，通常把这个属性称为“方法”，它可以像函数那样调用。

```javascript
let p1='name',
let obj={p1:'frank'}    //属性名为'p1'
let obj={[p1]:'frank'}  //p1属性名的值就是‘name’
```

- 对比：

1. 不加[]的属性名回自动变成字符串
2. 加了[]则回当作变量求值（先求变量的值，再转化为字符串）
3. 值如果不是字符串，会自动变成字符串

- 枚举一个的对象的所有属性：从 ECMAScript5 开始，就有三种原生方法可以枚举或列出对象的属性：

  1. for...in 循环：该方法依次访问一个对象及其原型链中所有可枚举的属性
  2. Object.keys(o):该方法返回对象 o 自身包含（不包含原型中）的所有可枚举属性的名称的数组
  3. Object.getOwnPropertyNames(o):该方法返回对象 o 自身包含（不包含原型中）的所有属性（无论是可枚举的）名称的数组

### 如何删除对象的属性

---

- delete 命令用于删除对象的属性，删除成功后返回 true

```javascript
var obj = { p: 1 };
Object.keys(obj); //['p']

delete obj.p; //true
obj.p; //undefined
Object.keys(obj); //[]
```

- 上面代码中，delete 命令删除对象 obj 的 p 的属性。删除后，再读取 p 的属性就会返回 undefined，而且 Object.keys 方法的返回值也不再包含该属性
- 注意，删除一个不存在的属性，delete 不会报错，而且返回 true

```javascript
var obj = {};
delete obj.p; //true
```

- 上面代码中，对象 obj 并没有 p 属性，但是 delete 命令照样返回 true。因此，不能根据 delete 命令的结果，认定某个属性是存在的。

- 只有一种情况，delete 命令会返回 false，那就是该属性存在，且不得删除

```javascript
var obj = Object.defineProperty({}, "p"{
    value:123,
    configurable:false,
});

obj.p //123
delete obj.p //false
```

- 上面代码之中，对象 obj 的 p 属性是不能删除的，所以 delete 命令返回 false
- 注意：delete 命令只能删除对象本身的属性，无法删除继承的属性

### 如何查看对象的属性

---

- 查看一个对象的自身所有属性：Object.keys(obj)
- 查看一个对象自身+共有属性：

1. console.dir(obj)
2. Object.keys 打印出 obj.**proto**

- 判断一个对象是自身属性还是共有属性：
  obj.hasOwnProperty('toString')

```javascript
var obj2 = { name: "frank", age: 18 }; //undefined
Object.keys(obj); //['name','age']
Object.value; //['frank',18]
Object.entries(obj2); //[Array(2),Array(2)]查看obj2的value和key
console.dir(obj2); //Objec
```

### 如何修改或增加对象的属性

---

- 直接赋值

```javascript
let obj ={name:'frank'}  //name是字符串
obj.name='frank'
obj['name']='frank'
obj[name]='frank'  //是错误的，因为name的值不确定
obj['na'+'me']='frank'
let key='name', obj[key]:'frank' //声明变量
let key='name';obj.key='frank' //错误，因为obj.key等价于obj.['key']
```

- 批量赋值

```javascript
Object.assign(obj.{age:18,gender:'man'})
```

- 无法通过自身修改或增加共有属性

```javascript
let obj = {},
  obj2 = {}; //共有属性toString
obj.toString = "xxx"; //'xxx'只会修改obj自身属性
obj2.toString; //还是再原型上
```

- 修改隐藏属性:Object.create

```javascript
let obj=Object.create(common),
obj.name='frank',
let obj2=Object.create(common),
obj2.name='jack'
```

### 'name' in obj 和 obj.hasOwnProperty('name') 的区别

---

```javascript
'toString' in obj  //true,in是查看，但并不知道是自身还是共有属性
obj.hasOwnProperty('toString')  //false 说明toString不是自身属性
var obj={name='frank',age:18}  //undefined
obj.hasOwnProperty('name')  //true
obj.hasOwnProperty('toString')  //false
```

- 'xxx' in obj 并不知道查看的是自身属性还是共有属性；
- obj.hasOwnProperty(‘xxx')查看的是自身的属性
