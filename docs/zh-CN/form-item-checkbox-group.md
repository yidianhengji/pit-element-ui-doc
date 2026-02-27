## FormItemCheckboxGroup 属性

| 参数      | 说明          | 类型      | 可选值                           | 默认值  |
|---------- |-------------- |---------- |--------------------------------  |-------- |
| value / v-model | 绑定值           | string / number  | — | — |
| prop    | 表单域 model 字段，在使用 validate、resetFields 方法的情况下，该属性是必填的 | string    | 传入 Form 组件的 `model` 中的字段 | — |
| label | 标签文本 | string | — | — |
| label-width | 表单域标签的的宽度，例如 '50px'。支持 `auto`。 | string |       —       | — |
| required | 是否必填，如不设置，则会根据校验规则自动生成 | boolean | — | false |
| rules    | 表单验证规则 | string | [见下方](#/zh-CN/component/form-item-checkbox-group#rules-shi-yong-shuo-ming) | — |
| nodeKey | 主键字段 | string | - | id |
| nodeLabel | 指定节点标签为节点对象的某个属性值 | string | - | label |
| dataList | 静态数据，用于默认不调用接口使用 | array | - | - |
| placeholder | 输入框占位文本 | string | - | label |
| clearable | 是否可清空 | boolean | - | true |
| filterable | 是否可搜索 | boolean | - | true |
| showWordLimit | 是否显示输入字数统计 | boolean | - | true |
| maxlength | 原生属性，最大输入长度 | number | - | 200 |
| isSearch | 是否为查询条件 | boolean | - | false |
| disabled | 是否禁用该选项 | boolean | - | false |
| readonly | 原生属性，是否只读 | boolean | - | false |

## rules 使用说明

如在rules中使用一些特点的校验规则，可以参考以下的规则

| 参数      | 说明          |
|---------- |-------------- |
| required      | 必填标识          |
| min      | 最小字符校验 min:2          |
| max      | 最大字符校验 max:5         |
| iphone      | 请输入正确的手机号          | 
| password      | 6-16位字符的密码（数字和字母组成）          | 
| idCard      | 请输入正确的身份证号码          |
| email      | 请输入正确的邮箱地址          |
| plateNumber      | 请输入正确的车牌号          | 
| chinese      | 请输入中文          |
| enCode      | 编码只能输入英文、数字和小数点且小数点不能放在首尾          | 
| enCode2      | 编码只能输入1位以上英文、数字和小数点且小数点不能放在首尾的组合          |
| userAccount      | 只能输入数字英文          |
| userAccount1      | 只能输入数字英文中文          |
| english      | 只能输入英文          |
| fullName      | 名称不能含有特殊符号          |
| userCode      | 编码只能输入数字          |
| allDate      | 时间只能是整数和0.5的倍数          |
| bigInt      | 请输入正整数          |
| netUrl      | 请输入正确url地址 (http、ftp、file)          |

