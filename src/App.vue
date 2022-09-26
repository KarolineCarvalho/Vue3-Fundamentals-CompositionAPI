<script setup>
import { ref, computed } from "vue";

const header = ref("Shopping List App");
const editing = ref(false);
const doEdit = (edit) => {
  editing.value = edit;
  newItem.value = "";
  newItemHighPriority.value = "";
};
const items = ref([
  { id: 1, label: "10 party hats", purchased: false, highPriority: true },
  { id: 2, label: "2 board games", purchased: false, highPriority: false },
  { id: 3, label: "20 cups", purchased: true, highPriority: true },
  { id: 4, label: "3 pizzas", purchased: true, highPriority: false },
]);
const newItem = ref("");
const newItemHighPriority = ref(false);
const saveItem = () => {
  items.value.push({
    id: items.value.length + 1,
    label: newItem.value,
    highPriority: newItemHighPriority.value,
  });
  newItem.value = "";
  newItemHighPriority.value = "";
};

const togglerPurchased = (item) => {
  item.purchased = !item.purchased;
};
const reversedItems = computed(() => {
  return [...items.value].reverse();
});
const characterCount = computed(() => {
  return newItem.value.length;
});
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>

    <button v-if="editing" class="btn" @click="doEdit(false)">Cancel</button>

    <button v-else class="btn btn-primary" @click="doEdit(true)">
      Add Item
    </button>
  </div>

  <form v-on:submit.prevent="saveItem" class="add-item-form" v-if="editing">
    <div>
      <input
        v-model.trim="newItem"
        :maxlength="100"
        type="text"
        placeholder="Add an item"
      />

      <label class="highPriority">
        <input type="checkbox" v-model="newItemHighPriority" />
        High Priority
      </label>
    </div>
    <p class="counter">{{ characterCount }}/100</p>

    <button v-bind:disabled="newItem.length < 2" class="btn btn-primary">
      Save Item
    </button>
  </form>

  <ul>
    <li
      v-for="item in reversedItems"
      v-on:click="togglerPurchased(item)"
      :key="item.id"
      :class="{ strikeout: item.purchased, priority: item.highPriority }"
      class="listItem"
    >
      {{ item.label }}
    </li>
  </ul>

  <p v-if="!items.length">Nothing to see here.</p>
</template>

<style>
@import "./assets/App.css";
</style>
