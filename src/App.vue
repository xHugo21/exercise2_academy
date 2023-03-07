<!--Second challenge. Create an application using Vue.js to fetch and filter characters from the Rick and Morty API. Created by Hugo García Cuesta-->
<template>
	<header>
		<SearchBar v-on:input="getCharacters"></SearchBar>
 	</header>

  	<main>
		<aside>
			<h2>Filters</h2>
			<h3>Status</h3>
			<FiltersGrid v-bind:filters="filters.status" v-slot="slotProps">
				<FilterComponent v-on:clickCheckbox="updateChecked(slotProps.filter)">{{ slotProps.filter }}</FilterComponent>
			</FiltersGrid>

			<h3>Gender</h3>
			<FiltersGrid v-bind:filters="filters.gender" v-slot="slotProps">
				<FilterComponent v-on:clickCheckbox="updateChecked(slotProps.filter)">{{ slotProps.filter }}</FilterComponent>
			</FiltersGrid>
		</aside>

		<CharactersGrid>
			<CharacterCard v-for="character in characters" v-bind:key="character.id" v-bind:character="character"></CharacterCard>
		</CharactersGrid>
		
  	</main>
	
	<div class="loadmorediv">
		<button class="loadmorebutton" v-on:click="loadMoreResults()">Load More Results</button>
	</div>

  	<footer @scroll="onScroll">
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
				data: {},
				characters: [],
				filters: {
					status: [],
					gender: [],
				},
				checked: [],
			}
		},
		methods: {
			// Fetches the characters from the API and updates this.characters
			async getCharacters(event) {
				let name;
				if (event){
					name = event.target.value;
				} else {
					name = "";
				}
				const response = await fetch('https://rickandmortyapi.com/api/character/?name='+name);
				const data = await response.json();
				this.data = data;
				this.characters = data.results;

				this.getFilters();
			},

			// Updates this.filters depending on the characters that are currently being displayed
			getFilters() {
				this.filters.status = [];
				for (let i = 0; i < this.characters.length; i++) {
					if (!this.filters.status.includes(this.characters[i].status)){
						this.filters.status.push(this.characters[i].status);
					}
				}
				
				for (let i = 0; i < this.characters.length; i++) {
					if (!this.filters.gender.includes(this.characters[i].gender)){
						this.filters.gender.push(this.characters[i].gender);
					}
				}
			},
			
			/*
			updateChecked(filter) {
				// Check if filter is in checked
				if (this.checked.includes(filter)){
					// If it is, remove it
					this.checked.splice(this.checked.indexOf(filter), 1);
				} else {
					// If it isn't, add it
					this.checked.push(filter);
				}
				console.log("checked "+this.checked);
				this.applyFilter();
			},

			applyFilter(){
				let currentfilters = [];
				for (let i = 0; i < this.checked.length; i++) {
					if (this.checked[i]){
						currentfilters.push(this.filters[i]);
					}
				}
				if (currentfilters.length == 0){
					currentfilters = this.filters;
				}
				this.getCharacters();
				this.characters = this.characters.filter(character => currentfilters.includes(character.status));
			},

			onScroll ({ target: { scrollTop, clientHeight, scrollHeight }}) {
      			if (scrollTop + clientHeight >= scrollHeight) {
        			this.loadMoreResults()
      			}
    		},*/

			async loadMoreResults(){
				// If there isn´t more data -> display message and return
				if (this.data.info.next == null){
					console.log("THERE AREN'T MORE RESULTS TO DISPLAY");
					return;
				}

				// Save new url
				let url = this.data.info.next;

				// Fetch new data
				const api = await fetch(url);
				const data = await api.json();
				this.data = data;

				this.characters = this.characters.concat(data.results);	
			}
		
		},
		mounted() {
			this.getCharacters(); // Calls getCharacters when the page is loaded
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

	main {
		display: grid;
		grid-template-columns: 1fr 5fr;
	}

	aside {
		display:flex;
		flex-direction: column;
		align-content: center;
		border: 2px solid purple;
		border-radius: 15px;
		padding: 2%;
		background-color: white;
	}

	footer {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin-top: 2%;
		border-top: 2px solid purple;
		border-bottom: 2px solid purple;
		width: max;
	}

	.loadmorediv{
		display:flex;
		justify-content: center;
		margin-top: 3%;
	}

</style>
