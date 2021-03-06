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
        >
                  <!--通过slot插入到表格组件的部分-->
                  <el-table-column
                      type="selection"
                      width="55">
                  </el-table-column>
                  <el-table-column
                          label="日期"
                          >
                      <template slot-scope="scope">
                          <i class="el-icon-time"></i>
                          <span style="margin-left: 10px">{{ scope.row.time }}</span>
                      </template>
                  </el-table-column>
                  <el-table-column
                          label="姓名"
                          >
                      <template slot-scope="scope">
                          <el-popover trigger="hover" placement="top">
                              <p>姓名: {{ scope.row.name }}</p>
                              <div slot="reference" class="name-wrapper">
                                  <el-tag size="medium">{{ scope.row.name }}</el-tag>
                              </div>
                          </el-popover>
                      </template>
                  </el-table-column>
                  <el-table-column
                      label="年龄"
                      >
                    <template slot-scope="scope">
                        <p>{{ scope.row.age }}</p>
                    </template>
                  </el-table-column>
                  <el-table-column
                      label="性别"
                      >
                    <template slot-scope="scope">
                      <p>{{ scope.row.gender===false?"男":"女" }}</p>
                    </template>
                  </el-table-column>
                  <el-table-column
                      label="手机号代码"
                      >
                    <template slot-scope="scope">
                      <p>{{ scope.row.phone }}</p>
                    </template>
                  </el-table-column>
        </basic-table>
        <complete-pagination
        :pagination-data="paginationData"
        @change-current-page="changeCurrentPage"
        @change-page-size="changePageSize"
        ></complete-pagination>
        <form-dialog :dialog-status="dialogStatus"
        @close-dialog="cancelDialog"
        @commit-dialog="commitDialog"
        :form-title="formTitle"
        >
          <!--使用slot插入弹出层的表单-->
          <el-form :model="form"  :label-position="labelPosition" ref="staffForm">
            <el-form-item label="日期" :label-width="formLabelWidth">
              <el-date-picker
                  v-model="form.time"
                  type="date"
                  style="width: 100%"
                  placeholder="选择日期">
              </el-date-picker>
            </el-form-item>
            <el-form-item label="姓名" :label-width="formLabelWidth">
              <el-input v-model="form.name" clearable></el-input>
            </el-form-item>
            <el-form-item label="年龄" :label-width="formLabelWidth">
              <el-input v-model="form.age" clearable></el-input>
            </el-form-item>
            <el-form-item label="性别" :label-width="formLabelWidth">
              <el-radio v-model="form.gender" :label="false">男</el-radio>
              <el-radio v-model="form.gender" :label="true">女</el-radio>
            </el-form-item>
            <el-form-item label="手机号" :label-width="formLabelWidth">
              <el-input v-model="form.phone" clearable></el-input>
            </el-form-item>
          </el-form>
        </form-dialog>
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
                },
                //新增编辑弹窗属性
                form: {
                  name: '',
                  age: '',
                  time: '',
                  gender: false,
                  phone: '',
                },
                formLabelWidth: '80px',
                formTitle : "",
                labelPosition : 'right',//dialog中每一行的对齐方式
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
                        "phone" : ()=>Random.string(11),
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

                this.form = {};
                this.formTitle = "新增员工";
                this.dialogStatus.show = true;
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
                //如果当前表单属性中存在id说明是编辑
                if('id' in this.form){
                  console.log("dialog编辑数据");
                  console.log(this.form);
                }else{//否则是新增
                  console.log("dialog新增数据");
                  console.log(this.form);
                }
            },
            tableEdit(index,row){
              console.log("点击表格编辑");
              console.log("↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑");
              console.log(index);
              console.log(row);
              this.formTitle = "编辑员工";
              //先把当前行数据代入弹出层数据form中再打开
              this.form = row;
              this.dialogStatus.show = true;

            },
            tableDelete(index,row){
              console.log("点击表格删除");
              //this.dialogStatus.show = true;
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