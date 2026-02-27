## FormItemDate 属性

### 设置可选时间禁用状态 disabledDate

设置可选时间禁用状态 disabledDate。

:::demo 开始时间不能大于结束时间限制
```html
<el-form :model="addForm">
    <el-form-item-date
        label="开始日期"
        prop="startDate"
        v-model="addForm.startDate"
        :disabledEndDate="addForm.endDate">
    </el-form-item-date>

    <el-form-item-date
        label="结束日期"
        prop="endDate"
        v-model="addForm.endDate"
        :disabledStartDate="addForm.startDate">
    </el-form-item-date>
</el-form>
<script>
export default {
    data() {
        return {
            addForm: {
                startDate: '',
                endDate: ''
            }
        }
    }
}
</script>
```
:::

| 参数      | 说明          | 类型      | 可选值                           | 默认值  |
|---------- |-------------- |---------- |--------------------------------  |-------- |
| value / v-model | 绑定值           | string / number/ array  | — | — |
| prop    | 表单域 model 字段，在使用 validate、resetFields 方法的情况下，该属性是必填的 | string    | 传入 Form 组件的 `model` 中的字段 | — |
| label | 标签文本 | string | — | — |
| label-width | 表单域标签的的宽度，例如 '50px'。支持 `auto`。 | string |       —       | — |
| required | 是否必填，如不设置，则会根据校验规则自动生成 | boolean | — | false |
| rules    | 表单验证规则 | string | [见下方](#/zh-CN/component/form-item-checkbox-group#rules-shi-yong-shuo-ming) | — |
| type | 显示类型 | string | year/month/date/dates/week/ datetime/datetimerange/ daterange/monthrange | date |
| placeholder | 输入框占位文本 | string | - | label |
| clearable | 是否可清空 | boolean | - | true |
| filterable | 是否可搜索 | boolean | - | true |
| isSearch | 是否为查询条件 | boolean | - | false |
| disabled | 是否禁用该选项 | boolean | - | false |
| readonly | 原生属性，是否只读 | boolean | - | false |
| valueFormat | 可选，绑定值的格式。不指定则绑定值为 Date 对象 | string | - | - |
| startPlaceholder | 范围选择时开始日期的占位内容 | string | - | "请选择开始日期" |
| endPlaceholder | 范围选择时结束日期的占位内容 | string | - | "请选择结束日期" |
| startTime | 范围选择时开始日期的占位内容 | 绑定的开始时间，必须配合 .sync 修饰符 | - | - |
| endTime | 范围选择时结束日期的占位内容 | 绑定的结束时间，必须配合 .sync 修饰符 | - | - |
| disabledStartDate | 设置开始禁用状态 | string /date /number | - | - |
| disabledEndDate | 设置开始禁用状态 | string /date /number | - | - |
