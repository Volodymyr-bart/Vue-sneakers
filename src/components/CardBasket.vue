<script setup>
defineProps({
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function,
});
</script>

<template>
  <div class="border border-slate-100 p-4 rounded-xl flex items-center gap-4">
    <img :src="imageUrl" alt="Snekear" class="w-16 h-16" />
    <div class="flex flex-col justify-between">
      <p>{{ title }}</p>
      <div class="flex justify-between mt-2">
        <b>{{ price }}</b>
        <img
          src="/close.svg"
          alt="Plus"
          class="opacity-40 hover:opacity-100 cursor-pointer transition"
        />
      </div>
    </div>
  </div>
</template>
