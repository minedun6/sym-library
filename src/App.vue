<template>
    <div id="app">
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
                                   @input="searchForNode"
                            />
                        </span>
                    </div>
                    <div class="p-3 bg-white p-4">
                        <div class="tree bg-white p-4 rounded overflow-y-scroll h-80">
                            <transition-group name="list">
                                <div v-for="node in tree" :key="node.id">
                                    <tree-menu :node="node" :tree="node.children" :depth="0" @bus="bus"
                                               :selectedNode="selectedNode"></tree-menu>
                                </div>
                            </transition-group>
                        </div>
                    </div>
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
                selectedNode: null,
                query: ''
            };
        },
        methods: {
            bus: function (data) {
                this.selectedNode = data.id
            },
            findAll: function (data, query) {
                let results = [];

                if (!Array.isArray(data)) {
                    return;
                }

                data.forEach((node) => {
                    if (node.text.toLowerCase().includes(query.toLowerCase())) {
                        results.push(node);
                    }
                    results = Array.prototype.concat.apply(results,
                        this.findAll(node.children, query)
                    );
                });

                return results;
            },
            searchForNode: _.debounce(function () {
                console.log(this.findAll(this.tree, 'pdf'))
            }, 1000)
        }
    };
</script>

<style lang="scss">
    .h-80 {
        height: 20rem;
    }

    /* Custom styles for scrollbar */
    ::-webkit-scrollbar-track {
        -webkit-border-radius: 10px;
        border-radius: 10px;
        -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
    }

    ::-webkit-scrollbar {
        -webkit-appearance: none;
        width: 8px;
        height: 8px;
    }

    ::-webkit-scrollbar-track-piece {
        background-color: #d6dadc;
    }

    ::-webkit-scrollbar-thumb {
        border-radius: 4px;
        background-color: darkgrey;
        -webkit-box-shadow: 0 0 1px rgba(255, 255, 255, .5);
    }

    .outline-0 {
        outline: none;
    }

    .mt-055 {
        margin-top: 0.55rem;
    }

    .list-enter-active, .list-leave-active {
        transition: all .2s ease-in-out;
    }

    .list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */
    {
        opacity: 0;
        transform: translateX(-30px);
    }

    .far {
        -webkit-transition: all 0.5s;
        transition: all 0.5s;
    }
</style>
