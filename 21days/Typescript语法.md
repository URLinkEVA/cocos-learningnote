# cocos介绍

发展历史 08python版

# 变量与常量

Typescript

类型，面向对象

` documnet.write("hello TS");`

 声明变量let, var

const常量

`var personName:string = "kongla";`

限制类型

`var tmp:number = 111;`

# 变量类型

number, string, boolean, any, array 

 `let tmp:number = 3;`

`document.write("数字是${tmp}");`

`let a:number[] = [1, 2, 3, 4,5];`

联合类型

`let tmp:number|string = 3;`

# 枚举

自己定义一个属于自己的类型

```
let color:number = 0; //0白色 1黑色
let state:number = 0; //0走路 1跑步 2死亡
```

```
enum Color{
	red,
	green,
	blue 
}

enum State{
	walk,
	run,
	die
}

let color:Color = Color.blue;
let state:State = State.run;
```

类型验证

 `documnet.write(typeof tmp);`

# 运算符

// + - * / % ++ --

先输出后运算

比较运算符

`let res:boolean = 5 > 3;`



# 条件控制语句

跟c++一样

三目运算符

`num = num > 100 ? 100 : num;`

# 循环控制语句

```typescript
let nums:string[] = ["1", "2", "3"]
for(let i = 0; i < 3; i++)
{
	documnet.write(nums[i]);
}
```

 # 函数

流水线

```typescript
function func(char: string)
{
    let arr: string[] = ['a', 'b', 'c'];
    for(let i = 0; i < n; i++){
        if(char == arr[i]){
            document.write("当前是第" + i + "个字符");
        }
    }
}

func('a');

function add(num1:number, num2:number): number {
    let num = num1 + num2;
    return num;
}

let test = add(3, 4);
```

# 面向对象

类和对象，类是抽象，对象是实体

```typescript
// 类：人   对象：venti zhongli
class Person{
    name: string = "默认";
    age: number = 0;
    
    say(){
        document.write(this.name);
    }
}

// 实例化对象
let a = new Person();
a.name = "venti";
a.age = 1000;
a.say();

let b = new Person();
b.name = "zhongli";
b.age = 5000;
b.say();
```



# 构造与静态

成员属性，成员方法

静态属性，静态方法

```typescript
class Person{
    static des: string = "这是一个Person类";
    static test(){
        
    }
    name: string = "默认";
    age: number = 0;
	// 构造方法
	constructor(name: string, age: number){
    	this.name = name;
	}
    
    say(){
        document.write(this.name);
    }
}

// 调用des
Person.des = "a";
Person.test();

let a = new Person("venti", 1000);
a.say();
```



# 继承与抽象类

单继承

```typescript
// 继承
class Person{
    name: string = "";
    // age: number = 1000;

    say(){
        document.write("叫" + this.name);
    }
}

class Student extends Person{
    num: number = 0;
    score: number = 0;

    say(){
        super.say(); // 父类
        document.write("学生叫" + this.name);
    }
}

let a = new Student();
a.name = "venti";
a.say();
```

```typescript
// 抽象类 本省不能被实例化为对象，可以被继承
abstract class Person{
    name: string = "";
    run(){

    }

    abstract say();
}

class Student extends Person{
    say(){

    }
}

let a :Person = new Student();
a.say();
a.run();
```



# 接口与属性拓展

接口是一系列抽象方法的声明，是一些方法特征的集合，这些方法都应该是抽象的，需要由具体的类去实现，然后第三方就可以通过这组抽象方法调用，让具体的类执行具体的方法。

接口就是实现一个多继承，也算是一个协议。继承更倾向于继承父类的特性，接口更像需要实现的特定行为，如果是特性就用继承，如果是行为就用接口。

```typescript
// 狼人
class Person{
	name: string;    
}

interface IWolf{
    attack();
}

interface IDog{
    eat();
}

class Wolfman extends Person implements IWolf, IDog{
    attack(){

    }

    eat(){
        
    }
}
```

```typescript
// 属性寄存器
class Person{
    _hp: number = 100;

    // 取值
    get hp(){
        return this._hp;
    }

    // 赋值
    set hp(value){
        if(value < 0){
            this._hp = 0;
        }else{
            this._hp = value;
        }
    }

}

let a = new Person();
a.hp -= 20;
console.log(a.hp + "");
```

或者直接sethp()写个方法，不用get、set



# 名称空间

合并，解决重名问题

```typescript
namespace aa{
    export class Person{
        name: string;
    }
}

namespace bb{
    export class Person{
        // ...
    }
}

let person = new aa.Person();
let person2 = new bb.Person();
```



# 泛型

```typescript
function add(num: any): any{
    if(typeof num == "number"){
        num++;
        return num;
    }else return num;
}

console.log(add("3") + "");
```

```typescript
function add<T>(num: T): T{
    if(typeof num == "number"){
        num++;
        return num;
    }else return num;
}

console.log(add<number>(3) + "");
```

限制类型相同

# 元组数组字典

```typescript
let mytuple = ["kongla", 10];

console.log(mytuple[0]);
console.log(mytuple[1]);
mytuple.push("tmp");
console.log(mytuple[2]);

[LOG]: "kongla"
[LOG]: 10 
[LOG]: "tmp"
```

```typescript
// array.length 数组长度
let arr1: number[] = [1,2,3];
let arr2: Array<number> = new Array<number>();
// 数组后加元素
arr1.push(4);
// 数组前加元素
arr1.unshift(0);
// 删除最后元素
arr1.pop();
// 删除前面元素
arr1.shift();
// 从第几位开始删除几个
arr1.splice(0,1);
// 合并两个数组
arr1 = arr1.concat(arr2);
// 查找数组位置
let index = arr1.indexOf(3);
// 排序
arr1.sort();
arr1.reverse();
```

```typescript
// 字典
let dic: {[key: string]: string}  = {
    "name1": "kongkla",
    "name2": "kiro"
};

dic["name3"] = "appendix";
console.log(dic["name3"]);

```

# 回调



# 正则表达式

# 访问修饰符

# 单例模式

# 代理模式

# 观察者模式

# 工厂模式

# 链表

