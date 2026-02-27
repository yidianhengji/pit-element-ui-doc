## virtualTree 虚拟树

用于FormBase选项卡切换，用于点击切换不同功能状态。

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
    <div>
        <el-button type="colour-10" @click="add">赋值</el-button>
    </div>
    <br>
    <el-input-select v-model="value1" :clearable="true" @change-select="changeSelect" @change-view="changeView"></el-input-select>

<script>
    export default {
        data() {
            return {
                value1: ""
                
            };
        },
        methods: {
            add(){
              this.value1 = '1234'
            },
            changeSelect(){
              alert("选择")
            },
            changeView(){
              alert("查看")
            }
        }
    };
</script>
```
:::

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
    <div>
        <el-button type="colour-10" @click="add">赋值</el-button>
        <el-button type="colour-10" @click="add1">隐藏选择按钮</el-button>
    </div>
    <br>
    <el-input-select style="width: 100%" v-model="value1" :clearable="true" :disabled="disabled"></el-input-select>

<script>
    export default {
        data() {
            return {
                value1: "",
                disabled: false
            };
        },
        methods: {
            add(){
              this.value1 = '1234'
            },
            add1(){
              this.disabled = !this.disabled
            }
        }
    };
</script>
```
:::

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
    <div style="height: 200px;"><el-virtual-tree :data="data" /></div>

<script>
    export default {
        data() {
            return {
                data: [{
                    label: '一级 1',
                    children: [{
                        label: '二级 1-1',
                        children: [{
                            label: '三级 1-1-1'
                        }]
                    }]
                }, {
                    label: '一级 2',
                    children: [{
                        label: '二级 2-1',
                        children: [{
                            label: '三级 2-1-1'
                        }]
                    }, {
                        label: '二级 2-2',
                        children: [{
                            label: '三级 2-2-1'
                        }]
                    }]
                }, {
                    label: '一级 3',
                    children: [{
                        label: '二级 3-1',
                        children: [{
                            label: '三级 3-1-1'
                        }]
                    }, {
                        label: '二级 3-2',
                        children: [{
                            label: '三级 3-2-1'
                        }]
                    }]
                }],
                
            };
        },
        methods: {

        }
    };
</script>
```
:::
