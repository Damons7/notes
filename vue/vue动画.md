#### 单元素/组件 过渡

**6个属性**  （v为用户可自定义值）

- v-enter-active  :进入过渡生效时的状态
- v-enter			：进入过渡的开始状态
- v-enter-to		：进入过渡的结束状态
- v-leave-active	: 离开过渡时生效
- v-leave			：离开过渡的开始状态
- v-leave-to		：离开过渡的结束状态
  简单来说，v-enter-active是整个的过程状态，v-enter是开始的状态，v-enter-to是结束的状态

![image-20201212155859875](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201212155859875.png)

```html
    <style>
        .fade-enter-active{
            transition: all .5s;
        }
        .fade-ente,.fade-leave-to{
          opacity: 0;
          color: red;
            transform:translateX(50px);
        }
        ,.fade-leave-active{
             transition: all .5s cubic-bezier(.81,.1,.11,.88);
        }
    </style>
</head>

<body>
    <div id="app">
        <button type="button" @click="show=!show">111</button>
        <transition name="fade">
            <p v-if="show">{{msg}}</p>
        </transition>
    </div>
    <script>

        let vm = new Vue({
            el: "#app",
            data: {
                show: true,
                msg:"hello"
            },
        })
    </script>
</body>
```

换一种style写法

```css
    <style>
        .fade-enter-active{
         	animation:bounce .5s;
        }
        .fade-leave-active{
         	animation:bounce .5s reverse;
        }
        
        @keyframes bounce{
        	0%{
        		transform:scale(0);
        	}
        	50%{
        		transform:scale(1.5);
        	}
        	100%{
        		transform:scale(1);
        	}
        }
    </style>


```

假如是引入其他css库，vue提供方法可以定义css

```html
<transition name="fade"
            enter-active-class="xxxx" 
            leave-active-class="xxxx">
            <p v-if="show">{{msg}}</p>
</transition>
```



#### 当为多元素是  (需要加上key识别才有动画)

```html
 <transition name="fade">
            <p v-if="show" key="one">{{msg}}</p>
            <p else key="two">{{msg}}</p>
        </transition>
```

简写版本(不建议)

```html
 <transition name="fade">
            <p v-bind:key="show">{{show?msg:"hehe"}}</p>
 </transition>
```

#### 列表渲染

**<transiton-group>**       (此标签会渲染到页面上，默认为span)

**<transiton-group tag="div">**       (可以通过tag改变为其他标签)



