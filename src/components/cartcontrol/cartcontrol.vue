<template>
  <div class="cartcontrol">

		<span class="icon-remove_circle_outline" v-if="foods.count" @click="reduce($event)"></span>
		<span class="num" v-if="foods.count">{{foods.count}}</span>
		<span class="icon-add_circle" @click="add($event)"></span>
  </div>
</template>

<script>

	import Vue from 'vue';
	export default {
		props:{
			foods:{
				return: Object
			}
		},
		methods: {
			add(event){
				if(!event._constructed){
					return
				}
				if(!this.foods.count){
					Vue.set(this.foods,'count',1)
				}else{
					this.foods.count++
				}
			},
			reduce(event){
				if(!event._constructed){
					return false
				}
				this.foods.count--
			}
		},
		// computed:{

		// }
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	@import './../common/common.scss';
	
	.cartcontrol{
		line-height: 24px;
		font-size: 0;
		color: rgb(0,160,220);
		span{
			display: inline-block;
			margin-left: 12px;
		}
		.num{
			vertical-align: top;
			font-size: 10px;
			color: rgb(147,153,159);
		}
		.icon-remove_circle_outline{
			font-size: 24px;
		}
		.icon-add_circle{
			font-size: 24px;
		}
	}
</style>