<!--Component that defines each character card extracted from the Rick and Morty API-->
<template>
    <article class="character">
      	<img class="character__image" v-bind:alt="'character image of'+character.name" v-bind:src="character.image">
      	<div class="character__description">
			<div class="character__name">{{ character.name }}</div>
			<div v-bind:class="['character__div','character__status--'+character.status.toLowerCase()]">{{ character.status }}</div>
        	<div v-bind:class="['character__div','character__species--'+character.species.toLowerCase()]">{{ character.species }}</div>
        	<div v-bind:class="['character__div','character__gender--'+character.gender.toLowerCase()]">{{ character.gender }}</div>
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
		display:flex;
		justify-content: space-around;
		align-items: center;
	}

	.character__div {
		padding: 2%;
		border-radius:15px;
		border: 2px solid purple;
		margin-top: 3%;
		margin-bottom: 3%;
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

	.character__gender--male{
		background-color: aqua;
	}

	.character__gender--female{
		background-color: pink;
	}

	.character__gender--unknown{
		background-color: grey;
	}

	.character__species--human{
		background-color: #dbb798;
	}

	.character__species--alien{
		background-color: #b7d7d7;
	}

	.character__species--humanoid{
		background-color: #cb6e1d;
	}

	.character__species--cronenberg{
		background-color:darkkhaki;
	}

	.character__species--unknown{
		background-color:grey;
	}

	.character__species--poopybutthole{
		background-color: #1f5104;
	}

</style>