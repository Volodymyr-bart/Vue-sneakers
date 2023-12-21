<script setup>
const props = defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickToFavorite: Function,
});
</script>
<template>
  <div
    class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition"
  >
    <img
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="like"
      class="absolute top-4 left-4"
      @click="props.onClickToFavorite"
    />
    <img :src="imageUrl" alt="Snekear" />
    <p class="mt-2">{{ title }}</p>
    <div class="mt-5 flex justify-between">
      <div class="flex flex-col">
        <span class="text-slate-400">Price:</span>
        <b>{{ price }}</b>
      </div>
      <img
        :src="!isAdded ? '/plus.svg' : '/checked.svg'"
        alt="Add"
        @click="props.onClickAdd"
      />
    </div>
  </div>
</template>
