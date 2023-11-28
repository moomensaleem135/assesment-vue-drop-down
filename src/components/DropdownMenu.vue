<template>
  <div class="relative">
    <button
      class="px-4 py-2 bg-blue-500 text-white rounded-md"
      @click="toggleDropdown"
    >
      Toggle Dropdown
    </button>
    <div
      v-if="isOpen"
      class="absolute mt-2 w-64 rounded-md shadow-lg bg-white z-50 transition duration-300"
      ref="dropdown"
    >
      <div class="p-4">
        <h2 class="text-lg font-semibold mb-2">Select an Item</h2>
        <ul>
          <li
            v-for="item in items"
            :key="item.value"
            class="px-4 py-2 hover:bg-gray-100 cursor-pointer"
            @click="selectItem(item)"
          >
            {{ item.title }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted } from "vue";

export default {
  name: "DropdownMenu",
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
  setup(props, { emit }) {
    const isOpen = ref(false);
    const dropdownRef = ref(null);

    const toggleDropdown = () => {
      isOpen.value = !isOpen.value;
    };

    const selectItem = (item) => {
      // Handle item selection logic here
      console.log(`Selected item: ${item.title}`);

      // Emit an event to notify the parent component about the selected item
      emit("item-selected", item);
    };

    const onClickOutside = (event) => {
      if (
        isOpen.value &&
        dropdownRef.value &&
        !dropdownRef.value.contains(event.target)
      ) {
        isOpen.value = false;
      }
    };

    onMounted(() => {
      window.addEventListener("click", onClickOutside);
    });

    onUnmounted(() => {
      window.removeEventListener("click", onClickOutside);
    });

    return { isOpen, toggleDropdown, selectItem, dropdownRef };
  },
};
</script>

<style scoped>
/* You can add custom TailwindCSS styles if needed */
</style>
