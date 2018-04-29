<template>
    <div id="app">
        <div class="min-h-screen bg-grey-darker p-4">
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
                        <div class="tree bg-white p-4 rounded">
                            <tree
                                :data="items"
                                :filter="query"
                                ref="tree"
                            />
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import TreeRoot from '@/Tree/components/TreeRoot'
    import sourceData from "@/data";

    export default {
        components: {
            'tree': TreeRoot
        },
        data() {
            return {
                items: [
                    {text: 'Item 1'},
                    {
                        text: 'Item 2', state: {expanded: true}, children: [
                            {text: 'Item 2.1'},
                            {text: 'Item 2.2'},
                            {text: 'Item 2.3'}
                        ]
                    },
                    {text: 'Item 3', state: {selected: true}},
                    {text: 'Item 4'}
                ],
                query: '',
                options: {}
            }
        }
    };
</script>

<style lang="scss">
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

    .h-80 {
        height: 20rem;
    }

    .tree-node.matched > .tree-content {
        background: #f7f2e7;
    }

    .mt-055 {
        margin-top: 0.55rem;
    }
</style>
