---
layout:     post
title:		react 父子传值，交互，调用方法
date:       2018-08-08
author:     "handsameliu"
header-img: "img/post-bg-react.jpg"
tags:
  - 总结
  - react
---

### 父子通信

**父传子，通过props传递参数**

<!-- more -->

```javascript
import React, {Component} from 'react'

export default class Parent extends Component {
    constructor () {
        this.state = {
            name: 'testliu'
        }
    }
    render () {
        return (
            <Child myName={this.state.name}/>
        )
    }
}

export class Child extends Component {
    constructor (props) {
        super(props)
    }
    render () {
        return (
            <div>
                {this.props.myName}
            </div>
        )
    }
}
```

**子传父，通过调用父级方法实现**

```javascript
import React, {Component} from 'react'

export default class Parent extends Component {

    sonNameFn (name) {
        console.log(`my son's name is ${name}`)
    }

    render () {
        return (
            <Child printName={this.sonNameFn}/>
        )
    }
}

export class Child extends Component {

    handleChange (e) {
        this.props.printName(e.target.value)
    }

    render () {
        return (
            <div>
                <input onChange={this.handleChange}/>
            </div>
        )
    }
}
```

> 注意：**父组件加载子组件，子组件的的构造函数只会加载一次**。
> 如果父组件(容器组件)获取到远程数据，传递到子组件，不要在子组件的状态里写入数据，只用使用 **props.** 的方式去获取，以下为错误示例：

```javascript
import React, {Component} from 'react'

export default class Parent extends Component {

    constructor () {
        this.state = {
            sonInfo: {}
        }
    }

    componentDidMount () {
        this.setState({
            sonInfo: this.getChildInfo()
        })
    }

    getChildInfo () {
        ...
        return {
            name: 'testliu',
            gender: 1,
            age: 18,
            address: 'china'
        }
    }

    render () {
        return (
            <Child info={sonInfo}/>
        )
    }
}

export class Child extends Component {
    constructor () {
        this.state = {
            info: this.props.info
        }
    }

    render () {
        console.log(this.props.info) // {}
        console.log(this.state.info) // {name: 'testliu',gender: 1,age: 18,address: 'china'}
        // 出现这样的情况是因为父组件在获取到请求结果之前就已经加载了子组件，但是子组件在一个声明周期中，构造函数只会运行一次，所以父组件在更新状态后，是无法通过构造函数去改变子组件的内容。
        return (
            <div>
                {info.name}
                {info.gender}
                {info.age}
                {info.address}
            </div>
        )
    }
}
```

**父调子方法**

```javascript
import React, {Component} from 'react'

export default class Parent extends Component {

    parentFn (childThis) {
        this.child = childThis
    }

    callChildFn () {
        this.child.printInfo()
    }

    render () {
        return (
            <div>
                <button click={()=>{this.callChildFn()}}>点我执行子组件方法</button>
                <Child refFn = {this.parentFn}/>
            </div>
        )
    }
}

export class Child extends Component {

    constructor (props) {
        super(props)
        this.state = {
            name: 'testliu',
            gender: 1,
            age: 18,
            address: 'china'
        }
    }

    componentDidMount () {
        this.props.refFn(this)
    }

    printInfo () {
        console.log(`info: ${this.state.name},${this.state.gender},${this.state.age},${this.state.address}`)
    }

    render () {
        return (
            <div>
                child
            </div>
        )
    }
}
```

**ref**

通过ref你可以拿到真实的DOM元素或组件实例

> 16.3 后的版本推荐使用 **React.createRef()** API, 之前的推荐使用回调

```javascript
import React, {Component} from 'react'

export default class Parent extends Component {

    constructor (props) {
        super(props)
        this.state = {
            inputRef: React.createRef()
        }
    }

    render () {
        return (
            <div>
                <input ref={this.inputRef} />
            </div>
        )
    }
}
```

访问 ref

```javascript
const ref = this.inputRef.current
```

ref的值根据节点类型的不同会有不同：

- 用HTML DOM节点的时候得到的是真实的dom

- 自定义组件节点时，得到的是组件的实例

- 函数式组件因为没有实例，所以不应该使用 ref ，会打印错误(回调的方式得到的是undefined，createRef的current得到的是null)

callback ref

```javascript
<input ref={el=>this.inputRef=el}/>
```

ref接受一个回调函数，回调函数会把input元素当做参数。

ref回调会在组件挂载时调用，并传入DOM，卸载时会调用并传入null，会在conponentDidMount 和 componentDidUpdate 之前调用

```javascript
import React, {Component} from 'react'
const Child = () => {
    return (
        <div>
            <input ref={this.props.inputRef}/>
        </div>
    )
}

export default class Parent extends Component {
    render () {
        return (
            <div>
                <Child inputRef={el=>this.inputEle=el}/>
            </div>
        )
    }
}
```

如果ref回调是一个行内函数，在更新时期会调用两次，第一次是null，第二次才是DOM，因为新的函数实例，在每次渲染的时候，react需要先清除旧的ref，然后设置一个新的，大多数时候不必担心，想修正可以传一个固定的函数



