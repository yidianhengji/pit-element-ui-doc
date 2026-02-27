## VirtualSelect 虚拟下拉框

当选项数据量过多时，使用虚拟下拉菜单展示并选择内容。

:::tip
VirtualSelect 的使用只需传入 options 数据，里面必须绑定一个 key 做为唯一标识，具体用法请查看下面的示例及相关文档。
:::

### 基础用法
适用广泛的基础单选，主要传入 options 数据源。显示的字段默认为：label，绑定的字段默认为：value

:::demo
```html
<div>
    <el-virtual-select v-model="value" :options="items" placeholder="基础用法" />
</div>

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
    export default {
        data() {
            return {
                items: [],
                value: ""
            };
        },
        mounted () {
            // this.$nextTick(this.generateItems)
        },
        methods: {
            generateItems(){
                this.items = Array(1000)
                        .fill(1)
                        .map((_, index) => {
                            return {
                                value: index + "A",
                                label: index * 10 + "",
                            };
                        });
            }
        }
    };
</script>
```
:::

### 整个禁用
:::demo
```html
<div>
    <el-virtual-select v-model="value" disabled :options="items" placeholder="有禁用选项" />
</div>

<script>
    export default {
        data() {
            return {
                items: [],
                value: ""
            };
        },
        mounted () {
            this.$nextTick(this.generateItems)
        },
        methods: {
            generateItems(){
                this.items = Array(1000)
                        .fill(1)
                        .map((_, index) => {
                            return {
                                value: index + "A",
                                label: index * 10 + "",
                            };
                        });
            }
        }
    };
</script>
```
:::

### 有禁用选项
:::demo
```html
<div>
    <el-virtual-select v-model="value" :options="items" placeholder="有禁用选项" />
</div>

<script>
    export default {
        data() {
            return {
                items: [],
                value: ""
            };
        },
        mounted () {
            this.$nextTick(this.generateItems)
        },
        methods: {
            generateItems(){
                this.items = Array(1000)
                        .fill(1)
                        .map((_, index) => {
                            return {
                                value: index + "A",
                                label: index * 10 + "",
                                disabled: index % 5 === 0,
                            };
                        });
            }
        }
    };
</script>
```
:::

### 可清空单选
包含清空按钮，可将选择器清空为初始状态

:::demo
```html
<div>
    <el-virtual-select v-model="value" :options="items" clearable placeholder="可清空单选" />
</div>

<script>
    export default {
        data() {
            return {
                items: [],
                value: ""
            };
        },
        mounted () {
            this.$nextTick(this.generateItems)
        },
        methods: {
            generateItems(){
                this.items = Array(1000)
                        .fill(1)
                        .map((_, index) => {
                            return {
                                value: index + "A",
                                label: index * 10 + "",
                            };
                        });
            }
        }
    };
</script>
```
:::

### 基础多选
适用性较广的基础多选，用 Tag 展示已选项

:::demo
```html
<div>
    <el-virtual-select v-model="value" :options="items" multiple clearable placeholder="多选" />
    <el-virtual-select v-model="value2" :options="items" multiple collapse-tags clearable placeholder="多选合并标签" />
</div>

<script>
    export default {
        data() {
            return {
                items: [],
                value: "",
                value2: ""
            };
        },
        mounted () {
            this.$nextTick(this.generateItems)
        },
        methods: {
            generateItems(){
                this.items = Array(1000)
                        .fill(1)
                        .map((_, index) => {
                            return {
                                value: index + "A",
                                label: index * 10 + "",
                            };
                        });
            }
        }
    };
</script>
```
:::

### 自定义模板
可以自定义备选项

:::demo
```html
<div>
    <el-virtual-select v-model="value" :options="items" clearable placeholder="自定义模板">
        <template slot-scope="{item, index, active}">
            <span style="float: left">{{ item.label }}</span>
            <span style="float: right; color: #8492a6; font-size: 13px">{{ item.value }}</span>
        </template>
    </el-virtual-select>
</div>

<script>
    export default {
        data() {
            return {
                items: [],
                value: ""
            };
        },
        mounted () {
            this.$nextTick(this.generateItems)
        },
        methods: {
            generateItems(){
                this.items = Array(1000)
                        .fill(1)
                        .map((_, index) => {
                            return {
                                value: index + "A",
                                label: index * 10 + "",
                            };
                        });
            }
        }
    };
</script>
```
:::

### 自定义配置选项
可以自定义配置选项

:::demo
```html
<div>
    <el-virtual-select v-model="value" :options="items" :props="defaultProps" clearable placeholder="自定义配置选项" />
</div>

<script>
    export default {
        data() {
            return {
                items: [],
                value: "",
                defaultProps: {
                    options: 'options',
                    label: 'name',
                    disabled: 'disabled',
                    value: 'id'
                }
            };
        },
        mounted () {
            this.$nextTick(this.generateItems)
        },
        methods: {
            generateItems(){
                this.items = Array(1000)
                        .fill(1)
                        .map((_, index) => {
                            return {
                                id: index + "A",
                                name: index * 10 + "",
                            };
                        });
            }
        }
    };
</script>
```
:::

### 分组
备选项进行分组展示

:::demo
```html
<div>
    <el-virtual-select v-model="value" :options="listGroup" group clearable placeholder="分组" />
</div>

<script>
    export default {
        data() {
            return {
                items: [],
                value: "",
                listGroup: []
            };
        },
        mounted () {
            this.$nextTick(this.generateItems)
        },
        methods: {
            generateItems(){
                this.items = Array(1000)
                        .fill(1)
                        .map((_, index) => {
                            return {
                                value: index + "A",
                                label: index * 10 + "",
                            };
                        });

                this.items.forEach((item, index) => {
                    if (index % 10 === 0) {
                        this.listGroup.push({
                            label: index + " group",
                            value: index + " group",
                            options: [],
                        });
                    }

                    this.listGroup.slice(-1)[0].options.push(item);
                });
            }
        }
    };
</script>
```
:::

### 可搜索
可以利用搜索功能快速查找选项

:::demo
```html
<div>
    <div>
        可搜索分组
        <el-virtual-select v-model="value" :options="listGroup" filterable group clearable placeholder="可搜索分组" />
    </div>
    <div style="margin: 20px 0">
        可分组多选
        <el-virtual-select v-model="value1" :options="listGroup" filterable group multiple clearable placeholder="可分组多选" />
    </div>
    <div>
        多选并合并
        <el-virtual-select v-model="value2" :options="listGroup" filterable group multiple collapse-tags clearable placeholder="多选并合并" />
    </div>
    <div style="margin: 20px 0 0">
        可单选搜索
        <el-virtual-select v-model="value3" :options="items" filterable clearable placeholder="可单选搜索" />
    </div>
</div>

<script>
    export default {
        data() {
            return {
                items: [],
                value: "",
                value1: "",
                value2: "",
                value3: "",
                listGroup: []
            };
        },
        mounted () {
            this.$nextTick(this.generateItems)
        },
        methods: {
            generateItems(){
                this.items = Array(1000)
                        .fill(1)
                        .map((_, index) => {
                            return {
                                value: index + "A",
                                label: index * 10 + "",
                            };
                        });

                this.items.forEach((item, index) => {
                    if (index % 10 === 0) {
                        this.listGroup.push({
                            label: index + " group",
                            value: index + " group",
                            options: [],
                        });
                    }

                    this.listGroup.slice(-1)[0].options.push(item);
                });
            }
        }
    };
</script>
```
:::

### Attributes
| 参数      | 说明          | 类型      | 可选值                           | 默认值  |
|---------- |-------------- |---------- |--------------------------------  |-------- |
| value / v-model | 绑定值 | boolean / string / number | — | — |
| options | 展示数据 | array | — | [] |
| props                 | 配置选项，具体看下表                               | object                      | —    | —     |
| multiple | 是否多选 | boolean | — | false |
| disabled | 是否禁用 | boolean | — | false |
| size | 输入框尺寸 | string | medium/small/mini | — |
| clearable | 是否可以清空选项 | boolean | — | false |
| collapse-tags | 多选时是否将选中值按文字的形式展示 | boolean | — | false |
| name | select input 的 name 属性 | string | — | — |
| placeholder | 占位符 | string | — | 请选择 |
| filterable | 是否可搜索 | boolean | — | false |
| popper-append-to-body | 是否将弹出框插入至 body 元素。在弹出框的定位出现问题时，可将该属性设置为 false | boolean | — | true |

### Methods
| 事件名称 | 说明 | 回调参数 |
|---------|---------|---------|
| change | 选中值发生变化时触发 | 目前的选中值 |
| visible-change | 下拉框出现/隐藏时触发 | 出现则为 true，隐藏则为 false |
| remove-tag | 多选模式下移除tag时触发 | 移除的tag值 |
| clear | 可清空的单选模式下用户点击清空按钮时触发 | — |

### Slot
| name | 说明 |
|------|--------|
| item | 展示中的元素个体 |
| index | 返回每个元素在 items 中的位置 index |
| active | 元素是否处于 active 状态 |

### props
| 参数       | 说明                | 类型     | 可选值  | 默认值  |
| -------- | ----------------- | ------ | ---- | ---- |
| label    | 指定节点标签为节点对象的某个属性值 | string | —    | —    |
| value    | 指定节点标签为节点对象的某个绑定值 | string | —    | —    |
| options | 指定子树为节点对象的某个属性值 | string | —    | —    |
| disabled | 指定节点选择框是否禁用为节点对象的某个属性值 | boolean | —    | —    |
