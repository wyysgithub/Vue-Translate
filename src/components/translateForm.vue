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

<style>

</style>
