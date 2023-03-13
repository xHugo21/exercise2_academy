<!--Second challenge. Create an application using Vue.js to fetch and filter characters from the Rick and Morty API. Created by Hugo García Cuesta-->
<template>
  <header>
    <img class="header__logo" src="/Rick-and-Morty.png" alt="Rick and Morty Logo" />
    <SearchBar v-on:input="debounce(getCharacters($event), 300)"></SearchBar>
  </header>

  <main>
    <aside>
      <h2 v-if="show_characters" class="titlefilters">Filters</h2>
      <h3 v-if="show_characters">Status</h3>
      <FiltersGrid v-if="show_characters" v-bind:filters="filters.status" v-slot="slotProps">
        <FilterComponent v-on:clickCheckbox="updateChecked(slotProps.filter)">{{
          slotProps.filter
        }}</FilterComponent>
      </FiltersGrid>

      <h3 v-if="show_characters">Species</h3>
      <FiltersGrid v-if="show_characters" v-bind:filters="filters.species" v-slot="slotProps">
        <FilterComponent v-on:clickCheckbox="updateChecked(slotProps.filter)">{{
          slotProps.filter
        }}</FilterComponent>
      </FiltersGrid>

      <h3 v-if="show_characters">Gender</h3>
      <FiltersGrid v-if="show_characters" v-bind:filters="filters.gender" v-slot="slotProps">
        <FilterComponent v-on:clickCheckbox="updateChecked(slotProps.filter)">{{
          slotProps.filter
        }}</FilterComponent>
      </FiltersGrid>

      <h2 class="titlefilters">View</h2>
      <button class="toggleviewbutton" v-on:click="toggleView()">
        Toggle view (Characters/Episodes)
      </button>
    </aside>

    <CharactersGrid v-if="show_characters">
      <CharacterCard
        v-for="character in characters"
        v-bind:key="character.id"
        v-bind:character="character"
      ></CharacterCard>
    </CharactersGrid>

    <CharactersGrid v-else>
      <EpisodeCard
        v-for="episode in episodes"
        v-bind:key="episode.id"
        v-bind:episode="episode"
      ></EpisodeCard>
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
			datacharacters: {},
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
			dataepisodes: {},
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
			// If event has a value then assign it to name, otherwise assign an empty string
			if (event){
				console.log(event.target.value);
				name = event.target.value;
			} else {
				name = "";
			}

			// Fetch the specific characters from the API and save full data in this.datacharacters and characters inside this.characters
			const response = await fetch('https://rickandmortyapi.com/api/character/?name='+name);
			const data = await response.json();
			this.datacharacters = data;
			this.characters = data.results;

			// Call getFilters to update results based on which filters are checked
			this.getFilters();
			//this.countStatus();
		},

		// Function called when switched view mode (characters/episodes). It gets the episodes from the API and updates this.episodes and this.dataepisodes
		async getEpisodes(){
			// Fetch all episodes from the API
			const response = await fetch('https://rickandmortyapi.com/api/episode');
			const data = await response.json();
			this.dataepisodes = data;
			this.episodes = data.results;
		},

		// Updates filters that can be shown based on characters loaded
		getFilters() {
			// Assign array of each filter to an empty array every time the function is called
			this.filters.status = [];
			this.filters.species = [];
			this.filters.gender = [];

			// Loop throgh all characters and push the status found at least once to the filters array
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

		// Updates this.checked based on filters currently checked. Called each time a filter is checked/unchecked
		updateChecked(filter) {
			// Check if filter is in checked
			if (this.checked.includes(filter)){
				// If it is, remove it
				this.checked.splice(this.checked.indexOf(filter), 1);
			} else {
				// If it isn't, add it
				this.checked.push(filter);
			}

			// Call applyFilter to update characters displayed
			this.applyFilter();
		},

		// This method updates the characters displayed depending on checked filters
		applyFilter(){
			// If there are no filters checked, display all characters
			if (this.checked.length == 0){
				this.characters = this.datacharacters.results;
				return;
			}

			// Create an auxiliary array to store the filtered characters
			let filteredCharacters = [];

			// Loop through all characters
			for (let i = 0; i < this.datacharacters.results.length; i++) {
				// Loop through all checked filters
				for (let j = 0; j < this.checked.length; j++) {
					// If the character matches the filter, add it to the filteredCharacters array
					if (this.datacharacters.results[i].status == this.checked[j]){
						filteredCharacters.push(this.datacharacters.results[i]);
					}

					if (this.datacharacters.results[i].species == this.checked[j]){
						filteredCharacters.push(this.datacharacters.results[i]);
					}

					if (this.datacharacters.results[i].gender == this.checked[j]){
						filteredCharacters.push(this.datacharacters.results[i]);
					}
				}
			}

			// Update the characters displayed
			this.characters = filteredCharacters;
		},

		// Method called when bottom of the page is reached, loading more characters/episodes depending on the view mode
		async loadMoreResults(){
			var url;
			// Uncheck all filters
			const checkboxes = document.querySelectorAll('.checkboxfilter');

			// Loop through all checkboxes
			for (let i = 0; i < checkboxes.length; i++) {
				// If the checkbox is checked, uncheck it
				if (checkboxes[i].checked){
					checkboxes[i].checked = false;
				}
			}

			// If there isn´t more data -> display message and return
			/*if (this.datacharacters.info.next == null || this.dataepisodes.info.next == null){
				console.log("THERE AREN'T MORE RESULTS TO DISPLAY");
				return;
			}*/

			// Depending on the view mode, load more characters or episodes
			if (this.show_characters){
				url = this.datacharacters.info.next;
				const api = await fetch(url);
				const data = await api.json();
				this.datacharacters = data;
				this.characters = this.characters.concat(data.results);
			}
			else{
				url = this.dataepisodes.info.next;
				const api = await fetch(url);
				const data = await api.json();
				this.dataepisodes = data;
				this.episodes = this.episodes.concat(data.results);
			}

			//this.applyFilter(); // Apply filters to new characters added
		},

		toggleView(){
			console.log("toggleView");
			this.show_characters = !this.show_characters;
			console.log(this.show_characters);

			if (!this.show_characters){
				this.getEpisodes();
			}

			else{
				this.getCharacters();
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
		/*window.addEventListener('scroll', function() {
			if (window.innerHeight + window.scrollY >= document.body.offsetHeight) {
				this.debounce(this.loadMoreResults(), 500);
			}
		}.bind(this));*/

		// Add infinite scroll using observer API
		const observer = new IntersectionObserver((entries) => {
			if (entries[0].isIntersecting) {
				this.loadMoreResults();
			}
		});

		// Call observer through footer element. Each time the footer is in the viewport, the loadMoreResults() function is called
		observer.observe(document.querySelector('footer'));
	},

 	}
</script>

<style scoped>
header {
  border-top: 2px solid purple;
  border-bottom: 2px solid purple;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  padding-bottom: 1.5%;
  padding-top: 1.5%;
  margin-bottom: 2%;
}

.header__logo {
  width: 10%;
  height: 10%;
}

main {
  display: grid;
  grid-template-columns: 1fr 5fr;
}

aside {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-left: 5%;
  border: 2px solid purple;
  border-radius: 15px;
  background-color: white;
  height: max-content;
  padding: 10%;
}
/* CSS */
.toggleviewbutton {
  align-items: center;
  background-color: #ffffff;
  border: 1px solid rgba(0, 0, 0, 0.1);
  border-radius: 0.25rem;
  box-shadow: rgba(0, 0, 0, 0.02) 0 1px 3px 0;
  box-sizing: border-box;
  color: rgba(0, 0, 0, 0.85);
  cursor: pointer;
  display: inline-flex;
  font-family: system-ui, -apple-system, system-ui, 'Helvetica Neue', Helvetica, Arial, sans-serif;
  font-size: 16px;
  font-weight: 600;
  justify-content: center;
  line-height: 1.25;
  margin: 0;
  min-height: 3rem;
  padding: calc(0.875rem - 1px) calc(1.5rem - 1px);
  position: relative;
  text-decoration: none;
  transition: all 250ms;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  vertical-align: baseline;
  width: auto;
}

.toggleviewbutton:hover,
.toggleviewbutton:focus {
  border-color: rgba(0, 0, 0, 0.15);
  box-shadow: rgba(0, 0, 0, 0.1) 0 4px 12px;
  color: rgba(0, 0, 0, 0.65);
}

.toggleviewbutton:active {
  background-color: #f0f0f1;
  border-color: rgba(0, 0, 0, 0.15);
  box-shadow: rgba(0, 0, 0, 0.06) 0 2px 4px;
  color: rgba(0, 0, 0, 0.65);
  transform: translateY(0);
}

.titlefilters {
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
