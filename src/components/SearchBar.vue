<!--Component that defines the search bar-->
<template>
    <input v-on:input="$emit('search', $event.target.value)" class="search_bar" type="search" placeholder="Search by name...">
</template>

<script lang="js">
    export default{

    }
</script>

<style scoped>
    .search_bar {
		display: flex;
		justify-content: center;
		background-color: white;
		border-radius: 15px;
		border-color: purple;
		width: 20%;
		height: 100%;
		padding: 0.8%;
	}
</style>