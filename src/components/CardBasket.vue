<script setup>
defineProps({
  title: String,
  imageUrl: String,
  price: Number,
  isAdded: Boolean,
  removeFromBasket: Function,
});
</script>

<template>
  <div class="border border-slate-100 p-4 rounded-xl flex items-center gap-4">
    <img :src="imageUrl" :alt="title" class="w-16 h-16" />
    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between mt-2">
        <b class="flex-1">{{ price }}</b>
        <img
          @click="removeFromBasket"
          src="/close.svg"
          alt="Plus"
          class="opacity-40 hover:opacity-100 cursor-pointer transition"
        />
      </div>
    </div>
  </div>
</template>
