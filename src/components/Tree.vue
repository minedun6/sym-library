<template>
  <div class="tree">
    <div v-for="node in tree" :key="node.id">
      <tree-menu-item :node="node" :tree="node.children" :depth="0"></tree-menu-item>
    </div>
  </div>
</template>

<script>
  import TreeMenuItem from '../components/TreeMenuItem' 
  export default { 
    name: 'tree',
    props: ['tree'],
    components: {
      TreeMenuItem
    }
  }
</script>

<style>
  .tree {
    transition: all .5s ease;
  }
</style>