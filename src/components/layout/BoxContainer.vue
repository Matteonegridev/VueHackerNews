<!--todo Inserire la chiamata in questa sezione, e creare il v-for per i box -->

<script setup lang="ts">
import NewsBox from "./NewsBox.vue";
import { ref } from "vue";
import axios from "axios";

const dataUrl = "https://hacker-news.firebaseio.com/v0/newstories.json";

// Items we need to create and parse data:
const newsStart = ref(0);
const newsLimit = ref(10);
const isLoading = ref(false);
const newsItems = ref<{ title: string; url: string; time: string }[]>([]);

const fetchData = async () => {
  if (isLoading.value) return;

  isLoading.value = true;

  try {
    //Fetch data, the response return an array of ids:
    const response = await axios.get(dataUrl);
    // Slice ids: we need 10 at times
    const getIds = response.data.slice(newsStart.value, newsStart.value + newsLimit.value);

    // Once we get the ten ids, we need to resolve them mapping the array that cointains them:
    const promises = getIds.map((ids: number[]) =>
      axios.get(`https://hacker-news.firebaseio.com/v0/item/${ids}.json`).then((res) => res.data)
    );
    // We resolve the promise:
    const resolvePromises = await axios.all(promises);

    // We push in newsItems the result which is a destructured object cointaining title,time and url. We spread the resolved array of promises so new are going to be added each time and map.
    const pushedItems = newsItems.value.push(
      ...resolvePromises.map((result: any) => ({
        title: result.title,
        url: result.url,
        time: new Date(result.time * 1000).toLocaleString(),
      }))
    );
    console.log("pushed Items:", pushedItems);

    newsStart.value += newsLimit.value;
  } catch (error) {
    console.log("Error:", error);
  } finally {
    isLoading.value = false;
  }
};

fetchData();
</script>

<template>
  <div v-for="(items, index) in newsItems" :key="index" class="container">
    <NewsBox :title="items.title" :url="items.url" :time="items.time" />
  </div>
  <button @click="fetchData" :disabled="isLoading">
    {{ isLoading ? "Loading..." : "Load More" }}
  </button>
</template>

<style lang="css" scoped>
.container {
  position: relative;
  box-shadow: 0 0 4px 2px rgba(138, 138, 231, 0.568);
  background-color: #5c6172;
  margin-block: 2rem;
  padding: 1rem;
}

.container > * + * {
  margin-top: 1.1em;
}
</style>
