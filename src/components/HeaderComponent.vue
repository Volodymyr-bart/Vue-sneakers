<script setup>
const emit = defineEmits(["openDrawer"]);
defineProps({ totalPrice: Number });
</script>

<template>
  <header class="flex justify-between border-b border-slate-300 px-10 py-8">
    <div class="flex items-center gap-4">
      <img src="/logo.png" alt="Logo" class="w-10" />
      <div>
        <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
        <p class="text-slate-400">Magazine the best sneakers</p>
      </div>
    </div>
    <ul class="flex items-center gap-10">
      <li
        class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer"
        @click="() => emit('openDrawer')"
      >
        <img src="/cart.svg" alt="cart" />
        <b>{{ totalPrice }} грн</b>
      </li>
      <li
        class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer"
      >
        <img src="/heart.svg" alt="heart" />
        <span>Закладки</span>
      </li>
      <li
        class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer"
      >
        <img src="/profile.svg" alt="heart" />
        <span>Профіль</span>
      </li>
    </ul>
  </header>
</template>
