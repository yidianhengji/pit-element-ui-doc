## DialogHeader dialog-header

### 基本用法

DialogHeader 弹出一个对话框，适合需要定制性更大的场景。

:::demo

```html
<el-button type="text" @click="visible = true">点击打开 DialogHeader</el-button>

<el-dialog-header
    title="提示"
    v-show="visible"
    @back="visible = false"
    style="top: 70px; left: 0px;"
>
    <span>这是一段信息</span>
    <template slot="footer" class="dialog-footer">
        <el-button type="primary" @click="visible = false">确 定</el-button>
    </template>
</el-dialog-header>

<script>
    export default {
        data() {
            return {
                visible: false
            };
        }
    };
</script>
```
:::

### DialogHeader Attributes
| 参数      | 说明          | 类型      | 可选值                           | 默认值  |
|---------- |-------------- |---------- |--------------------------------  |-------- |
| title   | 标题 | string      |                  —                |  提示 |
| noCancelBtn   | 是否隐藏取消按钮 | Boolean      |                  —                |  true |

### Slot
| name | 说明 |
|------|--------|
| — | DialogHeader 的内容 |
| header | DialogHeader 标题区的内容 |
| footer | DialogHeader 按钮左边操作区的内容 |
| footer-right | DialogHeader 按钮右边操作区的内容 |

### DialogHeader Events
| 事件名称      | 说明    | 回调参数      |
|---------- |-------- |---------- |
| back  | DialogHeader 关闭的回调 | — |
