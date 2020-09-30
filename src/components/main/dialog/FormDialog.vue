<template>
        <el-dialog title="收货地址" :visible.sync="dialogFormVisible" @close="close">
            <el-form :model="form">
                <el-form-item label="活动名称" :label-width="formLabelWidth">
                    <el-input v-model="form.name" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="活动区域" :label-width="formLabelWidth">
                    <el-select v-model="form.region" placeholder="请选择活动区域">
                        <el-option label="区域一" value="shanghai"></el-option>
                        <el-option label="区域二" value="beijing"></el-option>
                    </el-select>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="cancel">取 消</el-button>
                <el-button type="primary" @click="ensure">确 定</el-button>
            </div>
        </el-dialog>
</template>

<script>
    export default {
        name: "FormDialog",
        data() {
            return {
                gridData: [{
                    date: '2016-05-02',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1518 弄'
                }, {
                    date: '2016-05-04',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1518 弄'
                }, {
                    date: '2016-05-01',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1518 弄'
                }, {
                    date: '2016-05-03',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1518 弄'
                }],
                dialogTableVisible : false,
                dialogFormVisible : false,
                form: {
                    name: '',
                    region: '',
                    date1: '',
                    date2: '',
                    delivery: false,
                    type: [],
                    resource: '',
                    desc: ''
                },
                formLabelWidth: '80px'
            };
        },
        props : {
            dialogStatus : {
                type : Object,
                default : {},
            }
        },
        watch : {
            "dialogStatus.show" : {
              handler(newVal,oldVal){
                if(typeof newVal==="boolean"){
                  console.log("弹出层子组件监听到数据变动");
                  console.log(newVal);
                  this.dialogFormVisible = newVal;
                }
              },
              immediate: true,
              //deep: true
            }
        },
        methods : {
            cancel(){
                this.$emit("close-dialog");
            },
            ensure(){
                this.$emit("commit-dialog");
            },
            close(){
                this.$emit("close-dialog");
            },
        }
    }
</script>

<style scoped>

</style>