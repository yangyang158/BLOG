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
## 二、条件渲染
    1、控制元素显示与隐藏: 使用v-if、v-else、v-else-if
    eg：
    <div>
        <div v-if="num > 5">{{num}}大于5</div>
        <div v-else-if="num > 2">{{num}}大于2小于5</div>
        <div v-else>{{num}}小于2</div>
    </div>
    2、控制元素显示与隐藏: v-show
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
    使用: v-modal
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
    eg：监听数据变化, 做出响应
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
