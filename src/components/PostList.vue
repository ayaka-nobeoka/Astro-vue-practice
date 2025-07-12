<script setup>
import { ref, onMounted } from "vue";

const posts = ref([]);

onMounted(async () => {
  const res = await fetch("https://techcrunch.com/wp-json/wp/v2/posts"); // ✅ 修正
  const data = await res.json();
  console.log("取得した記事", data);
  posts.value = data;
});
</script>

<template>
  <div>
    <h2>記事一覧</h2>
    <ul>
      <li v-for="post in posts" :key="post.id">
        <a :href="post.link" target="_blank">{{ post.title.rendered }}</a>
      </li>
    </ul>
  </div>
</template>
