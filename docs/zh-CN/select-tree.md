## 树型选择器

### 基础用法

在select的基础上添加树形选择功能，并提供丰富的操作模式。

:::demo 通过options进行赋值，默认为单选模式。允许在data上设置`isDisabled`为true，禁用选项。通过设置`clearable`为true（默认），开启清空功能。defaultExpandLevel加载时应自动扩展多少级分支节点。设置 Infinity 为默认使所有分支节点扩展。

```html

<template>
    <div>
        <div style="
                    margin-bottom: 1rem; 
                    background-color: #F5F7FA; 
                    padding: 10px; 
                    border-radius: 5px">
            value: {{ value }}
        </div>
        <el-select-tree
                :options="options"
                v-model="value"
                :limit="1"
                multiple
                :limitText="limitText"
                valueConsistsOf="ALL"
                noOptionsText="暂无数据"
                noChildrenText="没有数据了"
                noResultsText="暂无数据"
                placeholder="请选择"
        />
    </div>
</template>

<script>
    function dig(path = '0', level = 3) {
        const list = [];
        for (let i = 0; i < 10; i += 1) {
            const key = `${path}-${i}`;
            const treeNode = {
                label: key,
                key,
                id: key
            };

            if (level > 0) {
                treeNode.children = dig(key, level - 1);
            }

            list.push(treeNode);
        }
        return list;
    }
    const OPTIONS = dig()

    export default {
        data() {
            return {
                value: '',
                checked: null,
                size: 'mini', // medium small mini
                limitText: (count) => `还有 ${count} 个`,
                options: OPTIONS
            }
        },

        computed: {
            showMultiple() {
                return ['FLAT', 'LIMIT TWO', 'CLEAR_ON_SELECT'].includes(this.checked)
            },

            checkedOptionsKeys() {
                return Object.keys(this.checkedOptions)
            },

            checkedOptionsValues() {
                return Object.values(this.checkedOptions)
            },

            checkedOptionsLen() {
                const len = Math.floor(this.checkedOptionsKeys.length / 3)
                const isMode = this.checkedOptionsKeys.length % 3 === 0
                return !isMode ? len + 1 : len
            }
        },

        watch: {
            checked: {
                handler(val) {
                    if (val === 'NO_OPTIONS') this.options = []
                    else this.options = OPTIONS
                }
            }
        },
    }
</script>
```
:::

:::demo 通过options进行赋值，默认为单选模式。允许在data上设置`isDisabled`为true，禁用选项。通过设置`clearable`为true（默认），开启清空功能。defaultExpandLevel加载时应自动扩展多少级分支节点。设置 Infinity 为默认使所有分支节点扩展。

```html

<template>
    <div>
        <div style="
                    margin-bottom: 1rem; 
                    background-color: #F5F7FA; 
                    padding: 10px; 
                    border-radius: 5px">
            value: {{ value }}
        </div>
        <el-radio-group
                v-model="checked"
                style="padding: 1rem 0"
                v-for="index in checkedOptionsLen"
                :key="index"
        >
            <template v-for="jndex in 3">
                <el-radio
                        v-if="checkedOptionsKeys[(index - 1) * 3 + (jndex - 1)]"
                        :key="checkedOptionsKeys[(index - 1) * 3 + (jndex -1)]"
                        :label="checkedOptionsKeys[(index - 1) * 3 + (jndex -1)]">
                    {{ checkedOptionsValues[(index - 1) * 3 + (jndex -1)] }}
                </el-radio>
            </template>
        </el-radio-group>
        <el-select-tree
                :options="options"
                v-model="value"
                :size="size"
                :multiple="showMultiple"
                :flat="checked === 'FLAT'"
                :always-open="checked === 'ALWAYS_OPEN'"
                :disabled="checked === 'DISABLED'"
                :disable-branch-nodes="checked === 'DISABLED_BRANCH_NODES'"
                :branch-nodes-first="checked === 'BRANCH_NODES_FIRST'"
                :limit="checked === 'LIMIT TWO' ? 2 : Infinity"
                :show-count="checked === 'SHOW_COUNT'"
                :searchable="checked !== 'NO_SEARCHABLE'"
                :backspace-removes="checked !== 'BACK_SPACE_REMOVES'"
                :clearable="checked !== 'CLEARABLE'"
                :clear-on-select="checked === 'CLEAR_ON_SELECT'"
                :close-on-select="checked !== 'CLOSE_ON_SELECT'"
                :flatten-search-results="checked === 'FLATTEN_SEARCH_RESULT'"
                :disable-fuzzy-matching="checked === 'DISABLED_FUZZY_MATCHING'"
                :search-nested="checked === 'SEARCH_NESTED'"
                :default-expand-level="1"
                :append-to-body="checked === 'appendToBody'"
        />
    </div>
</template>

<script>
    function dig(path = '0', level = 3) {
        const list = [];
        for (let i = 0; i < 10; i += 1) {
            const key = `${path}-${i}`;
            const treeNode = {
                label: key,
                key,
                id: key
            };

            if (level > 0) {
                treeNode.children = dig(key, level - 1);
            }

            list.push(treeNode);
        }
        return list;
    }
    const OPTIONS = dig()

    export default {
        data() {
            return {
                value: '',
                checked: null,
                size: 'mini', // medium small mini
                checkedOptions: {
                    'DISABLED': '禁用操作',
                    'DISABLED_BRANCH_NODES': '禁用分支选项',
                    'ALWAYS_OPEN': '保持打开菜单',
                    'BRANCH_NODES_FIRST': '在叶节点之前显示分支节点',
                    'FLAT': '平铺模式（同联合模式相对）',
                    'LIMIT TWO': '限制显示长度模式为2（仅多选模式）',
                    'NO_OPTIONS': '不存在Options值时',
                    'SHOW_COUNT': '分支节点显示数量统计',
                    'CLEAR_ON_SELECT': '选择选项后是否清除搜索输入（仅多选模式）',
                    'NO_SEARCHABLE': '禁用搜索（默认开启）',
                    'BACK_SPACE_REMOVES': '没有输入时，不允许删除最后一项（默认允许）',
                    'CLEARABLE': '是否显示重置值的“×”按钮（默认显示）',
                    'CLOSE_ON_SELECT': '单选模式，选择不关闭menu',
                    'FLATTEN_SEARCH_RESULT': '搜索时是否展平树（仅同步）',
                    'DISABLED_FUZZY_MATCHING': '设置为true禁用默认情况下启用的模糊匹配功。',
                    'SEARCH_NESTED': '巢状搜索',
                    'appendToBody': '添加到body'
                },
                options: OPTIONS
            }
        },

        computed: {
            showMultiple() {
                return ['FLAT', 'LIMIT TWO', 'CLEAR_ON_SELECT'].includes(this.checked)
            },

            checkedOptionsKeys() {
                return Object.keys(this.checkedOptions)
            },

            checkedOptionsValues() {
                return Object.values(this.checkedOptions)
            },

            checkedOptionsLen() {
                const len = Math.floor(this.checkedOptionsKeys.length / 3)
                const isMode = this.checkedOptionsKeys.length % 3 === 0
                return !isMode ? len + 1 : len
            }
        },

        watch: {
            checked: {
                handler(val) {
                    if (val === 'NO_OPTIONS') this.options = []
                    else this.options = OPTIONS
                }
            }
        },
    }
</script>
```
:::

### 节点选择模式

对于非固定和多选模式，如果选中了分支节点及其所有后代，则vue-treeselect会将它们组合到值数组中的单个项目中，如以下示例所示。通过使用valueConsistsOf道具，您可以更改该行为。该道具有四个选项：

* "ALL" - 选中的所有节点都将包含在 value 数组中
* "BRANCH_PRIORITY"（默认）-如果选中了分支节点，则其所有后代将被排除在value 数组之外
* "LEAF_PRIORITY" - 如果选中了分支节点，则此节点本身及其分支后代将从value阵列中排除，但其叶后代将包括在内
* "ALL_WITH_INDETERMINATE" -选中的任何节点将包括在value 数组中，另外还有不确定的节点

:::demo 通过使用valueConsistsOf道具，来变更选择模式。
```html

<template>
    <div>
        <div style="
                    margin-bottom: 1rem; 
                    background-color: #F5F7FA; 
                    padding: 10px; 
                    border-radius: 5px ">
            value: {{ value }} 
        </div>
        <el-select-tree :options="options" :multiple="true" v-model="value"  :value-consists-of="checked" />
        <el-radio-group v-model="checked" style="padding: 1rem 0">
            <el-radio v-for="check in checkedOptions" :key="check" :label="check" >{{ check }}</el-radio>
        </el-radio-group>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                value: null,
                checked: 'BRANCH_PRIORITY',
                checkedOptions: [
                    'ALL',
                    'BRANCH_PRIORITY',
                    'LEAF_PRIORITY',
                    'ALL_WITH_INDETERMINATE'
                ],
                options: [{
                    id: 'fruits 1',
                    label: 'Fruits',
                    children: [{
                        id: 'apple',
                        label: 'Apple 🍎',
                        isNew: true,
                    }, {
                        id: 'grapes',
                        label: 'Grapes 🍇',
                        isDisabled: true,
                    }, {
                        id: 'pear',
                        label: 'Pear 🍐',
                        isDisabled: true,
                    }, {
                        id: 'strawberry',
                        label: 'Strawberry 🍓',
                    }, {
                        id: 'watermelon',
                        label: 'Watermelon 🍉',
                    }],
                }, {
                    id: 'vegetables',
                    label: 'Vegetables',
                    children: [{
                        id: 'corn',
                        label: 'Corn 🌽',
                    }, {
                        id: 'carrot',
                        label: 'Carrot 🥕',
                    }, {
                        id: 'eggplant',
                        label: 'Eggplant 🍆',
                    }, {
                        id: 'tomato',
                        label: 'Tomato 🍅',
                    }],
                }],
            }
        }
    }
</script>
```
:::

### 平面模式和排序值

在前面的所有示例中，我们使用了默认的非平坦模式vue-treeselect，这意味着：
* 每当分支节点被检查时，其所有子节点也将被检查
* 每当分支节点检查所有子节点时，分支节点本身也将被检查

有时我们不需要那种机制，并且希望分支节点和叶子节点不会相互影响。在这种情况下，应使用平面模式，如下所示。

如果要控制所选选项的显示顺序，请使用sortValueBy道具。该道具有三个选择：
* "ORDER_SELECTED" （默认）-选择订单
* "LEVEL" - 选择级别: C 🡒 BB 🡒 AAA
* "INDEX" - 选项索引: AAA 🡒 BB 🡒 C

:::demo  首先通过flat开启平面模式，再请使用sortValueBy道具，来变更排序模式。
```html

<template>
    <div>
        <div style="
                    margin-bottom: 1rem; 
                    background-color: #F5F7FA; 
                    padding: 10px; 
                    border-radius: 5px ">
            value: {{ sortedValue }}
        </div>
        <el-select-tree 
                flat
                :options="options"
                :multiple="true"
                v-model="sortedValue"
                :sort-value-by="sorted"
        />
        <el-radio-group v-model="sorted" style="padding: 1rem 0">
            <el-radio v-for="check in sortedOptions" :key="check" :label="check">{{ check }}</el-radio>
        </el-radio-group>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                sorted: 'ORDER_SELECTED',
                sortedValue: ['C', 'AAA', 'BB'],
                sortedOptions: ['ORDER_SELECTED', 'LEVEL', 'INDEX'],
                options: [
                    {
                    id: 'A',
                    label: 'A',
                    children: [{
                        id: 'AA',
                        label: 'AA',
                    }, {
                        id: 'AB',
                        label: 'AB',
                    }, {
                        id: 'AC',
                        label: 'AC',
                        children: [
                            { id: 'AAA', label: 'AAA' }
                        ]
                    }, ],
                }, {
                    id: 'B',
                    label: 'B',
                    children: [{
                        id: 'BA',
                        label: 'BA',
                    },{
                        id: 'BB',
                        label: 'BB',
                    }],
                }, {
                        id: 'C',
                        label: 'C',
                        children: [{
                            id: 'CA',
                            label: 'CA',
                        }],
                    }],
            }
        }
    }
</script>
```
:::

### 延迟加载

如果您有大量深度嵌套的选项，则可能只希望在初始加载时加载最顶层的选项，而仅在需要时加载其余选项。您可以通过执行以下步骤来实现：

* 通过设置声明一个卸载的分支节点children: null
* 添加 loadOptions 道具
* 每当卸载的分支节点被扩展时， loadOptions({ action, parentNode, callback, instanceId }) 都会被调用，然后您就可以执行从远程服务器请求数据的作业

:::demo
```html

<template>
    <div>
        <div style="
                    margin-bottom: 1rem; 
                    background-color: #F5F7FA; 
                    padding: 10px; 
                    border-radius: 5px ">
            value: {{ value }}
        </div>
        <el-select-tree
                :multiple="true"
                :options="options"
                :load-options="loadOptions"
                placeholder="尝试展开任意分支节点"
                v-model="value"
        />
    </div>
</template>

<script>
    const simulateAsyncOperation = fn => {
        setTimeout(fn, 200)
    }

    export default {
        data() {
            return {
                value: [],
                options: [{
                    id: 'success',
                    label: '存在子选项',
                    // Declare an unloaded branch node.
                    children: null,
                }, {
                    id: 'no-children-show-message',
                    label: '没有子选项，显示提示信息',
                    children: null,
                }, {
                    id: 'no-children',
                    label: '没有子选项，不显示提示信息（改为隐藏箭头标识）—— 不建议使用',
                    children: null,
                    isLeaf: false,
                },{
                    id: 'failure',
                    label: '由于各种原因造成的失败',
                    children: null,
                }, {
                    id: 'is Leaf',
                    label: '允许设置isLeaf为true将节点标记为叶子节点，优先级大children',
                    children: null,
                    isLeaf: true,
                }],
            }
        },

        methods: {
            loadOptions({action, parentNode, callback}) {
                // LOAD_CHILDREN_OPTIONS
                if (action === 'LOAD_CHILDREN_OPTIONS') {
                    switch (parentNode.id) {
                        case 'success': {
                            simulateAsyncOperation(() => {
                                parentNode.children = [{
                                    id: 'child',
                                    label: 'Child option',
                                }]
                                callback()
                            })
                            break
                        }
                        case 'no-children': {
                            simulateAsyncOperation(() => {
                                // this.$set(parentNode, 'isLeaf', true)
                                parentNode.isLeaf = true
                                callback()
                            })
                            break
                        }
                        case 'no-children-show-message': {
                            simulateAsyncOperation(() => {
                                // parentNode.children = null
                                callback()
                            })
                            break 
                        }
                        case 'failure': {
                            simulateAsyncOperation(() => {
                                callback(new Error('加载失败，网络错误。'))
                            })
                            break
                        }
                        default: /* empty */
                    }
                }
            }
        }
    }
</script>
```
:::

### 异步搜索

vue-treeselect支持根据用户类型动态加载和更改整个选项列表。默认情况下，vue-treeselect将缓存每个AJAX请求的结果，因此用户可以减少等待时间。

:::demo
```html

<template>
    <div>
        <div style="
                    margin-bottom: 1rem; 
                    background-color: #F5F7FA; 
                    padding: 10px; 
                    border-radius: 5px ">
            value: {{ value }}
        </div>
        <el-select-tree
                :multiple="true"
                :async="true"
                :load-options="loadOptions"
                v-model="value"
        />
    </div>
</template>

<script>
    const simulateAsyncOperation = fn => {
        setTimeout(fn, 2000)
    }

    export default {
        data() {
            return {
                value: [],
            }
        },

        methods: {
            loadOptions({action, searchQuery, callback}) {
                // LOAD_CHILDREN_OPTIONS
                if (action === 'ASYNC_SEARCH') {
                    simulateAsyncOperation(() => {
                        const options = [ 1, 2, 3, 4, 5 ].map(i => ({
                            id: `${searchQuery}-${i}`,
                            label: `${searchQuery}-${i}`,
                        }))
                        callback(null, options)
                    })
                }
            }
        }
    }
</script>
```
:::

### 自定义键名 & 自定义选项标签 & 自定义值标签

如果通过AJAX加载的选项数据与vue-treeselect要求的数据结构不同，
例如，您的数据具有name属性，但vue-treeselect需要label，则可能需要自定义键名。
在这种情况下，您可以提供一个称为函数的propnormalizer，在数据初始化期间它将传递给树中的每个节点。
使用此函数创建并返回转换后的对象。

:::demo 您可以通过option-label自定义每个选项的标签、value-label自定义选项后的输出值
```html

<template>
    <div>
        <div style="
                    margin-bottom: 1rem; 
                    background-color: #F5F7FA; 
                    padding: 10px; 
                    border-radius: 5px ">
            value: {{ value }}
        </div>
        <el-select-tree
                :options="options"
                :multiple="true"
                :normalizer="normalizer"
                value-consists-of="ALL"
                v-model="value"
        >
            <label 
                    slot="option-label" 
                    slot-scope="{ node, shouldShowCount, count, labelClassName, countClassName }" 
                    :class="labelClassName">
                {{ node.isBranch ? 'Branch' : 'Leaf' }}: {{ node.label }}
                <span v-if="shouldShowCount" :class="countClassName">({{ count }})</span>
            </label>

            <div slot="value-label" slot-scope="{ node }">{{ node.isBranch ? 'Branch' : 'Leaf' }}: {{ node.label }}</div>
        </el-select-tree>
    </div>
</template>

<script>
    const simulateAsyncOperation = fn => {
        setTimeout(fn, 2000)
    }

    export default {
        data() {
            return {
                value: [],
                options: [
                    { 
                        value: 'A', 
                        name: 'A', 
                        child: [
                            {
                                value: 'AA',
                                name: 'AA',
                                child: []
                            },
                            {
                                value: 'AB',
                                name: 'AB',
                                child: [],
                                leaf: true
                            }
                        ] 
                    }
                ]
            }
        },

        methods: {
            normalizer(node) {
                return {
                    id: node.value,
                    label: node.name,
                    children: node.child,
                    isLeaf: node.leaf
                }
            },
        }
    }
</script>
```
:::
