<template>
  <div class="supports" :class="[className]">
  	<ul>
  		<li v-for="(item,index) in supports">
				<span class="icon" :class="classMap[supports[index].type]"></span>
  			{{item.description}}
  		</li>
  	</ul>
  </div>
</template>

<script>
	export default {
		props:{
			supports:{
				return: Array
			},
			className:{
				return:String
			}
		},
		created(){
			this.classMap = ['decrease','discount','special','invoice','guarantee'];
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	@import './../common/common.scss';
	
	.supports{
		ul{
			li{
				color: #fff;
				line-height: 12px;
				margin-bottom: 12px;
				.icon{
					float: left;
					margin-right: 4px;
					width: 12px;
					height: 12px;
					background-size: cover;
					&.decrease{
						@include pic2x(decrease_1);
					}
					&.discount{
						@include pic2x(discount_1);
					}
					&.guarantee{
						@include pic2x(guarantee_1);
					}
					&.invoice{
						@include pic2x(invoice_1);
					}
					&.special{
						@include pic2x(special_1);
					}
				}
			}
		}
		&.big{
			li{
				margin-bottom: 12px;
				line-height: 12px;
				font-size: 12px;
				font-weight: 200;
				color: #fff;
				.icon{
					float: left;
					margin-right: 4px;
					width: 12px;
					height: 12px;
					background-size: cover;
					&.decrease{
						@include pic2x(decrease_2);
					}
					&.discount{
						@include pic2x(discount_2);
					}
					&.guarantee{
						@include pic2x(guarantee_2);
					}
					&.invoice{
						@include pic2x(invoice_2);
					}
					&.special{
						@include pic2x(special_2);
					}
				}
			}
		}
		&.supports3{
			li{
				position: relative;
				margin-bottom: 0;
				padding: 16px 12px;
				line-height: 16px;
				font-size: 12px;
				font-weight: 200;
				color: rgb(7,17,27);
				@include after1px(rgba(7,17,27,0.1));
				&:last-child{
					&:after{
						display: none;
					}
				}
				.icon{
					float: left;
					margin-right: 6px;
					width: 16px;
					height: 16px;
					background-size: cover;
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
	}
</style>