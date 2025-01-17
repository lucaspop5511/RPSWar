<template>
  <div id="app" class="app">
    <GameScreen />
  </div>
</template>

<script>
import GameScreen from './components/GameScreen.vue';

export default {
  name: 'App',
  components: {
    GameScreen,
  },
};
</script>

<style>
.app {
  text-align: center;
  background-color: #303030;
  color: #fff;
  font-family: Arial, sans-serif;
  padding: 0;
  margin: 0;
}
</style>