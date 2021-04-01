#### 组件名称要求

(编辑器按字母顺序排序，名称最好前面有功能前缀)

反例：myButton.vue  , VueTable.vue  , Icon.vue等等。。。

正例：1.BaseButton.vue, BaseTable , BaseIcon.vue   2.AppButton.vue ,AppTable.vue    3.VButton ,VTable

**两种命名方式**

1.驼峰

2.以‘-’分隔 如：kebab-case  短横线分隔命名

但是标签最好不使用驼峰，因为浏览器不识别，所以组件标签名最好使用第二种。

#### 自定义事件之事件名称

（不使用驼峰命名）



#### 组件注册

**全局注册**

```js
//定义一个名为button-counter的全局组件
Vue.component('button-counter',{
	data:function(){
		return{
			count:0
		}
	},
	template: '<button v-on:click="count++">you clicked me {{conut}} times <button>'
})
```

**局部注册**

```js
//定义一个名为button-counter 的新组件，并局部注册。
//1.通过一个普通的 JavaScript 对象来定义组件
var ComponentA = {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
}

//2.注册组件，在使用ComponentA的components 选项中注册想要使用的组件
var ComponentB = {
components: {
	' button-counter': ComponentA    //组件名称：选项对象 ，当两个名称相同时，可以省略一个
},
// ...
}
```

#### 插槽

类似于es6的函数默认参数

例子  （v-slot为vue2.6以上支持，摒弃了slot）

```html
<div id="app">
        <test v-on:click='clickfn' v-on:dblclick='dbclickfn'>
        	<!--写法1 子组件传参-->
            <template v-slot:slot1="slot1props">
                   444{{slot1props.msgdata}}
            </template>
            <!--写法2 子组件传参，解构赋值-->
               <!--slot1两次写完会覆盖-->
			 <template v-slot:slot1={msgdata}>
                   555{{msgdata}}
            </template>
            <template slot="slot2">
                666
            </template>
    		<!-- v-slot可以定义默认插槽内容，slot不可以 -->
            <template v-slot="default">
                777
            </template>
        </test>
    </div>
    <script>
        let vm2 = Vue.component('test', {
    		data() {
                return {
                    msg: 'hello world',
                    name: "zhangsan"
                }
            },
            template: `<div>
            			<h5>test测试</h5>
                        <slot name="slot1" v-bind:msgdata="msg">11111</slot> 
						<p>分割111111</p>
                        <slot name="slot2">22222</slot>
						<p>分割222222</p>
						<slot name="slot3">33333</slot>
                        </div>`
        });

        let vm = new Vue({
            el: "#app"   
        })
    </script>
```

