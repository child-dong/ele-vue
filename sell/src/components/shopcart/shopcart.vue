<template>
  <div class="shopcart">
  	<div class="content">
  		<div class="content-left">
  			<div class="logo pull-left" @click="toggleList">
  				<span class="icon icon-shopping_cart" :class="{
  				  				'shopcartBg':totalCount>0
  				  				}">
  					<span class="num" v-show="totalCount>0">{{totalCount}}
  					</span>
  				</span>
  			</div>
  			<div class="price pull-left">
  				<span class="total" :class="{'totalColor':totalCount>0
  				}">￥{{totalPrice}}</span>
  				<span class="delivery">另需配送费￥{{delivery}}元</span>
  			</div>
  		</div>
  		<div class="content-right" :class="{enough:totalPrice>=minPrice}">
  			{{pay}}
  		</div>
  	</div>
  	<transition name="fold">
	  	<div class="list-mask" v-show="listShow"> 		
	  	</div>
  	</transition>
  	<transition name="fade">
	  	<div class="shopcart-cover" v-show="listShow" @click="toggleList">
	  		<div class="selected-list">
	  			<div class="cleanAll clearfix">
	  				<span class="cart pull-left">购物车</span>
	  				<span @click="empty" class="clean pull-right">清空</span>
	  			</div>
	  			<div ref="listWraper" class="lsWrapper">
						<ul>
							<li class="food" v-for="item in selectFoods">
								<div class="clearfix">				
									<span class="name pull-left">{{item.name}}</span>
									<span class="count pull-right">
										<cartcontrol :food="item"></cartcontrol>
									</span>
									<span class="price pull-right">￥{{item.price*item.count}}</span>
								</div>
							</li>
						</ul>
	  			</div>
	  		</div>
	  	</div>
		</transition>
  </div>
</template>

<script>
	import Bscroll from 'better-scroll';
	import cartcontrol from "../cartcontrol/cartcontrol.vue";
	export default {
		data(){
			return{
				fold: true
			}
		},
		// watch:{
		// 	'goods'(){
		// 		this._initScroll()
		// 	}
		// },
		props:{
			goods:{
				return: Object
			},
			delivery:{
				return: Number,
				default: 0
			},
			minPrice:{
				return: Number,
				default: 20
			},
			selectFoods:{
				type: Array,
				default(){
					return[
						{
							price: 10,
							count: 0
						},			
					]
				}
			}
		},
		components:{
			cartcontrol
		},
		methods:{
			toggleList(){
				if(!this.totalCount){
					return;
				}
				this.fold = !this.fold
			},
			empty(){
				this.selectFoods.forEach((food)=>{
					food.count = 0
				})
			}
		},

		// created(){
		// 	this._initScroll();
		// },
		computed:{
			totalPrice(){
				let price = 0;
				this.selectFoods.forEach((item) =>{
					price += item.price*item.count
				})
				return price
			},
			totalCount(){
				let count = 0;
				this.selectFoods.forEach((item) =>{
					count += item.count
				})
				return count
			},
			pay(){
				if(this.totalPrice === 0){
					return `￥${this.minPrice}元起送`
				}else if(this.totalPrice < this.minPrice){
						let diff = this.minPrice - this.totalPrice
						return `还差￥${diff}元起送`
				}else{
					return '去结算'
				}
			},
			listShow(){
				if( !this.totalCount ){
					this.fold = true;
					return false;
				}
				let show = !this.fold;
				if(show){
					this.$nextTick( () => {
						if(!this.listScroll){
							this.listScroll = new Bscroll(this.$refs.listWraper,{
								click : true
							})
						}else{
							this.listScroll.refresh()
						}
					})
				}
				return show;
			}
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	@import './../common/common.scss';

	.shopcart{
		position: absolute;
		left: 0;
		bottom: 0;
		width: 100%;
		height: 47px;
		color: rgba(255,255,255,0.4);
		background-color: #141d27;
		z-index: 40;
		.content{
			display: flex;
			position: relative;
			z-index: 35;
			.content-left{
				color: rgba(255,255,255,0.4);
				background-color: #141d27;
				flex: 1;
				.logo{
					position: relative;
					box-sizing: border-box;
					margin: -10px 12px 0; 
					padding: 6px;
					width: 56px;
					height: 56px;		
					border-radius: 50%;
					background-color: #141d27;
					z-index: 35;
					.icon{
						position: relative;
						display: block;
						width: 44px;
						height: 44px;
						line-height: 44px;
						font-size: 25px;
						text-align: center;
						background-color: rgba(255,255,255,0.1);
						border-radius: 50%;
						z-index: 35;
						&.shopcartBg{
							background-color: rgb(0,160,220);
							color: #fff;
						}
						.num{
							position: absolute;
							top: -6px;
							right: -6px;
							width: 24px;
							height: 16px;
							line-height: 16px;
							text-align: center;
							font-size: 9px;
							color: #fff;
							border-radius: 12px;
							background-color: rgb(240,20,20);
						}
					}
				}
				.price{
					box-sizing: border-box;
					margin: 12px 0;
					line-height: 24px;
					color: rgba(255,255,255,0.4);
					.total{
						position: relative;
						float: left;
						padding-right: 12px;
						font-size: 16px;
						font-weight: 700;
						&:after{
							display: block;
							content: "";
							position: absolute;
							top: 0;
							right: 0;
							height: 100%;
							width: 1px;
							background-color: rgba(255,255,255,0.1);
						}
						&.totalColor{
							color: #fff;
						}
					}
					.delivery{
						float: left;
						padding-left: 12px;
						font-size: 10px;
						font-weight: 200;
						&.delivery-color{
							color: #fff;
						}
					}
				}
			}
			.content-right{
				margin-left: -1px;
				box-sizing: border-box;
				padding: 0 8px;
				flex: 0 0 28%;
				line-height: 47px;
				font-size: 12px;
				font-weight: 700;
				text-align: center;
				background-color: #2b333b;
				&.enough{
					color: #fff;
					background-color: #00b43c;
				}
			}
		}
		.fold-enter-active, .fold-leave-active {
	    transition: all .5s;
	  }
	  .fold-enter, .fold-leave-active {
	    opacity:0;
	  }
    .list-mask{
	    position: fixed;
	    top: 0;
	    left: 0;
	    width: 100%;
	    height: 100%;
	    z-index: 20;
	    background:rgba(7,17,27,.6);
	    // filter: blur(10px);
	  }
	  .fade-enter-active, .fade-leave-active {
      transition: all .5s;
    }
    .fade-enter, .fade-leave-active {
      transform: translate3D(0,100%,0);
      opacity:0;
    }
		.shopcart-cover{
			position: fixed;
			bottom: 47px;
			left: 0;
			height: 100%;
			width: 100%;
	    // background:rgba(7,17,27,0.6);
			z-index: 25;
			.selected-list{
				position: absolute;
				bottom: 0;
				left: 0;
				width: 100%;
				background-color: #fff;
				z-index: 25;
				.cleanAll{
					position: relative;
					padding: 0 18px;
					height: 40px;
					background-color: #f3f5f7;
					@include after1px(rgba(7,17,27,0.1))
					.cart{
						line-height: 40px;
						font-size: 14px;
						font-weight: 200;
						color: rgb(7,17,27);
					}
					.clean{
						line-height: 40px;
						font-size: 12px;
						color: rgb(0,160,220);
					}
				}
			}
			.lsWrapper{
				position: relative;
				padding: 0 18px;
				max-height: 230px;
				overflow: hidden;
				z-index: 25;	
				.food{
					position: relative;
					padding: 12px 0;
					line-height: 24px;
					color: rgb(7,17,27);
					@include after1px(rgba(7,17,27,0.1))
					.name{
						line-height: 24px;
						font-size: 14px;
						color: rgb(7,17,27);
					}
					.price{
						line-height: 24px;
						font-size: 14px;
						font-weight: 700;
						color: rgb(240,20,20);
					}
				}
			}
		}
	}
</style>