<template>
    <div id="app">
        <div class="min-h-screen bg-grey-darker p-4">
            <div class="max-w-lg mx-auto">
                <div class="mb-2 border-solid border-grey-light rounded border shadow-sm">
                    <div
                        class="flex justify-between items-center bg-grey-lighter px-2 py-3 border-solid border-grey-light border-b">
                        <span>Modules and permissions</span>
                        <span class="relative">
                            <i class="absolute pin-r pin-t mt-055 mr-3 fas fa-search"></i>
                            <input type="text"
                                   class="border border-solid border-grey-light bg-grey-white leading-normal p-1 pl-2 pr-8 rounded outline-0 shadow-inner"
                                   title=""
                                   v-model="query"
                            />
                        </span>
                    </div>
                    <div class="p-3 bg-white p-4 shadow">
                        <div class="tree bg-white p-4 rounded">
                            <tree
                                :data="items"
                                :filter="query"
                                ref="tree"
                                @node:selected="onSelectedNode"
                            >
                                <span class="tree-text" slot-scope="{ node }">
                                    <template v-if="!node.hasChildren()">
                                        <i :class="node.data.icon"></i>
                                        {{ node.text }}
                                    </template>

                                    <template v-else>
                                        <i :class="[node.expanded() ? 'far fa-folder-open' : 'far fa-folder']"></i>
                                        {{ node.text }}
                                    </template>
                                </span>
                            </tree>
                            <div class="form-group mb-4" :class="{'text-red' : errors.has('firstName')}">
                                <label for="first-name" class="inline-flex mb-2">First Name</label>
                                <input type="text"
                                       v-validate="'required'"
                                       v-model="form.firstName"
                                       name="firstName"
                                       id="first-name"
                                       :class="{'border border-red' : errors.has('firstName')}"
                                       class="w-full mb-2 relative outline-0 flex-shrink flex-grow flex-auto leading-normal w-px flex-1 border h-10 border-grey-light rounded rounded-l-none px-3 relative focus:border-blue focus:shadow"
                                       placeholder="First Name"/>
                                <span v-show="errors.has('firstName')">{{ errors.first('firstName') }}</span>
                            </div>
                            <div class="form-group mb-4" :class="{'text-red' : errors.has('lastName')}">
                                <label for="last-name" class="inline-flex mb-2">Last Name</label>
                                <input type="text"
                                       v-validate="'required'"
                                       v-model="form.lastName"
                                       name="lastName"
                                       id="last-name"
                                       :class="{'border border-red' : errors.has('lastName')}"
                                       class="w-full mb-2 relative outline-0 flex-shrink flex-grow flex-auto leading-normal w-px flex-1 border h-10 border-grey-light rounded rounded-l-none px-3 relative focus:border-blue focus:shadow"
                                       placeholder="Last Name"/>
                                <span v-show="errors.has('lastName')">{{ errors.first('lastName') }}</span>
                            </div>
                            <div class="form-group mb-4" :class="{'text-red' : errors.has('email')}">
                                <label for="email" class="inline-flex mb-2">Email Address</label>
                                <input type="text"
                                       v-validate="'required|email'"
                                       v-model="form.email"
                                       name="email"
                                       id="email"
                                       :class="{'border border-red' : errors.has('email')}"
                                       class="w-full mb-4 relative outline-0 flex-shrink flex-grow flex-auto leading-normal w-px flex-1 border h-10 border-grey-light rounded rounded-l-none px-3 relative focus:border-blue focus:shadow"
                                       placeholder="Email Address"/>
                                <span v-show="errors.has('email')">{{ errors.first('email') }}</span>
                            </div>
                            <custom-check
                                class="mb-2"
                                v-for="(module, index) in form.modules" :key="index"
                                :text="module.name"
                                @input="toggleModulePermissions(module)"
                                v-model="module.checked">
                                <div class="permissions mt-2 ml-6">
                                    <custom-check
                                        class="mb-2"
                                        v-for="(permission, i) in module.permissions" :key="i"
                                        :text="permission.pretty"
                                        @input="togglePermission(module, permission)"
                                        v-model="permission.checked">
                                    </custom-check>
                                </div>
                            </custom-check>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import TreeRoot from '@/Tree/components/TreeRoot'
    import uuidV4 from "@/Tree/utils/uuidV4"
    import Chec from '@/components/Check'

    function fetchData() {
        return fetch('/static/data.json').then(r => r.json()).then(items => {

            items.forEach(setData)

            function setData(item) {
                item.data = item

                if (item.children) {
                    item.children.forEach(setData)
                }
            }

            return items
        })
    }

    export default {
        components: {
            'tree': TreeRoot,
            'custom-check': Chec
        },
        data() {
            return {
                items: fetchData(),
                query: '',
                options: {},
                form: {
                    firstName: '',
                    lastName: '',
                    email: '',
                    modules: [
                        {
                            "id": uuidV4(),
                            "name": "Briefcases",
                            "checked": true,
                            "permissions": [
                                {
                                    "id": uuidV4(),
                                    "name": "create",
                                    "pretty": "Create",
                                    "checked": false
                                },
                                {
                                    "id": uuidV4(),
                                    "name": "edit",
                                    "pretty": "Edit",
                                    "checked": false
                                },
                                {
                                    "id": uuidV4(),
                                    "name": "delete",
                                    "pretty": "Delete",
                                    "checked": false
                                }
                            ]
                        },
                        {
                            "id": uuidV4(),
                            "name": "Regulatory",
                            "checked": false,
                            "permissions": [
                                {
                                    "id": uuidV4(),
                                    "name": "send-documents",
                                    "pretty": "Send documents",
                                    "checked": false
                                },
                                {
                                    "id": uuidV4(),
                                    "name": "validate-documents",
                                    "pretty": "Validate documents",
                                    "checked": false
                                },
                                {
                                    "id": uuidV4(),
                                    "name": "review-documents",
                                    "pretty": "Review documents",
                                    "checked": false
                                },
                                {
                                    "id": uuidV4(),
                                    "name": "import-documents",
                                    "pretty": "Import documents",
                                    "checked": false
                                },
                                {
                                    "id": uuidV4(),
                                    "name": "export-documents",
                                    "pretty": "Export documents",
                                    "checked": false
                                }
                            ]
                        }

                    ]
                }
            }
        },
        mounted() {
            this.form.modules.forEach(m => {
                if (m.checked) {
                    m.permissions.forEach(p => {
                        p.checked = true
                    })
                } else {
                    m.permissions.forEach(p => {
                        p.checked = false
                    })
                }
                if (m.permissions.every(p => p.checked)) {
                    m.checked = true
                } else {
                    m.checked = false
                }
            })
        },
        methods: {
            trigger() {
                let targetNode = this.$refs.tree.find({id: 849})
                console.log(targetNode)
            },
            toggleModulePermissions(module) {
                if (module.checked) {
                    module.permissions.forEach(p => {
                        p.checked = true
                    })
                } else {
                    module.permissions.forEach(p => {
                        p.checked = false
                    })
                }
            },
            togglePermission(module, permission) {
                if (module.permissions.every(p => p.checked)) {
                    module.checked = true
                } else {
                    module.checked = false
                }
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
