## VirtualList 虚拟列表

在前端的业务开发中，前期可能的数据量可能是比较少的。但随着项目不断的运行，数据量的增加就会给前端的网页带来较差的体验。为提高用户操作体验，特在element组件库中集成虚拟列表来供大家使用。

### 基础用法
基础的虚拟列表。

:::demo
```html
<div>
    <el-virtual-list key-field="value" :items="list" :item-size="100" ref="virtualScroller"
                     style="height: 240px" class="scroller-list">
        <template slot-scope="{item, index, active}">
            <div class="scroller-list-item">
                <el-image
                        style="width: 90px; height: 90px"
                        :src="index % 2 ? url2 : url"
                        fit="cover"
                        lazy
                ></el-image>
                <div class="scroller-list-item-text">&nbsp;<el-link type="primary">{{item.label}}</el-link> 下标: {{index}}</div></div>
        </template>
    </el-virtual-list>
</div>

<script>
    
    export default {
        data() {
            return {
                items: [],
                url: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg',
                url2: 'https://cube.elemecdn.com/6/94/4d3ea53c084bad6931a56d5158a48jpeg.jpeg'
            };
        },
        computed: {
            list () {
                return this.items
            },
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

### 方向
基础的水平虚拟列表。

:::demo
```html
<div>
    <el-virtual-list key-field="value" :items="list" :item-size="100" ref="virtualScroller"
                     style="height: 240px" class="scroller-list-horizontal" direction="horizontal">
        <template slot-scope="{item, index, active}">
            <div class="scroller-list-item">
                <el-image
                        style="width: 90px; height: 90px"
                        :src="index % 2 ? url2 : url"
                        fit="cover"
                        lazy
                ></el-image>
                <div class="scroller-list-item-text"><el-link type="primary">{{item.label}}</el-link><br> 下标: {{index}}</div></div>
        </template>
    </el-virtual-list>
</div>

<script>
    
    export default {
        data() {
            return {
                items: [],
                url: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg',
                url2: 'https://cube.elemecdn.com/6/94/4d3ea53c084bad6931a56d5158a48jpeg.jpeg'
            };
        },
        computed: {
            list () {
                return this.items
            },
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

### Attribute
| 参数      | 说明          | 类型      | 可选值                           | 默认值  |
|---------- |-------------- |---------- |--------------------------------  |-------- |
| items | 展示数据 | array | — | [] |
| direction | 滚动方向 | string | vertical, horizontal | 'vertical' |
| itemSize | 每一项元素的高度，需要设置这个高度 | number | — | null |
| gridItems | 网格的形式展示 | number | — | null |
| itemSecondarySize | 设置gridItems时，网格中项目的像素大小（垂直模式下的宽度，水平模式下的高度） | number | — | null |
| minItemSize | 如果项目的高度（或水平模式下的宽度）未知，则使用的最小大小 | Number, String | — | null |
| sizeField  | 用于在可变大小模式下获取项目大小的字段 | string | — | 'size' |
| typeField | 用于区分列表中不同类型组件的字段 | string | — | 'type' |
| keyField | 用于标识项目和优化管理渲染视图的字段 | string | — | 'id' |
| pageMode | 启用页面模式 | boolean | — | false |
| prerender | 为服务器端渲染（SSR）渲染固定数量的项目 | number | — | 0 |
| buffer | 要添加到滚动可见区域边缘以开始渲染更远项目的像素量 | number | — | 200 |
| emitUpdate | 每次更新虚拟滚动条内容时发出'更新'事件（可能会影响性能）。 | boolean | — | false |
| updateInterval | 滚动后检查视图更新的时间间隔（毫秒）。设置为0时，将在下一个动画帧中选中。 | number | — | 0 |
| listClass | list集合的class类 | string, object, array | — | '' |
| itemClass | item项的class类 | string, object, array | — | '' |
| listTag | list集合的标签 | string | - | 'div' |
| itemTag | item项的标签 | string | — | 'div' |

### Slot
| name | 说明 |
|------|--------|
| item | 展示中的元素个体 |
| index | 返回每个元素在 items 中的位置 index |
| active | 元素是否处于 active 状态 |

### Methods
| 方法名      | 说明          | 参数 |
|----------- |-------------- | -- |
| resize | 当滚动条的大小改变时发出 | — |
| visible | 当滚动条认为自己在页面中可见时发出 | - |
| hidden | 当滚动条隐藏在页面中时发出 |  —                                |
| update | 仅当emitUpdate属性为true时，才会在每次更新视图时发出 |  startIndex, endIndex, visibleStartIndex, visibleEndIndex                                |
| scroll-start | 当渲染开始时发出 |  —                                |
| scroll-end | 在渲染完成时发出 |  —                                |
