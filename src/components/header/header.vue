<template>
  <div class="header">
  	<div class="content"> 	
  		<div class="avatar clearfix">	
		   	<img class="pull-left" :src="seller.avatar" alt="">
		   	<div class="store pull-left">
		   		<div class="clearfix">   			
				   	<span class="pull-left"></span>
				   	<h2 class="pull-left">{{seller.name}}</h2>
		   		</div>
		   		<p>{{seller.description}}/{{seller.deliveryTime}}分钟送达</p>
  				<div class="supports-wrapper" >
  					<supports v-if="seller.supports" :supports="seller.supports"></supports>
  				</div>	
		   	</div>
  		</div>
  	</div>
  	<div class="counts" v-on:click="detailShow" v-if="seller.supports">
			<p>
				{{seller.supports.length}}个
				<span class="icon-keyboard_arrow_right"></span>
			</p>
  	</div>
  	<div class="bulletin" v-on:click="detailShow">
  		<span class="icon"></span>
			<span class="word">{{seller.bulletin}}</span>
  		<i class="icon-keyboard_arrow_right pull-right"></i>
  	</div>
  	<div class="bg"></div>
  	<transition name="fade">		
			<div class="detail-wrapper" v-show="seen" v-on:click="detailClose">
				<div class="detail">
					<h2>{{seller.name}}</h2>
					<div class="star">
						<star :star="seller.score"></star>
					</div>
					<div class="title">
						<div class="line"></div>
						<div class="text">优惠信息</div>
						<div class="line"></div>
					</div>
					<div class="supports-wrapper" >
  					<supports v-if="seller.supports" :supports="seller.supports" :className="big"></supports>
  				</div>
  				<div class="title">
						<div class="line"></div>
						<div class="text">商家公告</div>
						<div class="line"></div>
					</div>
					<div class="bulle">
			  		{{seller.bulletin}}
			  	</div>	
					

					<div class="close" v-on:click="detailClose">			
						<i class="icon-close"></i>
					</div>
				</div>
			</div>
  	</transition>
  </div>
</template>

<script>

	import supports from '../supports/supports.vue'
	import star from '../star/star.vue'
	export default {
		props:{
			seller:{
				return:Object
			}
		},
		data(){
			return{
				seen: true,
				big: "big"
			}
		},
		methods:{
			detailShow(){
				this.seen = true;
			},
			detailClose(){
				this.seen = false;
			}
		},
		components: {
	  	supports: supports,
	  	star: star
	  }
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	@import './../common/common.scss';
	.header{
		position: relative;
		background-color: rgba(7,17,27,0.5);
		.content{
			padding: 24px 12px 18px 24px;
			.avatar{
				overflow: hidden;			
				img{
					height: 64px;
					width: 64px;
					border-radius: 2px;
				}
				.store{		
					margin-top: 2px;
					margin-left: 16px;
					color: #fff;	
					span{
						float: left;
						margin-right: 6px;
						height: 18px;
						width: 30px;
						@include pic2x(brand)
						background-size: cover;
					}
					h2{
						line-height: 18px;
						font-size: 16px;
						font-weight: bold;
					}
					p{
						clear: both;
						padding-top: 8px;
						font-size: 12px;
						font-weight: 200;
					}
					.supports-wrapper{
						overflow: hidden;
						margin-top: 10px;
						height: 15px;
						line-height: 12px;
						font-size: 10px;
						font-weight: 200;
						position: relative;
					}
				}
			}
		}
		.counts{
			position: absolute;
			right: 12px;
			bottom: 38px;
			padding: 7px 8px;
			font-size: 10px;
			line-height: 12px;
			font-weight: 200px;
			border-radius: 24px;
			color: #fff;
			background-color: rgba(255,255,255,.1);
		}
		.bulletin{
			position: relative;
			padding: 0 26px 0 12px;
			height: 28px;
			line-height: 28px;
			font-size: 10px;
			font-weight: 200;
			color: #fff;
			background-color: rgba(7,17,27,0.2);
			@include beyond-hide(1);
			.icon{
				display: inline-block;
				vertical-align: top;
				margin-top: 8px;
				margin-right: 4px;
				width: 24px;
				height: 12px;
				@include pic2x(bulletin);
				background-size: cover;
			}
			i{
				position: absolute;
				right: 12px;
				top: 9px;
			}
		}
		.bg{
			position: absolute;
			top: 0;
			left: 0;
			width: 100%;
			height: 100%;
			background-image: url(bg.jpg); 
			background-size: cover;
			filter: blur(10px);
			z-index: -1;
		}
		.detail-wrapper{
			position: fixed;
			top: 0;
			left: 0;
			height: 100%;
			width: 100%;
			overflow: auto;
			background-color:rgba(7,17,27,0.8); 
			z-index: 1000;
			.detail{
				box-sizing: border-box;
				position: relative;
				padding: 64px 32px 96px; 
				min-height: 100%;
				color: #fff;
				backdrop-filter: blur(10px);
				h2{
					line-height: 16px;
					font-size: 16px;
					font-weight: 700;
					text-align: center;
				}
				.star{
					margin: 16px auto 28px;
					text-align: center;
				}
				.title{
					display: flex;
					.line{
						position: relative;
						top: -7px;
						flex: 1;
						border-bottom: 1px solid rgba(255,255,255,0.2)
					}
					.text{
						padding: 0 12px;
						line-height: 14px;
						font-size: 14px;
						font-weight: 700;
						color: rgb(255,255,255);
					}
				}
				.supports-wrapper{
					margin: 24px 12px 28px;
				}
				.bulle{
					margin: 24px 12px 0;
					line-height: 24px;
					font-size: 12px;
					font-weight: 200;
					color: #fff;
				}

				

				.close{
					position: absolute;
					bottom: 32px;
					left: 0;
					width: 100%;
					text-align: center;
					i{
						font-size: 32px;
						color: #fff;
					}	
				}
			}
		}
		.fade-enter-active,.fade-leave-active{
			transition: opacity 500ms;
		}
		.fade-leave-to,.fade-enter{
			opacity: 0;
		}
	}
</style>