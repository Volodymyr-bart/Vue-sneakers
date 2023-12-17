<script setup>
import DrawerHead from "./DrawerHead.vue";
import CardListBasket from "./CardListBasket.vue";
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8 flex flex-col">
    <DrawerHead />
    <CardListBasket />
    <div class="flex flex-col gap-4 mt-auto">
      <div class="flex gap-2">
        <span>Всього:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>12900</b>
      </div>

      <div class="flex gap-2">
        <span>Податок 5%:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>900</b>
      </div>
      <button
        disabled
        class="bg-lime-500 w-full rounded-xl py-4 text-white cursor-pointer transition disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700"
      >
        Замовити
      </button>
    </div>
  </div>
</template>
