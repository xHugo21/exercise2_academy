<!--Component that defines a filter-->
<template>
    <label class="filterlabel">
      <slot></slot>
      <input class="checkboxfilter" type="checkbox" v-on:change="$emit('clickCheckbox', $event)" name="status-filter">
    </label>
  </template>
  
  <script lang="js">
    export default {
    };
  </script>
  
  <style scoped>
	

  </style>