<!--Second challenge. Create an application using Vue.js to fetch and filter characters from the Rick and Morty API. Created by Hugo García Cuesta-->
<template>
	<header>
		<img class = "headerlogo" src="../public/Rick-and-Morty.png" alt="Rick and Morty Logo">
		<SearchBar v-on:input="debounce(getCharacters($event), 300)"></SearchBar>
 	</header>

  	<main>
		<aside>
			<h2 class="titlefilters">Filters</h2>
			<h3>Status</h3>
			<FiltersGrid v-bind:filters="filters.status" v-slot="slotProps">
				<FilterComponent v-on:clickCheckbox="updateChecked(slotProps.filter)">{{ slotProps.filter }}</FilterComponent>
			</FiltersGrid>

			<h3>Species</h3>
			<FiltersGrid v-bind:filters="filters.species" v-slot="slotProps">
				<FilterComponent v-on:clickCheckbox="updateChecked(slotProps.filter)">{{ slotProps.filter }}</FilterComponent>
			</FiltersGrid>

			<h3>Gender</h3>
			<FiltersGrid v-bind:filters="filters.gender" v-slot="slotProps">
				<FilterComponent v-on:clickCheckbox="updateChecked(slotProps.filter)">{{ slotProps.filter }}</FilterComponent>
			</FiltersGrid>

			<button class="button-6" v-on:click="toggleView()">Toggle view (Characters/Episodes)</button>
		</aside>

		<CharactersGrid v-if="show_characters">
			<CharacterCard v-for="character in characters" v-bind:key="character.id" v-bind:character="character"></CharacterCard>
		</CharactersGrid>

		<CharactersGrid v-else>
			<EpisodeCard v-for="episode in episodes" v-bind:key="episode.id" v-bind:episode="episode"></EpisodeCard>
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
	import EpisodeCard from './components/EpisodeCard.vue';

	export default {
		name: 'App',
		components: {
			CharacterCard,
			CharactersGrid,
			FilterComponent,
			FiltersGrid,
			SearchBar,
			EpisodeCard
		},
		data() {
			return {
				data: {},
				characters: [],
				filters: {
					status: [],
					species: [],
					gender: [],
				},
				count: {Alive: 0, Dead: 0, unknown: 0},
				checked: [],
				show_characters: true,
				episodes: [],
			}
		},
		methods: {
			// Debounce function to avoid calling the API too many times
			debounce(func, delay) {
				let timerId;
				return function() {
					const context = this;
					const args = arguments;
					clearTimeout(timerId);
					timerId = setTimeout(() => func.apply(context, args), delay);
				}
			},

			// Fetches the characters from the API and updates this.characters
			async getCharacters(event) {
				let name;
				if (event){
					console.log(event.target.value);
					name = event.target.value;
				} else {
					name = "";
				}
				const response = await fetch('https://rickandmortyapi.com/api/character/?name='+name);
				const data = await response.json();
				this.data = data;
				this.characters = data.results;

				this.getFilters();
				//this.countStatus();
			},

			async getEpisodes(){
				// Fetch all episodes from the API
				const response = await fetch('https://rickandmortyapi.com/api/episode');
				const data = await response.json();
				this.data = data;
				this.episodes = data.results;
				console.log(this.data);
			},

			// Updates this.filters depending on the characters that are currently being displayed
			getFilters() {
				this.filters.status = [];
				this.filters.species = [];
				this.filters.gender = [];
				for (let i = 0; i < this.characters.length; i++) {
					if (!this.filters.status.includes(this.characters[i].status)){
						this.filters.status.push(this.characters[i].status);
					}

					if (!this.filters.species.includes(this.characters[i].species)){
						this.filters.species.push(this.characters[i].species);
					}

					if (!this.filters.gender.includes(this.characters[i].gender)){
						this.filters.gender.push(this.characters[i].gender);
					}
				}
				
			},

			// This method updates the checked variable depending on checked filters
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
			
			// This method updates the characters displayed depending on checked filters
			applyFilter(){
				// If there are no filters checked, display all characters
				if (this.checked.length == 0){
					this.characters = this.data.results;
					return;
				}

				// Create a new array to store the filtered characters
				let filteredCharacters = [];

				// Loop through all characters
				for (let i = 0; i < this.data.results.length; i++) {
					// Loop through all checked filters
					for (let j = 0; j < this.checked.length; j++) {
						// If the character matches the filter, add it to the filteredCharacters array
						if (this.data.results[i].status == this.checked[j]){
							filteredCharacters.push(this.data.results[i]);
						}

						if (this.data.results[i].species == this.checked[j]){
							filteredCharacters.push(this.data.results[i]);
						}

						if (this.data.results[i].gender == this.checked[j]){
							filteredCharacters.push(this.data.results[i]);
						}
					}
				}
				// Update the characters displayed
				this.characters = filteredCharacters;
			},


			async loadMoreResults(){
				console.log(this.data.info.next);
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

				//this.applyFilter();

				this.characters = this.characters.concat(data.results);	
				//this.applyFilter(); // Apply filters to new characters added
			},

			toggleView(){
				console.log("toggleView");
				this.show_characters = !this.show_characters;
				console.log(this.show_characters);

				if (!this.show_characters){
					this.getEpisodes();
				}
			}

			// Create a function that counts the number of characters displayed of each status
			/*countStatus(){
				
				for (let i = 0; i < this.characters.length; i++) {
					// For loop that loops through all the statuses
					for (let status in this.count){
						// If the status of the character matches the status in the loop
						if (this.characters[i].status == status){
							// Add 1 to the position of this.count that matches the status
							this.count[status] += 1;
						}
					}
				}
				
				console.log(this.count.Alive);
			},*/
		
		},
		mounted() {
			this.getCharacters(); // Calls getCharacters when the page is loaded

			// Create an event listener that checks if we are at the bottom of the page
			// If we are, it calls the loadMoreResults() function
			window.addEventListener('scroll', function() {
				if (window.innerHeight + window.scrollY >= document.body.offsetHeight) {
					this.debounce(this.loadMoreResults(), 500);
				}
			}.bind(this));
		},
	
  	}

	


</script>

<style scoped>
	header {
		border-top: 2px solid purple;
		border-bottom: 2px solid purple; 
		display: flex;
		justify-content:space-evenly;
		align-items: center;	
		padding-bottom: 1.5%;
		padding-top: 1.5%;
		margin-bottom: 2%;
	}

	.headerlogo{
		width: 10%;
		height: 10%;
	}

	main {
		display: grid;
		grid-template-columns: 1fr 5fr;
	}

	aside {
		display:flex;
		flex-direction: column;
		align-items: flex-start;
		margin-left: 5%;
		padding-left: 5%;
		padding-right: 5%;
		border: 2px solid purple;
		border-radius: 15px;
		background-color: white;
		height: max-content;
		padding-bottom: 5%;
	}
/* CSS */
	.button-6 {
		align-items: center;
		background-color: #FFFFFF;
		border: 1px solid rgba(0, 0, 0, 0.1);
		border-radius: .25rem;
		box-shadow: rgba(0, 0, 0, 0.02) 0 1px 3px 0;
		box-sizing: border-box;
		color: rgba(0, 0, 0, 0.85);
		cursor: pointer;
		display: inline-flex;
		font-family: system-ui,-apple-system,system-ui,"Helvetica Neue",Helvetica,Arial,sans-serif;
		font-size: 16px;
		font-weight: 600;
		justify-content: center;
		line-height: 1.25;
		margin: 0;
		min-height: 3rem;
		padding: calc(.875rem - 1px) calc(1.5rem - 1px);
		position: relative;
		text-decoration: none;
		transition: all 250ms;
		user-select: none;
		-webkit-user-select: none;
		touch-action: manipulation;
		vertical-align: baseline;
		width: auto;
	}

	.button-6:hover,
	.button-6:focus {
		border-color: rgba(0, 0, 0, 0.15);
		box-shadow: rgba(0, 0, 0, 0.1) 0 4px 12px;
		color: rgba(0, 0, 0, 0.65);
	}

	.button-6:hover {
		
	}

	.button-6:active {
		background-color: #F0F0F1;
		border-color: rgba(0, 0, 0, 0.15);
		box-shadow: rgba(0, 0, 0, 0.06) 0 2px 4px;
		color: rgba(0, 0, 0, 0.65);
		transform: translateY(0);
	}

	.titlefilters{
		align-self: center;
		padding-left: none;
		margin-left: none;
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

</style>
