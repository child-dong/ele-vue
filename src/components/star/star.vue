<template>
  <div class="star" :class="size">
	  <ul>
	  	<li v-for="item in itemClasses" :class="item" class="star-item"></li>
	  </ul>
  </div>
</template>

<script>
	const star_length = 5;
	export default {
		props:{
			star:{
				return: Number
			},
			size:{
				return: String
			}
		},
		computed:{
			// typeSize(){
			// 	return "type" + this.size
			// },
			itemClasses(){
				let result = [];
				let star = Math.floor(this.star * 2) / 2;
				let integer = Math.floor(star);
				let decimals = star % 1 !== 0;
				for(let i = 0; i<integer; i++){
					result.push("on");
				}
				if(decimals){
					result.push("half");
				}
				while(result.length<star_length){
					result.push("off");
				}
				return result;
			}
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	@import './../common/common.scss';
	.star{
  	font-size: 0;
	  .star-item{
	    display: inline-block;
	    background-repeat: no-repeat;
	    margin-right: 22px;
	    background-size: cover;
	    &:last-child{
	    	margin-right: 0;
	    }
	  }
	  &.star24{
	  	.star-item{  
		  	width: 24px;
		    height: 24px;		
			  &.on{
			   	@include pic2x(star24_on);
			  }
			  &.half{
			    @include pic2x(star24_half);
			  }
			  &.off{
			   @include pic2x(star24_off);
			  }
	  	}
	  }
	  &.star36{
	  	.star-item{  
	  		width: 36px;
		    height: 36px;		
			  &.on{
			   	@include pic2x(star36_on);
			  }
			  &.half{
			    @include pic2x(star36_half);
			  }
			  &.off{
			   @include pic2x(star36_off);
			  }
	  	}
	  }
	  &.star48{
	  	.star-item{ 
	  		width: 48px;
		    height: 48px; 		
			  &.on{
			   	@include pic2x(star48_on);
			  }
			  &.half{
			    @include pic2x(star48_half);
			  }
			  &.off{
			   @include pic2x(star48_off);
			  }
	  	}
	  }
  }
	
</style>