<template>
  <div class="goods">
		<div class="menu-wrapper" ref="menuWrapper">
			<ul>
				<li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex === index}" @click="_foodScroll(index,$event)">
					<span class="text after1px">
						<span class="icon" v-if="item.type>0" :class="classMap[item.type]"></span>
						{{item.name}}
					</span>
				</li>
			</ul>
		</div>
		<div class="foods-wrapper" ref="foodsWrapper">
			<ul>
				<li v-for="(i,index1) in goods" class="food-list-hook">
					<h2 class="title">{{i.name}}</h2>
					<ul>
						<li class="foods after1px" v-for="(item,index) in i.foods" @click="choiceFood(item,$event)">
								<img class="pull-left" :src="item.icon" alt="">
								<dl>
									<dt>{{item.name}}</dt>
									<dd class="desc" v-if="item.description">{{item.description}}</dd>
									<dd>
										<span>月售{{item.sellCount}}份</span>
										<span>好评率{{item.rating}}%</span>
									</dd>
									<dd class="price">￥{{item.price}} <del v-if="item.oldPrice!=''">￥{{item.oldPrice}}</del></dd>
								</dl>
								<div class="cartcontrol-wrapper">
									<cartcontrol :food="item"></cartcontrol>
								</div>
						</li>
					</ul>
				</li>
			</ul>
		</div>
		<!-- <div class="specific-wrapper" v-show="specificShow">
			<img :src="sItem.icon" alt="" @click="specificShow=!specificShow">
			<div class="spFood">			
				<h2 class="spTitle">{{sItem.name}}</h2>
				<p class="spDesc" v-if="sItem.description">{{sItem.description}}</p>
				<p class="spSell"><span>月售{{sItem.sellCount}}份</span> <span>好评率{{sItem.rating}}%</span></p>
				<p class="spPrice"><span></span>￥{{sItem.price}}<del v-if="sItem.oldPrice">￥{{sItem.oldPrice}}</del></p>
				<p class="join" @click="_join">加入购物车</p>
			</div>
		</div> -->
		<div class="food-wrapper">
			<foods :food="food" ref="food"></foods>
		</div>
		<shopcart :delivery="seller.deliveryPrice" :minPrice="seller.minPrice" :selectFoods="selectFoods"></shopcart>
  </div>
</template>

<script>
	import foods from "../foods/foods.vue";
	import bScroll from 'better-scroll';
	import shopcart from "../shopcart/shopcart.vue";
	import cartcontrol from "../cartcontrol/cartcontrol.vue";
	import Vue from 'vue';

	export default {
		props:{
			seller:{
				return:Object
			}
		},
		data(){
			return{
				food: Array,
				// sItem: String,
				// specificShow: false,
				goods: [],
				listHeight: [],
				scrollY: 0,
			}
		},
	 	created(){
	  	this.$http.get('/api/goods').then(response =>{
	  		this.goods = response.body.data;
	  		this.$nextTick(() =>{
	  			this._initScroll();
	  			this._calculateHeight()
	  		})
	  	});
  		this.classMap = ['decrease','special','discount','invoice','guarantee'];
	  },
	  methods:{
	  	// _info(index1,index){
	  	// 	if(!this.specificShow){
	  	// 		this.specificShow = true;
	  	// 		this.sItem = this.goods[index1].foods[index]
	  	// 	}
	  	// },
	  	// _join(){
	  	// // 	if(!event._constructed){
				// // 	return
				// // }
				// if(!this.sItem.count){
				// 	Vue.set(this.sItem,'count',1)
				// }else{
				// 	this.sItem.count++
				// }
	  	// },
	  	choiceFood(item,event){
	  		if(!event._constructed){
					return
				}
				this.food = item;
				this.$refs.food.show()
	  	},
	  	_initScroll(){
	  		this.menuScroll = new bScroll(this.$refs.menuWrapper,{
	  			click: true
	  		})
	  		this.foodScroll = new bScroll(this.$refs.foodsWrapper,{
	  			probeType: 3,
	  			click: true
	  		})
	  		this.foodScroll.on("scroll",(pos)=>{
	  			this.scrollY = Math.abs(Math.floor(pos.y));
	  		})
	  	},
	  	_calculateHeight(){
	  		let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
	  		let height = 0;
	  		this.listHeight.push(height);
	  		for(let i=0;i<foodList.length;i++){
	  			height += foodList[i].clientHeight;
	  			this.listHeight.push(height);
	  		}
	  	},
	  	_foodScroll(index,event){
	  		let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
	  		if(!event._constructed){
					return false
				}
				// if(event._constructed){
				this.foodScroll.scrollToElement(foodList[index], 300)
				// }
	  	}
	  },
	  computed:{
	  	currentIndex(){
	  		for(let i=0;i<this.listHeight.length;i++){
	  			let height1 = this.listHeight[i];
	  			let height2 = this.listHeight[i+1];
	  			if(!height2||this.scrollY>=height1&&this.scrollY<height2){
	  				return i;
	  			}
	  		}
  			return 0
	  	},
	  	selectFoods(){
	  		let foodList = [];
	  		this.goods.forEach((good) => {
	  			good.foods.forEach((food) => {
	  				if(food.count){
	  					foodList.push(food)
	  				}
	  			})
	  		})
  			return foodList
	  	}
	  },
	  components: {
	  	shopcart: shopcart,
	  	cartcontrol: cartcontrol,
	  	foods: foods
	  }
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	@import './../common/common.scss';

	.goods{
		position: absolute;
		top: 175px;
		left: 0;
		bottom: 0;
		padding-bottom: 48px;
		display: flex;
		width: 100%;
		overflow: hidden;
		.menu-wrapper{
			flex: 0 0 80px;
			background-color: #f3f5f7;
			.menu-item{
				display: table;
				padding-left: 12px;
				padding-right: 12px;
				width: 100%;
				height: 54px;
				box-sizing: border-box;
				.text{
					position: relative;
					display: table-cell;
					vertical-align: middle;
					line-height: 14px;
					font-size: 12px;
					font-weight: 200;
					color: rgb(7,17,27);
					@include after1px(rgba(7,17,27,.1));
					.icon{
						display: inline-block;
						margin-bottom: -1px;
						width: 12px;
						height: 12px;
						background-size: cover;
						@include pic2x(special_3);
						&.decrease{
						@include pic2x(decrease_3);
						}
						&.discount{
							@include pic2x(discount_3);
						}
						&.guarantee{
							@include pic2x(guarantee_3);
						}
						&.invoice{
							@include pic2x(invoice_3);
						}
						&.special{
							@include pic2x(special_3);
						}
					}
				}
			}
			.current{
				background-color: #fff;
			}
		}
		.foods-wrapper{
			flex: 1;
			ul{
				li{			
					.title{
						padding-left: 14px;
						height: 26px;
						line-height: 26px;
						font-size: 12px;
						color: rgb(147,153,159);
						background-color: #f3f5f7;
						border-left: 3px solid #d9dde1;
					}
					ul{
						.foods{			
							position: relative;
							margin: 0 18px;
							padding: 18px 0;	
							box-sizing: border-box;
							@include after1px(rgba(7,17,27,0.1));
							img{
								margin-right: 10px;
								width: 57px;
								height: 57px;
							}
							dl{
								margin-top: 2px;
								margin-left: 67px;
								dt{
									line-height: 14px;
									font-size: 14px;
									color: rgb(7,17,27);
								}
								dd{
									margin-top: 8px;
									line-height: 10px;
									font-size: 10px;
									color: rgb(147,153,159);
									span{
										margin-right: 12px;
									}
									&.price{
										margin-top: 3px;
										margin-right: 8px;
										line-height: 24px;
										font-size: 14px;
										font-weight: 700;
										color: rgb(240,20,20);
										del{
											line-height: 10px;
											font-size: 10px;
											color: rgb(147,153,159);
										}
									}
								}
							}
							.cartcontrol-wrapper{
								position: absolute;
								right: 0;
								bottom: 0;
							}
						}
					}
				}
			}
		}
		.specific-wrapper{
			position: fixed;
			top: 0;
			left: 0;
			width: 100%;
			height: 100%;
			background-color: white;
			img{
				width: 100%;
				height: 375px;
			}
			.spFood{
				position: relative;
				padding: 18px;
				.spTitle{
					line-height: 14px;
					font-size: 14px;
					font-weight: 700;
					color: rgb(7,17,27);
				}
				.spDesc{
					margin: 8px 0 18px 0;
					line-height: 10px;
				}
				.spSell{
					margin: 8px 0 18px 0;
					line-height: 10px;
					font-size: 10px;
					color: rgb(147,153,159);
					span{
						margin-right: 12px;
					}
				}
				.spPrice{
					line-height: 24px;
					font-size: 14px;
					font-weight: 700;
					color: rgb(240,20,20);
					del{
						font-size: 10px;
						color: rgb(147,153,159);
					}
				}
				.join{
					position: absolute;
					bottom: 18px;
					right: 18px;
					padding: 6px 12px;
					line-height: 12px;
					font-size: 10px;
					color: rgb(255,255,255);
					border-radius: 12px;
					background-color: rgb(0,160,220);
				}
			}
		}
	}
</style>