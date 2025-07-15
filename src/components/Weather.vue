<script setup lang="ts">
import { ref, watch } from "vue";
import axios from "axios";

// 状態を管理する箱をつくる
const city = ref<string>("Tokyo"); // ✅ この書き方OK// 入力された都市名
const weather = ref<string>(""); // 天気情報
const errorMessage = ref<string>(""); // エラーメッセージ
const isLoading = ref<boolean>(false); // 読み込み中かどうか

// 都市名が変わったときにAPIを呼び出す
watch(city, async (newCity) => {
  if (!newCity.trim()) return;

  isLoading.value = true;
  weather.value = "";
  errorMessage.value = "";

  try {
    const response = await axios.get(`https://wttr.in/${newCity}?format=j1`);
    weather.value = response.data.current_condition[0].weatherDesc[0].value;
  } catch (error) {
    errorMessage.value = "天気情報の取得に失敗しました。";
  } finally {
    isLoading.value = false;
  }
});
</script>

<template>
  <input v-model="city" placeholder="都市名を入力" />

  <p v-if="isLoading">データ取得中...</p>
  <p v-if="errorMessage">{{ errorMessage }}</p>
  <p v-else-if="weather">天気: {{ weather }}</p>
</template>
