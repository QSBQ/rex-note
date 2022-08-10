# ES6

## let声明

```
let name = "洛岚歌";
```

## const声明

```
const sex = "女";
```

## 解构赋值

```
let queens = ["紫女", "晓梦", "焰灵姬", "暂无1", "暂无2"];
let [queen5, queen2, queen1, queen3, queen4] = queens;
```

## 模板字符串

```
let acline = `君临天下的皇帝${name}`;
```

## 简化对象写法

ES6允许在大括号里面，直接写入变量和函数，作为对象的属性和方法

```
let u7 = {
     name,
     sex,
     queen2
};
```

## 箭头函数

箭头函数：

- 如果形参只有一个，则小括号可以省略；
- 函数体如果只有一条语句，则花括号可以省略，并省略return，函数的返回值为该条语句的执行结果；

```
//正常函数1
function op1(){
    console.log("齐疯子");
}
//正常函数2
let op2=function(){
    return console.log("齐疯子");
}
//箭头函数
let op3 = () => console.log("齐疯子");
```

1. 箭头函数 this 指向声明时所在作用域下 this 的值；
2. 箭头函数不能作为构造函数实例化；
3. 不能使用 arguments；

## rest运算符

- rest==>代替arguments,用于获取函数的不知数实参==>形参里写上...args(自定义名称)即可
- 注1：rest获取的是数组，arguments获取的是对象
- 注2：如果有复数形参,那么rest要放到形参最后(arguments不需要)

```
function oy(name, age, ...rest) {
    console.log(name, age, ...rest);
};
```

### 函数形参赋初始值

```
function oy(name = "齐疯子", age = 17) {
    console.log(name, age);
};
```

## spread扩展运算符

把数组展开成一系列用逗号分隔的值

[...数组名]==>数组克隆

[...数组名,...数组名]==>合并数组

[...伪数组名(arguments函数参数)]==>伪数组变真数组

[...字符串名]==>把字符串转变为数组(每一个字符占一个位置)

```
let whoking = [...name];
```

## 第七种数据类型:Symbol

解决命名冲突

```
let x = Symbol("范青雁");
```

- 值唯一(这里指的是同值不同编号)，解决命名冲突的问题
- 不能与任何数据运算
- 定义的对象属性不能使用for...in循环遍历，但可以用Reflect.ownKeys获取对象的所有键名 

注：JS七种数据类型: undefined，string，symbol，object，null，number，boolean

## 迭代器

自定义遍历数据

- 迭代器==>for(let 变量 of  对象名){}==>无视数据类型遍历数据
- for...in==>for(let 变量 in 对象名){}==>遍历数据的索引号

```
for (let k of queens) {
      console.log(k);
};
```

## 生成器

- 生成器是特殊的函数,用于异步编程
- yield是函数代码的分隔符

```
//生成器
function a1() {
    setTimeout(() => {
        console.log("恕瑞玛");
        ov.next();
    }, 1000);
};
function a2() {
    setTimeout(() => {
        console.log("烬");
        ov.next();
    }, 2000);
};
function* gen() {
    yield a1();
    yield a2();
};
//调用生成器函数
let ov = gen();
ov.next();
```

## set集合

set集合是没有重复元素的有序列表

使用:

```
let king = new Set(['齐疯子', '崔道盛', '洛岚歌', '齐疯子', '武夜零', '武夜伽']);
console.log(king);
```

set的方法:

```
//增
king.add('武夜剑流');
//删
king.delete('洛岚歌');
//全删
king.clear();
//查
let k1 = king.has('武夜零')
```

set的应用:

```
//1.数组去重
let queen = [...new Set(myNumber)];
//2.交集
let queen2 = [...new Set(myNumber)].filter(item => new Set(myNumber2).has(item));
//3.并集
let queen3 = [...new Set([...myNumber, ...myNumber2])];
//4.差集
let queen4 = [...new Set(myNumber)].filter(item => !new Set(myNumber2).has(item));
```

## map集合

Map集合是存储键值对的有序列表

- Map集合的键名和值支持任意类型的数据

```
//声明一个map
let crazy = new Map();
//增
crazy.set('name', '齐疯子');
//删
crazy.delete('name');
//获取
crazy.get(name);
//清空
crazy.clear();
```

对象存储的问题

1. 键名只能是字符串
2. 获取数据的数量的时候不方便
3. 键名容易和原型上的名称发生冲突

## class类

语法糖

类的使用:

```
class Crazy {
      constructor(knife) {
      this.knife = knife;
    }
}
```

类的继承:

```
class Crazy2 extends Crazy {
      constructor(knife, sword) {
         super(knife);
         this.sword = sword;
    }
}
```

方法重写:

```
class King {
    ability() {
        console.log('枭力');
    }
}
class Queen {
    ability() {
        console.log('赤雷灾体');
    }
}
```

类的静态成员:

```
class Gun {
    static name = '狙击枪';
}
console.log(Gun.name);//非正常方式可以调用
console.log(new Gun().name);//正常方式无法调用
```

类的get和set:

```
class God {
    get servant() {
        console.log('get已调用');
        return '少司命';
    }
    set servant(newVal) {
        console.log('set已调用');
    }
}
let n7m9=new God();
n7m9.servant
n7m9.servant = '王与后'
```

## 数值扩展

```
//解决纯小数的计算结果不符合数学逻辑的问题
function gun(a, b) {
    if (Math.abs(a - b) < Number.EPSILON) {
        return true
    } else {
        return false
    }
}
console.log(gun(0.9 * 0.4, 0.36));
//进制
let b = 0b100; //二进制
let o = 0o100; //八进制
let d = 100; //十进制
let x = 0x100; //十六进制
//检测
Number.isFinite(100)//检测一个数值是否为有限数
Number.isNaN(123)//检测一个数值是否为 NaN 
Number.isInteger(5)//判断一个数是否为整数
Math.trunc(3.5)//将数字的小数部分抹掉  
Math.sign(100)//判断一个数到底为正数 负数 还是零
```

## 对象方法扩展

```
Object.is(120, 120)//判断两个值是否完全相等 
//对象的合并
let king = {
    name: '齐疯子',
    age: 17
}
let queen = {
    name: '洛岚歌'
}
let kq = Object.assign(king, queen);
//设置原型对象Object.getPrototypeof
let knife = {
    name: '毁书'
}
let sword = {
    age: 3000
}
console.log(Object.setPrototypeOf(knife, sword));
```

## ES6模块化

模块化是指将一个大的程序文件，拆分成许多小的文件，然后将小文件组合起来

模块化的好处：

1. 防止命名冲突
2. 代码复用
3. 高维护性

模块功能主要由两个命令构成：export 和 import 

- export 命令用于规定模块的对外接口
- import 命令用于输入其他模块提供的功能

分别暴露:

- k1.js:

```
export let knife1 = '毁书';
export function explain1() {
    console.log('这把刀的威力毁天灭地!!!');
}
```

- index.html:

```
<script type="module">
     import * as k1 from './src/js/k1.js';
     console.log(k1.knife1);
</script>
```

统一暴露:

- k2.js:

```
let knife2 = '裂线';
function explain2() {
    console.log('这把刀是洛岚歌所有，将陪伴她直到白云毕业');
}
export { knife2, explain2 };
```

默认暴露:

- k3.js:

```
export default {
    knife3: '纵云切',
    explain3: function() {
        console.log('斩裂天空的力量!!!');
    }
}
```

