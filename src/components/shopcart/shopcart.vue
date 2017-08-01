<template>
  <div class="shopcart">
  	<div class="content">
  		<div class="content-left">
  			<div class="logo pull-left">
  				<span class="icon icon-shopping_cart"></span>
  			</div>
  			<div class="price pull-left">
  				<span class="total">￥0</span>
  				<span class="delivery">另需配送费￥4元</span>
  			</div>
  		</div>
  		<div class="content-right">
  			20元起送
  		</div>
  	</div>
  </div>
</template>

<script>
	export default {

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
						display: block;
						width: 44px;
						height: 44px;
						line-height: 44px;
						font-size: 25px;
						text-align: center;
						background-color: rgba(255,255,255,0.1);
						border-radius: 50%;
						&.shopcart-bg{
							background-color: rgb(0,160,220);
							color: #fff;
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
						&.total-color{
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