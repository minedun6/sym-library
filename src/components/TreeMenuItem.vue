<template>
  <div class="tree-menu-item">
    <div class="inline-flex hover:bg-blue-lighter p-2 rounded cursor-pointer" :style="indent" :class="labelClasses">
      <i class="far mr-1" :class="iconClasses" @click="toggleChildren" v-if="isFolder(node)"></i> 
      <i class="mr-2" :class="node.icon" v-if="!isFolder(node)"></i>   
      <span class="node-text">{{ node.text }}</span>
    </div>
    <div v-for="node in node.children" :key="node.id">
      <tree-menu-item :node="node" :tree="node.children" :depth="depth + 1" v-show="showChildren"></tree-menu-item>
    </div>
  </div>
</template>

<script>
export default {
  props: ["node", "tree", "depth"],
  name: "tree-menu-item",
  data() {
    return {
      showChildren: false
    };
  },
  methods: {
    toggleChildren() {
      this.showChildren = !this.showChildren;
    },
    isFolder(node) {
      return node.type_id == '2';
      
    }
  },
  computed: {
    indent() {
      return { "transform": `translate(${this.depth * 30}px)` };
    },
    iconClasses() {
      return {
        "fa-folder": !this.showChildren,
        "fa-folder-open": this.showChildren
      };
    },
    labelClasses() {
      return { "has-children": this.node.children.length > 0 };
    }
  }
};
</script>

<style lang="scss">
  .tree-menu-item {
    transition: display .5s ease-in-out;
  }
</style>
