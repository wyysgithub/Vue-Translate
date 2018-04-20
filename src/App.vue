<template>
  <div id="app" class="container">
  	<h1>在线翻译</h1>
  	<h4 class="text-muted">简单  /  易用  /  便捷</h4>
    <translateForm @formS="translateText"></translateForm>
    <translateOutput v-text="result"></translateOutput>
  </div>
</template>

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

<style>
#app{
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
