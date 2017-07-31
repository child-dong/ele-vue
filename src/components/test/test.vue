<template>
	<div class="test">
		<ul>
			<li v-for="item in test">{{item.description}}</li>
		</ul>	
	</div>
</template>

<script>
	export default{
		props:{
			test:{
				return: Array
			}
		}
	}
</script>

<style lang="scss" scoped>
	.test{
		height: 100px;
		background: blue;
	}
</style>

