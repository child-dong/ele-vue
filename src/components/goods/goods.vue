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
				<li v-for="item in goods" class="food-list-hook">
					<h2 class="title">{{item.name}}</h2>
					<ul>
						<li class="foods after1px" v-for="item in item.foods">
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
						</li>
					</ul>
				</li>
			</ul>
		</div>
  </div>
</template>

<script>
	import bScroll from 'better-scroll';
	export default {
		data(){
			return{
				goods: Object,
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
	  	_initScroll(){
	  		this.menuScroll = new bScroll(this.$refs.menuWrapper,{
	  			click: true
	  		})
	  		this.foodScroll = new bScroll(this.$refs.foodsWrapper,{
	  			probeType: 3
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
	  	}
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
		bottom: 48px;
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
						}
					}
				}
			}
		}
	}
</style>