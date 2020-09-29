<template>
    <div id="staff" class="flex-column">
        <div id="staff-button-group" class="flex-simple">
            <el-button type="primary" @click="addDialog">新增</el-button>
            <el-button type="danger">删除</el-button>
        </div>
        <basic-table
                @table-edit="tableEdit"
                @table-delete="tableDelete"
                :table-data-arr="tableData"
        ></basic-table>
        <complete-pagination></complete-pagination>
        <form-dialog :dialog-status="dialogStatus"
        @close-dialog="cancelDialog"
        @commit-dialog="commitDialog"
        ></form-dialog>
    </div>
</template>

<script>
    import BasicTable from "../components/main/table/BasicTable";
    import CompletePagination from "../components/main/pagination/CompletePagination";
    import FormDialog from "../components/main/dialog/FormDialog";

    export default {
        name: "Staff",
        data(){
            return{
                dialogStatus : {
                    show : false,
                },
                tableData : {},
            }
        },
        mounted(){

            //上下两个url必须一致,这样将来只需要删掉mock代码即可
            this.$axios.request({
                'method' : 'get',
                'url' : 'http://lzc.com/staff',
            }).then(res => {
                console.log(res.data);
                console.log("mock生成数据类型->"+typeof res.data);
                console.log("mock生成数据↓");
                console.log(res.data);
                console.log("mock生成数据.list↓");
                console.log(res.data.list);
                this.tableData = res.data;//传送
            });

            const Random = this.$Mock.Random;
            const data = this.$Mock.mock('http://lzc.com/staff',{
                'list|100-200' : [
                    {
                        'id|+1' : 1,
                        "name" : "@cname",
                        "phone" : Random.string(11),
                        "age|1-100" : 100,
                        "gender" : "@boolean",
                        "time" : "@date('yyyy-MM-dd')",
                    }
                ]
            });

        },
        components : {
            BasicTable,
            CompletePagination,
            FormDialog
        },
        methods : {
            addDialog(){
                console.log(this.dialogStatus.show);
                this.dialogStatus.show = true;
                // console.log(this.dialogStatus.show);
            },
            cancelDialog(){
                console.log("点击取消");
                this.dialogStatus.show = false;
            },
            commitDialog(){
                console.log("点击确定");
                this.dialogStatus.show = false;
            },
            tableEdit(){
                this.dialogStatus.show = true;
            },
            tableDelete(){
                this.dialogStatus.show = true;
            },
        },
    }
</script>

<style scoped>

</style>