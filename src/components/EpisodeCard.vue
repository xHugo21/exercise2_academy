<!--Component that defines each episode card extracted from the Rick and Morty API-->
<template>
    <article class="episode">
      	<!--<img class="episode__image" v-bind:alt="'episode image of'+episode.name" v-bind:src="episode.image">-->
      	<div class="episode__description">
			<div class="episode__info">{{ episode.name }}</div>
			<div class="episode__info">{{ episode.air_date }}</div>
			<div class="episode__info">{{ episode.episode }}</div>
			
      	</div>
    </article>
</template>
  
<script lang="js">
    export default {
    	props: ['episode']
    };
</script>
  
<style scoped>
    .episode__description{
		display:flex;
		justify-content: space-around;
		align-items: center;
        padding: 2%;
		border-radius:15px;
		border: 2px solid purple;
		margin-top: 3%;
		margin-bottom: 3%;
	}

    .episode__info{
        padding: 2%;
		border-radius:15px;
		border: 2px solid purple;
		margin-top: 3%;
		margin-bottom: 3%;
    }

</style>