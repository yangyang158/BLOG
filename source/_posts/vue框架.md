---
title: vue框架
date: 2020-05-21 13:57:45
tags:
---

## 一、数据双向绑定
    1、标签内：使用{{}}
    eg：<p>{{message}}</p>
    2、标签的属性值：v-bind:属性名(v-bind可以省略)
    eg：<p v-bind:title="message">热烈欢迎</p>
    eg：<p :title="message">热烈欢迎</p>
    eg：<p :title="index === 0 ? '我是title' : ''">热烈欢迎</p>
## 二、条件渲染
    1、控制元素显示与隐藏(dom节点不会生成): 使用v-if、v-else、v-else-if
    eg：
    <div>
        <div v-if="num > 5">{{num}}大于5</div>
        <div v-else-if="num > 2">{{num}}大于2小于5</div>
        <div v-else>{{num}}小于2</div>
    </div>
    2、控制元素显示与隐藏(dom节点会生成，通过display为none或block进行隐藏): v-show
    eg: 
    <p v-show="false">隐藏</p>
    <p v-show="true">显示</p>
## 三、循环
    使用: v-for, 形式: site in sites
    eg：
    <div>
        <div v-for="(itemNum, index) in numLists" :key="index">
            {{ index+1 }}、{{ itemNum }}
        </div>
    </div>
## 四、事件绑定
    1、绑定使用：v-on, 简写：@click
    eg：
    <Template>
        <div>
            <div v-for="(itemNum, index) in numLists" :key="index" @click="sayNum(itemNum)">
                {{ index+1 }}、{{ itemNum }}
            </div>
        </div>
    </Template>
    <script>
        export default {
            "name": 'App',
            data: function() {
                return {
                    numLists: [200, 121, 4343, 456]
                }
            },
            methods:  {
                sayNum(itemNum) {
                    this.numLists.push(itemNum)
                }
            }
        }
    </script>
    2、事件修饰符
    v-on:click.once: 只调用一次
    v-on:click.stop: 阻止事件冒泡
    v-on:click.self: 只当事件在该元素本身（而不是子元素）时触发 
    v-on:click.prevent
    v-on:click.capture
## 五、表单与业务数据的双向绑定
    使用: v-model
    eg:
    <div>
        <p>{{ test }}</p>
        <input v-model="test">
    </div>

## 六、计算属性computed
    eg：监听到numLists数组发生改变, lastNum 就会重新计算值
    <Template>
        <div>
            <div v-for="(itemNum, index) in numLists" :key="index" @click="addNum(itemNum)">
                {{ index+1 }}、{{ itemNum }}
            </div>
            <div>获取数组的最后一个元素{{ lastNum }}</div>
        </div>
    </Template>
    <script>
        export default {
            "name": 'App',
            data: function() {
                return {
                    numLists: [200, 121, 4343, 456]
                }
            },
            methods:  {
                addNum(itemNum) {
                    this.numLists.push(itemNum)
                }
            }
            computed: {
                lastNum() {
                    const length = this.numLists.length
                    return this.numLists[length -1 ]
                }
            }
        }
    </script>

    computed vs methods 
    我们可以使用 methods 来替代 computed，效果上两个都是一样的，但是 computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用 methods ，在重新渲染的时候，函数总会重新调用执行。

## 七、监听属性watch
    eg：监听数据变化, 执行对应操作
    <template>
        <div>
            <input v-model="meters" type="text">米
            <input v-model="kilometers" type="text">千米
        </div>
    </template>
    <script>
    export default {
        "name": 'App',
        data: function() {
            return {
                meters: 0,
                kilometers: 0
            }
        },
        watch: {
            meters(val) {
                this.kilometers = val * 1000
            },
            kilometers(val) {
                this.meters = val / 1000
            }
        }
    }
    </script>
## 八、自定义 全局指令和局部指令
    eg: 注册一个全局指令
    <template>
        <div>
            <div v-changeColor="{text: '使用自定义指令', color: 'red'}"></div>
        </div>
    </template>
    <script>
        import Vue from 'vue'
        Vue.directive('changeColor', function(el, binding){
            el.innerHTML = text
            el.style.color = color
        })
    </script>
## 九、生命周期
    beforeCreated —— 创建组件实例之前
    created —— 创建组件实例
    beforeMounted —— 组件挂载到页面之前
    mounted —— 组件挂载到页面, 在此处虚拟的dom节点被替换成真实的dom节点, 并插入dom树中
    beforeUpdated —— 组件更新之前
    updated —— 组件更新
    beforeDestoryed —— 组件销毁之前
    destoryed —— 组件销毁
    activated —— 组件被激活时调用
    deactivated —— 组件被移除时调用
## 十、注册全局组件和局部组件
    注册：Vue.component(tagName, {
        // 声明 props集合
        props: {
            propA: Number,
            propB: [String, Number],
            ...
        },
        // 渲染模板
        template: '<div></div>',
        data: function () {
            return {}
        },
        methods: {},
        ...
    })
    调用：<tagName></tagName>
## 十一、组件之间的通信
    1、父传子：
        a、通过props——单向传递, 子组件不能修改
        b、通过this.$children[0]字段 获取子组件的属性和方法
        c、在子组件上声明ref, 通过this.$refs字段 获取子组件的属性和方法
    2、子传父
        a、通过$emit派发事件
        b、通过this.$parent字段 获取父组件的属性和方法
    3、隔代通信
        $attrs可以获取父作用域传入的值(不包括props中的).
        $listeners相当于父作用域的事件监听器
    4、兄弟组件之间的通信
        a、先子传父, 再父传子
        b、借助中央事件总线
            var bus = new Vue();
            A组件发送事件: bus.$emit('test', '我是A组件');
            B组件监听: bus.$on('test', (params)=> {});
    5、嵌套很深的组件之间的通信
        父组件中通过provide来提供变量。子组件中通过inject来注入变量
    6、状态管理-vuex
        Mutation集合：声明一系列方法 操作修改state中的值 .....

## 十一、.vue文件代码模板
``` bash
    <template>
        <div id="app">
            <router-view />
        </div>
    </template>
    <script>
        import userImg from '../assets/img/user-unknown.png'
        export default {
            "name": 'App',
            // 页面绑定的数据
            data: function() {
                return {}
            },
            // 计算属性
            computed: {},
            // 监听属性
            watch: {},
            // 方法集合
            methods: {},
            // 局部自定义指令
            directives: {},
            // 封装局部组件
            components: {}
        }
    </script>
    <style lang='less'>
        @import './index.less';
        .photo {
            height: 20px;
            background: red;
        }
    </style>
```

## 十二、页面跳转传参
    1、location.href
    2、router-link或a标签
    3、this.$router.push({
        name: 'page1', // 用path接的话，获取不到参数
        params: {
            id: 1723
        }
    })
    4、this.$router.push({
        path: '/main/role',// 或者name: 'Role
        query: {
            id: 1723
        }
    })
    在页面里通过this.$route获取url的参数
    params和query的区别：
    a、query用path或name引入, url地址显示为：http://localhost:8080/page1?id=1723, 页面大刷新 数据不会丢失
    b、params用name引入, url地址显示为：http://localhost:8080/page1, 页面大刷新 数据会丢失

    route和router的区别：
    a、route：获取当前页面的路由信息，解析参数
    b、router：是全局路由VueRouter的一个实例，可以跳转页面
## 十三、vuex——管理共享数据
    核心模块：
    state：定义变量，保存数据
    getters：定义一系列方法, 方法的作用类似computed
    mutations：定义一系列更改state数据的回调方法, 不可以含异步回调
    action: 定义一系列更改state数据的回调方法, 可以含异步回调

    dispatch('方法名') 会触发action里面的方法
    commit('方法名') 会触发mutations里面的方法
``` bash
    //main.js
    const store = new Vuex.Store({
    state:{
        products: [
            {name: '鼠标', price: 20},
            {name: '键盘', price: 40},
            {name: '耳机', price: 60},
            {name: '显示屏', price: 80}
        ]
    },
    getters: {
        saleProducts: (state) => {
            let saleProducts = state.products.map( product => {
                return {
                name: product.name,
                price: product.price / 2
                }
            })
            return saleProducts;
        }
    },
    mutations:{
        minusPrice (state, payload ) {
            let newPrice = state.products.forEach( product => {
                product.price -= payload
            })
        }
    },
    actions:{ //添加actions
        minusPriceAsync( context, payload ) {
            setTimeout( () => {
                context.commit( 'minusPrice', payload ); //context提交
            }, 2000)
        }
    }
    })
    // page.vue
    export default {
        data () {
            return {
                products: this.$store.state.products
            }
        },
        methods: {
            minusPrice() {
                this.$store.commit('minusPrice', 2);
            },
            minusPriceAsync() {
                this.$store.dispatch('minusPriceAsync', 5); //分发actions中的minusPriceAsync这个异步函数
            }
        }
    }
```
## 十四、vue 双向绑定的原理
通过重写Object.defineProperty的set和get方法去更改属性值。

    let Person = {}
    Object.defineProperty(Person, 'name', {
        value: '杨洋'
    })
    Person.name = '小刚'
    // Person = {name: '杨洋'} name值不可改
    // delete Person.name  false 不能删除
    // Object.keys(Person) []
    Object.defineProperty(Person, 'name', {
        value: '杨洋',
        writable: true,
        configurable: true,
        enumerable: true
    }) 
    Person.name = '小红'
    // Person = {name: '小红'} name值可改
    // Object.keys(Person) ['name']
    // delete Person.name  true 可删除
    let age = 12
    Object.defineProperty(Person, 'age', {
        writable: true,
        get: function() { 
            console.log('获取属性值')
            return value 
        },
        set: function(value ) { 
            console.log('设置属性值', value)
            age = value 
        },
    }) 
    // Person.age  12
    Person.age = 80
    // Person.age  80

| 描述符 | 作用 | 默认值
| :------ | :------ |:---|
|writable|属性值是否可修改|false
|value|设置属性值|undefined
|configurable |属性值是否可删除、可配置|false
|enumerable |描述属性是否会出现在for in 或者 Object.keys()的遍历中|false
|get |获取属性值|undefined
|set |设置属性值|undefined