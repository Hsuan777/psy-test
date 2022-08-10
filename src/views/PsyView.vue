<script setup>
import { ref, reactive } from "vue";

const groupScore = reactive([[], [], [], [], [], [], [], [], [], [], [], []]);

const topic = [
  ["有條理", "有創意", "獨立", "很有熱忱"],
  ["很守時", "重溝通", "好奇", "追求樂趣"],
  ["注重細節", "能靈活變通", "沉著", "有好勝心"],
  ["肯負責", "有愛心", "注重分析", "足智多謀"],
  ["能全心投入", "觀察敏銳", "深思熟慮", "有勇氣"],
  ["謹慎", "重合作", "熟技術", "精力旺盛"],
  ["勇於任事", "重感情", "自主", "有冒險精神"],
  ["受人尊重", "真心誠意", "有才幹", "寬大為懷"],
  ["表現沉穩", "能栽培後進", "愛追根究柢", "自動自發"],
  ["擬個計畫", "請教別人", "蒐集所有實據", "憑直覺"],
  ["教練", "團隊的一份子", "解決問題的人", "調解糾紛的人"],
  ["安定", "和諧", "隱私", "自由"],
];
const titleScore = ref([]);

const checkGroupScore = (num) => {
  let score = 0;
  groupScore[num].map((item) => {
    score += parseInt(item);
  });
  if (score !== 10) {
    alert(`題組 ${num + 1} 有重複選擇`);
    return false;
  } else {
    return true;
  }
};

const sumScore = (num) => {
  let score = 0;
  for (let x = 0; x < 12; x++) {
    score += parseInt(groupScore[x][num]);
  }
  return score;
};

const getScore = () => {
  let isPass = false;
  for (let x = 0; x < 12; x++){
    if (!checkGroupScore(x)) {
      isPass = false;
      break;
    } else {
      isPass = true;
    }
  }
  if (isPass) {
    for (let x = 0; x < 4; x++) {
      titleScore.value.push(sumScore(x));
    }
  }
};
const reset = (num) => {
  groupScore[num] = [];
};
</script>

<template>
  <form action="" class="p-5" @submit.prevent="getScore">
    <h2 class="mb-5">請判斷各列哪一個最像你，每行分數從 1 到 4 只能擇一</h2>
    <ul
      v-for="(item, groupIndex) in topic"
      :key="groupIndex"
      class="flex justify-between flex-wrap border-b pb-3 mb-3"
    >
      <li class="w-full">
        <h3 class="text-teal-500">題組 {{groupIndex + 1}}</h3>
      </li>
      <li
        v-for="(item, index) in topic[groupIndex]"
        :key="index + item"
        class="flex items-center mr-5 w-2/12"
      >
        <select
          class="mr-2 border rounded cursor-pointer"
          v-model="groupScore[groupIndex][index]"
        >
          <option hidden value="">請選擇</option>
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
          <option value="4">4</option>
        </select>
        <label class="form-check-label mb-0" for="flexRadioDefault1"
          >{{ item }}
        </label>
      </li>
      <li>
        <input
          type="button"
          value="Reset"
          class="border p-1 text-gray-500 rounded cursor-pointer"
          @click="reset(groupIndex)"
        />
      </li>
    </ul>
    <div class="flex items-center mt-5">
      <input
        type="submit"
        value="送出"
        class="border p-2 text-teal-500 border-teal-500 rounded cursor-pointer"
      />
      <p class="mx-5">您的分數</p>
      <ul class="flex">
        <li class="mr-5">A : {{ titleScore[0] }}</li>
        <li class="mr-5">B : {{ titleScore[1] }}</li>
        <li class="mr-5">C : {{ titleScore[2] }}</li>
        <li>D : {{ titleScore[3] }}</li>
      </ul>
    </div>
  </form>
</template>

<style>

</style>
