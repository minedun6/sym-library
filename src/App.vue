<template>
  <div id="app">
    <tree :tree="tree"></tree>
  </div>
</template>

<script>
  import _ from 'lodash'
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

<style lang="scss">
  .outline-0 {
    outline: none;
  }

  .mt-055 {
    margin-top: 0.55rem;
  }
</style>
