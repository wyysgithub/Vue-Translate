# vue-cli 制作在线翻译小demo

=====

翻译用到的API:https://tech.yandex.com/translate/

=====

### 主要入口文件main.js

```
import Vue from 'vue'
import App from './App'
import router from './router'
import VueResource from 'vue-resource'

/*要使用http方法，则需要安装Vue-Resource
再使用中间件把resource引入进来
*/
Vue.use(VueResource)
Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})

```
* 使用Vue.use(VueResource)中间件引入vue-resource

### 组件

**三个组件，一个父组件，两个子组件**

* App.vue
* translateForm.vue
* translateOutput.vue


#### App.vue

* 父组件

```
<template>
  <div id="app" class="container">
  	<h1>在线翻译</h1>
  	<h4 class="text-muted">简单  /  易用  /  便捷</h4>
    <translateForm @formS="translateText"></translateForm>
    <translateOutput v-text="result"></translateOutput>
  </div>
</template>
```

* 用'<translateForm></translateForm>'和'<translateOutput></translateOutput>'将两个实例渲染到界面中。

* @formS是子组件translateForm内注册并传过来的，由此触发translateText函数。

* v-text="result"即把得到的result值传给子组件，子组件空调通过"props:['result']",{{result}}获取并使用result.

```
<script>
import translateForm from './components/translateForm.vue'
import translateOutput from './components/translateOutput.vue'
export default {
  name: 'App',
  data(){
  		return{
  			result:""
  		}
  },
  components:{
  	translateForm,
  	translateOutput
  },
  methods:{
  	translateText(text,language){
  		this.$http.get('https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20180420T062133Z.7805ec392aea6763.9c937c57ab4108801651d43707e351b274d43c84&lang='+language+'&text='+text).then((response)=>{
  			console.log(response.body.text[0])
  			this.result=response.body.text[0]
  		});
  	}
  }
}
</script>
```

* 通过import将两个组件引入进来，后缀.vue可省略。

* 使用components注册两个组件，{translateForm:translateForm}直接简写成{translateForm}

* 使用$http必须先安装 vue-resource

* .then((response)=>{}) 为ES6箭头函数，获取$http返回的数据

* 使用response.body.text[0]获取并使用最终翻译出来的数据，并赋给result



#### translateForm.vue

```
<template>
	<div id="translateForm" class="row form-group">
		<div class="col-md-6 col-md-offset-3">
		<form class="well form-inline" @submit="fromSubmit">
			<input class="form-control" type="text" v-model="inputValue" >
			<select class="form-control" v-model="language">
				<option value="en">English</option>
				<option value="ru">Russian</option>
				<option value="ko">Korean</option>
				<option value="ja">Japenese</option>
			</select>
			<input class="btn btn-primary" type="submit" value="翻译">
		</form>
		</div>
	</div>
</template>
```
* 主要文本输入、提交区域


```
<script>
export default {
	
  name: 'translateForm',
  data(){
		return{
			inputValue:'',
			language:'en'
		}
	},
  methods:{
  	/*将even事件传入，并取消默认事件，避免页面刷新*/
  	fromSubmit(e){
  		/*事件注册方法，默认参数为要注册的事件和参数*/
  		this.$emit("formS",this.inputValue,this.language)
  		/*this.inputValue=''*/
  		e.preventDefault();
  	}
  }
}
</script>
```
* 用"this.$emit()"方法，注册一个formS事件，并带两个参数，返回给父组件App.vue

* 用'e'将even事件传入，并使用'e.preventDefault()'取消默认事件，避免页面刷新


#### translateForm.vue

```
<template>
	<div id="translateOutput">{{result}}</div>
</template>

<script>
export default {
  name: 'translateOutput',
  props:['result']
}
</script>
```

* 使用"props:['result']"接收父组件传入的数据 result，并在模版中用插值表达式引用。


## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests
npm test
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
