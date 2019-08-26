## Today I Learned (20181105)
### about JavaScript Object

``` JavaScript
function iConstructor(tag, href){
	if(arguments.length > 1){
		this.tag = arguments[0]
		this.href = arguments[1]
	}
}
const instance = new iConstructor()
iConstructor.prototype.r = function(){
	// do something with the object
	// iConsttructor's attribute here
}
```
#### 数据类型:  
**about primitive type**:  
`Number`, `Boolean`, `String`,`null` and `undefined` (sec. 3.4  
all of which are *immutable*  
technically speaking only objects have methods,  
but `Number`, `Boolean`, `String` also do  
('cause they can be [wrapped](#wrapped object)  

**about object type**:  
**defination**:  
object is the collection of property,  
every property consists of a name/value,  
**class object**:  
collection of objects defined by constructor funtion  
(some common class object defined by JavaScript:  
`Array`, `Function`(**VIO**), `Date`, `RegExp`, `Error`  
**Global object**  
JavaScript解释器启动时会创建一个全局对象，并给这个对象一组定义的初始属性，主要包括:  
全局属性，如`undefined`, `Infinity`, `NaN`  
全局函数，如`isNaN()`,  `parseInt`, `eval()`  
构造函数，如`Date()`, `RegExp()`, `String()`, `Array()`, `Object()`  
全局对象，如`Math`, `JSON`
在最上层(所有函数体以外)可以通过`this`引用它  
(ES2015以上有所区别，check about it later)  
  
more about **type convert**: sec. 3.8


### some JavaScript Browser API methods

```
// a function, execute when the page finished loading
window.onload 
// create an element taged with String arguments
document.createElement
// some DOM operation: 
eleme2Add.appendTo(elemeAdded2)
window.localStorage
```
---
### some JavaScript style guide
**通过`if()`, `&&`, type convert判断是否会引起undefined或者type error再调用**

```
if(image.addEventHandler)
	image.addEventHandler(...)
else
	// ie
	image.attachEvent(..)

// ******saved input in localStorage******
const inputBars = document.getElementsByTagName("input")

function save(){
	if(window.localStorage && localStorage.SOMETHING)
		for(int i = 0; i < inputBars.length-1; i++)
			SOMETHING[inputBars[i].name] = inputBars[i].value
}

window.onload = function(){
	if(window.localStorage && localStorage.SOMETHING)
		for(int i = 0; i < inputBars.length-1; i++){
			if(SOMETHING[inputBars[i].name])
				inputBars[i].value = SOMETHING[inputBars[i].name]
		}
}
```
**optional divides ','**  
JavaScript并不是在所有换行处都填补分号，而是没有分号就无法解析的时候才填补分号
e.g. ：

```
const y = x + f
(a+b).toString()
// => const y = x + f(a+b).toString();
```
也有例外：

```
return, break, continue 以及 ++, --
```
**声明放在函数体头部以清晰标明作用范围**

**just sth about CSS class**

```
var list = document.querySelector('ul');
list.addEventListener('click', function(ev) {
  if( ev.target.tagName === 'LI') {
     ev.target.classList.toggle('done'); 
  }
}, false);
```
---
### some confusing facts
**小数比较**  
JavaScript使用 **IEEE-754** 浮点数表示法（一种二进制表示法  
1/2， 1/4， 1/1024 都可以精确表示  
但是1/10， 1/1000 这样的十进制分数无法精确表示，只能表示近似值  
所以才有这个很出名的例子:

```
const f1 = .3 - .2
const f2 = .2 - .1
f1 == f2 // => false
f1 == .1 // => false
f2 == .1 // => true
```
其实值非常接近，这种情况也只在比较时才会出现  
**解决办法：** 涉及金融计算尽量用整数避免使用小数

**值转换成boolean类型**  
只有 `undefined`, `null`, `0`, `-0`, `NaN`, `""` (也被统称为**falsy** value)会被转换成 `false`.  
所有其他值，包括所有对象都会转换成 `true`

**null & undefined**  
`null` 是关键字，用于表示数字，串，对象值只为空，对其执行`typeof`返回`'object'`  
`undefined` 不是关键字，表示变量未初始化，对其执行`typeof`返回`'undefined'`

```
null == undefined // => true , 应该用 === 来区分它们
```
<a name="wrapped object">**包装对象**</a>  
通过`.`调用原生类型的属性时，JavaScript会通过对应构造方法创建一个class对象来执行，这个临时对象只供读取，赋值会被忽略.

**类型转换**  

值|(转为:)字符串|数字|布尔值|对象
---|---|---|---|---
undefined|"undefined"|NaN|fasle|throws TypeError
null|"null"|0|false|throws TypeError
true|"true"|1||new Boolean(true)
false|"false"|0||new Boolean(false)
""(空字符串)||0|false|new String("")
NaN|"NaN"||false|new Number(NaN)
Infinity|"Infinity"||true|new Number(Infinity)
[] >> (任意数组)|""|0|true|
[9]|"9"|9|true|
['a']|调用join|NaN|true
**Number to String & String to Number**  

```
const x = 100.129  
x.toString(10) // 转成10进制  
x.toFixed(2) // => 100.13  精确到小数点后2位  
x.toExponential(2)  // => 1.00E2 科学计数法小数点后2位  
x.toPrecision(5) // => 100.13  5位有效数字  
Number('100.129') // 参数仅限十进制，不能有非法尾随字符  
parseInt('   1001.29 sd gundam ') // => 1 忽略左起任意位空格，识别连续的尽可能多的数字,忽略后面的内容  
parseInt('.134') // => NaN 第一个非空格不是数字直接返回NaN  
Boolean(new Boolean(false)) // => true  
```
**Object to Number & String**  
通过`valueOf()` 和 `toString()`从对象向数字，字符串转化(Date是特例)  
( to number时，若`valueOf()`返回值不是原生值，调用`toString()`,仍然不是,throws Type Error  
( to string时，若`toString()`返回值不是原生值，调用`valueOf()`,仍然不是,throws Type Error  
`＋`, `==`, `!=` 运算符会执行这种对象到原始值的转化,其他运算符比较明确  

**Function Scope, Hoisting(声明提前)**  
as the titles indicates...  
**Scope Chain:**  
每一段JavaScript代码都有一个与之关联的作用域.  
**作用域链**是一个**对象列表**或链表，这组对象定义了这段代码"作用域中"的变量.  
当JavaScript需要查找变量x的值时(variable resolution), 它会从链中的第一个对象开始查找, 找到则使用, 没找到则下一个...  
defining a function will save a scope chain.  
calling a func will create a new obj to store local variables of the func and add it to the chain, then create a new longer chain(function invocation   scope).

**执行优先级,结合性**  
`.`(属性访问表达式，调用表达式)优先级高于一切  
P78查看详细...  

**关系表达式**  

***`==` & `!==` (相等与不等运算符)***  
`NaN`与任何对象都不等,包括自己 (判断是否为`NaN`: `x!==x`,`isNaN()`  
两字符串内容相同编码不同,`==`和`===`判定为不等  

***`>`, `<`, `>=`, `<=` (比较运算符)***  
两个字符串：比较unicode字符索引  
有一个是NaN(或转化成NaN)：false  
`+`倾向于字符串，操作数有一个String就做连接操作，  
而比较运算符倾向于数字,两个操作数均为String才比较字符串  

**`in`, `instanceof`, 原型链**  
***
***关于eval***  
eval("2+3") // => 5  
可改变局部变量  

 |直接调用(`eval()`)|间接调用(`geval = eval`)
---|---|---
作用域|调用eval()的上下文作用域|全局对象(无法读,写,定义局部变量,函数)  
全局`eval()`**可用于**执行对上下文无依赖的全局脚本代码段

**关于标签语句**  
`identifier: statements`  
语句的标签，可以在别处引用(`break`, `continue`跳转  

---
### 关于Object
also known as 'hash', 'hashtable', 'dictionary', 'associative array'  
prototypal inheritance, prototype chain  
1. property attributes [value(get), writable(set), enumerable, configurable]  
2. object attributes [class, prototype, extensible flag]  
**Object Type:**  

* native object
* host object
* user-defined object

**Property Type**  

* own property
* inherited property

**How To Create**  

1. {} // 直接量创建  **prototype**: Object.prototype
2. new Constructor() // 构造方法创建  **prototype**: Constructor.prototype
3. Object.create(SOME_PROTOTYPE) // 根据prototype创建
4. based on 2 and 3:  

```
inherite(p){
	if(p == null) throw TypeError()
	if(Object.create) return Object.create(p)
	var t = typeof p
	if( t !== 'object' && t !== 'function') throw TypeError()
	function f(){}
	f.prototype = p
	return new f()
}
```

**Get and Set**  

1. 属性访问运算符: `.`, `[]`
2. 检查属性: `in` >> `hasOwnProperty()` >> `propertyIsEnumerable()`
3. 枚举: `for(in)` >> `Object.keys()` (based on `for(in)`) >> `Object.getOwnPropertyNames()`  
4. getter & setter:  
	* `{ get accessor_prop(){ /* do something */ }; }` // 直接量  
	2. `Object.defineProperty(obj, propName, propDesciptorObj)` // define one
	3. `Object.defineProperties(objID, {propName: propDescriptor})` // define a lot

**Talk is Cheap, Show Me The Code**  

```
Object.defineProperty(Object.prototype, 'deepClone', {
	writable: true,
	configurable: false,
	extensible: true,
	value: function(o){
		var p = {}
		var names = Object.getOwnPropertyNames(o)
		for(var desc, int i = 0; i < names.length; i++){
			// if(names[i] in p) continue; // don't know if it will help
			desc = Object.getOwnPropertyDescriptor(names[i])
			Object.defineProperty(p, names[i], desc)
		}
		return p
	}
})
```

**关于Prototype**  

```
Object.create(Object.prototype) 
obj.constructor.prototype // 获取obj的prototype (ES3 ver.
Object.getPrototypeOf(obj) // 获取obj的prototype (ES5 ver.
proto_obj.isPrototypeOf(obj) // obj是否在proto_obj原型链上 (作用类似instanceof运算符
__proto__ // Mozilla的JavaScript对外暴露了这个用于直接查询/设置prototype
```
**关于class**  
可以通过调用`Object.prototype.toString.call(obj)`来显示obj的class attribute, 通常为`'[object SOMETHING]' // .slice(8, -1)即类名SOMETHING`    

**关于CORS & XSS**  
1. host  
2. port  
3. protocol

---
### 关于Function
**how to call it**  
1. call as a func,  
2. call as a method,  
3. call as a construct func,  
4. call by call() and apply(), which can pass the value of `this` from one context to another.  
**closure**  
embedded functions have access to the parent context:  
how =>  

```
window
```  
## all about `this` 
In most case, the value of `this` in a func depends on how a function is called, and it can't be set by assignment during execution.  
In **ES5**, there is `bind()` to set val of this regardless off how it's called  
In **ES2015**, there is arrow function(`=>`) which don't provide their `this` binding and retains the value of its enclosing lexical context.  

**simple call**  
=> if(use strict) this === what it's setted, `undefined` by default.  
else this === its enclosing lexical context.

**arrow function**  

```
// Create obj with a method bar that returns a function that
// returns its this. The returned function is created as 
// an arrow function, so its this is permanently bound to the
// this of its enclosing function. The value of bar can be set
// in the call, which in turn sets the value of the 
// returned function.
var obj = {
  bar: function() {
    var x = (() => this);
    return x;
  }
};

// Call bar as a method of obj, setting its this to obj
// Assign a reference to the returned function to fn
var fn = obj.bar();

// Call fn without setting this, would normally default
// to the global object or undefined in strict mode
console.log(fn() === obj); // true

// But caution if you reference the method of obj without calling it
var fn2 = obj.bar;
// Then calling the arrow function this is equals to window because it follows the this from bar.
console.log(fn2()() == window); // true

//how about call obj.bar()() directly?
//
console.log(obj.bar()() === obj) // true
```
**called as method**  
when called as a method, its `this` will be set to the object the method is called on, wherever the func is defined.  
so it is if called as getter, setter or inherited from prototype chain.  
but as a constructor, its `this` will be the created object.

**called as eventHandler**  
`this` refers to the element the event fired from.  
while inline `on-eventHandler`'s `this` is set to the element on which it placed on.

---
**arguments, caller & callee**  
`arguments` is a special array object, has access to the arguments with `argumens[index]`  
`caller` & `callee` is current func caller & the called func  

```
var factoria = function(x){
	if(x===1) return x
	return arguments.callee(x-1)
}
```
**defined func props**  
`function` is kind of special object,  
thus can has its own props

```
function factoria(n){
	if(Number.isFinite(n) && n > 0 && n ){
		factoria[n] = n * factoria[n-1]
		return factoria[n]
	}
}
factoria[1] = 1
```
**some functional styles**  

```
// some func supposed to be called immediately
const val = (function (x){return x * x}(10))
// attention to the ( before 'function', it's required 
```


---
## 乱七八糟的感想
### 短得只适合发推
**thoughts on distraction**  
感觉如今相比几十年前娱乐方式太多了，也许更多的人因此被分散了注意力，个体从事研究的时间和效率总体来说降低了。从这个角度来说游戏,社交网络这些是否对科技进步的影响负面居多？(也许靠堆人数所以总体呈现还是在加速...)(也许只是我庸人自扰多虑了...)  
由"所有我所能想到的课题一定有前人做过了"定律可得:  

1. 先查下，应该有现成的统计资料; // => ?  
2. 没有的话,有什么可作为参考的统计数据来源?  
	* PC,移动端游戏年发行量
	* 地区平均个人游戏时间/人天
	* 社交网络年动态条数
	* 科技发展速度的较客观指标/人口总数(暴力的除法,可按地区/行业划分)

也许根本不存在这种客观指标, 干扰因素太多了...(有
  
左右两边是不同单位的y轴,x轴为时间(年),中间是不同颜色的曲线连接统计数据的那个图叫啥...  
  
* 有没有什么动物拥有科幻作品中的那种群体意识?(或者说蜂群思维...所以它们算是真的群体意识吗?  
* 个体意识与创造性有多大程度的相关性, 有何证据?
* to what extend is self-conscious relevant to creativeness? any prove to that?
* One best thing about winter is that you won't be/don't worry about mosquitos anymore  
* I'm humble and honest, kinda hard-working, eager to prove myself but not very well at communicating in real world, lack of self-descipline when alone. What kind of job could be suitable for me?
* Unfortunately, nothing, sir. Just keep that in mind, practice with those weak point. And u'd better be really at something, so as to be competitive.
* what's even more unfortunate is that you just get distracted way 2 easy...
* MDN🐂🍺, 方便我这样的菜鸡快乐地[递归式学习](https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-child#Formal_syntax)
* 如果有科技产品能检测到分心就施加肉体惩罚提醒我愿意第一个试用...
* 怎么还有程序员连递归都不懂的...

**WHICH ONE IS BETTER**  

* Failed interview, feeling depressed, I realized at last that I'm supposed to make contribution ___to the real world, as much as I'm capable of,___ instead of to the virtual world.  
* Failed interview, feeling depressed, I realized at last that I'm supposed to make ___as much___ contribution ___as I'm capable of, to the real world,___ instead of to the virtual world.



---
## NOTES ON CSS
Here are different pieces of CSS, in roughly the order I found most helpful to learn them, although the steps will overlap a bit.

1. Selection  
know your HTML elements and attributes really well.  You will not understand CSS without a good knowledge of HTML.  Understanding the tree structure of HTML, the element names, the difference between ids and classes will help you know how to target your styles.  Try to structure your HTML so you have to use as few selectors as possible in your CSS, because more selectors require more browser processing and, worse, make it really difficult to override styles for special cases or theme variants.  

2. Basic layout  
know the old school forms of layout: the box model, the difference between block and inline and inline-block, and how these affect padding, border, margin and width.  Learn about floats, and when you would want to use them instead of inline block for example.  Learn about positioning, especially relative, static, and absolute (fixed is more rare).  Also learn the newer flexbox model: it's sometimes quirky (the last row of justified content is a travesty), but it also can come in handy and save you some of the work in creating a layout yourself in floats, for example.  A good exercise is to try to recreate the layout of reasonable complex sites yourself with lots of columns and different types of lists.  A good exercise is creative a dropdown menu with only CSS, or a 5 star-rating widget.

3. Get familiar with other basic rules.  
Rules that aren't layout are usually pretty straight forward to understand (although animation is a little more complex).  Get to know how to style backgrounds, type, shadows, transformations, etc.  Lear how to style elements using the fewest possible images to reduce browser load times: when you can use CSS alone and what you have to use images for, how to crop and optimize images and sprite images.

4. Check out the differences between browsers.  
This is most noticeable in form styling--browsers differ greatly not only in default styles for forms, but even in the style rules they'll let you apply to various elements, even which elements they support at all.

5. Learn the hacks/tricks.  
There are a lot of common problems solved over and over again in what you might as a beginner think are slightly tricky ways.  One example, which comes up less often than it used to, is when you have to use an image instead of text, how do you keep the actual text on the page for search engines and screen readers?  How do you deal with images of different resolutions is a newer problem.

6. When you understand the fundamentals, start with the advanced tools.  
Learn about CSS preprocessors like Less and Sass, and how to use them efficiently so you don't end up with ridiculous amounts of code bloat.  Learn how to concatenate and minify style sheets, and use automatic spriters and image optimizers.  All of these tools are great, but learning the fundamentals yourself first will help you have a thorough knowledge of how the more advanced tools work.
  
### selector types
* **simple selectors**: match based on element type, class, id;  
* **attribute selectors**: match based on attributes values;
	* how to match: `[attr[~^$*]=val]`  
	* attrs that start with `data-` also accessible in JS via `el.dataset.camelAttr`
* **pseudo classes**: match elements that exist in a certain state (e.g. hovered, checked, 1st child of parents in DOM);  
	* syntax: `el:kindaState`  
	* often used: `:visited`, `:active`, `:focus`, `:hover`, `:checked`, `:required`...
	* with arguments: `:lang()`, `:nth-child(odd):nth-child(-n+15)`, `:dir()`, `:matches()`, `:not()`, `:nth-last-child()`, `:nth-last-of-type()`, `:nth-of-type()`
* **pseudo elements**: match **content** in a certain position in relation to an element.  
	* syntax: `el::kindaPostion`
	* often used: `::before`, `::after`,  `::first-letter`, `::first-line`, `::selection`, `::backdrop`
* **conbinators**: 
* **multiple selectors**: put multiple selectors to the same CSS rule.

```  
:nth-child( <nth> [ of <complex-selector-list> ]? )
where 
<nth> = <an-plus-b> | even | odd
<complex-selector-list> = <complex-selector>#

where 
<complex-selector> = <compound-selector> [ <combinator>? <compound-selector> ]*

where 
<compound-selector> = [ <type-selector>? <subclass-selector>* [ <pseudo-element-selector> <pseudo-class-selector>* ]* ]!
<combinator> = '>' | '+' | '~' | [ '||' ]

where 
<type-selector> = <wq-name> | <ns-prefix>? '*'
<subclass-selector> = <id-selector> | <class-selector> | <attribute-selector> | <pseudo-class-selector>
<pseudo-element-selector> = ':' <pseudo-class-selector>
<pseudo-class-selector> = ':' <ident-token> | ':' <function-token> <any-value> ')'

where 
<wq-name> = <ns-prefix>? <ident-token>
<ns-prefix> = [ <ident-token> | '*' ]?  | 
<id-selector> = <hash-token>
<class-selector> = '.' <ident-token>
<attribute-selector> = '[' <wq-name> ']' | '[' <wq-name> <attr-matcher> [ <string-token> | <ident-token> ] <attr-modifier>? ']'

where 
<attr-matcher> = [ '~' |  |  | '^' | '$' | '*' ]? '='
<attr-modifier> = i
```

### cascade & inheritance

#### cascade
1. importance  (`!importance` after a prop pair
2. specificity  
3. source order

selector|score(forEach)
---|---
element & psudo-element|1
class, attribute & psudo-class|10
id|100
inline style props|1000
* conbinator(`>`, `+`, `~`, ` `), `*` and `:not`has no effect on specificity.  

#### inheritance
some property values applied to an element will be inherited by that element's children, and some won't. Which prop inherites by default and which don't is down to **common sense** (mag, pad, bg-img, font, color for example)

**Controlling inheritance**  
`inherit`  
`initial`  
`unset`  
`revert`  


---  
### float & clear (disencouraged though
`display: float;`  
**how to clear it**  
`.container::after {clear: left | right | both;}`
`.container {overflow: auto; display:flow-root }`

(and some other tricks[^byNico]:  
`:first-child::before {display: table}`  
will prevent first-child's margin-top from combine, or collapse[^hacks]  

### <a name="position">position</a>
`position: static` (by default in 'normal flow'  
other options include:  
`relative`: relative to static position;  
`absolute`: relative to window, or container( if an ancestor ele isn't static, `relative` for example;  
`fixed`: relative to window, or container( if ancestor's `transform`|`perspective`|`filter` setted, stay where it is;  
`sticky`: relative to window, act like static, but fixed when moved to certain position;  

### visual formatting model
视觉盒子模型将页面元素转化成0,1或多个符合[CSS box model](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model)的box, 每个box被下列因素所定义: 
 
* box的维度, 被精确定义或约束了, 或者没有
* box的类型: inline, inline-level, atomic inline-level, block  
* position scheme: static, relative, absolute & [more](#position)
* ...(unfinished)
  
### box formatting model
a BFC 是一个网页CSS渲染的一部分. 也是块级盒子布局出现, float元素和其他元素交互的区域  
BFC由以下方式创建:  

* 根元素和包括根元素的啥
* 

---
## 面试反思
一些后来觉得答得不是很好的问题
### 为何离职(下次这么说
__想过好几次这个问题该怎么回答, 我就直说了__  

1. 效率低，外包项目客户, 上司不满意, 于是被开了  
 * 容易沉迷于技术细节, 看文档时容易走神看别的; (但还是技术分享，从不摸鱼！
 * 看着不舒服就写了删删了写, 老是重构; (虽然只是自我感觉良好, 没同事评价  
2. 我也觉得我自己基础挺差的, 项目时间紧压力又大没多少时间自学充电, 离职了正好有整块的时间补一下 (虽然是大学时就该学的基础...) 所以实际上被开了当时还挺轻松的...

### 你了解TCP/IP ？
(你塔玛不熟就别写上去...


### 什么是伪类，选择器优先级？
(这些概念都不知道你面个🐔🚌前端, **你可以滚了**

[^hacks]: [css-using-displaytable-with-before-pseudo-element](https://stackoverflow.com/questions/25699262/css-using-displaytable-with-before-pseudo-element)
[^byNico]: [micro-clearfix-hack by nicolasgallagher](http://nicolasgallagher.com/micro-clearfix-hack)