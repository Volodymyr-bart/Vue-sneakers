<script setup>
import CardItem from "./CardItem.vue";
defineProps({ items: Array });

const onClickAdd = () => {
  console.log("click");
};
const onClickFavorite = () => {
  console.log("click 1");
};
</script>
<template>
  <slot></slot>
  <div class="grid grid-cols-4 gap-5 mt-4">
    <CardItem
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :onClickAdd="onClickAdd"
      :onClickFavorite="onClickFavorite"
    />
  </div>
</template>
