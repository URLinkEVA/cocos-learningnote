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



