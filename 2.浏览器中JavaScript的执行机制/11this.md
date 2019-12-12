# this:从JavaScript执行上下文的视角讲清除this

作用域链和this是两套不同的系统  

![](img/执行上下文this.png)  

**this是和执行上下文绑定的**，也就是说每个执行上下文中都有一个this

## 全局执行上下文中的this
全局执行上下文中的this是指向window对象的

## 函数执行上下文中的this
```
function foo(){
  console.log(this)
}
foo()

```
默认情况下调用一个函数，其执行上下文中的this也是指向window对象的。

### 设置函数执行上下文的this
- 通过函数的call方法设置
- 通过对象调用方法设置
- 通过构造函数中设置

### 1.通过函数的call方法设置
```
let bar = {
  myName : " 极客邦 ",
  test1 : 1
}
function foo(){
  this.myName = " 极客时间 "
}
foo.call(bar)
console.log(bar)
console.log(myName)

```
bar的myName属性由“极客邦”变为“极客时间”

### 2.通过对象调用方法设置
```
var myObj = {
  name : " 极客时间 ", 
  showThis: function(){
    console.log(this)
  }
}
myObj.showThis()

```
this值指向myObj  

**使用对象来调用其内部的一个方法，该方法的this是指向对象本身的**

```
var myObj = {
  name : " 极客时间 ",
  showThis: function(){
    this.name = " 极客邦 "
    console.log(this)
  }
}
var foo = myObj.showThis
foo()

```
this指向全局window对象  

- 在全局环境中调用一个函数，函数内部的this指向的是全局变量window
- 通过一个对象来调用其内部的一个方法，该方法的执行上下文中的this指向对象本身

### 3.通过构造函数中设置
```
function CreateObj(){
  this.name = " 极客时间 "
}
var myObj = new CreateObj()

```
当执行new CreateObj()的时候，JS引擎做了以下的事情  

- 首先创建了一个空对象tempObj
- 接着调用CreateObj.call方法，并将tempObj作为call方法的参数，这样当CreateObj的执行上下文创建时，它的this就指向了tempObj对象
- 然后执行CreateObj函数，此时的CreateObj函数执行上下文中的this指向了tempObj对象
- 最后返回tempObj对象

```
  var tempObj = {}
  CreateObj.call(tempObj)
  return tempObj

```

## 嵌套函数中的this不会从外层函数中继承
```
var myObj = {
  name : " 极客时间 ", 
  showThis: function(){
    console.log(this)
    function bar(){console.log(this)}
    bar()
  }
}
myObj.showThis()

```
函数bar中的this指向全局window对象，函数showThis中的this指向myObj对象

```
var myObj = {
  name : " 极客时间 ", 
  showThis: function(){
    console.log(this)
    var self = this
    function bar(){
      self.name = " 极客邦 "
    }
    bar()
  }
}
myObj.showThis()
console.log(myObj.name)
console.log(window.name)

```
在showThis函数中声明一个变量self用来保存this，把this体系转换为了作用域的体系

```
var myObj = {
  name : " 极客时间 ", 
  showThis: function(){
    console.log(this)
    var bar = ()=>{
      this.name = " 极客邦 "
      console.log(this)
    }
    bar()
  }
}
myObj.showThis()
console.log(myObj.name)
console.log(window.name)

```
箭头函数中的this取决于它的外部函数


