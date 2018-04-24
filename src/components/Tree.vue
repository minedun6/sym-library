<template>
  <div class="min-h-screen bg-grey-darker p-8">
    <div class="max-w-md mx-auto">
      <div class="mb-2 border-solid border-grey-light rounded border shadow-sm">
        <div
          class="flex justify-between items-center bg-grey-lighter px-2 py-3 border-solid border-grey-light border-b">
          <span>List of existing assets</span>
          <span class="relative">
              <i class="absolute pin-r pin-t mt-055 mr-3 fas fa-search"></i>
              <input type="text"
                     class="border border-solid border-grey-light bg-grey-white leading-normal p-1 pr-8 rounded outline-0 shadow-inner"
                     title=""
                     v-model="query"
              />
            </span>
        </div>
        <div class="p-3 bg-white p-4">
          <div class="tree">
            <div v-for="node in tree" :key="node.id">
              <tree-menu-item :node="node" :tree="node.children" :depth="0" :query="query"></tree-menu-item>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import TreeMenuItem from '../components/TreeMenuItem'

  export default {
    name: 'tree',
    props: ['tree'],
    data() {
      return {
        query: ''
      }
    },
    components: {
      TreeMenuItem
    },
    methods: {

    }
  }
</script>

<style>
  .tree {
    transition: all .5s ease;
  }
</style>
