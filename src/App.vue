<template>
  <!-- Add a background color to the entire page -->
  <div
    class="min-h-screen bg-gray-100 flex flex-col items-center justify-center"
  >
    <!-- Add a heading with larger font size and bold text -->
    <h1 class="text-4xl font-bold mb-8">Welcome to Our Website</h1>

    <!-- Include the DropdownMenu Component -->
    <DropdownMenu :items="dropdownItems" @item-selected="handleItemSelected" />
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
      dropdownItems: [
        { title: "Item 1", value: "1" },
        { title: "Item 2", value: "2" },
        // Add more items as needed
      ],
    };
  },
  methods: {
    handleItemSelected(item) {
      // Handle item selection logic here
      console.log(`Selected item in App.vue: ${item.title}`);
    },
  },
};
</script>

<style>
/* You can add global styles here if needed */
</style>
