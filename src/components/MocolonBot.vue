<!-- MocolonBot.vue -->
<script setup>
import { ref, onMounted } from "vue";

const userInput = ref("");
const messages = ref([]);

const knowledge = {
  "v-for": "リストをくるくる回すための文法だよ！",
  "v-if": "ある条件のときだけ表示するための文法だよ！",
  ref: "Vueで変化する値をリアルタイムで管理する箱みたいなものだよ！",
  computed: "自動で計算してくれるおりこうさんだよ！",
};

const handleSend = () => {
  if (!userInput.value) return;

  const keyword = Object.keys(knowledge).find((k) =>
    userInput.value.includes(k)
  );
  const answer = keyword
    ? knowledge[keyword]
    : "うーん、まだそれは勉強中なんだ…！";

  messages.value.push({
    question: userInput.value,
    answer,
  });

  userInput.value = "";
};
// ひとこと用の豆知識一覧
const tips = [
  "Vueのv-modelは双方向バインディングっていって、値のやり取りがらくらくなんだ〜！",
  "computedは、何かが変わったときだけ再計算される省エネさんだよ✨",
  "refはリアクティブな値をつくるときに使うんだよ〜！",
  "v-ifとv-show、見え方は似てるけど動きがちがうんだ！",
  "Vue 3では Composition API っていう新しい書き方があるよ📘",
];

const dailyTip = ref("");

// ページ表示時にひとことをセット
onMounted(() => {
  const random = Math.floor(Math.random() * tips.length);
  dailyTip.value = tips[random];
});
</script>

<template>
  <div class="chatbox">
    <h2>チャットボット🩵</h2>

    <!-- 🧠 今日のひとこと -->
    <div class="daily-tip">💬 もころんのひとこと：「{{ dailyTip }}」</div>

    <div class="messages">
      <div v-for="(msg, i) in messages" :key="i">
        <p>あなた：{{ msg.question }}</p>
        <p>もころん：{{ msg.answer }}</p>
      </div>
    </div>
    <div class="input-area">
      <input
        v-model="userInput"
        placeholder="Vueのこと聞いてね！"
        @keyup.enter="handleSend"
      />
      <button @click="handleSend">おしえて！</button>
    </div>
  </div>
</template>

<style scoped>
.chatbox {
  background-color: #fff3f7;
  border: 2px dashed #aad7ff;
  border-radius: 12px;
  padding: 2rem;
  margin: 2rem auto;
  font-family: "Arial Rounded MT", sans-serif;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  max-width: 600px;
}

.messages {
  margin-bottom: 1rem;
}

.input-area {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

input {
  flex: 1 1 70%;
  padding: 0.5rem;
  border-radius: 8px;
  border: 1px solid #ccc;
  font-size: 1rem;
}

button {
  flex: 1 1 25%;
  padding: 0.5rem 1rem;
  background-color: #5bc0ff;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1rem;
}

button:hover {
  background-color: #00aaff;
}

/* 📱 スマホなど小さい画面用 */
@media (max-width: 600px) {
  .chatbox {
    padding: 1rem;
  }

  .input-area {
    flex-direction: column;
  }

  input,
  button {
    width: 100%;
    font-size: 0.95rem;
  }
}
</style>

<!-- Vue でチャットボットを作る！
質問を入力すると、それに合った答えを表示してくれる仕組みです！

入力された質問は userInput に入り、送信されると messages に記録されます。
知っている言葉は knowledge という辞書で答えて、知らないときは「勉強中だよ」って返します。

さらに、ページを開いたときにはランダムでVueの豆知識を表示する dailyTip 機能も入れてあります。
どの変数も ref() でリアクティブにしていて、変化したら画面も変わるようになっています！-->
