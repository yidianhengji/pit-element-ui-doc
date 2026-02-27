## VirtualSelectTree 虚拟下拉树

当选项数据量过多时，使用虚拟下拉菜单展示并选择内容。

### 单选+可清空
适用广泛的基础单选，主要传入 options 数据源。

:::demo
```html
<div>
    <el-button type="text" @click="dialogVisible = true">点击打开 Dialog</el-button>

    <el-dialog
            title="提示"
            :visible.sync="dialogVisible"
            width="30%"
            top="400px">
        <el-virtual-select-tree :props="{
                      label: 'fullName',
                      value: 'id',
                      children: 'children',
                      disabled: 'disabled',
                    }" filterable style="width: 100%" v-model="value" :options="items" clearable placeholder="基础用法" />
        <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
  </span>
    </el-dialog>
    
</div>

<script>
    export default {
        data() {
            return {
                dialogVisible: false,
                items: [],
                value: "0-9-0-1"
            };
        },
        created(){
          setTimeout(()=>{
            this.items = [{"fullName":"顶级节点","hasChildren":true,"id":"0","children":[{"id":"1762669651030937600","parentId":"0","parentIds":null,"level":null,"hasChildren":false,"relationId":"1762669651030937600","companyCode":"SJLHT","companyType":null,"companyTypeName":"","shortName":"","fullName":"设计联合体","phone":null,"createdDate":"2024-02-28 10:42:48","createdBy":null,"lastModifiedDate":null,"lastModifiedBy":null,"enabled":true,"deleted":null,"sort":null,"remark":null,"enCode":"SJLHT","description":"","propertyJson":{"shortName":"","webSite":"","industry":"","foundedTime":"","address":"","managerName":"","managerTelePhone":"","managerMobilePhone":"","manageEmail":"","bankName":"","bankAccount":"","businessscope":"","enterpriseNature":"","fax":"","telePhone":""},"sortCode":0,"icon":null,"createdByName":null,"lastModifiedByName":null,"unitId":null,"bidId":null,"bidName":null,"addType":null,"bidCategoryName":null,"unitType":null,"associatedCompanies":null,"thirdPartyCode":null,"treeUniqueIdentifier":null,"isConsortium":null,"consortiumMember":null},{"id":"1750701295431647232","parentId":"0","parentIds":null,"level":null,"hasChildren":false,"relationId":"1750701295431647232","companyCode":"GXLQ","companyType":null,"companyTypeName":"","shortName":"","fullName":"广西路桥工程集团有限公司","phone":null,"createdDate":"2024-01-26 10:04:50","createdBy":null,"lastModifiedDate":null,"lastModifiedBy":null,"enabled":true,"deleted":null,"sort":null,"remark":null,"enCode":"GXLQ","description":"","propertyJson":{"shortName":"","webSite":"","industry":"","foundedTime":"","address":"","managerName":"","managerTelePhone":"","managerMobilePhone":"","manageEmail":"","bankName":"","bankAccount":"","businessscope":"","enterpriseNature":"","fax":"","telePhone":""},"sortCode":0,"icon":null,"createdByName":null,"lastModifiedByName":null,"unitId":null,"bidId":null,"bidName":null,"addType":null,"bidCategoryName":null,"unitType":null,"associatedCompanies":null,"thirdPartyCode":null,"treeUniqueIdentifier":null,"isConsortium":null,"consortiumMember":null},{"id":"1704678803321708544","parentId":"0","parentIds":null,"level":null,"hasChildren":false,"relationId":"1704678803321708544","companyCode":"HBGX","companyType":null,"companyTypeName":"","shortName":"环北公司","fullName":"环北部湾广西水资源配置有限公司","phone":null,"createdDate":"2023-09-21 10:07:53","createdBy":null,"lastModifiedDate":null,"lastModifiedBy":null,"enabled":true,"deleted":null,"sort":null,"remark":null,"enCode":"HBGX","description":"","propertyJson":{"shortName":"环北公司","webSite":"","industry":"","foundedTime":"","address":"","managerName":"","managerTelePhone":"","managerMobilePhone":"","manageEmail":"","bankName":"","bankAccount":"","businessscope":"","enterpriseNature":"","fax":"","telePhone":""},"sortCode":1,"icon":null,"createdByName":null,"lastModifiedByName":null,"unitId":null,"bidId":null,"bidName":null,"addType":null,"bidCategoryName":null,"unitType":null,"associatedCompanies":null,"thirdPartyCode":null,"treeUniqueIdentifier":null,"isConsortium":null,"consortiumMember":null},{"id":"1704679790744756224","parentId":"0","parentIds":null,"level":null,"hasChildren":false,"relationId":"1704679790744756224","companyCode":"GXSDY","companyType":null,"companyTypeName":"","shortName":"广西水电设计院","fullName":"广西壮族自治区水利电力勘测设计研究院有限责任公司","phone":null,"createdDate":"2023-09-21 10:11:48","createdBy":null,"lastModifiedDate":null,"lastModifiedBy":null,"enabled":true,"deleted":null,"sort":null,"remark":null,"enCode":"GXSDY","description":"","propertyJson":{"shortName":"广西水电设计院","webSite":"","industry":"","foundedTime":"","address":"","managerName":"","managerTelePhone":"","managerMobilePhone":"","manageEmail":"","bankName":"","bankAccount":"","businessscope":"","enterpriseNature":"","fax":"","telePhone":""},"sortCode":201,"icon":null,"createdByName":null,"lastModifiedByName":null,"unitId":null,"bidId":null,"bidName":null,"addType":null,"bidCategoryName":null,"unitType":null,"associatedCompanies":null,"thirdPartyCode":null,"treeUniqueIdentifier":null,"isConsortium":null,"consortiumMember":null,"children":null},{"id":"1704679889751302144","parentId":"0","parentIds":null,"level":null,"hasChildren":false,"relationId":"1704679889751302144","companyCode":"ZSZJSJY","companyType":null,"companyTypeName":"","shortName":"中水珠江设计院","fullName":"中水珠江规划勘测设计有限公司","phone":null,"createdDate":"2023-09-21 10:12:12","createdBy":null,"lastModifiedDate":null,"lastModifiedBy":null,"enabled":true,"deleted":null,"sort":null,"remark":null,"enCode":"ZSZJSJY","description":"","propertyJson":{"shortName":"中水珠江设计院","webSite":"","industry":"","foundedTime":"","address":"","managerName":"","managerTelePhone":"","managerMobilePhone":"","manageEmail":"","bankName":"","bankAccount":"","businessscope":"","enterpriseNature":"","fax":"","telePhone":""},"sortCode":202,"icon":null,"createdByName":null,"lastModifiedByName":null,"unitId":null,"bidId":null,"bidName":null,"addType":null,"bidCategoryName":null,"unitType":null,"associatedCompanies":null,"thirdPartyCode":null,"treeUniqueIdentifier":null,"isConsortium":null,"consortiumMember":null},{"id":"1704679394215256064","parentId":"0","parentIds":null,"level":null,"hasChildren":false,"relationId":"1704679394215256064","companyCode":"YJGSJLDW","companyType":null,"companyTypeName":"","shortName":"应急监理","fullName":"应急供水工程施工监理单位","phone":null,"createdDate":"2023-09-21 10:10:14","createdBy":null,"lastModifiedDate":null,"lastModifiedBy":null,"enabled":true,"deleted":null,"sort":null,"remark":null,"enCode":"YJGSJLDW","description":"","propertyJson":{"shortName":"应急监理","webSite":"","industry":"","foundedTime":"","address":"","managerName":"","managerTelePhone":"","managerMobilePhone":"","manageEmail":"","bankName":"","bankAccount":"","businessscope":"","enterpriseNature":"","fax":"","telePhone":""},"sortCode":301,"icon":null,"createdByName":null,"lastModifiedByName":null,"unitId":null,"bidId":null,"bidName":null,"addType":null,"bidCategoryName":null,"unitType":null,"associatedCompanies":null,"thirdPartyCode":null,"treeUniqueIdentifier":null,"isConsortium":null,"consortiumMember":null},{"id":"1704679121916846080","parentId":"0","parentIds":null,"level":null,"hasChildren":false,"relationId":"1704679121916846080","companyCode":"ZGAN","companyType":null,"companyTypeName":"","shortName":"中国安能","fullName":"中国安能集团第一工程局有限公司","phone":null,"createdDate":"2023-09-21 10:09:09","createdBy":null,"lastModifiedDate":null,"lastModifiedBy":null,"enabled":true,"deleted":null,"sort":null,"remark":null,"enCode":"ZGAN","description":"","propertyJson":{"shortName":"中国安能","webSite":"","industry":"","foundedTime":"","address":"","managerName":"","managerTelePhone":"","managerMobilePhone":"","manageEmail":"","bankName":"","bankAccount":"","businessscope":"","enterpriseNature":"","fax":"","telePhone":""},"sortCode":401,"icon":null,"createdByName":null,"lastModifiedByName":null,"unitId":null,"bidId":null,"bidName":null,"addType":null,"bidCategoryName":null,"unitType":null,"associatedCompanies":null,"thirdPartyCode":null,"treeUniqueIdentifier":null,"isConsortium":null,"consortiumMember":null},{"id":"1704679200467771392","parentId":"0","parentIds":null,"level":null,"hasChildren":false,"relationId":"1704679200467771392","companyCode":"ZTSJ","companyType":null,"companyTypeName":"","shortName":"中铁十局","fullName":"中铁十局集团有限公司","phone":null,"createdDate":"2023-09-21 10:09:27","createdBy":null,"lastModifiedDate":null,"lastModifiedBy":null,"enabled":true,"deleted":null,"sort":null,"remark":null,"enCode":"ZTSJ","description":"","propertyJson":{"shortName":"中铁十局","webSite":"","industry":"","foundedTime":"","address":"","managerName":"","managerTelePhone":"","managerMobilePhone":"","manageEmail":"","bankName":"","bankAccount":"","businessscope":"","enterpriseNature":"","fax":"","telePhone":""},"sortCode":402,"icon":null,"createdByName":null,"lastModifiedByName":null,"unitId":null,"bidId":null,"bidName":null,"addType":null,"bidCategoryName":null,"unitType":null,"associatedCompanies":null,"thirdPartyCode":null,"treeUniqueIdentifier":null,"isConsortium":null,"consortiumMember":null},{"id":"1704679302456467456","parentId":"0","parentIds":null,"level":null,"hasChildren":false,"relationId":"1704679302456467456","companyCode":"ZTSBJ","companyType":null,"companyTypeName":"","shortName":"中铁十八局","fullName":"中铁十八局集团有限公司","phone":null,"createdDate":"2023-09-21 10:09:52","createdBy":null,"lastModifiedDate":null,"lastModifiedBy":null,"enabled":true,"deleted":null,"sort":null,"remark":null,"enCode":"ZTSBJ","description":"","propertyJson":{"shortName":"中铁十八局","webSite":"","industry":"","foundedTime":"","address":"","managerName":"","managerTelePhone":"","managerMobilePhone":"","manageEmail":"","bankName":"","bankAccount":"","businessscope":"","enterpriseNature":"","fax":"","telePhone":""},"sortCode":403,"icon":null,"createdByName":null,"lastModifiedByName":null,"unitId":null,"bidId":null,"bidName":null,"addType":null,"bidCategoryName":null,"unitType":null,"associatedCompanies":null,"thirdPartyCode":null,"treeUniqueIdentifier":null,"isConsortium":null,"consortiumMember":null},{"id":"1712678114101850112","parentId":"0","parentIds":null,"level":null,"hasChildren":false,"relationId":"1712678114101850112","companyCode":"HBJGXMZ","companyType":null,"companyTypeName":"","shortName":"建管项目组","fullName":"环北建管系统研发项目组","phone":null,"createdDate":"2023-10-13 11:54:17","createdBy":null,"lastModifiedDate":null,"lastModifiedBy":null,"enabled":true,"deleted":null,"sort":null,"remark":null,"enCode":"HBJGXMZ","description":"","propertyJson":{"shortName":"建管项目组","webSite":"","industry":"","foundedTime":"1697126400000","address":"","managerName":"","managerTelePhone":"","managerMobilePhone":"","manageEmail":"","bankName":"","bankAccount":"","businessscope":"","enterpriseNature":"","fax":"","telePhone":"18260860662"},"sortCode":9999,"icon":null,"createdByName":null,"lastModifiedByName":null,"unitId":null,"bidId":null,"bidName":null,"addType":null,"bidCategoryName":null,"unitType":null,"associatedCompanies":null,"thirdPartyCode":null,"treeUniqueIdentifier":null,"isConsortium":null,"consortiumMember":null}]}]
          },2000)
        }
    };
</script>
```
:::

### 单选+可清空+可搜索
适用广泛的基础单选，主要传入 options 数据源。

:::demo
```html
<div>
    <el-virtual-select-tree style="width: 100%" v-model="value" filterable :options="items" clearable placeholder="基础用法" />
</div>

<script>
    function dig(path = '0', level = 2) {
        const list = [];
        for (let i = 0; i < 10; i += 1) {
            const key = `${path}-${i}`;
            const treeNode = {
                label: key + '-'+  Math.round( Math.random()*1000),
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
                items: dig(),
                value: ""
            };
        },
    };
</script>
```
:::

### 多选 collapse-tags 可清空
适用广泛的基础多选，主要传入 options 数据源。

:::demo
```html
<div>
    <el-virtual-select-tree style="width: 100%" multiple collapse-tags v-model="value" :options="items" clearable placeholder="基础用法" />
</div>

<script>
    function dig(path = '0', level = 2) {
        const list = [];
        for (let i = 0; i < 10; i += 1) {
            const key = `${path}-${i}`;
            const treeNode = {
                label: key + '这是label',
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
                items: dig(),
                value: ""
            };
        },
    };
</script>
```
:::

### 多选 可清空
适用广泛的基础多选，主要传入 options 数据源。

:::demo
```html
<div>
    <el-virtual-select-tree style="width: 100%" multiple v-model="value" :options="items" clearable placeholder="基础用法" />
</div>

<script>
    function dig(path = '0', level = 2) {
        const list = [];
        for (let i = 0; i < 10; i += 1) {
            const key = `${path}-${i}`;
            const treeNode = {
                label: key + '这是label',
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
                items: dig(),
                value: ""
            };
        },
    };
</script>
```
:::

### 多选 collapse-tags 可清空 搜索
适用广泛的基础多选，主要传入 options 数据源。

:::demo
```html
<div>
    <el-virtual-select-tree style="width: 100%" multiple collapse-tags filterable v-model="value" :options="items" clearable placeholder="基础用法" />
</div>

<script>
    function dig(path = '0', level = 2) {
        const list = [];
        for (let i = 0; i < 10; i += 1) {
            const key = `${path}-${i}`;
            const treeNode = {
                label: key + '-'+  Math.round( Math.random()*1000),
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
                items: dig(),
                value: ""
            };
        },
    };
</script>
```
:::

### 多选 搜素 可清空
适用广泛的基础多选，主要传入 options 数据源。

:::demo
```html
<div>
    {{value}}
    <el-virtual-select-tree style="width: 100%" multiple filterable v-model="value" :options="items" clearable placeholder="基础用法" />
</div>

<script>
    function dig(path = '0', level = 2) {
        const list = [];
        for (let i = 0; i < 10; i += 1) {
            const key = `${path}-${i}`;
            const treeNode = {
                label: key + '-'+  Math.round( Math.random()*1000),
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
                items: dig(),
                value: ""
            };
        },
    };
</script>
```
:::

<div style="height: 500px;"></div>
<br>
<br>
<br>
<br>
<br>
<br>
