<script setup lang="ts">
import { Ref, ref } from 'vue';

const textValue: Ref<string> = ref('');
const copied: Ref<boolean> = ref(false);

const handleCopy = (): void => {
  navigator.clipboard.writeText(textValue.value).then(() => {
    copied.value = true;
    setTimeout(() => copied.value = false, 2000);
  });
};

const handleDelete = (): void => {
  textValue.value = '';
};
</script>

<template>
  <div class="input-container">
    <p class="input-container__title">中文剪切板</p>
    <div class="input-group">
      <input class="input-container__textbox" v-model="textValue" type="text" placeholder="在这里输入中文" />
      <button class="input-container__button input-container__button--copy"
        :class="{ 'input-container__button--copied': copied }" @click="handleCopy">
        <img src="../assets/copy-icon.svg" alt="复制" class="input-container__icon" />
      </button>
      <button class="input-container__button input-container__button--delete" @click="handleDelete">
        <img src="../assets/delete-icon.svg" alt="删除" class="input-container__icon" />
      </button>
    </div>
  </div>
</template>

<style scoped>
div.input-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  position: relative;
  gap: 10px;
}

div.input-group {
  display: flex;
  align-items: center;
}

p.input-container__title {
  margin: 0;
  font-size: 18px;
  text-align: center;
}

input[type="text"].input-container__textbox {
  height: 35px;
  font-size: 18px;
  border: 1px solid gray;
  border-radius: 5px 0 0 5px;
  width: 200px;
  padding: 0 10px;
  margin-right: 0;
  box-sizing: border-box;
}

button.input-container__button {
  height: 35px;
  font-size: 16px;
  border: 1px solid gray;
  padding: 0 10px;
  cursor: pointer;
  transition: background-color 0.2s, transform 0.1s;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: transparent;
}

button.input-container__button:hover {
  background-color: #cdcdcd;
}

button.input-container__button:active {
  transform: scale(0.9);
}

button.input-container__button--copy {
  border-radius: 0;
}

button.input-container__button--copied {
  background-color: #4CAF50;
  color: white;
}

button.input-container__button--delete {
  border-radius: 0 5px 5px 0;
}

button.input-container__button--delete:hover {
  background-color: #f8d7da;
}

img.input-container__icon {
  width: 18px;
  height: 18px;
}
</style>
