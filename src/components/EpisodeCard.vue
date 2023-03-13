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
  	props: ['episode'],

      /*data(){
	return{
		numberOfCharacters: 0
	}
},
mounted: {
	getNumberOfCharacters(){
              this.numberOfCharacters = this.episode.characters.length;
	}
      }*/
  };
</script>

<style scoped>
.episode__description {
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 2px solid purple;
  padding: 10% 0 10% 0;
}

.episode__info {
  border-radius: 15px;
  border: 2px solid purple;
  margin-top: 2%;
  margin-bottom: 2%;
}
</style>
