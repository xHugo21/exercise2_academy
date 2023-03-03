<template>
  <header>
      <input v-on:change="getCharacters" class="search_bar" type="search" placeholder="Search">
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
    <p>Rick and Morty Search Webpage</p>
    <p>Hugo García Cuesta | Academy Frontend Developer</p>
  </footer>
</template>

<script lang="js">
	import CharacterCard from './components/CharacterCard.vue';
	import CharactersGrid from './components/CharactersGrid.vue';
	import FilterComponent from './components/FilterComponent.vue';
	import FiltersGrid from './components/FiltersGrid.vue';
	

	export default {
		name: 'App',
		components: {
    CharacterCard,
    CharactersGrid,
    FilterComponent,
    FiltersGrid
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
		mounted() {
			this.getCharacters();
		},	
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
	header .search_bar {
		display: flex;
		justify-content: center;
		background-color: white;
		border-radius: 10px;
		border-color: purple;
		width: 20%;
		height: 100%;
	}

	main {
		display: grid;
		grid-template-columns: 1fr 5fr;
	}

	aside {
		border: 2px solid purple;
		border-radius: 15px;
		padding: 2%;
		background-color: white;
	}
	aside h2 {
	display: flex;
	justify-content: center;
	}

	.search_results {
		display: flex;
		justify-content: justify;
		flex-wrap: wrap;
	}

	.article_result {
		margin: 1%;
	}

	.result_footer {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}
	.result_footer .alive {
		background-color: rgb(57, 184, 65);
	}
	.result_footer .dead {
		background-color: red;
	}
	.result_footer .unknown {
		background-color: darkgrey;
	}

	footer {
		display: flex;
		flex-direction: column;
		align-items: center;
		border-top: 2px solid purple;
		border-bottom: 2px solid purple;
		width: max;
	}

</style>
