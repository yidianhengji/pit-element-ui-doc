## formBase 增删改查基础组件

抽象基于增删改查系列业务模板，实现快速开发增删改查界面功能。

### 全局配置

在引入 Element 时，可以传入一个全局配置对象。按照引入 Element 的方式，具体操作如下：

完整引入 Element：

```js
import Vue from 'vue';
import Element from 'pit-element-ui';
Vue.use(Element, { 
  size: 'small',
  zIndex: 3000,
  tableHeadBackground: '#fff', // 用于改变表格表头的颜色值
  isHiddenAddBtn: 'hide', // 用于控制所有的新增按钮是否显示隐藏：默认值：show，可选：show、hide
  isHiddenEditBtn: 'hide', // 用于控制所有的编辑按钮是否显示隐藏：默认值：show，可选：show、hide
  isHiddenDelBtn: 'hide', // 用于控制所有的删除按钮是否显示隐藏：默认值：show，可选：show、hide
  isHiddenBatchBtn: 'hide', // 用于控制所有的批量删除按钮是否显示隐藏：默认值：show，可选：show、hide
  paginationPageSize: 'pageSize', // 分页条数key
  paginationPageNum: 'currentPage', // 分页页码key
  paginationTotal: 'total', // 分页总数key
  dialogTop: '20px', // 设置全局的dialog组件top属性
  buttonArea: 'default' // 默认操作按钮在table上方，值为search显示在搜索区域的右边
});
```

### 准备工作

```js
Vue.prototype.request = request                             // 在Vue实例上挂载request请求函数
Vue.prototype.isOk = 200                                    // 在Vue实例上挂载isOk，后台成功状态码，默认为200
Vue.prototype.dialogHeaderClass = 'dialog-header-class'     // 在Vue实例上挂载弹出层dialogHeaderClass，默认为preview-warp
```

### 基本用法

表格+表单 新增弹窗展示

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
<el-form-base
    ref="peFormBase"
    key="peFormBase1"
    primary-key="departmentId"
    row-key="departmentId"
    query-interface="basics/department/page"
    query-one-interface="basics/department/info"
    save-interface="basics/department/save"
    update-interface="basics/department/update"
    delete-interface="basics/department/delete"
    :form-inline="formInline"
    :columns="columns"
    :add-form="addForm"
    :isDefaultQuery="false"
    :rules="rules"
    :noBatchBtn="false"
    :row-single="false"
>
    <template slot="searchForm" slot-scope="data">
        <el-form-item label="关键词" prop="keyword">
            <el-input
                v-model="data.formInline.keyword"
                placeholder="请输入关键词查询"
                clearable
            />
        </el-form-item>
    </template>
    <template slot="add" slot-scope="data">
        <el-form-item label="部门名称" prop="fullName">
            <el-input v-model="data.addForm.fullName" placeholder="输入名称" />
        </el-form-item>
        <el-form-item label="部门编码" prop="enCode">
            <el-input v-model="data.addForm.enCode" placeholder="输入编码" />
        </el-form-item>
        <el-form-item label="部门主管" prop="managerId">
            <el-input v-model="data.addForm.managerId" placeholder="输入部门主管" />
        </el-form-item>
        <el-form-item label="排序" prop="orderId">
            <el-input-number v-model="data.addForm.orderId" :min="0" :max="9999" />
        </el-form-item>
        <el-form-item label="部门说明" prop="description">
            <el-input v-model="data.addForm.description" type="textarea" :rows="6" />
        </el-form-item>
    </template>
</el-form-base>

<script>
    export default {
        data() {
            return {
                operatingTypeProps: 'add',
                columns: [
                    {
                        prop: 'fullName',
                        label: '名称',
                        align: 'left'
                    },
                    {
                        prop: 'enCode',
                        label: '编码',
                        align: 'left'
                    },
                    {
                        prop: 'managerId',
                        label: '部门主管',
                        align: 'left'
                    },
                    {
                        prop: 'orderId',
                        label: '排序',
                        align: 'center',
                        width: 80
                    },
                    {
                        prop: 'createTime',
                        label: '创建时间',
                        align: 'center',
                        width: 160
                    },
                    {
                        prop: '',
                        label: '操作',
                        align: 'center',
                        width: 160,
                        render: (params) => (
                                <div>
                                    <a
                                        onClick={() => this.$refs.peFormBase.info(params['departmentId'])}
                                        class='text-primary mr-2'
                                    >
                                        编辑
                                    </a>
                                    <a
                                        onClick={() => this.$refs.peFormBase.delete(params['departmentId'])}
                                        class='text-danger mr-2'
                                    >
                                        删除
                                    </a>
                                </div>
                        )
                    }
                ],
                formInline: {
                    id: '',
                    keyword: ''
                },
                addForm: {
                    organizeId: '',
                    departmentId: '',
                    description: '',
                    enCode: '',
                    fullName: '',
                    managerId: '',
                    orderId: 0,
                    parentId: ''
                },
                rules: {
                    parentId: [
                        { required: true, message: '所属上级不能为空', trigger: 'input' }
                    ],
                    fullName: [
                        { required: true, message: '请输入部门名称', trigger: 'blur' },
                        { max: 50, message: '部门名称最多为50个字符！', trigger: 'blur' }
                    ]
                },
                treeData: [],
                natureData: [],
                industryData: []
            };
        },
        methods: {
            
        }
    };
</script>
```
:::

表格+表单

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
<el-form-base
    ref="peFormBase"
    primary-key="departmentId"
    row-key="departmentId"
    query-interface="basics/department/page"
    query-one-interface="basics/department/info"
    save-interface="basics/department/save"
    update-interface="basics/department/update"
    delete-interface="basics/department/delete"
    :form-inline="formInline"
    :columns="columns"
    :add-form="addForm"
    :isDefaultQuery="false"
    dialogType="dialog-header"
    :rules="rules"
>
    <template slot="searchForm" slot-scope="data">
        <el-form-item label="关键词" prop="keyword">
            <el-input
                v-model="data.formInline.keyword"
                placeholder="请输入关键词查询"
                clearable
            />
        </el-form-item>
    </template>
    <template slot="add" slot-scope="data">
        <el-form-item label="部门名称" prop="fullName">
            <el-input v-model="data.addForm.fullName" placeholder="输入名称" />
        </el-form-item>
        <el-form-item label="部门编码" prop="enCode">
            <el-input v-model="data.addForm.enCode" placeholder="输入编码" />
        </el-form-item>
        <el-form-item label="部门主管" prop="managerId">
            <el-input v-model="data.addForm.managerId" placeholder="输入部门主管" />
        </el-form-item>
        <el-form-item label="排序" prop="orderId">
            <el-input-number v-model="data.addForm.orderId" :min="0" :max="9999" />
        </el-form-item>
        <el-form-item label="部门说明" prop="description">
            <el-input v-model="data.addForm.description" type="textarea" :rows="6" />
        </el-form-item>
    </template>
</el-form-base>

<script>
    export default {
        data() {
            return {
                operatingTypeProps: 'add',
                columns: [
                    {
                        prop: 'fullName',
                        label: '名称',
                        align: 'left'
                    },
                    {
                        prop: 'enCode',
                        label: '编码',
                        align: 'left'
                    },
                    {
                        prop: 'managerId',
                        label: '部门主管',
                        align: 'left'
                    },
                    {
                        prop: 'orderId',
                        label: '排序',
                        align: 'center',
                        width: 80
                    },
                    {
                        prop: 'createTime',
                        label: '创建时间',
                        align: 'center',
                        width: 160
                    },
                    {
                        prop: '',
                        label: '操作',
                        align: 'center',
                        width: 160,
                        render: (params) => (
                                <div>
                                    <a
                                        onClick={() => this.$refs.peFormBase.info(params['departmentId'])}
                                        class='text-primary mr-2'
                                    >
                                        编辑
                                    </a>
                                    <a
                                        onClick={() => this.$refs.peFormBase.delete(params['departmentId'])}
                                        class='text-danger mr-2'
                                    >
                                        删除
                                    </a>
                                </div>
                        )
                    }
                ],
                formInline: {
                    id: '',
                    keyword: ''
                },
                addForm: {
                    organizeId: '',
                    departmentId: '',
                    description: '',
                    enCode: '',
                    fullName: '',
                    managerId: '',
                    orderId: 0,
                    parentId: ''
                },
                rules: {
                    parentId: [
                        { required: true, message: '所属上级不能为空', trigger: 'input' }
                    ],
                    fullName: [
                        { required: true, message: '请输入部门名称', trigger: 'blur' },
                        { max: 50, message: '部门名称最多为50个字符！', trigger: 'blur' }
                    ]
                },
                treeData: [],
                natureData: [],
                industryData: []
            };
        },
        methods: {
            
        }
    };
</script>
```
:::

复杂表头

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
<el-form-base
    ref="peFormBase"
    key="peFormBase2"
    primary-key="departmentId"
    query-interface="basics/department/page"
    query-one-interface="basics/department/info"
    save-interface="basics/department/save"
    update-interface="basics/department/update"
    delete-interface="basics/department/delete"
    :form-inline="formInline"
    :columns="columns"
    :add-form="addForm"
    :isDefaultQuery="false"
    dialogType="dialog-header"
    :rules="rules"
>
    <template slot="searchForm" slot-scope="data">
        <el-form-item label="关键词" prop="keyword">
            <el-input
                v-model="data.formInline.keyword"
                placeholder="请输入关键词查询"
                clearable
            />
        </el-form-item>
    </template>
    <template slot="add" slot-scope="data">
        <el-form-item label="部门名称" prop="fullName">
            <el-input v-model="data.addForm.fullName" placeholder="输入名称" />
        </el-form-item>
        <el-form-item label="部门编码" prop="enCode">
            <el-input v-model="data.addForm.enCode" placeholder="输入编码" />
        </el-form-item>
        <el-form-item label="部门主管" prop="managerId">
            <el-input v-model="data.addForm.managerId" placeholder="输入部门主管" />
        </el-form-item>
        <el-form-item label="排序" prop="orderId">
            <el-input-number v-model="data.addForm.orderId" :min="0" :max="9999" />
        </el-form-item>
        <el-form-item label="部门说明" prop="description">
            <el-input v-model="data.addForm.description" type="textarea" :rows="6" />
        </el-form-item>
    </template>
</el-form-base>

<script>
    export default {
        data() {
            return {
                operatingTypeProps: 'add',
                columns: [
                    {
                        prop: 'fullName',
                        label: '名称',
                        align: 'left'
                    },
                    {
                        prop: 'enCode',
                        label: '编码',
                        align: 'left'
                    },
                    {
                        prop: 'managerId',
                        label: '部门主管',
                        align: 'left'
                    },
                    {
                        prop: 'orderId',
                        label: '排序',
                        align: 'center',
                        width: 240,
                        children: [
                            {
                                prop: 'sort',
                                label: '排序1',
                                align: 'center',
                                width: 120
                            },
                            {
                                prop: 'dictionarydataValue',
                                label: '排序2',
                                align: 'center',
                                width: 120
                            }
                        ]
                    },
                    {
                        prop: 'createTime',
                        label: '创建时间',
                        align: 'center',
                        width: 160
                    },
                    {
                        prop: '',
                        label: '操作',
                        align: 'center',
                        width: 160,
                        render: (params) => (
                                <div>
                                    <a
                                        onClick={() => this.$refs.peFormBase.info(params['departmentId'])}
                                        class='text-primary mr-2'
                                    >
                                        编辑
                                    </a>
                                    <a
                                        onClick={() => this.$refs.peFormBase.delete(params['departmentId'])}
                                        class='text-danger mr-2'
                                    >
                                        删除
                                    </a>
                                </div>
                        )
                    }
                ],
                formInline: {
                    id: '',
                    keyword: ''
                },
                addForm: {
                    organizeId: '',
                    departmentId: '',
                    description: '',
                    enCode: '',
                    fullName: '',
                    managerId: '',
                    orderId: 0,
                    parentId: ''
                },
                rules: {
                    parentId: [
                        { required: true, message: '所属上级不能为空', trigger: 'input' }
                    ],
                    fullName: [
                        { required: true, message: '请输入部门名称', trigger: 'blur' },
                        { max: 50, message: '部门名称最多为50个字符！', trigger: 'blur' }
                    ]
                },
                treeData: [],
                natureData: [],
                industryData: []
            };
        },
        methods: {
            
        }
    };
</script>
```
:::

### FormBase Attributes
| 参数      | 说明          | 类型      | 可选值                           | 默认值  |
|---------- |-------------- |---------- |--------------------------------  |-------- |
| primaryKey   | 业务表主键 | string      |                  —                |  — |
| rowKey    | 是否为树标识 | string | — | — |
| rowSingle    | 单选还是多选，默认多选 | boolean | — | true |
| dialogType | 表单显示类型，默认为弹窗 | string | dialog / dialog-header | dialog |
| queryInterface | 查询接口 | string | - | - |
| queryOneInterface | 单个查询接口 | string | - | - |
| saveInterface | 保存接口 | string | - | - |
| updateInterface | 修改接口 | string | - | - |
| deleteInterface | 删除接口 | string | - | - |
| queryInterfaceMethod | 查询接口类型 | string | - | GET |
| queryOneInterfaceMethod | 单个查询接口类型 | string | - | GET |
| saveInterfaceMethod | 保存接口类型 | string | - | POST |
| updateInterfaceMethod | 修改接口类型 | string | - | PUT |
| deleteInterfaceMethod | 删除接口类型 | string | - | DELETE |
| formInline | 搜索实体 | object | - | - |
| columns | 表头数据 | array | - | - |
| addForm | 新增、修改实体 | Object | - | - |
| rules | 新增、修改校验规则 | Object | - | - |
| labelWidth | 表单域标签的宽度 | string | - | 100px |
| isDefaultQuery | 是否默认查询 | Boolean | - | true |
| operatingTypeProps | 用于判断是新增还是修改 新增add 修改edit | string | add / edit / info | add |
| isDrag | 是否开启弹窗拖动 | Boolean | - | true |
| noAddBtn | 是否需要新增按钮 | Boolean | - | true |
| noEditBtn | 是否需要编辑按钮 | Boolean | - | true |
| noDelBtn | 是否需要删除按钮 | Boolean | - | true |
| noSearch | 是否需要查询按钮 | Boolean | - | true |
| noClear | 是否需要清空按钮 | Boolean | - | true |
| noBatchBtn | 是否需要批量删除按钮 | Boolean | - | true |
| noSearchForm | 是否隐藏搜索区域 | Boolean | - | true |
| noFormInlineBtn | 是否隐藏搜索按钮 | Boolean | - | true |
| noOperation | 是否功能按钮区域 | Boolean | - | true |
| noRefreshBtn | 是否隐藏刷新按钮 | Boolean | - | true |
| noTableIndex | 是否隐藏序号 | Boolean | - | true |
| noPagination | 是否隐藏分页 | Boolean | - | true |
| addBtnText | 自定义新增按钮的文本 | string | - | 新增 |
| editBtnText | 自定义修改按钮的文本 | string | - | 修改 |
| delBtnText | 自定义删除按钮的文本 | string | - | 删除 |
| batchBtnText | 自定义批量删除按钮的文本 | string | - | 批量删除 |
| searchBtnText | 自定义插查询按钮的文本 | string | - | 查询 |
| clearBtnText | 自定义清空按钮的文本 | string | - | 清空 |
| dialogWidth | 弹窗的默认宽度 | string | - | 35% |
| dialogBeforeClose | 弹窗关闭前的回调 | Function | - | |
| searchBeforeCallback | 点击查询按钮前回调 | Function | - | |
| addBeforeCallback | 点击新增按钮前回调 | Function | - | |
| infoBeforeCallback | 点击详情按钮前回调 | Function | - | |
| addParamControllerCallback | 新增请求前回调 | Function | - | |
| addControllerCallback | 新增/编辑 成功后的回调 | Function | - | |
| delControllerCallback | 删除 成功后的回调 | Function | - | |
| infoControllerCallback | 详情请求回调成功 | Function | _ | |
| clearCallback | 取消的回调 | Function | - | |
| textDelStr | 自定义删除提示信息 | string | - | 是否删除这条数据 |

### FormBase Slot
| name | 说明 |
|------|--------|
| searchForm | 搜索的表单区域 |
| toolbar | 功能按钮 |
| addWarp | 新增编辑的表单区域 |
| footer | 弹窗功能按钮区域 |
| tabList | tab切换区域 |
| table | 主体区域 |

### FormBase 功能说明
| 方法名称 | 说明 |
|------|--------|
| query | 查询列表方法 |
| info | 查询详情方法（primaryKey主键id）|
| delete | 删除方法（ids主键数组） |

### FormBase 事件说明
| 方法名称 | 说明 | 参数 |
|------|--------|--------|
| table-selection-change | 当选择项发生变化时会触发该事件 | selection |
| current-change | 当表格的当前行发生变化的时候会触发该事件，如果要高亮当前行，请打开表格的 highlight-current-row 属性 | currentRow |
| resultTableBack | 查询接口请求成功的回调，返回列表的数据 | tableData |

### FormBase columns字段说明
| 字段 | 说明 | 类型 | 可选值 | 默认值 |
|------|--------|-------|-------|-------|
| prop | 对应列内容的字段名 | string | - | - |
| label | 显示的标题 | string | - | - |
| align | 对齐方式 | string | left/center/right | left |
| width | 对应列的宽度 | string | - | - |
| sortable | 对应列是否可以排序 | boolean, string | true, false, 'custom' | false |
| render | 自定义列内容的函数（建议使用jsx语法） | Function | - | rows |
| renderH | 自定义列内容的函数 | Function | - | rows |
| showOverflow | 当内容过长被隐藏时显示 tooltip | Boolean | - | false |
| fixed | 列是否固定在左侧或者右侧 | string | left, right | - |
| hidden | 对应列内容显示隐藏，多数用于权限控制 | boolean | - | true |
| children | 递归数据，用于复杂表头 | array | - | - |
