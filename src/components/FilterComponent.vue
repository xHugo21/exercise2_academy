<!--Component that defines a filter-->
<template>
    <label class="filter">
      <slot></slot>
      <input type="radio" v-on:change="$emit('clickCheckbox',$event)" name="status-filter">
    </label>
  </template>
  
  <script lang="js">
    export default {
    };
  </script>
  
  <style scoped>
  </style>