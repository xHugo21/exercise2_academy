<!--Component that defines each character card extracted from the Rick and Morty API-->
<template>
    <article class="character">
      <img class="character__image" v-bind:alt="'character image of'+character.name" v-bind:src="character.image">
      <div class="character__description"> {{ character.name }}
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

  </style>