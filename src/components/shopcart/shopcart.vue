<template>
  <div class="shopcart">
  	<div class="content">
  		<div class="content-left">
  			<div class="logo pull-left">
  				<span class="icon icon-shopping_cart" :class='{
  				shopcartBg:totalCount>0
  				}'>
  					<span class="num" v-if="totalCount">{{totalCount}}
  					</span>
  				</span>
  			</div>

  			<div class="price pull-left">
  				<span class="total" :class='{totalColor:totalCount>0
  				}'>￥{{totalPrice}}</span>
  				<span class="delivery">另需配送费￥{{delivery}}元</span>
  			</div>
  		</div>
  		<div class="content-right" :class="{enough:totalPrice>=minPrice}">
  			{{pay}}
  		</div>
  	</div>
  </div>
</template>

<script>
	export default {
		props:{
			delivery:{
				return: Number,
				default: 0
			},
			minPrice:{
				return: Number,
				default: 0
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
		computed:{
			totalPrice(){
				let price = 0;
				this.selectFoods.forEach((foods) =>{
					price += foods.price*foods.count
				})
				return price
			},
			totalCount(){
				let count = 0;
				this.selectFoods.forEach((foods) =>{
					count += foods.count
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
		.content{
			display: flex;
			.content-left{
				flex: 1;
				.logo{
					box-sizing: border-box;
					margin: -10px 12px 0; 
					padding: 6px;
					width: 56px;
					height: 56px;		
					border-radius: 50%;
					background-color: #141d27;
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
				box-sizing: border-box;
				padding: 0 8px;
				flex: 0 0 28%;
				line-height: 47px;
				font-size: 12px;
				font-weight: 700;
				text-align: center;
				background-color: rgba(255,255,255,0.1);
				&.enough{
					color: #fff;
					background-color: #00b43c;
				}
			}
		}
	}
</style>