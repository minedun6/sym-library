<template>
  <div id="app">
    <div class="min-h-screen bg-grey-darker p-8">
      <div class="max-w-md mx-auto">
        <div class="tree bg-white p-4 rounded">
          <transition-group name="list">
            <div v-for="node in tree" :key="node.id">
              <tree-menu :node="node" :tree="node.children" :depth="0" @bus="bus" :selectedNode="selectedNode"></tree-menu>
            </div>
          </transition-group>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import _ from 'lodash'
  import sourceData from "@/data";
  import TreeMenu from "./components/TreeMenu";

  export default {
    components: {
      TreeMenu
    },
    data() {
      return {
        tree: sourceData,
        selectedNode: null
      };
    },
    methods: {
      bus: function (data) {
        this.selectedNode = data.id
      } 
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

  .list-enter-active, .list-leave-active {
    transition: all .2s ease-in-out;
  }
  .list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
    opacity: 0;
    transform: translateX(-30px);
  }
  .far {
    -webkit-transition: all 0.5s;
    transition: all 0.5s;
  }
</style>
