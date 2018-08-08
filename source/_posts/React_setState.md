title: React 之 setState
date: 2017-08-07 12:18:16
tags: [Javascript]
categories: frontend
---

## 用途
React里面的setState应该是最常用的api了。在React中，视图组件都是基于数据的变化而更新的。那么改变数据就离不开setState。下面看下基本的用法

```js
//...
constructor(){
    super();
    this.state = {
        index: 0
    };
}

handleClick(){
    this.setState({
        index: 1
    });
}

render(){
    return (
        <div>{this.state.index}</div>
    )
}
```

我们通过setState改变了state中index的值，那么引用该值的视图就会被更新。

<!-- more -->

## 异步更新
稍微修改一下上面的代码

```js
this.setState({
    index: 1
});

console.log(this.state.index); //0
```

我们在调用setState后立刻对index值进行了输出，此时我们会看到打印出来的值是0，并非我们设置的1。Why？

在官方的描述中，setState操作并不保证是同步的，也可以认为是异步的。

React在setState之后，会经对state进行diff，判断是否有改变，然后去diff dom决定是否要更新UI。
如果这一系列过程立刻发生在每一个setState之后，就可能会有性能问题。比如：你在短时间内频繁setState。
React会将state的改变压入栈中，在合适的时机，批量更新state和视图，达到提高性能的效果。

## 那么如何知道state已经被更新了呢？

方法一：

```js
//api setState(updater, [callback])

setState({
    index: 1
}}, function(){
    console.log(this.state.index);
})
```

方法二：
```js
componentDidUpdate(){
    console.log(this.state.index);
}
```

## setState还可以接收function作为参数

我们看下这样一个场景

```js
class Com extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            index: 0
        }
        this.add = this.add.bind(this);
    }

    add(){
        this.setState({
            index: this.state.index + 1
        });
        this.setState({
            index: this.state.index + 1
        });
    }
}
```

这里我们执行add方法后，得到的结果是state中的index值为1，并不是2。

原本的设想是每一次setState中，拿到的state都应该是已经改变过的值，但其实并不是。

这里需要在每一次setState中，拿到上一次改变state后的值，在这个值的基础上，再进行操作。显然，对于setState这种异步的操作使用上面的写法是达不到要求的，所以这里React提供另一种方式。

```js
//api (prevState, props) => stateChange

class Com extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            index: 0
        }
        this.add = this.add.bind(this);
    }

    add(){
        this.setState(prevState => {
            return {index: prevState.index + 1};
        });
        this.setState(prevState => {
            return {index: prevState.index + 1};
        });
    }
}
```

在setState的第一个参数中传入function，该function会被压入调用栈中，在state真正改变后，按顺序回调栈里面的function。该function的第一个参数为上一次更新后的state。这样
就能确保你下一次的操作拿到的是你预期的值。