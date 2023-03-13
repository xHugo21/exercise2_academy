<!--Component that defines the section where the characters cards are placed-->
<template>
  <section class="charactersgrid">
    <slot></slot>
  </section>
</template>

<script lang="js">
export default{
}
</script>

<style scoped>
.charactersgrid {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  margin-left: 5%;
  margin-right: 5%;
  flex-wrap: wrap;
  gap: 15px;
}
</style>
