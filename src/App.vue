<script setup>
import { onMounted, reactive, ref, watch } from "vue";
import HeaderComponent from "./components/HeaderComponent.vue";
import CardList from "./components/CardList.vue";
import Drawer from "./components/Drawer.vue";
import axios from "axios";
const items = ref([]);

const filters = reactive({ sortBy: "title", searchQuery: "" });

const onChangeSelect = (e) => {
  filters.sortBy = e.target.value;
};
const onChangeInput = (e) => {
  filters.searchQuery = e.target.value;
};

const fetchItems = async () => {
  try {
    const params = { sortBy: filters.sortBy };
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`;
    }
    const { data } = await axios.get(
      "https://604781a0efa572c1.mokky.dev/items",
      { params }
    );
    items.value = data;
  } catch (error) {
    console.log(error);
  }
};

onMounted(fetchItems);
watch(filters, fetchItems);
</script>

<template>
  <!-- <Drawer /> -->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <HeaderComponent />
    <div class="p-10">
      <CardList :items="items">
        <div class="flex justify-between items-center">
          <h2 class="mb-8 text-3xl">Всі кросівки</h2>
          <div class="flex gap-4">
            <select
              @change="onChangeSelect"
              name=""
              id=""
              class="py-2 px-3 border rounded-md outline-none"
            >
              <option value="name">По назві</option>
              <option value="price">Дешевші</option>
              <option value="-price">Дорожчі</option>
            </select>
            <div class="relative">
              <img
                src="/search.svg"
                alt="search"
                class="absolute top-3 left-4"
              />
              <input
                @input="onChangeInput"
                type="text"
                class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
                placeholder="Пошук..."
              />
            </div>
          </div>
        </div>
      </CardList>
    </div>
  </div>
</template>

<style scoped></style>
