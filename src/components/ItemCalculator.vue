<script setup lang="ts">
import { Directive, DirectiveBinding, Ref, ref } from "vue";

const totalItemCount: Ref<number> = ref<number>(0);

type StackedItem = {
  stacks: number;
  remainingItems: number;
};

const stackedItemCount: Ref<StackedItem> = ref<StackedItem>({
  stacks: 0,
  remainingItems: 0
});

// v 开头的小驼峰命名函数才能作为指令使用
const vHandelNumber: Directive = {
  mounted(el: HTMLInputElement, binding: DirectiveBinding<{ max: number | undefined }>) {
    el.addEventListener("input", (e: Event) => {
      const target: HTMLInputElement = e.target as HTMLInputElement;
      let value: string = target.value.replace(/[^0-9]/g, "");

      if (value === "") {
        value = "0";
      } else {
        value = value.replace(/^0+/, "") || "0";
      }

      if (binding.value?.max !== undefined) {
        const maxValue = binding.value.max;
        const numValue = parseInt(value, 10);
        if (numValue > maxValue) {
          value = maxValue.toString();
        }
      }

      if (target.value !== value) {
        target.value = value;
        target.dispatchEvent(new Event("input"));
      }
    });
  }
};


// 纯前端解决逻辑
const convertToStackedItem = () => {
  const total: number = totalItemCount.value;
  const stacks: number = Math.floor(total / 64);
  const remainingItems: number = total % 64;
  stackedItemCount.value = { stacks, remainingItems };
};

const convertToItemCount = () => {
  const stacks: number = stackedItemCount.value.stacks;
  const remainingItems: number = stackedItemCount.value.remainingItems;
  totalItemCount.value = stacks * 64 + remainingItems;
};

// 调试用
// watchEffect(() => {
//   console.log("totalItemCount:", totalItemCount.value);
//   console.log("stackedItemCount:", stackedItemCount.value);
// });

</script>

<template>
  <div class="item-calculator">
    <p class="item-calculator__title">物品数量换算</p>
    <div class="item-calculator__inputs">
      <div class="item-calculator__input-group">
        <input type="text" class="item-calculator__input-field" v-model.number="totalItemCount"
          @input="convertToStackedItem" title="物品总数" v-handel-number />
        <p class="item-calculator__unit">个</p>
      </div>

      <div class="item-calculator__input-group">
        <input type="text" class="item-calculator__input-field" v-model.number="stackedItemCount.stacks"
          @input="convertToItemCount" title="组数" v-handel-number />
        <p class="item-calculator__unit">组</p>
        <p class="item-calculator__operator">+</p>
        <input type="text" class="item-calculator__input-field" v-model.number="stackedItemCount.remainingItems"
          @input="convertToItemCount" title="剩余物品数（小于 64）" v-handel-number="{ max: 63 }" />
        <p class="item-calculator__unit">个</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
div.item-calculator {
  width: 100%;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 15px;
}

p.item-calculator__title {
  margin: 0;
  font-size: 18px;
  text-align: center;
}

div.item-calculator__inputs {
  width: 100%;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 5px;
}

div.item-calculator__input-group {
  display: flex;
  position: relative;
  flex-direction: row;
  margin-left: 20px;
}

input[type="text"].item-calculator__input-field {
  width: 100px;
}

p.item-calculator__unit {
  margin-left: 2px;
  font-size: 14px;
}

p.item-calculator__operator {
  margin-left: 10px;
  margin-right: 10px;
}
</style>