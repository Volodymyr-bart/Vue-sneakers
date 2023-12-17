<script setup>
import CardBasket from "./CardBasket.vue";
</script>
<template>
  <div class="flex flex-col gap-4">
    <CardBasket
      title="Information about"
      imageUrl="/sneakers/sneakers-1.jpg"
      :price="5000"
      :isAdded="false"
      :isFavorite="false"
      :onClickAdd="onClickAdd"
      :onClickFavorite="onClickFavorite"
    />
    <CardBasket
      title="Information about"
      imageUrl="/sneakers/sneakers-1.jpg"
      :price="5000"
      :isAdded="false"
      :isFavorite="false"
      :onClickAdd="onClickAdd"
      :onClickFavorite="onClickFavorite"
    />
  </div>
</template>
