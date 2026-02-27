## formBase 增删改查基础组件

抽象基于增删改查系列业务模板，实现快速开发增删改查界面功能。

[comment]: <> (### 全局配置)

[comment]: <> (在引入 Element 时，可以传入一个全局配置对象。按照引入 Element 的方式，具体操作如下：)

[comment]: <> (完整引入 Element：)

[comment]: <> (```js)

[comment]: <> (import Vue from 'vue';)

[comment]: <> (import Element from 'pit-element-ui';)

[comment]: <> (Vue.use&#40;Element, { )

[comment]: <> (  size: 'small',)

[comment]: <> (  zIndex: 3000,)

[comment]: <> (  tableHeadBackground: '#fff', // 用于改变表格表头的颜色值)

[comment]: <> (  isHiddenAddBtn: 'hide', // 用于控制所有的新增按钮是否显示隐藏：默认值：show，可选：show、hide)

[comment]: <> (  isHiddenEditBtn: 'hide', // 用于控制所有的编辑按钮是否显示隐藏：默认值：show，可选：show、hide)

[comment]: <> (  isHiddenDelBtn: 'hide', // 用于控制所有的删除按钮是否显示隐藏：默认值：show，可选：show、hide)

[comment]: <> (  isHiddenBatchBtn: 'hide', // 用于控制所有的批量删除按钮是否显示隐藏：默认值：show，可选：show、hide)

[comment]: <> (  paginationPageSize: 'pageSize', // 分页条数key)

[comment]: <> (  paginationPageNum: 'currentPage', // 分页页码key)

[comment]: <> (  paginationTotal: 'total', // 分页总数key)

[comment]: <> (  dialogTop: '20px', // 设置全局的dialog组件top属性)

[comment]: <> (  buttonArea: 'default', // 默认操作按钮在table上方，值为search显示在搜索区域的右边)

[comment]: <> (  treeItemSize: opts.treeItemSize || 32,)

[comment]: <> (  searchType: opts.searchType || 'colour-10', // 查询按钮类型)

[comment]: <> (  clearType: opts.clearType || 'default', // 清空按钮类型)

[comment]: <> (  addType: opts.addType || 'colour-7', // 新建按钮类型)

[comment]: <> (  updateType: opts.updateType || 'colour-10', // 修改按钮类型)

[comment]: <> (  deleteType: opts.deleteType || 'colour-8', // 删除按钮类型)

[comment]: <> (  batchDelType: opts.batchDelType || 'colour-8', // 批量删除按钮类型)

[comment]: <> (  expandType: opts.expandType || 'default', // 折叠按钮类型)

[comment]: <> (  saveType: opts.saveType || 'colour-10', // 弹窗确定按钮类型)

[comment]: <> (  cancelType: opts.cancelType || 'default' // 弹窗取消按钮类型)

[comment]: <> (}&#41;;)

[comment]: <> (```)

[comment]: <> (### 准备工作)

[comment]: <> (```js)

[comment]: <> (Vue.prototype.request = request                             // 在Vue实例上挂载request请求函数)

[comment]: <> (Vue.prototype.isOk = 200                                    // 在Vue实例上挂载isOk，后台成功状态码，默认为200)

[comment]: <> (Vue.prototype.dialogHeaderClass = 'dialog-header-class'     // 在Vue实例上挂载弹出层dialogHeaderClass，默认为preview-warp)

[comment]: <> (```)

### 基本用法

表格+表单 新增弹窗展示

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
<div>
    <el-button @click="handleClick">随机切换表格数据</el-button>
</div>
<el-form-base
    ref="peFormBase"
    key="peFormBase1"
    primary-key="id"
    row-key="id"
    query-interface="basics/department/page"
    query-one-interface="basics/department/info"
    save-interface="basics/department/save"
    update-interface="basics/department/update"
    delete-interface="basics/department/delete"
    :form-inline="formInline"
    :columns="cloneColumns"
    :add-form="addForm"
    :isDefaultQuery="false"
    :rules="rules"
    :defaultExpandAll="false"
    :noBatchBtn="false"
    :row-single="false"
    :duration="10000"
    :customizeDialogTitle="{
        addTitle: '自定义新增'
    }"
>
    <template slot="searchForm" slot-scope="data">
        <el-form-item label="关键词" prop="keyword">
            <el-select v-model="data.formInline.keyword" placeholder="请选择">
                <el-option
                        v-for="item in options"
                        :key="item.value"
                        :label="item.label"
                        :value="item.value">
                </el-option>
            </el-select>
        </el-form-item>
    </template>
    <template slot="add" slot-scope="data">
        <el-form-item label="部门名称" prop="fullName">
            <el-input v-model="data.addForm.fullName" placeholder="输入名称" />
        </el-form-item>
        <el-form-item label="部门类型" prop="type">
            <el-select v-model="data.addForm.type" placeholder="请选择活动区域" style="width: 100%">
                <el-option label="区域一" value="shanghai"></el-option>
                <el-option label="区域二" value="beijing"></el-option>
            </el-select>
        </el-form-item>
        <el-form-item label="部门编码" prop="enCode">
            <el-input
                    v-model="data.addForm.enCode"
                    placeholder="请选择图标"
                    readonly>
                <el-button slot="append">选择</el-button>
            </el-input>
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
                cloneColumns: [
                    {
                        prop: 'enCode',
                        label: '编码',
                        align: 'left',
                        renderTooltip: (params) => {
                          return `${params.fullName}（${params.enCode}）`
                        }
                    },
                    {
                        prop: 'orderId',
                        label: '排序11',
                        align: 'center',
                        width: 80,
                        renderHeader: "排序22"
                    },
                    {
                        prop: 'createTime',
                        label: '创建时间',
                        align: 'center',
                        width: 160,
                        dateFormatter: 'yyyy年mm月dd',
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
                columns: [],
                formInline: {
                    id: '',
                    keyword: ''
                },
                addForm: {
                    organizeId: '',
                    departmentId: '',
                    description: '',
                    enCode: '1111',
                    type: "shanghai",
                    fullName: '',
                    managerId: '',
                    orderId: 0,
                    parentId: ''
                },
                rules: {
                    type: [
                        { required: true, message: '所属上级不能为空', trigger: 'change' }
                    ],
                    fullName: [
                        { required: true, message: '请输入部门名称', trigger: 'blur' },
                        { max: 50, message: '部门名称最多为50个字符！', trigger: 'blur' }
                    ],
                    enCode: [
                        { required: true, message: '请输入编码', trigger: 'change' },
                    ]
                },
                treeData: [],
                natureData: [],
                industryData: [],
                options: [{
                    value: '选项1',
                    label: '黄金糕'
                }, {
                    value: '选项2',
                    label: '双皮奶',
                    disabled: true
                }, {
                    value: '选项3',
                    label: '蚵仔煎'
                }, {
                    value: '选项4',
                    label: '龙须面'
                }, {
                    value: '选项5',
                    label: '北京烤鸭'
                }],
            };
        },
        methods: {
            handleClick(){
                const indexItem = Math.floor(Math.random() * 10);
                const arr = []
                for(let i=0; i<indexItem; i++){
                    arr.push({
                        prop: 'sortCode',
                        label: i+'',
                        align: 'center',
                    })
                }
                
                const obj = {
                    prop: '',
                    label: '测试1',
                    align: 'center',
                    children: arr
                }
                this.columns = []
                this.columns = JSON.parse(JSON.stringify(this.cloneColumns))
                this.columns.push(obj)
                this.$refs.peFormBase.isShowDataTable = false
                this.$nextTick(()=>{
                    this.$refs.peFormBase.isShowDataTable = true
                })
                console.log(this.columns)
                console.log(this.cloneColumns)
            }
        },
        mounted() {
          this.columns = JSON.parse(JSON.stringify(this.cloneColumns))
          this.$nextTick(()=>{
              this.$refs.peFormBase.tableData = [
                  {
                      "id":"7234458129814e0c9c42bc89652b1fa0",
                      "hasChildren":true,
                      "parentId":"0",
                      "aa": {"bv": 1},
                      "enabled":true,
                      "fullName":"在线开发",
                      "icon":"icon-ym icon-ym-onlineDevelopment",
                      "urlAddress":"",
                      "type":1,
                      "createTime": "2023-10-18 15:10:08",
                      "children":[
                          
                      ],
                      "buttonAuthorize":false,
                      "columnAuthorize":false,
                      "dataAuthorize":false,
                      "formAuthorize":false,
                      "sortCode":1
                  },{
                      "id":"85cd7bca426e49ce83a061bf4611b1447",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "parentIds":null,
                      "level":null,
                      "hasChildren":false,
                      "children":null,
                      "fullName":"功能设计",
                      "buttonAuthorize":true,
                      "columnAuthorize":true,
                      "dataAuthorize":true,
                      "formAuthorize":true,
                      "enCode":"onlineDev",
                      "createTime": "2023-10-18 15:10:08",
                      "icon":"icon-ym icon-ym-webDesign",
                      "urlAddress":"onlineDev/webDesign",
                      "linkTarget":"_self",
                      "type":2,
                      "isData":null,
                      "enabled":true,
                      "sortCode":10,
                      "category":"Web",
                      "description":"应用开发、移动开发、流程表单",
                      "propertyJson":"{\"iconBackgroundColor\":\"#FF3B3B\",\"moduleId\":\"\"}"
                  },
                  {
                      "id":"f6b7ce17cef147b0bb97abdc6c95e12a",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "parentIds":null,
                      "level":null,
                      "hasChildren":false,
                      "children":null,
                      "fullName":"移动设计",
                      "buttonAuthorize":true,
                      "columnAuthorize":true,
                      "dataAuthorize":true,
                      "formAuthorize":true,
                      "enCode":"onlineDev.appDesign",
                      "icon":"icon-ym icon-ym-appDesign",
                      "urlAddress":"onlineDev/appDesign",
                      "linkTarget":"_self",
                      "type":2,
                      "isData":null,
                      "enabled":true,
                      "sortCode":11,
                      "category":"Web",
                      "description":"",
                      "propertyJson":"{\"iconBackgroundColor\":\"#282828\",\"moduleId\":\"\"}"
                  },
                  {
                      "id":"c7159f97177b420d9fc81ecdhdf8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },
                  {
                      "id":"c7159f97177b420d9fchdh81ec8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },{
                      "id":"c7159f97177b420d9fcdhdf81ec8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },
                  {
                      "id":"c7159f97177b420d9fdhdc81ec8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },
                  {
                      "id":"c7159f97177dhdb420d9fc81ec8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },{
                      "id":"c7159f971hdh77b420d9fc81ec8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },{
                      "id":"c7159fdfh97177b420d9fc81ec8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },{
                      "id":"c7159f97177b420ghd9fc81ec8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },{
                      "id":"c7159f97177b42dgda0d9fc81ec8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },{
                      "id":"c7159f97177b420d9fcdfg81ec8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },{
                      "id":"c7159f97177b420d9fsfsc81ec8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },{
                      "id":"c7159f97177b420d9sfsdfc81ec8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },{
                      "id":"c7159f971sdf77b420df9fc81ec8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },{
                      "id":"c7159f97177b420d9dfc81ec8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },{
                      "id":"c7159f97177b420d9fc8131ec8c74ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },{
                      "id":"c7159f97177b420d9fc81ec8c12374ae541b",
                      "parentId":"7234458129814e0c9c42bc89652b1fa0",
                      "fullName":"报表设计",
                      "enCode":"onlineDev.dataReport",
                  },
              ]
          })
        }
    };
</script>
```
:::

### 虚拟表格

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
<el-form-base
    ref="peFormBase"
    key="peFormBase1"
    primary-key="id"
    row-key="id"
    :rowSingle="false"
    :tableLazy="false"
    :isVirtuallyTable="true"
    :tableTransform="true"
    query-interface="basics/department/page"
    query-one-interface="basics/department/info"
    save-interface="basics/department/save"
    update-interface="basics/department/update"
    delete-interface="basics/department/delete"
    :form-inline="formInline"
    :columns="columns"
    :add-form="addForm"
    :noPagination="false"
    :isDefaultQuery="false"
    defaultExpandAll
    :rules="rules"
    :noBatchBtn="false"
    :duration="10000"
    :noSearchFormLeft="false"
    :customizeDialogTitle="{
        addTitle: '自定义新增'
    }"
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
        <el-form-item label="部门类型" prop="type">
            <el-select v-model="data.addForm.type" placeholder="请选择活动区域">
                <el-option label="区域一" value="shanghai"></el-option>
                <el-option label="区域二" value="beijing"></el-option>
            </el-select>
        </el-form-item>
        <el-form-item label="部门编码" prop="enCode">
            <el-input
                    v-model="data.addForm.enCode"
                    placeholder="请选择图标"
                    readonly>
                <el-button slot="append">选择</el-button>
            </el-input>
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
                        prop: 'name',
                        label: '名称',
                        align: 'left',
                        width: '700px',
                        treeNode: true
                    },
                    {
                        prop: 'size',
                        label: '尺寸',
                        align: 'left',
                    },
                    {
                        prop: 'type',
                        label: '类型',
                        align: 'left',
                        showOverflow: true,
                        render: (params) => (
                                <el-tag
                                        type="success"
                                        disable-transitions>
                                    正常
                                </el-tag>
                        ),


                    },
                    {
                        prop: 'date',
                        label: '时间',
                        align: 'center',
                        width: 80,
                        showOverflow: true
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
                    enCode: '1111',
                    type: "shanghai",
                    fullName: '',
                    managerId: '',
                    orderId: 0,
                    parentId: ''
                },
                rules: {
                    type: [
                        { required: true, message: '所属上级不能为空', trigger: 'change' }
                    ],
                    fullName: [
                        { required: true, message: '请输入部门名称', trigger: 'blur' },
                        { max: 50, message: '部门名称最多为50个字符！', trigger: 'blur' }
                    ],
                    enCode: [
                        { required: true, message: '请输入编码', trigger: 'change' },
                    ]
                },
                treeData: [],
                natureData: [],
                industryData: []
            };
        },
        methods: {
            
        },
        mounted() {
          this.$nextTick(()=>{
              this.$refs.peFormBase.tableData = [
                  {
                      id: 110000,
                      parentId: null,
                      name: 'vxe-table test abc1',
                      type: 'mp3',
                      size: 1024,
                      date: '08-01'
                  },
              ]
          })
        }
    };
</script>
```
:::

### 表格+表单

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
                        width: 160,
                        dateFormatter: 'yyyy年mm月dd',
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

### 复杂表头

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

### 懒加载表格

:::demo 在 Form 组件中，每一个表单域由一个 Form-Item 组件构成，表单域中可以放置各种类型的表单控件，包括 Input、Select、Checkbox、Radio、Switch、DatePicker、TimePicker
```html
<el-form-base
    ref="peFormBase"
    primary-key="id"
    row-key="id"
    :row-single="true"
    query-interface="api/system/Area/{id}"
    query-one-interface="api/message/info"
    save-interface="api/message"
    update-interface="api/message"
    delete-interface="api/message"
    :form-inline="formInline"
    :columns="columns"
    :add-form="addForm"
    :rules="rules"
    :isDefaultQuery="false"
    tableLazy
    dialog-top="400px"
    :no-batch-btn="false"
    isBatchDelete
>
</el-form-base>

<script>
    export default {
        data() {
            return {
                operatingTypeProps: 'add',
                columns: [
                    {
                        prop: "fullName",
                        label: "区域名称",
                    },
                    {
                        prop: "enCode",
                        label: "区域编码",
                    },
                    {
                        prop: "sortCode",
                        label: "排序",
                    },
                    {
                        prop: "createdDate",
                        label: "创建时间",
                    },
                ],
                formInline: {
                    id: ''
                },
                addForm: {},
                rules: {},
                treeData: [],
                natureData: [],
                industryData: []
            };
        },
        mounted(){
            this.$nextTick(()=>{
                this.$refs.peFormBase.tableData = [ {
                    "id" : "11",
                    "fullName" : "北京市",
                    "enCode" : "1134",
                    "enabledMark" : 1,
                    "isLeaf" : false,
                    "hasChildren" : true,
                    "sortCode" : 0
                }, {
                    "id" : "12",
                    "fullName" : "天津市",
                    "enCode" : "12",
                    "enabledMark" : 1,
                    "isLeaf" : false,
                    "hasChildren" : true,
                    "sortCode" : 0
                }, {
                    "id" : "13",
                    "fullName" : "河北省",
                    "enCode" : "13",
                    "enabledMark" : 1,
                    "isLeaf" : false,
                    "hasChildren" : true,
                    "sortCode" : 0
                }, {
                    "id" : "14",
                    "fullName" : "山西",
                    "enCode" : "14",
                    "enabledMark" : 1,
                    "isLeaf" : false,
                    "hasChildren" : true,
                    "sortCode" : 0
                }, {
                    "id" : "15",
                    "fullName" : "内蒙古自治区",
                    "enCode" : "15",
                    "enabledMark" : 1,
                    "isLeaf" : false,
                    "hasChildren" : true,
                    "sortCode" : 0
                }, {
                    "id" : "21",
                    "fullName" : "辽宁省",
                    "enCode" : "21",
                    "enabledMark" : 1,
                    "isLeaf" : false,
                    "hasChildren" : true,
                    "sortCode" : 0
                }, {
                    "id" : "22",
                    "fullName" : "吉林省",
                    "enCode" : "22",
                    "enabledMark" : 1,
                    "isLeaf" : false,
                    "hasChildren" : true,
                    "sortCode" : 0
                }, {
                    "id" : "23",
                    "fullName" : "黑龙江省",
                    "enCode" : "23",
                    "enabledMark" : 1,
                    "isLeaf" : false,
                    "hasChildren" : true,
                    "sortCode" : 0
                }, {
                    "id" : "31",
                    "fullName" : "上海市",
                    "enCode" : "31",
                    "enabledMark" : 1,
                    "isLeaf" : false,
                    "hasChildren" : true,
                    "sortCode" : 0
                } ]
            })
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
| isBatchDelete | 是否批量删除(添加了该属性则删除接口的参数就是拼接到请求url后面，否则是在请求body中) | Boolean | - | false |
| noAddBtn | 是否需要新增按钮 | Boolean | - | true |
| noEditBtn | 是否需要编辑按钮 | Boolean | - | true |
| noDelBtn | 是否需要删除按钮 | Boolean | - | true |
| noSearch | 是否需要查询按钮 | Boolean | - | true |
| noClear | 是否需要清空按钮 | Boolean | - | true |
| noBatchBtn | 是否需要批量删除按钮 | Boolean | - | true |
| noConfirmBtn | 是否隐藏弹窗确认按钮 | Boolean | - | true |
| noCancelBtn | 是否隐藏弹窗取消按钮 | Boolean | - | true |
| noSearchForm | 是否隐藏搜索区域 | Boolean | - | true |
| noSearchFormLeft | 是否隐藏搜索区域左边表单部分 | Boolean | - | true |
| noFormInlineBtn | 是否隐藏搜索按钮 | Boolean | - | true |
| noOperation | 是否功能按钮区域 | Boolean | - | true |
| noExpandsBtn | 是否隐藏折叠按钮 | Boolean | - | true |
| noRefreshBtn | 是否隐藏刷新按钮 | Boolean | - | true |
| noTableIndex | 是否隐藏序号 | Boolean | - | true |
| noPagination | 是否隐藏分页 | Boolean | - | true |
| addBtnText | 自定义新增按钮的文本 | string | - | 新增 |
| editBtnText | 自定义修改按钮的文本 | string | - | 修改 |
| delBtnText | 自定义删除按钮的文本 | string | - | 删除 |
| batchBtnText | 自定义批量删除按钮的文本 | string | - | 批量删除 |
| searchBtnText | 自定义插查询按钮的文本 | string | - | 查询 |
| clearBtnText | 自定义清空按钮的文本 | string | - | 清空 |
| confirmBtnText | 自定义确定按钮的文本 | string | - | 确定 |
| dialogWidth | 弹窗的默认宽度 | string | - | 35% |
| dialogBeforeClose | 弹窗关闭前的回调 | Function | - | |
| searchBeforeCallback | 点击查询按钮前回调 | Function | - | |
| addBeforeCallback | 点击新增按钮前回调 | Function | - | |
| infoBeforeCallback | 点击详情按钮前回调 | Function | - | |
| addParamControllerCallback | 新增请求前回调 | Function | - | |
| addParamPromiseControllerCallback | 新建请求前回调并，需return promise 并返回成功结果 | Function | - | |
| addParamControllerPreventCallback | 新建请求前回调并阻止后续的事件 | Function | - | |
| addControllerCallback | 新增/编辑 成功后的回调 | Function | - | |
| delControllerCallback | 删除 成功后的回调 | Function | - | |
| delBeforeCallback | 删除前的回调事件 | Function | - | |
| infoControllerCallback | 详情请求回调成功 | Function | _ | |
| clearCallback | 搜索区域清空前的回调 | Function | - | |
| clearPreventCallback | 搜索区域清空前的回调，要清空需要手动执行 | Function | - | |
| duration | 提示框显示时间, 毫秒。设为 0 则不会自动关闭 | number | - | 3000 |
| textDelStr | 自定义删除提示信息 | string | - | 是否删除这条数据 |
| tableLazy | 是否懒加载子节点数据 | Boolean | - | false |
| lazyKey | 懒加载匹配的key | string | - | 'id' |
| customizeDialogTitle | 自定义弹窗标题 | object | {} | { addTitle: 'xx新建', editTitle: "xx修改", infoTitle: "xx详情" } |
| isDialogVisible | 弹窗面板打开或关闭状态（异步属性:isDialogVisible.sync="isDialogVisible"） | Boolean | - | false |
| defaultExpandAll | 是否展开树形表格 | Boolean | - | true |
| selectionWidth | 多选 Checkbox 宽度  | number | - | 60 |
| selectionAlign | 多选 Checkbox 水平对齐 | string | - | center |
| selectionFixed | 多选 Checkbox 固定 | Boolean/string | - | false |
| indexWidth | 单选 Checkbox 宽度 | number | - | 60 |
| indexAlign | 单选 Checkbox 水平对齐 | string | - | true |
| indexFixed | 单选 Checkbox 固定 | Boolean/string | - | true |
| dialogTop | form-base 弹窗高度 | string | - | - |
| tableTransform | 自动将列表转为树结构（支持虚拟滚动） | Boolean | - | false |
| dialogCustomClass | 自定义弹窗面板class | string | - | - |
| reserveSelection | 仅对 type=selection 的列有效，类型为 Boolean，为 true 则会在数据更新之后保留之前选中的数据（需指定 row-key） | Boolean | - | false |
| isDoubleClickDetails | 是否双击查看详情 | Boolean | - | true |

### FormBase 流程相关属性
| 参数      | 说明          | 类型      | 可选值                           | 默认值  |
|---------- |-------------- |---------- |--------------------------------  |-------- |
| draftInterface   | 保存草稿接口 | string      |                  —                |  — |
| processInterface    | 发起流程接口 | string | — | — |
| draftInterfaceMethod    | 保存草稿接口类型 | string | — | POST |
| processInterfaceMethod | 发起流程接口类型 | string | - | POST |
| hasFlow | 是否开启流程 | Boolean | - | false |
| flowCode | 流程对应的编码 | string | - | - |
| noStorageBtn | 是否隐藏暂存按钮 | Boolean | - | false |
| noDraftBtn | 是否隐藏保存草稿按钮 | Boolean | - | true |
| noInitiateProcessBtn | 是否隐藏发起流程按钮 | Boolean | - | true |
| storageBtnText | 自定义暂存按钮的文本 | string | - | 暂存 |
| draftBtnText | 自定义保存草稿按钮的文本 | string | - | 保存草稿 |
| initiateProcessBtnText | 自定义发起流程按钮的文本 | string | - | 发起流程 |
| draftParamControllerCallback | 保存草稿请求前回调 | Function | - | |
| draftParamPromiseControllerCallback | 保存草稿请求前回调，需return promise 并返回成功结果 | Function | - | |
| draftParamControllerPreventCallback | 保存草稿请求前回调，没有需要处理返回值，不会往下执行 | Function | - | |
| draftControllerCallback | 保存草稿请求成功回调 | Function | - | |
| draftControllerErrorCallback | 保存草稿请求失败回调 | Function | - | |
| processParamControllerCallback | 发起流程请求前回调 | Function | - | |
| processParamPromiseControllerCallback | 发起流程请求前回调，需return promise 并返回成功结果 | Function | - | |
| processParamControllerPreventCallback | 发起流程请求前回调，没有需要处理返回值，不会往下执行 | Function | - | |
| processControllerCallback | 发起流程请求成功回调 | Function | - | |
| processControllerErrorCallback | 发起流程请求失败回调 | Function | - | |
| processRunFlowParamsControllerCallback | 流程发起前需要处理的参数 | Function | - | |
| textDraftSuccess | 流程保存成功提示 | string | - | |
| textProcessSuccess | 流程发起成功提示 | string | - | |
| storageCallback | 暂存按钮回调事件 | Function | - | |
| processCancelCallback | 弹窗取消按钮回调事件 | Function | - | |
| storageControllerPreventCallback | 暂存按钮请求前回调，没有需要处理返回值，不会往下执行 | Function | - | |
| beforeFlowBoxCallback | 预览流程前的回调事件 | Function | - | |
| flowStatusWidth | 流程状态下拉框宽度 | Number | - | |

### FormBase Slot
| name | 说明 |
|------|--------|
| searchForm | 搜索的表单区域 |
| toolbar | 功能按钮 |
| addWarp | 新增编辑的表单区域 |
| footer | 弹窗功能按钮区域 |
| tabList | tab切换区域 |
| table | 主体区域 |
| tableWarp | 主体区域+分页 |
| dialog-footer-left | 弹窗左侧按钮区域 |
| dialog-footer-center | 弹窗中间按钮区域 |
| dialog-footer-right | 弹窗右侧按钮区域 |

### FormBase 功能说明
| 方法名称 | 说明 |
|------|--------|
| query | 查询列表方法 |
| search | 查询列表方法（会把pageNum设置为1） |
| add | 打开新增面板方法 |
| info | 查询详情方法（primaryKey主键id）|
| details | 查询详情方法（primaryKey主键id）（会给整个表单设置为禁用状态）|
| delete | 删除方法（ids主键数组） |
| refreshBtn | 重定向当前页面（起到刷新当前页面作用） |
| doLayout | 对 Table 进行重新布局。当 Table 或其祖先元素由隐藏切换为显示时，可能需要调用此方法 |

### FormBase 事件说明
| 方法名称 | 说明 | 参数 |
|------|--------|--------|
| table-selection-change | 当选择项发生变化时会触发该事件 | selection |
| table-current-change | 当表格的当前行发生变化的时候会触发该事件，如果要高亮当前行，请打开表格的 highlight-current-row 属性 | currentRow |
| resultTableBack | 查询接口请求成功的回调，返回列表的数据 | tableData |
| table-row-dblclick | 当某一行被双击时会触发该事件 | row, column, event |

### FormBase columns字段说明
| 字段 | 说明 | 类型 | 可选值 | 默认值 |
|------|--------|-------|-------|-------|
| prop | 对应列内容的字段名 | string | - | - |
| label | 显示的标题 | string | - | - |
| align | 对齐方式 | string | left/center/right | left |
| width | 对应列的宽度 | string | - | - |
| sortable | 对应列是否可以排序 | boolean, string | true, false, 'custom' | false |
| render | 自定义列内容的函数（建议使用jsx语法） | Function | - | rows |
| expand | 自定义展开行的函数 | Function | - | rows |
| renderH | 自定义列内容的函数 | Function | - | rows |
| showOverflow | 当内容过长被隐藏时显示 tooltip | Boolean | - | false |
| fixed | 列是否固定在左侧或者右侧 | string | boolean | left, right | false |
| hidden | 对应列内容显示隐藏，多数用于权限控制 | boolean | - | true |
| children | 递归数据，用于复杂表头 | array | - | - |
| dateFormatter | 时间格式化 | string | - | - |
| renderTooltip | 自定义提示信息 | Function | - | - |
| isLink | 给单元格文字加蓝色 | boolean | - | false |
| isDetails | 查看详情，如果开启了流程查看流程，否则查看详情 | boolean | - | false |
