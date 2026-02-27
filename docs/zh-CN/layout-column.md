## LayoutColumn 左右布局，主要用于左边树右边表格

### 基本用法

树形+表格布局

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
<el-layout-column>
    <div slot="left" class="h-full" style="height: 100%;">
        <el-layout-tree no-search-input noFilterInput :dataList="dataList" :render-content="renderContent"></el-layout-tree>
    </div>
    <div slot="title-right">
        <el-tooltip content="字典管理" placement="top">
            <i class="el-icon-menu" />
        </el-tooltip>
    </div>
    <div slot="right" class="h-full" style="height: 100%;">
        <el-form-base
            ref="peFormBase"
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
    </div>
</el-layout-column>

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
                dataList: [
                    {
                      id: 1,
                      label: '一级 1',
                      children: [
                          {
                            id: 2,
                            label: '一级 1-1'
                          }
                      ]
                    },
                    {
                        id: 4,
                        label: '二级 1',
                    }
                ]
            };
        },
        methods: {
            renderContent(h, { node, data, store }) {
                if(data.id===4){
                    return (
                        <span class="custom-tree-node">
                            <span>{node.label}</span>
                            <span>
                                <el-button size="mini" type="text" on-click={ () => this.append(data) }>Append</el-button>
                                <el-button size="mini" type="text" on-click={ () => this.remove(node, data) }>Delete</el-button>
                            </span>
                        </span>);
                } else {
                    return (
                        <span class="custom-tree-node">
                            <span>{node.label}</span>
                        </span>);
                }
            }
        }
    };
</script>
```
:::

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
<el-layout-column>
    <div slot="left" class="h-full" style="height: 100%;">
        <p v-for="item in 100" :key="item">123</p>
        
    </div>
    <div slot="title-right">
        <el-tooltip content="字典管理" placement="top">
            <i class="el-icon-menu" />
        </el-tooltip>
    </div>
    <div slot="right" class="h-full" style="height: 100%;">
        <el-form-base
            ref="peFormBase"
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
    </div>
</el-layout-column>

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
                dataList: [
                    {
                      id: 1,
                      label: '一级 1',
                      children: [
                          {
                            id: 2,
                            label: '一级 1-1'
                          }
                      ]
                    },
                    {
                        id: 4,
                        label: '二级 1',
                    }
                ]
            };
        },
        methods: {
            renderContent(h, { node, data, store }) {
                if(data.id===4){
                    return (
                        <span class="custom-tree-node">
                            <span>{node.label}</span>
                            <span>
                                <el-button size="mini" type="text" on-click={ () => this.append(data) }>Append</el-button>
                                <el-button size="mini" type="text" on-click={ () => this.remove(node, data) }>Delete</el-button>
                            </span>
                        </span>);
                } else {
                    return (
                        <span class="custom-tree-node">
                            <span>{node.label}</span>
                        </span>);
                }
            }
        }
    };
</script>
```
:::

### LayoutColumn Attributes
| 参数      | 说明          | 类型      | 可选值                           | 默认值  |
|---------- |-------------- |---------- |--------------------------------  |-------- |
| title | 标题 | string | - | 菜单管理 |
| noTitle | 是否隐藏标题栏 | boolean | - | true |
| width | 左边宽度设置 | string | - | 260px |

### LayoutColumn Slot
| name | 说明 |
|------|--------|
| title-right | 标题栏功能区域 |
| left | 左边内容区域 |
| right | 右侧内容 |
