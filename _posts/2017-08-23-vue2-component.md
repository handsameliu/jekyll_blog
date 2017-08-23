---
layout:     post
title: Vue2组件通信
subtitle:   "Vue2"
date:       2017-08-23
author:     "handsameliu"
header-img: "img//post-bg-vue.jpg"
tags:
  - 总结
  - Vue2
---

## Vue2 组件之间的通信

在父子组件通信中，无论是子级向父级传值，还是父级向子级传值，都是需要一个中间介质作为中转。

当父级向子级传值时需要用到props属性，子级向父级传值时需要用到自定义事件。这个结合了ng和react。相比之下Vue简单易懂。

<!-- more -->

### 父组件向子组件传值

首先来写一个子组件，在子组件的new Vue对象中添加props属性，在属性中以数组的形式写入父级要传输过来的属性名称。

```
<template lang="html">
    <div id="app">
        测量数据 {{message}}
    </div>
</template>
<script>
    export default {
        name:'recordIndex',
        props:['message']
    }
</script>
<style scoped>
</style>
```

写完后就可以写父组件了，需要在组件上绑定一个属性，这个属性就是子组件接收时所用的。然后将想要传递的内容写入到=之后即可。

```
<template lang="html">
    <div id="app"> 
        <recordIndex v-bind:message='我是父级给子级传递的内容'></recordIndex>
    </div>
</template>
<script>
    import recordIndex from './components/recordIndex'
    export default {
        name:'records',
        components:{recordIndex},
    }
</script>
<style scoped>
</style>
```

这样，一个简单的父级向子级组件传值的功能就做好了。

> 子组件在props中创建一个属性，用以接收父组件传过来的值
> 父组件中注册子组件并在子组件标签中添加子组件props中创建的属性
> 把需要传给子组件的值赋给该属性即可

### 子级向父级传值

子级向父级传值就需要用到Vue自带的发射事件了。（ng也有这样的事件，不过只用过一次就被领导否决了，最后采用service和factory的方式进行父子通信）

还是先来看子组件，这个例子时点击发送消息，所以需要在页面中添加一个button按钮并绑定事件，事件内容如下

```
<template lang="html">
    <div id="app">
        <button v-on:click="sendParentMsg">点我给父级传值</button>
    </div>
</template>
<script>
    export default {
        name:'recordIndex',
        methods:{
            sendParentMsg:function(){
                this.$emit('listenToChild','我这里是子组件发出的信息')
            }
        }
    }
</script>
<style scoped>
</style>
```

这里用到了Vue自带的$emit方法，这个方法相当于广播，将内容发送出去。所需要的参数只有两个，一个是父级页面接受消息的方法另一个是想要传递的参数

那么父级组件就需要在页面的子组件标签上定义并绑定一个名为`listenToChild`的方法，代码如下：

```
<template lang="html">
    <div id="app">
        <recordIndex v-on:listenToChild="showChildMsg"></recordIndex>
    </div>
</template>
<script>
    import recordIndex from './components/recordIndex'
    export default {
        name:'records',
        methods:{
            showChildMsg(msg){
                alert(msg)
            }
        },
        components:{recordIndex},
    }
</script>
<style scoped>
</style>
```

绑定事件后，就指定了子级组件的事件名称，然后就需要把事件内容指定为父级组件中自定义的接受方法了，

然后再父级页面中点击子组件按钮即可，如果成功会弹出子组件发出的信息。

> 子组件中需要手动触发一个自定义事件
> 将需要传的值作为$emit的第二个参数，该值将作为实参传给父级响应自定义事件的方法
> 在父组件中注册子组件并在子组件标签上绑定对自定义事件的监听

### 同级组件之间的通信

同级组件可以用中央事件总线的方式通信，原理就是使用一个全局的中间介质。需要使用到的方法有$emit发送，$on接收，$off结束监听。

首先需要定义一个中间介质。

可以在当前文件夹中定义，名为eventBus的js文件。文件内只创建一个Vue实例，它就承担起了组件之间通信的桥梁，也就是中央事件总线。

```
import Vue from 'vue';
export default new Vue;
```

在a.js文件中建议写一个事件，这样可以清晰的控制。
在页面中引入之前定义好的介质文件eventBus.js。
事件方法中使用介质eventBus.$emit事件，将同级文件接收的自定义事件sendMsg和内容发射出去。
PS:$emit实例方法触发当前实例(这里的当前实例就是eventBus)上的事件,附加参数都会传给监听器回调。

```
<template>
	<div class="timeline-demo">
		<button type="primary" v-on:click='sendMsg'>给其他组件传值</button>
	</div>
</template>


<script>
    import eventBus from './eventBus'
    export default {
        name:'recordTimeLine',
        methods:{
            sendMsg(){
                var _this = this;
                eventBus.$emit('childMsg','my name is username')
            }
        }
    }
</script>
<style lang="less">
</style>
```

在b.js文件中也需要在mounted中写一个用于接收的监听事件,用来监听a页面发射出的childMsg事件。
`$on`：第一个参数是a页面$emit发射的第一个参数，第二个参数是一个回调方法，用于接收并操作$emit传递的参数
mounted:是一个Vue生命周期中的钩子函数，简单点说就类似于jquery的ready，Vue会在文档加载完毕后调用mounted函数。

```
<template lang="html">
    <div id="app">
        {{message}}
    </div>
</template>
<script>
    import eventBus from './eventBus' 
    export default {
        name:'recordIndex',
		data(){
			return{
				message:'我是初始值'
			}
		},
        mounted(){
            let _this = this;
            eventBus.$on('childMsg',(msg) => {
                _this.message = msg;
            })
        }
    }
</script>
<style scoped>
</style>
```

当两个组件写好后就需要用到一个放置两个组件的容器了，接下来创建一个index组件来存放a，b组件

```
<template lang="html">
    <div id="app">
        <recordIndex></recordIndex>
        <recordTimeLine></recordTimeLine>
    </div>
</template>
<script>
    import recordTimeLine from './components/recordTimeLine'
    import recordIndex from './components/recordIndex'
    export default {
        name:'records',
        components:{recordTimeLine,recordIndex},
    }
</script>
<style scoped>
</style>
```

这样当点击a组件的button时，触发了发射事件，b组件就可以接收并绑定显示到页面了

> 创建一个事件总线，例如demo中的eventBus，用它作为通信桥梁
> 在需要传值的组件中用eventBus.$emit触发一个自定义事件，并传递参数
> 在需要接收数据的组件中用eventBus.$on监听自定义事件，并在回调函数中处理传递过来的参数

