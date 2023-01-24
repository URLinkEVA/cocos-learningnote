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

# 继承与抽象类

# 接口与属性拓展

# 名称空间

# 泛型

# 元组数组字典

# 回调

# 正则表达式

# 访问修饰符

# 单例模式

# 代理模式

# 观察者模式

# 工厂模式

# 链表

