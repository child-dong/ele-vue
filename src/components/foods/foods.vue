<template>
	<transition name="move">
	  <div class="foods" v-show="showFlag" ref="foodsScroll">	
		  <div class="content">	  	
				<div class="image">
					<img :src="food.image" alt="">
					<div class="back" @click="showFlag=!showFlag">返回</div>
				</div>
				<div class="food">			
					<h2 class="title">{{food.name}}</h2>
					<p class="desc">{{food.description}}</p>
					<p class="sell"><span>月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span></p>
					<p class="price"><span>￥{{food.price}}</span><del v-if="food.oldPrice">￥{{food.oldPrice}}</del></p>
					<p class="join" @click="add($event)" v-if="!food.count">加入购物车</p>
					<p class="add" v-else><cartcontrol :food="food"></cartcontrol></p>
				</div>
				<div class="gap"></div>
				<div class="info">
					<h2 class="title">商品介绍</h2>
					<p class="info-content" v-if="food.info">{{food.info}}</p>
					<p class="info-content" v-else>该商品暂无介绍</p>
				</div>
				<div class="gap"></div>
				<div class="ratings" v-if="food.ratings">
					<h2 class="title">商品评价</h2>
					<ul>
		   			<li @click="all" :class="{active:selectType===0}">全部<span>{{food.ratings.length}}</span></li>
		   			<li @click="satis" :class="{active:selectType===1}">推荐<span>{{satisfaction}}</span></li>
		   			<li @click="unsatis" :class="{active:selectType===2}">吐槽<span>{{displeasure}}</span></li>
	   			</ul>
				</div>
				<div class="select" @click="textShow">
	   			<i class="icon-check_circle" :class="className"></i>
	   		<span>只看有内容的评价</span>
	   		</div>
	   		<div class="reviews" v-if="food.ratings">
	   			<ul>
	   				<li v-for="item in food.ratings" v-if="item.text.length>text&&item.rateType>ratetype&&item.rateType<ratetype2">
	   					<p class="user clearfix">
	   						<span class="time pull-left">{{formdata(item.rateTime)}}</span>
	   						<img class="pull-right" :src="item.avatar" alt="">
	   						<span class="username pull-right">{{item.username}}</span>
   						</p>
   						<p class="thumb">
	   						<i class="icon-thumb_up" v-if="item.rateType===0"></i>
	   						<i class="icon-thumb_down" v-else></i>
	   						<span class="text">{{item.text}}</span>
	   					</p>
	   				</li>
	   			</ul>
	   		</div>
		  </div>
	  </div>
  </transition>
</template>

<script>
	import cartcontrol from "../cartcontrol/cartcontrol.vue";
	import Bscroll from 'better-scroll';
	import Vue from 'vue';

	export default {
		props:{
			food:{
				return: Array
			}
		},
		data(){
			return{
				selectType: 0,
				ratetype: -1,
				ratetype2: 2,
				text: -1,
				className: "unchecked",
				showFlag: false
			}
		},
		components: {
	  	cartcontrol: cartcontrol
	  },
		methods: {
			textShow(event){
				if(!event._constructed){
					return
				}
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
			formdata(date){
			  var date = new Date(parseInt(date));

				var year = date.getFullYear();
				var month = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1);
				var day = (date.getDate() < 10 ? '0' + (date.getDate()) : date.getDate());
				var hours = (date.getHours() < 10 ? '0' + date.getHours() : date.getHours());
				var minites = (date.getMinutes() <10 ? '0' + date.getMinutes() : date.getMinutes());
				return year+"-"+month+"-"+day+" "+hours+":"+minites
			},
			show(){
				this.showFlag = true	
				this.$nextTick(() => {	
				if(!this.foodsScroll){
					this.foodsScroll = new Bscroll(this.$refs.foodsScroll,{
		  			click: true,
		  			startY: true
		  		})		
				}else{
					this.foodsScroll.refresh()
				}  			
  		})	
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
			all(event){
				if(!event._constructed){
					return
				}
				this.ratetype = -1;
				this.ratetype2 = 2;
				this.selectType = 0;
			},
			satis(event){
				if(!event._constructed){
					return
				}
				this.ratetype = -1;
				this.ratetype2 = 1;
				this.selectType = 1;
			},
			unsatis(event){
				if(!event._constructed){
					return
				}
				this.ratetype = 0;
				this.ratetype2 = 2;
				this.selectType = 2;
			}
		},
		computed:{
			satisfaction(){
				let leng = 0;
				for(let i=0;i<this.food.ratings.length;i++){
					if(this.food.ratings[i].rateType==0){
						leng += 1
					}
				}
				return leng
			},
			displeasure(){
				let unleng = 0;
				for(let i=0;i<this.food.ratings.length;i++){
					if(this.food.ratings[i].rateType==1){
						unleng += 1
					}
				}
				return unleng
			},
		},
		watch:{
			"text"(){
				this.$nextTick(() =>{
					this.foodsScroll.refresh()
				})
			},
			"ratetype"(){
				this.$nextTick(() =>{
					this.foodsScroll.refresh()
				})
			},
			"ratetype2"(){
				this.$nextTick(() =>{
					this.foodsScroll.refresh()
				})
			}
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	@import './../common/common.scss';
	.foods{
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: white;
		overflow: hidden;
		.content{
			padding-bottom: 47px;
		}
		.image{
			position: relative;
			width: 100%;
			height: 0;
			padding-top: 100%;
			.back{
				position: absolute;
				top: 10px;
				left: 10px;
				padding: 7px 7px;
				font-size: 12px;
				background-color: #000;
				color: #fff;
				border-radius: 7px;
			}
			img{
				position: absolute;
				left: 0;
				top: 0;
				width: 100%;
				height: 100%;
			}
		}
		.food{
			position: relative;
			padding: 18px;
			.title{
				line-height: 14px;
				font-size: 14px;
				font-weight: 700;
				color: rgb(7,17,27);
			}
			.desc{
				margin: 8px 0 18px 0;
				line-height: 10px;
				font-size: 10px;
				color: rgb(147,153,159);
			}
			.sell{
				font-size: 10px;
				color: rgb(147,153,159);
				margin: 8px 0 18px 0;
				line-height: 10px;		
				span{
					margin-right: 12px;
				}
			}
			.price{
				line-height: 24px;
				font-size: 14px;
				font-weight: 700;
				color: rgb(240,20,20);
				del{
					margin-left: 12px;
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
			.add{
				position: absolute;
				bottom: 18px;
				right: 18px;
			}
		}
		.info{
			padding: 18px;
			.title{
				line-height: 14px;
				font-size: 14px;
				color: rgb(7,17,27);
			}
			.info-content{
				padding: 6px 8px 0;
				line-height: 24px;
				font-size: 12px;
				color: rgb(77,85,93);
			}
		}
		.ratings{
			position: relative;
			padding: 18px 0;
			margin: 0 18px;
			@include after1px(rgba(7,17,27,0.1))
			.title{
				margin-bottom: 18px;
				line-height: 14px;
				font-size: 14px;
				font-weight: 700;
				color: rgb(7,17,27);
			}
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
						color: rgb(77,85,93);
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
		.reviews{
			padding: 0 18px;
			ul{
				li{		
					position: relative;
					padding: 16px 0;	
					@include after1px(rgba(7,17,27,0.1))
					.user{		
						.time,.username{
							line-height: 12px;
							font-size: 10px;
							color: rgb(147,153,159);
						}
						img{
							margin-left: 6px;
							width: 12px;
							height: 12px;
							border-radius: 50%;
						}
					}	
					.thumb{
						i{
							margin: 6px 4px 0 0;
							line-height: 24px;
							font-size: 12px;
							&.icon-thumb_up{
								color: rgb(0,160,220);
							}
							&.icon-thumb_down{
								color: rgb(147,153,159);
							}
						}
						.text{
							line-height: 16px;
							font-size: 12px;
							color: rgb(7,17,27);
						}
					}
				}
			}
		}
	}
	.move-enter-active, .move-leave-active{
    transition: all 0.2s linear;
    transform: translate3D(0,0,0);
  }
  .move-enter,.move-leave-active{
    transform: translate3D(100%,0,0);
  }
</style>