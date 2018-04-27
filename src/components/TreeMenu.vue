<template>
  <div class="tree-menu-item">
    <div href="#" class="no-underline inline-flex hover:bg-blue-lighter p-2 rounded cursor-pointer" @click="bus(node)" :style="indent" :class="labelClasses(node)">
      <i class="far mr-1" :class="iconClasses" @click.prevent.stop="toggleChildren" v-if="isFolder(node)"></i> 
      <i class="mr-2" :class="node.icon" v-if="!isFolder(node)"></i>   
      <span class="node-text">{{ node.text }}</span>
    </div>
    <div v-for="node in node.children" :key="node.id">
      <transition-group name="list">
        <tree-menu :node="node" :tree="node.children" :depth="depth + 1" v-if="showChildren" @bus="bus" :selectedNode="selectedNode" :key="node.id"></tree-menu>
      </transition-group>
    </div>
  </div>
</template>

<script>
export default {
  props: ["node", "tree", "depth", "selectedNode"],
  name: "tree-menu",
  data() {
    return {
      showChildren: false,
    };
  },
  methods: {
    bus: function (data) {
      this.$emit('bus', data)
    },
    toggleChildren() {
      this.showChildren = !this.showChildren;
    },
    isFolder(node) {
      return node.type_id == '2';
    },
    labelClasses(node) {
      return [{ "has-children": this.node.children.length > 0 }, {'bg-blue-light p-2 rounded' : this.selectedNode && this.selectedNode == node.id}];
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
    }
  }
};
</script>

