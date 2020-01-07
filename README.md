# 浅析MVC

## MVC是什么
**MVC是Model View Controller。**
Model:主要负责从服务器获取数据把数据传给Controller。还要将Controller监听到的用户提交的数据上传到服务器。所以它主要是用于封装与应用程序的业务逻辑相关的数据以及对数据的处理方法，会有一个或多个视图监听此模型。一旦模型的数据发生变化，模型将通知有关视图。

```JS
const model = {
    //数据
    data: ''
    //初始化
    init(){}
    //方法
    save(){}
    update(){}
}

```

View:是这个JS模块对应在HTML中的部分，当模型的数据发生变化，视图相应地得到刷新自己的机会。
```JS
const view = {
    init() {}
    template: '<h1>hello</h1'>   //视图渲染
}

```

Controller:用于调用model的数据来更新View，用于控制应用程序的流程，它处理用户的行为和数据model上的改变。
```JS
const controller = {
    view: null,
    model: null,
    init(view, model){
        this.view = view
        this.model = model
        this.bindEvents()
    }
    render(){
        this.view.querySelector('name').innerText = this.model.data.name
    },
    bindEvents(){}
}
```

## EventBus是什么
Eventbus是一种设计模式，主要用于组件或对象间的通信简化。其包含许多方法，例如：用on监听事件，trigger方法来触发事件。
```JS
const model = {
    data: {
        i: come from somewhere
    }，
    update(data) {
        Object.assign(model.data, data)
        eventBus.trigger('model:updated')   //触发 model:updated 事件
    }
}
const controller = {
    //初始化方法
    init() {
        //初始化渲染
        view.render(model.data.i)
        eventBus.on('model:updated', () => {    //监听 model:updated 方法并渲染更新页面
            v.render(model.data.i)
        })
    },

}
```

## 表驱动编程
所谓表驱动法(Table-Driven Approach),简单讲是指用查表的方法获取值。

假设我们要把一个数字的数组按星期几的形式输出，那么我们可能这样做：
```JS
let arr=[2,5,6,3]
let newArr =[]
arr.forEach(item=>{
  if(item===1){
    newArr.push('星期一')
  }else if(item===2){
    newArr.push('星期二')
  }else if(item===3){
    newArr.push('星期三')
  }else if(item===4){
    newArr.push('星期四')
  }else if(item===5){
    newArr.push('星期五')
  }else if(item===6){
    newArr.push('星期六')
  }else{
    newArr.push('星期日')
  }
})
console.log(newArr)
```

如上这样可能写很多类似的代码，如果用表驱动法可以这样做：
```JS
const week={
  1:"星期一",
  2:"星期二",
  3:"星期三",
  4:"星期四",
  5:"星期五",
  6:"星期六",
  7:"星期日"
}
arr.forEach(item=>{
  newArr.push(week[item])
})
console.log(newArr)
```
由此可看出，使用表驱动可以简化代码，使结构更加简洁清晰

## 模块化理解

如果没有模块化，那么引入外部文件将是这样的：
```JS
<script src="jquery.js"></script>
<script src="main.js"></script>
<script src="a.js"></script>
<script src="b.js"></script>
<script src="c.js"></script>
```
这样所有的文件都放在一起，而且这些文件的顺序不能错乱，否则将会出错。缺点如下：
* 污染全局变量
* 维护成本高
* 依赖关系不明显

而ES6使用import关键字引入模块，通过export关键字导出模块使用模块化。解决了这些问题。
```JS
//方法一：导出需要的数据
var arr=[]
var str='haha'
export {
	arr,str
}
import {arr,str} from './info.js'
//方法二：让导入者自己命名
export default function (){
	console.log("ooooo")
}
import myFunc from './info.js'
//方法三：定义时直接导出
export var num = 100
export function mul(num1,num2){
    return num1+num2
}
//导入全部模块
import * as aaa from './info.js'
console.log(aaa.属性)
```