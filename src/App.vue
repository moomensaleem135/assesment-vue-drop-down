<template>
  <div
    class="min-h-screen bg-gray-100 flex flex-col items-center justify-center"
  >
    <h1 class="text-4xl font-bold mb-8">Welcome to Our Website</h1>

    <DropdownMenu
      :title="dropdownTitle"
      :items="dropdownItems"
      :multiple="true"
      @item-selected="handleItemSelected"
    />
  </div>
</template>

<script>
import DropdownMenu from "./components/DropdownMenu.vue";

export default {
  components: {
    DropdownMenu,
  },
  data() {
    return {
      dropdownTitle: "Select Items", // Title for the DropdownMenu
      dropdownItems: [
        { title: "Item 1", value: "1" },
        { title: "Item 2", value: "2" },
        { title: "Item 3", value: "3" },
      ],
    };
  },
  methods: {
    handleItemSelected(selectedItems) {
      console.log("Selected items in App.vue:", selectedItems);
    },
  },
};
</script>
