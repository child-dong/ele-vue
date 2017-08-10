<template>
  <div class="seller" ref="ratingsScroll">
  	<div class="contentAll">	
	  	<div class="seller-wrapper after1px">		
	   		<h2 class="title">{{seller.name}}</h2>
	   		<p class="star">
	   			<span>
	   				<star :star="seller.score" :size="size"></star>
	   			</span>
	   			<span class="ratingCount">({{seller.ratingCount}})</span>
	   			<span class="sellCount">月售{{seller.sellCount}}单</span>
	   		</p>
	   		<div class="collection">
	   			<p class="icon icon-favorite" :class='collect' @click="set()"></p>
					<p class="word">{{coll}}</p>
	   		</div>
	  	</div>
	  	<div class="price-wrapper">
	  		<ul>
	  			<li class="border1px">
						<p>起送价</p>
						<p class="num">
							<span>{{seller.minPrice}}</span>
							<small>元</small>
						</p>
	  			</li>
	  			<li class="border1px">
						<p>商家配送</p>
						<p class="num">
							<span>{{seller.deliveryPrice}}</span>
							<small>元</small>
						</p>
	  			</li>
	  			<li class="border1px">
						<p>平均配送时间</p>
						<p class="num">
							<span>{{seller.deliveryTime}}</span>
							<small>分钟</small>
						</p>
	  			</li>
	  		</ul>
	  	</div>
	  	<div class="gap after1px"></div>
	  	<div class="bulletin-wrapper">
	  		<h2 class="title">公告与活动</h2>
	  		<p class="content">{{seller.bulletin}}</p>
	  	</div>
		  <div class="supports-wrapper">
		  	<supports v-if="seller.supports" :className="className" :supports="seller.supports"></supports>
		  </div>
		  <div class="gap"></div>
		  <div class="pics-wrapper">
		  	<p class="title" v-if="seller.pics">商家实景</p>
		  	<div class="pics" ref="picsScroll">		
			  	<ul ref="picWrapper">
			  		<li v-for="item in seller.pics">
			  			<img :src="item" alt="">
			  		</li>
			  	</ul>
		  	</div>
		  </div>
		  <div class="gap"></div>
		  <div class="infor-wrapper" v-if="seller.infos">
		  	<h2 class="title">商家信息</h2>
		  	<ul>
		  		<li class="after1px" v-for="item in seller.infos">{{item}}</li>
		  	</ul>
		  	<!-- <shopcart :delivery="seller.deliveryPrice" :minPrice="seller.minPrice"></shopcart> -->
		  </div>
  	</div>
  </div>
</template>

<script>
	import bScroll from 'better-scroll';
	import star from "../star/star.vue"
	import supports from "../supports/supports.vue"
	import shopcart from "../shopcart/shopcart.vue"
	export default {
		props:{
			seller:{
				return:Object
			}
		},
		data(){
			return{
				collect: "",
				coll: "未收藏",
				size: "star36",
				className: "supports3",
				// picsWidth: 0
			}
		},
		watch:{
			"seller"(){
				this._initScroll();
			}
		},
		mounted(){
			this._initScroll();
		},
		methods:{
			_initScroll(){
				this.$nextTick(() => {
					if(this.seller.pics){						
			  		this.picsScroll = new bScroll(this.$refs.picsScroll,{
			  			scrollX: 'true',
			  			click: true
			  		})
					}
		  		this.ratingsScroll = new bScroll(this.$refs.ratingsScroll,{
		  			scrollY: 'true',
		  			click: true
		  		})
				})
	  	},
	  	set(){
	  		if(this.collect==""){
	  			this.collect = "collect"
	  			this.coll = "已收藏"
	  		}else{
	  			this.collect = ""
	  			this.coll = "未收藏"
	  		}
	  	}
		},
		components:{
			star,
			supports,
			shopcart
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	@import '../common/common.scss';

	.seller{
		position: fixed;
		top: 174px;
		left: 0;
		height: 100%;
		overflow: hidden;
		.contentAll{
			padding-bottom: 174px;
		}
		.seller-wrapper{
			position: relative;
			padding: 18px 0;
			margin: 0 18px;
			@include after1px(rgba(7,17,27,0.1));
			.title{
				margin-bottom: 8px;
				line-height: 14px;
				font-size: 14px;
				color: rgb(7,17,27);
			}
			.star{
				font-size: 0;
				span{
					display: inline-block;
					line-height: 18px;
					font-size: 10px;
					color: rgb(77,85,93);
					&.ratingCount{
						vertical-align: top;
						margin: 0 12px 0 8px;
					}
				}
			}
			.collection{
				position: absolute;
				right: 0;
				top: 20px;
				text-align: center;
				.icon{
					margin-bottom: 4px;
					line-height: 24px;
					font-size: 24px;
					color: rgba(7,17,27,0.1);
					&.collect{
						color: rgb(240,20,20);
					}
				}
				.word{
					line-height: 10px;
					font-size: 10px;
					color: rgb(77,85,93);
				}
			}
		}
		.price-wrapper{
			padding: 18px 0;
			ul{
				font-size: 0;
				li{
					position: relative;
					display: inline-block;
					line-height: 10px;
					text-align: center;
					font-size: 10px;
					color: rgb(147,153,159);
					width: 33.3%;
					&:after{
						display: block;
						content: "";
						position: absolute;
						top: 0;
						right: 0;
						width: 1px;
						height: 100%;
						background-color: rgb(147,153,159);
					}
					&:last-child{
						&:after{
							display: none;
						}
					}
					.num{
						margin-top: 4px;
						line-height: 24px;
						font-size: 24px;
						font-weight: 200;
						color: rgb(7,17,27);
						small{
							font-size: 10px;
						}
					}
				}
			}
		}
		.bulletin-wrapper{
			position: relative;
			margin: 0 18px;
			padding: 18px 0 16px;
			@include after1px(rgba(7,17,27,0.1));
			.content{
				padding: 0 12px;
				line-height: 24px;
				font-size: 12px;
				font-weight: 200;
				color: rgb(240,20,20);
			}
		}
		.supports-wrapper{
			padding: 0 18px;
		}
		.pics-wrapper{
			padding: 18px;
			.title{
				margin-bottom: 12px;
			}
			.pics{
				position: relative;	
				width: 100%;
				height: 90px;
				white-space: nowrap;
				overflow: hidden;
				ul{
					position: absolute;
					top: 0;
					left: 0;
					font-size: 0;
					li{
						margin-right: 6px;
						display: inline-block;
						width: 120px;
						height: 90px;
						img{
							width: 100%;
							height: 100%;
						}
					}
				}
			}
		}
		.infor-wrapper{
			position: relative;
			padding: 18px 18px 0;
			.title{
				margin-bottom: 12px;
			}
			ul{
				li{
					position: relative;
					padding: 16px 12px;
					line-height: 16px;
					font-size: 12px;
					font-weight: 200;
					color: rgb(7,17,27);
					@include before1px(rgba(7,17,27,0.1))
				}
			}
		}
		.title{
			margin-bottom: 8px;
			line-height: 14px;
			font-size: 14px;
			color: rgb(7,17,27);
		}
	}
</style>