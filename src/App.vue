<template>
  <div id="app">
    <div class="min-h-screen bg-grey-darker p-8">
      <div class="max-w-md mx-auto">
        <div class="mb-2 border-solid border-grey-light rounded border shadow-sm">
          <div class="bg-grey-lighter px-2 py-3 border-solid border-grey-light border-b">
            List of existing assets
          </div>
          <div class="p-3 bg-white p-4">
            <tree :tree="tree" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import sourceData from "@/data";
import Tree from "./components/Tree";

export default {
  components: {
    Tree
  },
  data() {
    return {
      tree: sourceData
    };
  }
};
</script>

<style>

</style>
