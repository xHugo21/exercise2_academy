<!--Component that defines each character card extracted from the Rick and Morty API-->
<template>
    <article class="character">
      	<img class="character__image" v-bind:alt="'character image of'+character.name" v-bind:src="character.image">
      	<div class="character__description">
			<p>{{ character.name }}</p>
        	<div v-bind:class="['character__status','character__status--'+character.status.toLowerCase()]">{{ character.status }}</div>
      	</div>
    </article>
</template>
  
<script lang="js">
    export default {
    	props: ['character']
    };
</script>
  
<style scoped>

	.character__image{
		border: 2px solid purple;
	}
	.character__description{
		display: grid;
		grid-template-columns: 1fr 1fr;
		align-items: center;
	}

	.character__status {
		
	}
    .character__status--alive {
    	background-color: #00ff00;
    }

	.character__status--dead {
		background-color: #ff0000;
	}

	.character__status--unknown {
		background-color: grey;
	}

</style>