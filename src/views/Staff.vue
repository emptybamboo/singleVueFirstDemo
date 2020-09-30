<template>
    <div id="staff" class="flex-column">
        <div id="staff-button-group" class="flex-simple">
            <el-button type="primary" @click="addDialog">新增</el-button>
            <el-button type="danger">删除</el-button>
        </div>
        <basic-table
                @table-edit="tableEdit"
                @table-delete="tableDelete"
                :table-data-arr="currentPageTableData"
        ></basic-table>
        <complete-pagination
        :pagination-data="paginationData"
        @change-current-page="changeCurrentPage"
        @change-page-size="changePageSize"
        ></complete-pagination>
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
                tableData : {},//保存异步传递过来的所有数据
                currentPageTableData : {},//当前页面表格数据,放置当前页码页面该放置的所有表格数据
                paginationData : {
                  totalNum : 1,//数据总条数
                  currentPage : 1,//当前页码
                  pageSizeArr : [10, 20, 30, 40],//所有可选一页多少条数据的选项
                  pageSize : 10,//当前一页显示几条数据
                }
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
                this.tableData = res.data;//传送表格数据
                this.paginationData.totalNum = res.data.list.length;//传送当前数组总条数
                this.currentPageTableData = {
                    list : res.data.list.slice(
                        (this.paginationData.currentPage-1)*this.paginationData.pageSize,
                        this.paginationData.currentPage*this.paginationData.pageSize
                    )//根据页码分割表格数据再传送
                };
              console.log("当前页面表格数据.list↓");
              console.log(this.currentPageTableData);
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
                console.log("点击新增按钮");
                console.log(this.dialogStatus.show);
                console.log("↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑");
                this.dialogStatus.show = true;
                // console.log(this.dialogStatus.show);
            },
            cancelDialog(){
                console.log("点击dialog取消");
              console.log(this.dialogStatus.show);
              this.dialogStatus.show = false;
              console.log(this.dialogStatus.show);
              console.log("↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑");
            },
            commitDialog(){
                console.log("点击dialog确定");
                console.log(this.dialogStatus.show);
                this.dialogStatus.show = false;
              console.log(this.dialogStatus.show);
              console.log("↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑");
            },
            tableEdit(){
              console.log("点击表格编辑");
              console.log("↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑");
                this.dialogStatus.show = true;
            },
            tableDelete(){
              console.log("点击表格删除");
                this.dialogStatus.show = true;
            },
          //切换页码
          changeCurrentPage(page){
            console.log("切换页码");
            console.log(this.tableData.list);
            this.paginationData.currentPage = page;//把传递过来的页码值赋给主组件的页码值
            // this.currentPageTableData.list = this.tableData.list.slice(
            //     (this.paginationData.currentPage-1)*this.paginationData.pageSize,
            //     this.paginationData.currentPage*this.paginationData.pageSize
            // );//根据页码分割表格数据
            this.changeTable();//重新改变页面表格风闸UN改成了这个方法
            console.log(this.tableData.list);
            },
          //切换每页显示条数
          changePageSize(size){
            console.log("进入切换每页显示条数staff方法->changePageSize");
            console.log("传递过来的每页条数"+size);
            console.log(typeof size);
            if(typeof size==="number"){
              this.paginationData.pageSize = size;
              this.changeTable();//重新改变页面表格风闸UN改成了这个方法
            }
          },
          //重新规整当前页面显示数据
          changeTable(){
            this.currentPageTableData.list = this.tableData.list.slice(
                (this.paginationData.currentPage-1)*this.paginationData.pageSize,
                this.paginationData.currentPage*this.paginationData.pageSize
            );//根据页码分割表格数据
          }
        },
    }
</script>

<style scoped>

</style>