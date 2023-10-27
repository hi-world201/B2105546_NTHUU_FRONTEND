<script>
import AppHeader from "@/components/AppHeader.vue";

export default {
  components: {
    AppHeader,
  },
};
</script>

<template>
  <div id="app">
    <app-header></app-header>

    <div class="container mt-3">
      <router-view></router-view>
    </div>
  </div>
</template>

<style>
#app {
  margin: auto;
}
</style>
