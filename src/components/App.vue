<script setup>
import { ref, watch } from "vue";
const name = ref("");

const profile = {
  name: "もころん",
  food: "グミ",
  hobby: "電車でお出かけ",
};

// 質問と回答（localStorageから読み込み or 初期値）
const savedAnswers = JSON.parse(localStorage.getItem("questions") || "null");
const questions = ref(
  savedAnswers || [
    { question: "好きな色は？", answer: "" },
    { question: "苦手な食べ物は？", answer: "" },
  ]
);
// nameが変更されたら保存
watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});
// questionsが変更されたら保存（deepオプションが必要！）
watch(
  questions,
  (newVal) => {
    localStorage.setItem("questions", JSON.stringify(newVal));
  },
  { deep: true }
);
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
  max-width: 600px;
  margin: 2rem auto;
  padding: 2rem;
  background: #e6f7ff;
  border: 4px dotted #ff9900;
  border-radius: 20px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  font-family: "Comic Sans MS", "Rounded Mplus 1c", sans-serif;
  color: #333;
}

.card input {
  display: block;
  width: 100%;
  padding: 0.6rem;
  margin: 0.5rem 0;
  border: 2px solid #99ccff;
  border-radius: 12px;
  background-color: #fff;
  font-size: 1rem;
  font-family: inherit;
}

.card input:focus {
  outline: none;
  border-color: #ff9900;
  box-shadow: 0 0 8px #ffcc66;
}

.card h2 {
  color: #0066cc;
  margin-top: 1.5rem;
  font-size: 1.4rem;
}

.card p {
  font-size: 1rem;
  margin: 0.4rem 0;
}

.card ul {
  list-style: none;
  padding: 0;
}

.card li {
  background: #fff8e1;
  margin: 0.6rem 0;
  padding: 0.8rem;
  border-radius: 12px;
  border: 2px solid #ffd699;
}
@media (max-width: 480px) {
  .card {
    padding: 1rem;
  }

  .card h2 {
    font-size: 1.2rem;
  }

  .card p,
  .card input {
    font-size: 0.95rem;
  }
}
</style>

<!-- Vueで「自己紹介と質問に答えるカード」を作ります。
名前と回答は ref でリアクティブに管理し、watch を使って localStorage に保存します。
初期表示時に localStorage に保存されていたデータがあれば読み込み、なければ初期値を使います。
テンプレートでは v-model でリアルタイム入力、v-for で質問をループ表示し、
回答が入力されると A: としてリストに表示されるようにするです！

-->
