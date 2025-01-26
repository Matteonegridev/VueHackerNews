<script setup lang="ts">
defineProps<{
  title?: string;
  url?: string;
  time?: string;
}>();
</script>

<template>
  <h2>{{ title }}</h2>
  <a :href="url" target="_blank">Read More</a>
  <p>{{ time }}</p>
</template>
