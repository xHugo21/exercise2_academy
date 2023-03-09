<!--Component that defines the section where a filter form is placed-->
<template>
    <ul>
		<TransitionGroup name="animationfilters"> 
    	<li v-for="filter in filters" v-bind:key="filter">
        	<slot :filter="filter"></slot>
      	</li>
		</TransitionGroup>
    </ul>
</template>
  
<script lang="js">
    export default {
      	props: {
			['filters']: Array
		}
	};
</script>
  
<style scoped>
	ul {
		list-style: none;
		padding: 0;
	}

	.animationfilters-enter-active,
	.animationfilters-leave-active {
		transition: all 0.8s ease;
	}
	.animationfilters-enter-from,
	.animationfilters-leave-to {
		opacity: 0;
		transform: translateX(30px);
	}
</style>