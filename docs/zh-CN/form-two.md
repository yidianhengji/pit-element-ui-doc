:::demo 测试
```html
<el-table
        :data="tableData"
        border
        height="300"
        style="width: 100%"
        @cell-click="handleClickRow"
>
    <el-table-column
            type="selection"
            width="55">
    </el-table-column>
    <el-table-column
            prop="name"
            label="姓名">
        <template scope="scope">
            <div v-if="scope.row.isActive">
                <el-input
                        v-model="scope.row.name"
                        ref="input"
                        style="width:100%"
                        size="small"
                        placeholder="请输入"
                        @blur="handleBlur(scope)"
                        @input="handleInput($event,scope)"
                ></el-input>
            </div>
            <span v-else>{{ scope.row.name}}</span>
        </template>
    </el-table-column>
    <el-table-column
            prop="age"
            label="省份">
    </el-table-column>
    <el-table-column
            prop="num2"
            label="市区">
    </el-table-column>
</el-table>

<script>
    export default {
        data() {
            return {
                tableData: [],
                inputChange: true,
                prevRowData: null
            }
        },
        mounted(){
          this.tableData = this.init(10)
        },
        methods: {
            handleClickRow(row, column) {
                if(this.prevRowData){
                    this.$set(this.prevRowData, "isActive", false);
                }
                // this.tableData.map(item => item.isActive = false)
                if (column.label === "姓名") {
                    this.$set(row, "isActive", true);
                    this.$nextTick(() => {
                        this.$refs["input"].focus();
                    });
                    this.prevRowData = row
                }
            },
            handleBlur({ row, column }) {
                if (!this.inputChange) {
                    this.inputChange = false
                    this.$set(row, "isActive", false);
                    return;
                }
                if (column.label === "姓名") {
                    this.inputChange = false
                    this.$set(row, "isActive", false);
                }
            },
            handleInput(el,{row}){
                this.inputChange = true
            },
            init(size) {
                const list = []
                for (let index = 0; index < size; index++) {
                    list.push({
                        id: index,
                        name: `名称${index}`,
                        sex: '0',
                        num: 123,
                        age: 18,
                        num2: 234,
                        rate: 3,
                        address: 'shenzhen'
                    })
                }
                return list;
            }
        }
    }
</script>
```
:::
