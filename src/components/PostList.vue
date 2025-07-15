<script setup>
import { ref, onMounted } from "vue";

const posts = ref([]);

onMounted(async () => {
  const res = await fetch("http://admin.local/wp-json/wp/v2/posts"); // ✅ 修正
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

<!-- 「私はまず ref([]) で記事を入れる箱を作っておきます。
そして onMounted の中で WordPress のAPIにアクセスして記事を取り出し、
そのデータを posts.value にセットしています。
表示のときには v-for を使って posts の中を1つずつ post として取り出し、
post.title.rendered を表示し、post.link を使ってリンクもつけています。」-->
