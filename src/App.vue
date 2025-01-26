<script setup lang="ts">
import AppBody from "./components/layout/AppBody.vue";
import BoxContainer from "./components/layout/BoxContainer.vue";
</script>

<template>
  <AppBody class="news-container">
    <div>
      <h1>TechNews</h1>
    </div>
    <BoxContainer />
  </AppBody>
</template>

<style lang="css" scoped>
.news-container {
  text-align: center;
  padding-inline: 2rem;
}
</style>
