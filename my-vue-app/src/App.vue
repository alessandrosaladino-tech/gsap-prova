<template>
  <div id="app">
    <imageGrid class="box_size" />
  </div>
</template>

<script>
import imageGrid from './components/imageGrid.vue';

export default {
  components: {
    imageGrid,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
 height: 100%;
}
.box_size {
height: 100vh;
}
</style>