# ECMAScript6

### 变量的声明
1、使用var声明的变量支持预解析，let声明的变量不支持
```
console.log(a); // undefined
var a = 1;

console.log(b); //报错 ： ReferenceError: b is not defined
let b = 2;
```
2、let不支持变量的重复声明
```
var a = 1;
var a = 3;
console.log(a) // 3

let b = 3;
let b = 4;
console.log(b); //报错： SyntaxError: Identifier 'b' has already been declared
```
3、let支持块级作用域
4、const 声明的是常量，声明的时候就必须初始化，且之后不可更改；不支持预解析；不支持重复声明；支持块级作用域
5、用const声明一个常量对象，不可以对该常量直接修改，但是可以拓展该常量对象的属性。
```
const obj = {};
obj = 1; //报错：TypeError: Assignment to constant variable.
obj.name = 'tom';
console.log(obj); // {name: "tom"}
```
### 解构赋值
#### 对象的解构赋值
```
var obj = {
  name: 'tom',
  age: 23,
}
//常规的取值方式
var name = obj.name;
var age = obj.age;
console.log(name, age) // tom 23
//解构赋值方式取值
var {name , age} = obj;
console.log(name, age); // tom 23
```
1、如果变量已经声明了，在使用解构赋值进行赋值的时候，会将变量误认为块级作用域，解决方式是，将该语句用()快起来，让程序认为该语句是一个完整的整体。
```
var obj = {
  name: 'tom',
  age: 23,
}
var name = '';
{name} = obj; // 报错 ： SyntaxError: Unexpected token =
({name} = obj);
console.log(name); // tom
```
2、解构赋值支持别名(:) 和 默认值(=)，别名和默认值可同时使用
```
var obj = {
  name: 'tom',
  age: 23,
  c: undefined,
}
var {name:str} = obj;
console.log(str); // tom
// 如果一个变量使用解构赋值，赋值的是对象中没有的属性，那么，该变量的值是undefined，
var {gender: g} = obj;
console.log(g); // undefined
// 如果一个变量使用解构赋值，赋值的是对象中没有的属性或者改属性的值为undefined，那么，该变量的值是取得默认值，
var {gender: g = 'nan', c='123'} = obj;
console.log(g); // nan 123
```
3、支持剩余模式
```
var obj = {
  name: 'tom',
  age: 23,
  c: undefined,
  a: 1,
  b: 2,
};
var {a, b, ...rest} = obj;
console.log(a, b, rest); //1 2 {name: "tom", age: 23, c: undefined}
```
#### 数组的解构赋值
数组的解构赋值是严格按照数组的先后顺序的
```
var arr = 'hello'.split('');
//常规的取值
var first = arr[0];
var second = arr[1];
console.log(first, second); // h e
//使用解构赋值取值
var [first, second] = arr;
console.log(first, second); // h e
//如果我想跳过第二项（下标为1），直接去取第三项，需要用逗号占位
var [first,,third] = arr;
console.log(first, third); // h l

// 如果从数组中解构赋值超过该数组的长度，或者改数组中的某个值是undefined，那么变量的值是undefined，并且支持默认值。
arr[0] = undefined;
var [a, b, c, d, e, f] = arr;
console.log(a, b, c, d, e, f); // undefined "e" "l" "l" "o" undefined

var [a='123', b, c, d, e='666', f='234'] = arr;
console.log(a, b, c, d, e, f); //123 e l l o 234 如果某项的值是undefined，那么则使用默认值

//支持剩余模式
arr[0] = 'h';
var [a, b, c, ...rest] = arr;
console.log(a, b, c, rest); //h e l  ["l", "o"]
```
### Set es6新增的数据结构
```
var s = new Set(['a', 'b', 'c']);
console.log(s);//{"a", "b", "c"}
//size属性
console.log(s.size); // 3
```
1、add()方法，返回的是操作的set对象
```
var s = new Set(['a', 'b', 'c']);
console.log(s);//{"a", "b", "c"}
s.add(1).add(2); // add方法返回的是添加之后的set对象，所以可以连用add方法
console.log(s);//{"a", "b", "c", 1, 2}
//set中的值都是唯一的。
s.add(1);// set中的值是唯一的，再次添加 1 的时候，就添加不进去
console.log(s);//{"a", "b", "c", 1, 2}
// 虽然在原生js中，NaN !== NaN ， 但是在set中NaN == NaN，
s.add(NaN).add(NaN); //两次添加NaN，只添加进去一个NaN。
console.log(s);//{"a", "b", "c", 1, 2, NaN}
```
2、delete()方法，返回一个布尔值，表示删除是否成功。
```
var s = new Set(['a', 'b', 'c']);
console.log(s);//{"a", "b", "c"}
var res = s.delete('b');
console.log(res , s); //true  {"a", "c"}
 
 //删除set中不存在的值，返回false
var res = s.delete('z');
console.log(res , s); //false  {"a", "c"}
```
3、has()方法，返回一个布尔值，表示该项是否在set中存在
```
var s = new Set(['a', 'b', 'c']);
console.log(s);//{"a", "b", "c"}
console.log(s.has('a')); // true
console.log(s.has('z')); // false
```
4、clear()，清空该set
```
var s = new Set(['a', 'b', 'c']);
console.log(s);//{"a", "b", "c"}
s.clear();
console.log(s); // {}
```
5、遍历set解构
```
var s = new Set(['a', 'b', 'c']);
console.log(s);//{"a", "b", "c"}

//forEach回调函数的三个参数
//item -> set中的值
// index-> set中值对应的索引
// s -> Set
s.forEach(function(item, index, s){
  console.log(item , index, s); // 在set解构中，值和索引值是一样的。
});
//输出 a a  {"a", "b", "c"}
// b b  {"a", "b", "c"}
// c c  {"a", "b", "c"}

//将set中的所有项 按照插入的顺序 一个一个输出
var keys = s.keys();
console.log(keys); // SetIterator {"a", "b", "c"}
console.log(keys.next()); //{value: "a", done: false}
console.log(keys.next()); //{value: "b", done: false}
console.log(keys.next()); // {value: "c", done: false}
console.log(keys.next()); //{value: undefined, done: true}

var vals= s.values();
console.log(vals.next()); //{value: "a", done: false}
console.log(vals.next()); // {value: "b", done: false}
console.log(vals.next()); // {value: "c", done: false}
console.log(vals.next()); // {value: undefined, done: true}

var entries = s.entries();
console.log(entries.next()); //
console.log(entries.next()); // 
console.log(entries.next()); //
console.log(entries.next()); 

```
6、 数组去重
```
var arr = [1,2,3,4,2,2,1,3,4,NaN,NaN];
var s = new Set(arr);
console.log(s); // {1, 2, 3, 4, NaN}
 
```
### Map
```
var m = new Map([['name', 'momo']]);
console.log(m); //  {"name" => "momo"}
//size属性
console.log(m.size); // 1
```
1、get()方法，通过键名获取值
```
var m = new Map([['name', 'momo']]);
console.log(m); //  {"name" => "momo"}
console.log(m.get('name')); // momo
```
2、 set()方法，设置键值对。
```
var m = new Map([['name', 'momo']]);
console.log(m); //  {"name" => "momo"}
m.set('age', 23);
console.log(m); //{"name" => "momo", "age" => 23}

//使用一个对象作为另一个对象的键名，键值另外设置
var momo = {
  name: 'n'
};
m.set(momo, 11); 
console.log(m);//{"name" => "momo", "age" => 23, {…} => 11}
//{"name" => "momo"}
//{"age" => 23}
//{Object => 11}

```
3、delete()删除， 返回布尔值，表示删除的结果
```
var m = new Map([['name', 'momo']]);
console.log(m); //  {"name" => "momo"}
var momo = {
  name: 'n'
};
m.set(momo, 11); 
console.log(m);//{"name" => "momo", "age" => 23, {…} => 11}

var res = m.delete(momo);
console.log(res, m); //true  {"name" => "momo"}
//删除一个不存在的键，返回结果为false，原map解构不变
var res = m.delete(d);
console.log(res, m); //false Map(1) {"name" => "momo"}

```
4、has()方法，返回布尔值，表示键是否存在在Map中
```
var m = new Map([['name', 'momo']]);
console.log(m); //  {"name" => "momo"}
console.log(m.has('name')); // true
console.log(m.has('c')); // false
```
5、clear()方法，清空Map结构
```
var m = new Map([['name', 'momo']]);
console.log(m); //  {"name" => "momo"}
m.clear();
console.log(m); // {}

```
6、遍历Map
```
var m = new Map([['name', 'momo'], ['age', 34], ['gender', 'nv']]);
console.log(m); //   {"name" => "momo", "age" => 34, "gender" => "nv"}

m.forEach(function(item, key, map){
  console.log(item, key, map);
});
// momo name Map(3) {"name" => "momo", "age" => 34, "gender" => "nv"}
// 34 "age" Map(3) {"name" => "momo", "age" => 34, "gender" => "nv"}
// nv gender Map(3) {"name" => "momo", "age" => 34, "gender" => "nv"}

//keys()方法
var keys = m.keys();
console.log(keys); // {"name", "age", "gender"}
console.log(keys.next()); // {value: "name", done: false}
console.log(keys.next()); // {value: "age", done: false}
console.log(keys.next()); // {value: "gender", done: false}
console.log(keys.next()); // {value: undefined, done: true}

//values()方法
var vals = m.values();
console.log(vals); //  {"momo", 34, "nv"}
console.log(vals.next()); // {value: "momo", done: false}
console.log(vals.next()); // {value: 34, done: false}
console.log(vals.next()); // {value: "nv", done: false}
console.log(vals.next()); // {value: undefined, done: true}

// entries()方法
var entries = m.entries();
console.log(entries); //  MapIterator {"name" => "momo", "age" => 34, "gender" => "nv"}
console.log(entries.next()); // {value: Array(2), done: false}
console.log(entries.next()); //{value: Array(2), done: false}
console.log(entries.next()); //{value: Array(2), done: false}
console.log(entries.next()); //{value: undefined, done: true}

```

