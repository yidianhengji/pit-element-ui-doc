## LayoutTree 用于LayoutColumn左边树

默认采用了虚拟树，懒加载不变

### 基本用法

树形+表格布局

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
<el-layout-column>
    <div slot="left" class="h-full" style="height: 100%;">
        <el-layout-tree :dataList="dataList" label="label2" node-key="projectId" :default-expand-all="false"></el-layout-tree>
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
            function dig(path = '0', level = 2) {
                const list = [];
                for (let i = 0; i < 10; i += 1) {
                    const key = `${path}-${i}`;
                    const treeNode = {
                        label2: key,
                        key,
                        projectId: key
                    };

                    if (level > 0) {
                        treeNode.children = dig(key, level - 1);
                    }

                    list.push(treeNode);
                }
                return list;
            }
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
                dataList: dig(),
            };
        },
    };
</script>
```
:::

树形+表格布局+搜索提交

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
<el-layout-column>
    <div slot="left" class="h-full" style="height: 100%;">
        <el-layout-tree :dataList="dataList" noSearchInput></el-layout-tree>
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
            function dig(path = '0', level = 2) {
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
                dataList: dig()
            };
        },
    };
</script>
```
:::

自定义图标

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
<el-layout-column>
    <div slot="left" class="h-full" style="height: 100%;">
        <el-layout-tree :dataList="dataList">
          <template slot-scope="{ data }">
              <i v-if="data.type === 'type1'" class="el-icon-basics-jigou"></i>
              <i v-else class="el-icon-basics-kehu"></i>
              <span class="text">{{ data.label }}</span>
          </template>
        </el-layout-tree>
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
            function dig(path = '0', level = 2) {
                const list = [];
                for (let i = 0; i < 10; i += 1) {
                    const key = `${path}-${i}`;
                    const treeNode = {
                        label: key,
                        key,
                        id: key,
                        type: i % 10 ? 'type1' : 'type2'
                    };

                    if (level > 0) {
                        treeNode.children = dig(key, level - 1);
                    }

                    list.push(treeNode);
                }
                return list;
            }
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
                dataList: dig()
            };
        },
    };
</script>
```
:::

指定一个固定的图标及颜色

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
<el-layout-column>
    <div slot="left" class="h-full" style="height: 100%;">
        <el-layout-tree :dataList="dataList" icon="el-icon-notebook-2" iconColor="red"></el-layout-tree>
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
            function dig(path = '0', level = 2) {
                const list = [];
                for (let i = 0; i < 10; i += 1) {
                    const key = `${path}-${i}`;
                    const treeNode = {
                        label: key,
                        key,
                        id: key,
                        type: i % 10 ? 'type1' : 'type2'
                    };

                    if (level > 0) {
                        treeNode.children = dig(key, level - 1);
                    }

                    list.push(treeNode);
                }
                return list;
            }
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
                dataList: dig()
            };
        },
    };
</script>
```
:::

### LayoutTree 属性
| 参数      | 说明          | 类型      | 可选值                           | 默认值  |
|---------- |-------------- |---------- |--------------------------------  |-------- |
| dataList | 静态数据，用于默认不调用接口使用 | array | - | [] |
| nodeKey | 主键字段 | string | - | id |
| queryTreeInterface | 树形接口地址 | string | - | - |
| queryTreeInterfaceMethod | 树形接口类型 | string | - | GET |
| queryTreeParams | 树形接口参数 | object | - | {} |
| expandOnClickNode | 是否在点击节点的时候展开或者收缩节点， 默认值为 true，如果为 false，则只有点箭头图标的时候才会展开或者收缩节点。 | boolean | - | false |
| defaultExpandAll | 是否默认展开所有节点 | boolean | - | true |
| label | 指定节点标签为节点对象的某个属性值 | string | - | label |
| isTooltip | 是否显示文字提示，用于数据过多场景 | boolean | - | false |
| placement | Tooltip 的出现位置 | string | top/top-start/top-end/bottom/bottom-start/bottom-end/left/left-start/left-end/right/right-start/right-end | top |
| icon | 默认显示的图标 | string | - | el-icon-document |
| noSearchInput | 是否显示输入框 | boolean | - | false |
| filterPlaceholder | 输入框占位符 | string | - | 请输入 |
| noFilterInput | 执行input事件直接查询 | boolean | - | false |
| isRequestQuery | 是否使用服务端查询 | boolean | - | false |
| beforeSearchCallback | 输入框事件前回调 | Function | - | - |
| render-content | 树节点的内容区的渲染 Function | Function(h, { node, data, store } | - | - |
| clearable | 是否可清空 | boolean | - | true |
| lazy | 是否懒加载子节点数据 | boolean | - | false |
| lazyKey | 懒加载匹配的key | string | - | id |
| isDefaultQuery | 是否默认查询 | boolean | - | true |
| showCheckbox | 是否显示复选框 | boolean | - | false |
| forceOnSelectAll | 是否强制开启全部 | boolean | - | true |
| expandedLevel | 如果defaultExpandAll为false，则展开的级别生效，并且默认为1级 | number | - | 1 |
| showExpandIcon | 是否展开搜索区域图标 | boolean | - | true |

### LayoutTree 方法
| 方法名 | 说明 | 参数 |
|------|--------|--------|
| setCurrentKey | 通过 key 设置某个节点的当前选中状态，使用此方法必须设置 node-key 属性 | (key) 待被选节点的 key，若为 null 则取消当前高亮的节点 |
| setDataList | 赋值 | data |

### LayoutTree 事件
| 事件名称 | 说明 | 参数 |
|------|--------|--------|
| node-click | 节点被点击时的回调 | data |
| resultBack | 请求回调的事件 | dataTreeList |

### LayoutTree Slot
| name | 说明 |
|------|--------|
| right | 右侧内容 |
