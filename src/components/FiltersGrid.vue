<!--Component that defines the section where a filter form is placed-->
<template>
    <ul class="filters">
      <li v-for="filter in filters" v-bind:key="filter">
        <slot :filter="filter"></slot>
      </li>
    </ul>
  </template>
  
  <script lang="js">
    export default {
      props: ['filters']
    };
  </script>
  
  <style scoped>

  </style>