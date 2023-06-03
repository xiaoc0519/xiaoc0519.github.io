+++
title = "Basic"
date = 2023-05-24T12:02:40+08:00
menu = 'typescript'
+++

# **BASIC**

## **Variable**
```typescript
let lname :type [= value]; // 变量
const cname = value; // 常量
```

```typescript
let a = 'a';  
const b = 'b'; // const 常量  赋值后不允许修改

// 指定类型变量 , 指定后只能存指定的类型数据
let c:string = 'c';  // 字符串
 	 :number = 4; // 数字
	 :number; // undefined  可以不赋值
	 :number = null; // null 空值 
	 :boolean = false; // true / false 布尔值
	 :any = 1; // 可以储存任何类型
     :number | string; // 联合类型， 同时支持多个类型

typeof e; // 查看 变量e 的类型
type tname = type; // 为 type 类型添加一个 tname 的别名
e + "" // 数字转字符串 隐式转换
`this is ${e}` // `${}` 格式化字符串 
```

## **Array**
```typescript
let numarr :type[]= [a, b, v]; // 数组
```

```typescript
let numarr :number[] = [1, 2, 3];
let strarr :string[] = ['a', 'b', 'c'];

strarr[index] // 取值 strarr[0]
```

## **枚举** `ENUM`
```typescript
enum Ename{ v1, v2, v3};
```

```typescript
enum Color{ red, blue, green};
let color : Color = Color.red; 
```

## **运算符**
```typescript
% // 取余
variable++; // 自增1  先使用再自增
++variable; // 自增1 先自增再使用

variable > 10 && variable < 20;  // &&满足两个条件返回 true
variable > 10 || variable; <  5 // || 满足一个条件返回true
!(variable > 10) // ! 取反
variable += num // 自增Num  variable = variable + num
variable -=

// 条件 ? 真返回值 : 假返回值
variable = variable > 100 ? 100 : variable // 三元运算 
```

## **条件控制** `IF`

```typescript
if (boolean){
	// body
} else if (boolean) {
	// body
} else {
	// body
}

switch(state){
	case boolean:
    	// body;
        break;
    case boolean:
    	// body
        break;
    default:
    	// else body
}
```

```typescript
enum State{ idle, run, atk, die}
let state: State = State.idle;
switch(state){
	case State.idle:
    	// body
        break;
    case State.atk:
    	// body
        break;
    default:
    	// else body
}
```

## **循环** `LOOP`
```typescript
while ( ) { } // 判断再执行
do { } while ( boolean ) // do 执行一次后 再判断
for ( let variable = 0; boolean; variable++ ) { }
for ( let variable of array ) { } // 遍历数组 类似 for i in list
for ( let index in array ) { } // 遍历数组索引
```

## **函数** `FUNCTION`
```typescript
function func( variable: string, ... )[: number] {
	return variable;
}
let func = function( variable: string, ... ) { }
let func = ( variable: string, ... ) => {}
```

## **类** `CLASS`
### class
```typescript
class Classname{
	static dex:string = 'this is a calss'; // static variable 静态属性 类属性
	name:string;
    age:number;
    
    // 构造方法
    constructor(name:string, age:number){
    	this.name = name
        this.age = age
    }
    
    static test(){ // 类方法
    	// body
    }
    
    func(){
    	this.name // 类似于 self.name 类变量
    }
}

let a = new Classname('input name',22)
a.name = 'name'
a.func()
Classname.des // 类方法只能类调用， 实例化无法调用
```

### 继承
```typescript
class Person{
	name:string = '';
    
    say(){
    	console.log('i Am' + this.name)
    }
}

class Student extends Person{
	// extends class 继承类的所有属性方法
    // 此类中重复的会进行覆盖
    num: number = 0;
    
    say(){
    	super.say(); // 执行父类的 方法
        // 在父类的 方法 里面添加子类的语句
    	consolr.log('Student' + this.name)
    }
}

let a = new Student();
a.say()
```

### 抽象类
> 抽象类 本身不能被实例化为对象，可以被继承
```typescript
abstract class Person{
	name:string = '';
    
    run(){ }
    abstract say():void 
}
class Student extends Person{
	say(){
    }
}

let a: Person = new Student();
a.say();
```

### 接口 `interface`

```typescript
class Person{
	name: string;
}

interface IWolf{ // 接口
	attack();
}

interface IDog{
	eat()
}

class WolfMan extends Person implements IWolf, IDog{
	attack(){ // 必须实现接口里的函数
    eat()
    }
}
```

### 属性寄存器

```typescript
class Person{
	_hp: number = 100;
    
    get hp(){    // 取值
    	return this._hp;
    }
    
    set hp( value ) {    // 赋值
    	this._hp = value < 0 ? 0 : value;
    }
}

let a = new Person();
a.hp -= 180;
console.log(a.hp + '')
```

## **命名空间** `NAMESPACE`

```typescript
namespace aa{
	export class Person{
    	name:string;
    }
}
let person = new aa.Person();
```

## **泛型** 

```typescript
function add( num:any ): any{  // any 传入传出类型可能不一致
	if ( typeof num == 'number' ) {
    	num++;
        return num;
    }
    return num;
}

function add<T>(num: T): T{ // 指定传出类型为传入的类型
	if ( typeof num == 'number' ) {
    	num++;
        return num;
    }
    return num;
}
console.log(add<number>(3))
```

## **集合**
```typescript
// 元组
let turp: [string, number] = ['string', 100];
turp[index] = 'string';

// 数组
let array: number[] = [ 1, 2, 3 ];
let array: Array<number> = new Array<number>();
array.length; // 长度
array.push(1); // 追加元素
array.unshift(1); // 在数组前面添加元素
array.pop(); // 删除末尾元素
array.splice(0,1); // 从第几(0)位开始删除几(1)个
array.shift(); // 删除第一个元素
array = array.concat(array); // 合并两个元素
let index = array.indexOf(1); // 返回 1 在数组的 index
array.sort(); // 排序 正序
array.reverse(); // 倒叙

// 字典
let dic:{ [key:string]: string } {
	'name':'name',
    'name2':'name2'
}
dic['name3'] = 'name3' // 赋值
dic['name3'] // name3  取值
```

## **回调**
```typescript
function func(value: Function) {
	value();
}

function test(){
	console.log('test')
}
func(test)

func(function(){
	console.log('test2')
})
func(() => {
	console.log('test3')
})
```

```typescript
// 带参数
function func(value: Function) {
	value('arg');
}

function test(name){
	console.log(name)
}
func(test)

func(function(name){
	console.log('test2')
})
func((name) => {
	console.log('test3')
})
```

## **正则** 
> *[正则Pattern ✈](./../../python/standard/re/#pattern)*
*[常用正则表达式 ✈](./../re/)*

```typescript
let reg = /\d{4}-\d{7}/g
let str = '0345-123456' 
let res = reg.exec()
res.length // 匹配是否成功, 返回长度
res.forEach(function(value, index){
	console.log(value, index)
})
```

## **访问修饰符**
```typescript
class Person{
	public name: string; // 任何人可以访问
    protected name2: string; // 外部不可以访问
    private name3: string; // 外部和子类都不可以访问  
	say(){
		this.name1
		this.name2
		this.name3
    }
}

class Student extends Person{
	constructor(){
    	super();
        this.name1
        this.name2
    }
}
let a = new Person();
a.name1
```

## **单例模式**

```typescript
class SoundManafer{
	static Instance = new SoundManager()
    private constructor(){}
}
soundManager.Instance
```

```typescript
class SoundManafer{
	private static Instance: SoundManager;
    private constructor(){}

	static Instance() {
    	// 当前单例是否产生
        // 懒加载
        if (!SoundManager.instance){
        	SoundManager.instance = new SoundManager();
        }
        return SoundManager.instance
    }
}
soundManager.Instance()
```

## **代理模式**
```typescript
interface ICalc{
	calc(num1,num2):number
}

class Npc1 implements ICalc{
	calc(num1,num2){
    	return num1 + num2
    }
}

class Npc2 implements ICalc{
	calc(num1,num2){
    	return num1 - num2
    }
}

class Person{
	// 代理
    delegate: ICalc;
	GetNum(num1,num2){
    	let num = this.delegate.calc(num1,num2)
    }
}
let person = new Person();
// 设定一个代理
person.delegate = new Npc1();
person.GetNum(3,4)

```

## **监听模式**
```typescript


```