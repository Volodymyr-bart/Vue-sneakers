<script setup>
import { inject } from "vue";
import CardBasket from "./CardBasket.vue";
const { basketItems, removeFromBasket } = inject("cart");
</script>
<template>
  <div class="flex flex-col gap-4">
    <CardBasket
      v-for="item in basketItems"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :remove-from-basket="() => removeFromBasket(item)"
    />
  </div>
</template>
