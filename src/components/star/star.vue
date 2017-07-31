<template>
  <div class="star">
  	{{star}}
  </div>
</template>

<script>
	export default {
		props:{
			star:{
				return: Number
			}
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	@import './../common/common.scss';
	.star{
		
	}
	
</style>