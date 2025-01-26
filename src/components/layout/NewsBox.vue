<script setup lang="ts">
defineProps<{
  title?: string;
  url?: string;
  time?: string;
}>();
</script>

<template>
  <h2 class="title">{{ title }}</h2>
  <a class="links" :href="url" target="_blank">Read More</a>
  <p class="date">{{ time }}</p>
</template>

<style scoped>
@import "../../style.css";

.title {
  font-size: var(--step-1);
}

.links {
  font-size: var(--step--0);
  display: block;
  padding-top: 3rem;
}

.date {
  position: absolute;
  font-size: var(--step--4);
  bottom: 2px;
  right: 15px;
}
</style>
