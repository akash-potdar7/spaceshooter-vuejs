<template>
  <GameArea msg="Welcome to Your Vue.js App"/>
</template>

<script>
import GameArea from './components/GameArea.vue';

export default {
  name: 'App',
  components: {
    GameArea
  }
};
</script>

<style>
  body {
    margin: 0;
    overflow: hidden;
    background-color: black;
  }
</style>