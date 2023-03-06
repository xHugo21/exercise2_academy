<!--Second challenge. Create an application using Vue.js to fetch and filter characters from the Rick and Morty API. Created by Hugo García Cuesta-->
<template>
	<header>
		<SearchBar v-on:input="getCharacters"></SearchBar>
 	</header>

  	<main>
		<FiltersGrid>
			<FilterComponent></FilterComponent>
		</FiltersGrid>

		<CharactersGrid>
			<CharacterCard v-for="character in characters" v-bind:key="character.id" v-bind:character="character"></CharacterCard>
		</CharactersGrid>
  	</main>

  	<footer>
    	<p>Rick and Morty Search Webpage using Vue.js</p>
    	<p>Hugo García Cuesta | Academy Frontend Developer</p>
 	</footer>
</template>

<script lang="js">
	import CharacterCard from './components/CharacterCard.vue';
	import CharactersGrid from './components/CharactersGrid.vue';
	import FilterComponent from './components/FilterComponent.vue';
	import FiltersGrid from './components/FiltersGrid.vue';
	import SearchBar from './components/SearchBar.vue';
	

	export default {
		name: 'App',
		components: {
			CharacterCard,
			CharactersGrid,
			FilterComponent,
			FiltersGrid,
			SearchBar
		},
		data() {
			return {
				characters: [],
				filters: [],
			}
		},
		methods: {
			async getCharacters(event) {
				const name = event.target.value;
				const response = await fetch('https://rickandmortyapi.com/api/character/?name='+name);
				const data = await response.json();
				this.characters = data.results;
			},
		},
		/*mounted() {
			this.getCharacters();
		},	*/
  	}

</script>

<style scoped>
	
	body {
		background-color: white;
		font-family: Arial, sans-serif;
	}

	header {
		display: flex;
		justify-content: center;
		align-items: center;
		padding-bottom: 1.5%;
		padding-top: 1.5%;
	}

	main {
		display: grid;
		grid-template-columns: 1fr 5fr;
	}

	/*
	aside {
		border: 2px solid purple;
		border-radius: 15px;
		padding: 2%;
		background-color: white;
	}
	aside h2 {
		display: flex;
		justify-content: center;
	} */

	footer {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin-top: 2%;
		border-top: 2px solid purple;
		border-bottom: 2px solid purple;
		width: max;
	}

</style>
