<script setup>
import CardItem from "./CardItem.vue";
defineProps({
  items: Array,
});

const emit = defineEmits(["addToFavorite", "addToBasket"]);
</script>
<template>
  <slot></slot>
  <div class="grid grid-cols-4 gap-5 mt-4">
    <CardItem
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :isAdded="item.isAdded"
      :isFavorite="item.isFavorite"
      :onClickAdd="() => emit('addToBasket', item)"
      :onClickToFavorite="() => emit('addToFavorite', item)"
    />
  </div>
</template>
