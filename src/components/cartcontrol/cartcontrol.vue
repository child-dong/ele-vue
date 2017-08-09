<template>
  <div class="cartcontrol">

		<span class="icon-remove_circle_outline" v-show="food.count>0" @click.stop.prevent="reduce($event)"></span>
		<span @click="cons" class="num" v-show="food.count>0">{{food.count}}</span>
		<span class="icon-add_circle" @click.stop.prevent="add($event)"></span>
  </div>
</template>

<script>

	import Vue from 'vue';
	export default {
		props:{
			food:{
				return: Object
			}
		},
		methods: {
			cons(){		
				console.log(this.food.count)
			},
			add(event){
				if(!event._constructed){
					return
				}
				if(!this.food.count){
					Vue.set(this.food,'count',1)
				}else{
					this.food.count++
				}
			},
			reduce(event){
				if(!event._constructed){
					return false
				}
				this.food.count--
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