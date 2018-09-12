<template>
  <div id="app">
    <section class="section">
      <div class="container">
        <pokedex />
      </div>
    </section>
  </div>
</template>

<script>


import Pokedex from "./components/Pokedex";

export default {
    components: { Pokedex },
    name: 'app',
}
</script>

<style lang="scss">
  @import '../node_modules/bulma/css/bulma.css';
  @import './assets/sass/main.scss';
</style>
