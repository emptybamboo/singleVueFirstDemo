<template>
    <el-table
            :data="tableData"
            style="width: 100%"
            @selection-change="handleSelectionChange">
        <el-table-column
                type="selection"
                width="55">
        </el-table-column>
        <el-table-column
                label="日期"
                width="180">
            <template slot-scope="scope">
                <i class="el-icon-time"></i>
                <span style="margin-left: 10px">{{ scope.row.time }}</span>
            </template>
        </el-table-column>
        <el-table-column
                label="姓名"
                width="180">
            <template slot-scope="scope">
                <el-popover trigger="hover" placement="top">
                    <p>姓名: {{ scope.row.name }}</p>
                    <p>住址: {{ scope.row.address }}</p>
                    <div slot="reference" class="name-wrapper">
                        <el-tag size="medium">{{ scope.row.name }}</el-tag>
                    </div>
                </el-popover>
            </template>
        </el-table-column>
        <el-table-column label="操作">
            <template slot-scope="scope">
                <el-button
                        size="mini"
                        @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                <el-button
                        size="mini"
                        type="danger"
                        @click="handleDelete(scope.$index, scope.row)">删除</el-button>
            </template>
        </el-table-column>
    </el-table>
</template>

<script>
    export default {
        name: "BasicTable",
        data() {
            return {
                // tableData: [{
                //     date: '2016-05-02',
                //     name: '王小虎',
                //     address: '上海市普陀区金沙江路 1518 弄'
                // }, {
                //     date: '2016-05-04',
                //     name: '王小虎',
                //     address: '上海市普陀区金沙江路 1517 弄'
                // }, {
                //     date: '2016-05-01',
                //     name: '王小虎',
                //     address: '上海市普陀区金沙江路 1519 弄'
                // }, {
                //     date: '2016-05-03',
                //     name: '王小虎',
                //     address: '上海市普陀区金沙江路 1516 弄'
                // }],
                tableData: []
            }
        },
        props : {
            tableDataArr : {
                type : Object,
                default : {},
            }
        },
        watch:{
          "tableDataArr.list" : {
            handler(newVal,oldVal){
              if(newVal){
                console.log('我接收到了表格数据！');
                this.tableData = newVal;
              }
            },
            immediate: true,
          },
          },

        methods: {
            handleEdit(index, row) {
                // console.log(index, row);
                this.$emit("table-edit",index,row);
            },
            handleDelete(index, row) {
                // console.log(index, row);
                this.$emit("table-delete",index,row);
            },
            handleSelectionChange(val) {
                this.multipleSelection = val;
            }
        }
    }
</script>

<style scoped>

</style>