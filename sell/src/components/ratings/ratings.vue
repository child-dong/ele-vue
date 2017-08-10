<template>
  <div class="ratings" ref="ratingsScroll">
  	<div class="contentAll">
	   	<div class="score-wrapper">
	   		<div class="overallRating">
	   			<dl class="border1px">
	   				<dt>{{seller.score}}</dt>
	   				<dd class="desc">综合评分</dd>
	   				<dd class="rankrate">高于周边商家{{seller.rankRate}}%</dd>
	   			</dl>
	   		</div>
	   		<div class="service">
	   			<div class="attitude">
	   				<ul>		
		   				<li>服务态度</li>
							<li>
								<star :star="seller.serviceScore" :size="size"></star>
							</li>
							<li>{{seller.serviceScore}}</li>
	   				</ul>
	   				<ul>		
		   				<li>食物评分</li>
							<li>
								<star :star="seller.foodScore" :size="size"></star>
							</li>
							<li>{{seller.foodScore}}</li>
	   				</ul>
	   				<ul>		
		   				<li>送达时间</li>
							<li class="deliveryTime">{{seller.deliveryTime}}分钟</li>
	   				</ul>
	   			</div>
	   		</div>
	   	</div>
	   	<div class="gap after1px"></div>
	   	<div class="satisfaction-wrapper after1px" v-if="ratings">
	   		<ul>
	   			<li @click="all" :class="{active:selectType===0}">全部<span>{{ratings.length}}</span></li>
	   			<li @click="satis" :class="{active:selectType===1}">满意<span>{{leng}}</span></li>
	   			<li @click="unsatis" :class="{active:selectType===2}">不满意<span>{{unleng}}</span></li>
	   		</ul>
	   	</div>
	   	<div class="select" @click="textShow">
	   		<i class="icon-check_circle" :class="className"></i>
	   		<span>只看有内容的评价</span>
	   	</div>
	   	<div class="reviews-wrapper" v-if="ratings">
	   		<ul class="reviews">
	   			<li class="reviews-list" v-for="item in ratings" v-if="item.text.length>text&&item.rateType>ratetype&&item.rateType<ratetype2">
	   				<img class="avatar" :src="item.avatar" alt="">
	   				<div class="content">
	   					<p class="username">{{item.username}}</p>
	   					<p class="star">
	   						<span><star :star="item.score" :size="sizeSmall"></star></span>
	   						<span class="deliveryTime" v-if="item.deliveryTime">{{item.deliveryTime}}分钟送达</span>
	   					</p>
	   					<p class="text">
	   						{{item.text}}
	   					</p>
	   					<div class="thumb clearfix">
	   						<i class="icon-thumb_up pull-left" v-if="item.rateType===0"></i>
	   						<i class="icon-thumb_down pull-left" v-else></i>
	   						<ul class="pull-left" v-if="item.recommend">
	   							<li class="foods-type pull-left" v-for="(item,index) in item.recommend" v-if="index<3">{{item}}</li>
	   						</ul>
	   					</div>
	   				</div>		
	   			</li>
	   		</ul>
		   	<!-- <shopcart :delivery="seller.deliveryPrice" :minPrice="seller.minPrice">
		   	</shopcart> -->
	  	</div>
   	</div>
  </div>
</template>

<script>
	import bScroll from 'better-scroll';
  import star from "../star/star.vue"
  import shopcart from "../shopcart/shopcart.vue";

	export default {
		props:{
			seller:{
				return:Object
			}
		},
		data() {
			return{
				selectType: 0,
				ratetype: -1,
				ratetype2: 2,
				text: -1,
				ratings: Array,
				size: "star36",
				sizeSmall: "star48",
				leng: 0,
				unleng: 0,
				className: "unchecked"
			}
		},
		created(){
	  	this.$http.get('/api/ratings').then(response =>{
	  		this.ratings = response.body.data;
	  		this.satisfaction()
	  		this.displeasure()
	  		this.$nextTick(() => {
	  			this._initScroll()
	  		})
	  	})
		},
		methods:{
			_initScroll(){
	  		this.ratingsScroll = new bScroll(this.$refs.ratingsScroll,{
	  			click: true,
	  			startY: true
	  		})
				this.ratingsScroll.refresh()
	  	},
			satisfaction(){
				for(let i=0;i<this.ratings.length;i++){
					if(this.ratings[i].rateType==0){
						this.leng += 1
					}
				}
			},
			displeasure(){
				for(let i=0;i<this.ratings.length;i++){
					if(this.ratings[i].rateType==1){
						this.unleng += 1
					}
				}
			},
			textShow(){
				if(this.text === 0){
					this.text = -1
				}else{
					this.text = 0
				}
				if(this.className === "unchecked"){
					this.className = "checked"
				}else{
					this.className = "unchecked"
				}
			},
			all(){
				this.ratetype = -1;
				this.ratetype2 = 2;
				this.selectType = 0;
			},
			satis(){
				this.ratetype = -1;
				this.ratetype2 = 1;
				this.selectType = 1;
			},
			unsatis(){
				this.ratetype = 0;
				this.ratetype2 = 2;
				this.selectType = 2;
			}
		},
		watch:{
			"text"(){
				this.$nextTick(() =>{
					this.ratingsScroll.refresh()
				})
			},
			"ratetype"(){
				this.$nextTick(() =>{
					this.ratingsScroll.refresh()
				})
			},
			"ratetype2"(){
				this.$nextTick(() =>{
					this.ratingsScroll.refresh()
				})
			}
		},
		components:{
			star: star,
			shopcart: shopcart
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	@import '../common/common.scss';
	.ratings{
		position: fixed;
		top: 174px;
		width: 100%;
		height: 100%;
		overflow: hidden;
		.contentAll{
			padding-bottom: 174px;
		}
		.score-wrapper{
			position: relative;
			display: flex;
			.overallRating{
				flex: 1;
				padding: 18px 0;
				text-align: center;
				dl{
					position: relative;
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
					dt{
						line-height: 28px;
						font-size: 24px;
						color: rgb(255,153,0);
					}
					.desc{
						box-sizing: border-box;
						padding: 6px 0 8px;
						line-height: 12px;
						font-size: 12px;
						color: rgb(7,17,27);
					}
					.rankrate{
						box-sizing: border-box;
						padding-bottom: 6px;
						line-height: 12px;
						font-size: 10px;
						color: rgb(147,153,159);
					}
				}
			}
			.service{
				flex: 0 0 237.5px;
				box-sizing: border-box;
				padding: 18px 24px;
				ul{		
					font-size: 0;		
					li{
						display: inline-block;
						vertical-align: top;
						&:nth-child(1){
							margin-top: -2px;
							line-height: 18px;
							font-size: 12px;
							color: rgb(7,17,27);
						}
						&:nth-child(2){
							margin: 0 12px;
						}
						&:nth-child(3){
							margin-top: -2px;
							line-height: 18px;
							font-size: 12px;
							color: rgb(255,153,0);
						}
					}
					&:nth-child(2){
						margin: 8px 0;
					}
					&:nth-child(3){
						.deliveryTime{
							margin-top: -2px;
							line-height: 18px;
							font-size: 12px;
							color: rgb(147,153,159);
						}
					}
				}
			}
		}
		.satisfaction-wrapper{
			position: relative;
			margin: 0 18px;
			padding: 18px 0;
			@include after1px(rgba(7,17,27,0.1));
			ul{
				font-size: 0;
				li{
					box-sizing: border-box;
					display: inline-block;
					margin-right: 8px;
					padding: 8px 12px;
					line-height: 16px;
					text-align: center;
					font-size: 14px;
					border-radius: 2px;
					&:nth-child(1){
						color: #fff;
						background-color: rgb(0,160,220); 
					}
					&:nth-child(2){
						color: rgb(77,85,93);
						background-color: rgba(0,160,220,0.2); 
					}
					&:nth-child(3){
						color: rgb(77,85,93);
						background-color: rgba(77,85,93,0.2); 
					}
					&.active{
						color: #fff;
						background-color: #000;
					}
					span{
						margin-left: 2px;
						font-size: 8px;
					}
				}
			}
		}
		.select{
			position: relative;
			padding: 12px 18px;
			line-height: 24px;
			font-size: 0;
			color: rgb(147,153,159);
			@include after1px(rgba(7,17,27,0.1));
			span{
				display: inline-block;
				vertical-align: top;
				font-size: 12px;
			}
			i{
				margin-right: 4px;
				font-size: 24px;
				&.checked{
					color: #00b43c;
				}
				&.unchecked{
					color: rgb(147,153,159);
				}
			}
		}
		.reviews-wrapper{
			// position: relative;
			// padding-bottom: 48px;
			.reviews{
				.reviews-list{
					position: relative;
					margin: 0 18px;
					padding: 18px 0;
					display: flex;
					@include after1px(rgba(7,17,27,0.1));
					.avatar{
						flex: 0 0 28px;
						height: 28px;
					}
					.content{
						flex: 1;
						margin-left: 12px;
						.username{
							line-height: 12px;
							font-size: 10px;
							color: rgb(1,17,27);
						}
						.star{
							margin: 4px 0 6px;
							font-size: 0;
							span{
								display: inline-block;
							}
							.deliveryTime{
								margin-left: 6px;
								line-height: 12px;
								font-size: 10px;
								font-weight: 200;
								color: rgb(147,153,159);
							}
						}
						.text{
							margin-bottom: 8px;
							line-height: 18px;
							font-size: 12px;
							color: rgb(7,17,27);
						}
						.thumb{
							i{
								line-height: 16px;
								font-size: 12px;
								&.icon-thumb_up{
									color: rgb(0,160,220);
								}
								&.icon-thumb_down{
									color: rgb(183,187,191);
								}
							}
							ul{
								li{
									margin-left: 8px;
									padding: 0 6px;
									max-width: 63px;
									line-height: 16px;
									text-align: center;
									font-size: 9px;
									color: rgb(147,153,159);
									border: 1px solid rgba(7,17,27,0.1);
									border-radius: 2px;
									@include beyond-hide(1)
								}
							}
						}
					}
				}
			}
		}
	}
</style>