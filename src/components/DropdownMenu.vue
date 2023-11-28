<template>
  <div class="relative">
    <button
      class="px-4 py-2 bg-blue-500 text-white rounded-md"
      @click="toggleDropdown"
    >
      {{ title }}
    </button>
    <div
      v-if="isOpen"
      class="absolute mt-2 w-64 rounded-md shadow-lg bg-white z-50 transition duration-300"
      ref="dropdown"
    >
      <div class="p-4">
        <h2 class="text-lg font-semibold mb-2">{{ title }}</h2>
        <ul>
          <li
            v-for="item in items"
            :key="item.value"
            class="px-4 py-2 hover:bg-gray-100 cursor-pointer"
            @click="toggleSelection(item)"
            :class="{ 'bg-blue-200': isSelected(item) }"
          >
            {{ item.title }}
            <span v-if="isSelected(item)" class="ml-2">(Selected)</span>
          </li>
        </ul>
        <button
          class="bg-blue-500 text-white px-4 py-2 rounded-md mt-4"
          @click="confirmSelection"
        >
          Confirm Selection
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted, reactive } from "vue";

export default {
  name: "DropdownMenu",
  props: {
    title: String,
    items: {
      type: Array,
      required: true,
    },
    multiple: Boolean,
  },
  setup(props, { emit }) {
    const isOpen = ref(false);
    const dropdownRef = ref(null);
    const selectedItems = reactive([]);

    const toggleDropdown = () => {
      isOpen.value = !isOpen.value;
    };

    const toggleSelection = (item) => {
      if (props.multiple) {
        const index = selectedItems.findIndex(
          (selected) => selected.value === item.value
        );
        if (index === -1) {
          selectedItems.push(item);
        } else {
          selectedItems.splice(index, 1);
        }
      } else {
        selectedItems.length = 0;
        selectedItems.push(item);
        isOpen.value = false;
      }
    };

    const isSelected = (item) => {
      return selectedItems.some((selected) => selected.value === item.value);
    };

    const confirmSelection = () => {
      emit("item-selected", selectedItems);
      isOpen.value = false;
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

    return {
      isOpen,
      toggleDropdown,
      toggleSelection,
      isSelected,
      confirmSelection,
      dropdownRef,
    };
  },
};
</script>
