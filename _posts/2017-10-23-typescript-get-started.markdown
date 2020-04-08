---
layout: post
title: "Typescript Getting Started"
subtitle: ''
author: "Xiaozhe Yao"
header-style: text
tags:
  - Typescript
---

最近开始尝试在项目中使用Typescript, 简单来说，Typescript是一个JavaScript的超集（即支持JavaScript的全部语法，完全兼容），TS在JavaScript的基础上增加了静态类型检查和面向对象的特性。

Typescript可以在编译阶段"降级"为ES5或ES3，以在浏览器中运行，因此Typescript获得了和JavaScript一样的兼容性，可以在全部的现代浏览器中运行。

npm是安装TypeScript的最简洁的方式。npm install -g typescript即可全局安装，不过我通常会选择安装在项目的依赖中。即npm install typescript，然后通过node_modules/.bin/tsc来运行Typescript的编译器。

Hello World
-----------

Typescript的文件以ts结尾，我们首先来编写一个greeter.ts的文件。内容如下：

```
function greeter(person){
    return "Hello, "+person
}

let user = "World"

document.getElementById('app').innerHTML=greeter(user)

```

编写完ts文件之后，我们可以通过tsc greeter.ts来将ts文件转换为以js为后缀的JavaScript文件。这样，就可以在我们的浏览器中运行了。

所以，我们再编写一个对应的html文件，内容如下：

```
<html>
    <body>
        <div id="app"></div>
    </body>
    <footer>
        <script src="./greeter.js"></script>
    </footer>
</html>

```

在浏览器中打开这个文件，就可以看到 Hello World 在页面上了。

类型检查
----

不难看出，我们希望在greeter函数中，接收一个字符串来作为参数。如果我们在JavaScript中传入一个非字符串函数，会怎么样呢？例如，修改刚刚得到的js文件，我们传入一个空的对象：

```
function greeter(person) {
    return "Hello, " + person;
}
var user = {};
document.getElementById('app').innerHTML = greeter(user);

```

我们会看到，页面上的文字变成了Hello, [object Object]。这不是我们想要的。我们不希望其他的开发者在调用这个函数时，传入非字符串的函数。因此，我们要检查一下这个类型。因此，我们将ts文件修改为：

```
function greeter(person:string){
    return "Hello, "+person
}
var user = {}
document.getElementById('app').innerHTML=greeter(user)

```

此时我们也试图传入一个空的对象，重新运行tsc greeter.ts，我们会得到下面的信息：

![](https://pic3.zhimg.com/80/v2-4ee639abae56576ea165d85ffeb7077e_1440w.jpg)

error TS2345: Argument of type '{}' is not assignable to parameter of type 'string'.

Typescript在编译这个文件给出了报错，原因是'string类型的参数不能接受{}这个对象'，在编译阶段就将我们的想法拒之于门外。与传入参数类似，方法的返回值也可以指定类型：

```
function greeter(person:string): string {
    return {}
}

var user = {}

document.getElementById('app').innerHTML=greeter(user)

```

编译器同样会告诉我们：Type '{}' is not assignable to type 'string'.

值得注意的是，虽然编译器报了error，但是依然会生成对应的js文件。但它可能不会按预期的执行。

在较为大型的项目中，我们可能一不留神就将一个空的对象传入了类似的函数，有了静态检查之后，我们可以极大地避免这类问题，让我们的程序更加稳定。可以说，静态检查是一个非常必要的特性了。

接口和类
----

> *像鸭子一样走路并且嘎嘎叫的就叫鸭子。\
> -- James Whitecomb Riley*

不妨设想一个问题，如果有很多不同类型的对象，如Duck, Chicken, Fish等等，我们不能使用object来分辨他们(因为都是object)，这时候就可以使用接口或者类。Typescript使用鸭式辨型法，我们可以编写不同类型的接口，制定不同类型动物的规则来分辨他们，如：

```
interface Duck {
    isDuck:boolean,
    name:string
}
interface Chicken {
    isChicken:boolean
}
function greeter(duck:Duck) {
    return "This is a Duck, whose name is "+duck.name
}
var duck = {isDuck:true,name:"Donald"}
document.getElementById('app').innerHTML=greeter(duck)

```

这样，我们就可以限定传入greeter方法的参数，必须"实现"了Duck接口的内容，这里的实现只是形式上的实现，换句话说，只要有isDuck和name属性的对象，就会被认为是一个实现了Duck接口的对象。就是开头的那句话，"像鸭子一样走路并且嘎嘎叫的就叫鸭子"

不是所有的属性都是必须的，我们可以用"?"来标记一个可选的属性，如：

```
interface Duck {
    isDuck:boolean,
    name:string,
    color?:string
}

```

这样，无论有没有color属性，都会被认为是一个"实现"了Duck接口的类。

如果我们想要在创建完这个对象之后，就再也不修改它的一些属性，如isDuck，它应该是一个只读的属性。我们可以用readonly来标记。如下：

```
interface Duck {
    readonly isDuck:boolean,
    name:string,
    color?:string
}

```

readonly和const的区别：readonly标记的是属性，而const所声明的是一个变量。

除接口之外，Typescript也支持面向对象的编程。这是一个ES6的新特性，从ES6开始，JavaScript也开始支持面向对象编程了。

```
class Duck {
    name:string
    constructor(name:string){
        this.name = name
    }
    greet():string{
        return "Hello, "+this.name
    }
}

var duck = new Duck("Donald");
document.getElementById("app").innerHTML = duck.greet()

```

上面的代码创建了一个Duck类,其中包含了一个name属性，一个构造函数，和一个返回字符串的greet函数。我们可以通过new这一关键字来创建一个Duck类的对象。并调用其函数。在greet函数中，我们使用this来引用类成员。

在Typescript中，成员变量是默认为public的，即可以在类的外部调用，我们可以用private来将其限制在类内部访问，

类似于接口中，我们也可以使用readonly来将属性设置为只读。 只读属性必须在声明时或构造函数里被初始化。

面向对象中另一个很重要的特性是继承，我们可以使用extends来实现继承。

```
class Animal {
    protected name:string
    constructor(name:string){
        this.name = name
    }
    greet():string{
        return "Hello "+this.name
    }
}

class Duck extends Animal {
    greet():string{
        return "Hello, this is a Duck, whose name is "+this.name
    }
}

var duck = new Duck("Donald");
var animal = new Animal("Mickey")
document.getElementById("app").innerHTML = duck.greet()
document.getElementById("app").innerHTML = document.getElementById("app").innerHTML+' '+animal.greet()

```

这样，我们的Duck就继承了Animal中的name属性和contructor方法，注意，我们在这里使用protected属性，以使得name这一属性能够在Duck子类中使用。

工程配置
----

在项目的目录下，可以使用tsconfig.json这一文件来指定编译时的属性。如果我们直接在项目目录下执行tsc，Typescript解释器就会在当前目录寻找tsconfig.json这一文件，如果当前项目目录没有，就会逐级向上寻找。我们也可以使用--project或-p来指定项目目录。

```
{
    "compilerOptions": {
        "module": "commonjs",
        "noImplicitAny": true,
        "removeComments": true,
        "preserveConstEnums": true,
        "sourceMap": true
    },
    "files": [
        "ducks.ts"
    ]
}

```

上面是一个tsconfig.json文件的示例，全部的编译器选项可以在[编译选项](https://link.zhihu.com/?target=https%3A//www.tslang.cn/docs/handbook/compiler-options.html)里看到。

"files"指定一个包含相对或绝对文件路径的列表。 我们可以用"include"和"exclude"属性指定一个文件glob匹配模式列表，如：

```
   "include": [
        "src/**/*"
    ],
    "exclude": [
        "node_modules",
        "**/*.spec.ts"
    ]

```

Vue/Angular的结合
--------------

很多较大型的Web框架都被Typescript所支持，Angular 2已经整个项目都可以用Typescript来完成了。Vue 2.5也大大增强了对Typescript的支持。我们可以在[快速起步](https://link.zhihu.com/?target=https%3A//www.typescriptlang.org/samples/index.html)里看到官方完成的各种起步模板。