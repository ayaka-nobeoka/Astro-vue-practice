<script setup>
import { ref } from "vue";
const name = ref("");

const profile = {
  name: "あやか",
  food: "グミ",
  hobby: "読書",
};

const questions = ref([
  { question: "好きな色は？", answer: "" },
  { question: "苦手な食べ物は？", answer: "" },
]);
</script>

<template>
  <div class="card">
    <input v-model="name" placeholder="名前を入力してね" />
    <p>こんにちは、{{ name }}さん！</p>
    <div>
      <h2>{{ profile.name }}のプロフィール</h2>
      <p>好きな食べ物：{{ profile.food }}</p>
      <p>趣味：{{ profile.hobby }}</p>
    </div>
    <div v-for="(q, i) in questions" :key="i">
      <p>Q: {{ q.question }}</p>
      <input v-model="q.answer" placeholder="ここに答えてね" />
    </div>
    <ul>
      <li v-for="(q, i) in questions" :key="i">
        <p>Q: {{ q.question }}</p>
        <p v-if="q.answer">A: {{ q.answer }}</p>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.card {
  border: 1px solid #ccc;
  padding: 1rem;
  margin: 1rem 0;
  border-radius: 8px;
}
</style>
