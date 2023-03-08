<!--Component that defines the section where a filter form is placed-->
<template>
    <ul>
    	<li v-for="filter in filters" v-bind:key="filter">
        	<slot :filter="filter" :count="count"></slot>
      	</li>
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
</style>