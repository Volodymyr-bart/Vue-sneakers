<script setup>
import { computed, onMounted, provide, reactive, ref, watch } from "vue";
import HeaderComponent from "./components/HeaderComponent.vue";
import CardList from "./components/CardList.vue";
import Drawer from "./components/Drawer.vue";
import axios from "axios";
//
const items = ref([]);
const isCreatingOrder = ref(false);
const filters = reactive({ sortBy: "title", searchQuery: "" });
// Drawer
const drawerOpen = ref(false);
const openDrawer = () => {
  drawerOpen.value = true;
};
const closeDrawer = () => {
  drawerOpen.value = false;
};
// Search
const onChangeSelect = (e) => {
  filters.sortBy = e.target.value;
};
const onChangeInput = (e) => {
  filters.searchQuery = e.target.value;
};

// Fetch
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
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false,
    }));
  } catch (error) {
    console.log(error);
  }
};
// Favorite
const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      "https://604781a0efa572c1.mokky.dev/favorites"
    );
    items.value = items.value.map((item) => {
      const favorite = favorites.find(
        (favorite) => favorite.parentId === item.id
      );
      if (!favorite) {
        return item;
      }
      return { ...item, isFavorite: true, favoriteId: favorite.id };
    });
  } catch (error) {
    console.log(error);
  }
};

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = { parentId: item.id };
      item.isFavorite = true;
      const { data } = await axios.post(
        "https://604781a0efa572c1.mokky.dev/favorites",
        obj
      );
      item.favoriteId = data.id;
    } else {
      item.isFavorite = false;
      const { data } = await axios.delete(
        `https://604781a0efa572c1.mokky.dev/favorites/${item.favoriteId}`
      );
      item.favoriteId = null;
    }
  } catch (error) {
    console.log(error);
  }
};
// Basket
const basketItems = ref([]);
const addToBasket = (item) => {
  basketItems.value.push(item);
  item.isAdded = true;
};

const removeFromBasket = (item) => {
  basketItems.value.splice(basketItems.value.indexOf(item), 1);
  item.isAdded = false;
};
const onClickAddToPlus = (item) => {
  if (!item.isAdded) {
    addToBasket(item);
  } else {
    removeFromBasket(item);
  }
};

const totalPrice = computed(() =>
  basketItems.value.reduce((acc, item) => acc + item.price, 0)
);
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100));
const basketIsEmpty = computed(() => basketItems.value.length === 0);

const basketButtonDisabled = computed(() => {
  return basketIsEmpty.value || isCreatingOrder.value;
});

// orders
const createOrder = async () => {
  try {
    isCreatingOrder.value = true;

    const { data } = await axios.post(
      "https://604781a0efa572c1.mokky.dev/orders",
      {
        items: basketItems.value,
        totalPrice: totalPrice.value,
      }
    );
    basketItems.value = [];
    return data;
  } catch (error) {
    console.log(error);
  } finally {
    isCreatingOrder.value = false;
  }
};
// Hooks
onMounted(async () => {
  await fetchItems();
  await fetchFavorites();
});
watch(filters, fetchItems);
watch(
  basketItems,
  () => {
    items.value = items.value.map((item) => ({
      ...item,
      isAdded: false,
    }));
  },
  { deep: true }
);
provide("cart", {
  basketItems,
  addToBasket,
  removeFromBasket,
  openDrawer,
  closeDrawer,
});
</script>

<template>
  <Drawer
    v-if="drawerOpen"
    :total-price="totalPrice"
    :vat-price="vatPrice"
    @create-order="createOrder"
    :is-creating-order="isCreatingOrder"
    :basketButtonDisabled="basketButtonDisabled"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <HeaderComponent @open-drawer="openDrawer" :total-price="totalPrice" />
    <div class="p-10">
      <CardList
        :items="items"
        @add-to-favorite="addToFavorite"
        @add-to-basket="onClickAddToPlus"
      >
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
