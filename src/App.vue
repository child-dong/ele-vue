<template>
  <div id="app">
	  <iheader :seller="seller"></iheader>
	  <div class="tab after1px"> 	
		  <div class="tab-item">
	  	 	<router-link to="/goods">商品</router-link>
		  </div>
		  <div class="tab-item">
		  	<router-link to="/ratings">评价</router-link>
		  </div>
		  <div class="tab-item">
		  	<router-link to="/seller">商家</router-link>
		  </div>
	  </div>
	  <router-view></router-view>
		<shopcart></shopcart>
  </div>

</template>

<script>

	import iheader from "./components/header/header.vue";
	import shopcart from "./components/shopcart/shopcart.vue";
	export default {
	  name: 'app',
	  data() {
	  	return{
	  		seller: Object
	  	}
	  },
	  created(){
	  	this.$http.get('/api/seller').then(response =>{
	  		this.seller = response.body.seller;
	  	})
	  },
	  components: {
	  	iheader: iheader,
	  	shopcart: shopcart
	  }
	}
</script>

<style lang="scss" scoped>
	@import './components/common/common.scss';
	#app{
	  .tab{
	  	position: relative;
	  	display: flex;
	  	height: 40px;
	  	line-height: 40px;
	  	background-color: #fff;
	  	@include after1px(rgba(7,17,27,0.1));
	  	.tab-item{
				flex: 1;
				text-align: center;
				a{
					display: block;
					height: 100%;
					font-size: 14px;
					color: rgb(77,85,93);
					&.active{
						color: rgb(240,20,20);
					}
				}
	  	}
	  }
		
	}
</style>
