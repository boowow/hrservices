<template>
  <div>
    <!--整个账户管理模块容器-->
    <el-container>
      <!--容器头部-->
      <el-header style="padding: 0px;display:flex;justify-content:space-between;align-items: center">
        <div style="margin-left: 5px;margin-right: 20px;display: inline">

          <!--添加账户的按钮-->
          <el-button type="primary" size="mini" icon="el-icon-plus"
                     @click="showAddTalentInformationView()">
            添加账号
          </el-button>
        </div>
      </el-header>
      <!--头部结束-->

      <!--账户主体内容列表-->
      <el-main>
        <div>
          <el-row>
            <el-col :span="18">
              <el-table
                :data="HrUsers"
                border
                stripe
                size="mini"
                style="width: 100%">
                <el-table-column
                  prop="id"
                  align="left"
                  fixed
                  label="编号"
                  width="90">
                </el-table-column>

                <!--<el-table-column
                  prop="workerId"
                  align="left"
                  label="工号"
                  width="60">
                </el-table-column>-->

                <el-table-column
                  prop="name"
                  align="left"
                  label="姓名"
                  width="150">
                </el-table-column>

                <el-table-column
                  prop="phoneNumber"
                  align="left"
                  label="手机号码"
                  width="150">
                </el-table-column>

                <el-table-column
                  prop="address"
                  align="left"
                  label="联系地址"
                  width="170">
                </el-table-column>

                <el-table-column
                  prop="department"
                  align="left"
                  label="部门"
                  width="170">
                </el-table-column>

                <el-table-column
                  prop="roleName"
                  align="left"
                  label="角色"
                  width="170">
                </el-table-column>

                <el-table-column
                  prop="userName"
                  align="left"
                  label="用户名"
                  width="170">
                </el-table-column>

               <!-- <el-table-column
                  prop="renark"
                  align="left"
                  label="备注"
                  width="130">
                </el-table-column>-->

                <!--<el-table-column

                  label="操作"
                  width="400">
                  <el-button @click="" type="danger" style="padding: 3px 4px 3px 4px;margin: 2px"
                             size="mini">删除
                  </el-button>
                </el-table-column>-->
              </el-table>
              <div style="display: flex;justify-content: space-between;margin: 2px">

                <!--分页展示的组件()-->
                <el-pagination
                  background
                  :page-size="10"
                  :current-page="currentPage"
                  @current-change="currentChange"
                  :page-sizes="[5, 10, 20, 30]"
                  layout="prev,total, pager, next,jumper"
                  :total="totalCount">
                </el-pagination>
                <!-- layout="prev, pager, next"-->
              </div>
            </el-col>
          </el-row>
        </div>
      </el-main>
      <!--主体内容结束-->
    </el-container>

    <!--添加账户的弹出框-->
    <el-form :model="userModule"  style="margin: 0px;padding: 0px;" :rules="rules" ref="userForm">
      <div style="text-align: left">
        <!--弹出框主体内容-->
        <el-dialog
          :title="dialogTitle"
          style="padding: 0px;"
          :close-on-click-modal="false"
          :visible.sync="dialogVisible"
          width="50%">

          <el-row>
            <el-col :span="8">
              <div>
                <el-form-item label="姓名:" prop="name" >
                  <el-input prefix-icon="el-icon-edit" v-model.trim="userModule.name" size="mini" style="margin-left: 26px;width: 160px"
                            placeholder="请输入员工姓名"></el-input>
                </el-form-item>
              </div>
            </el-col>
            <el-col :span="8">
              <div>
                <el-form-item label="手机号码:" prop="phoneNumber">
                  <el-input prefix-icon="el-icon-phone" v-model.trim="userModule.phoneNumber" size="mini" style="width: 160px"
                            placeholder="手机号码..."></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="角色:" prop="roleName">
                  <el-select v-model.trim="userModule.roleName" style="margin-left: 15px;width: 160px" size="mini" placeholder="请选择角色">
                    <el-option
                      v-for="item in userModule.roleNames"
                      :key="item.id"
                      :label="item.name"
                      :value="item.name">
                    </el-option>
                  </el-select>
                </el-form-item>
              </div>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="8">
              <div>
                <el-form-item label="联系地址:" prop="address">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="userModule.address" size="mini" style="width: 160px"
                            placeholder="联系地址..."></el-input>
                </el-form-item>
              </div>
            </el-col>
            <el-col :span="8">
              <div>
                <el-form-item label="用户名:" prop="username">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="userModule.username" size="mini" style="margin-left: 13px;width: 160px"
                            placeholder="请输入用户名..."></el-input>
                </el-form-item>
              </div>
            </el-col>
            <el-col :span="8">
              <el-form-item label="部门:" prop="department">
                <el-select v-model.trim="userModule.department" style="margin-left: 26px;width: 160px" size="mini" placeholder="请选择部门">
                  <el-option
                    v-for="item in userModule.departments"
                    :key="item.id"
                    :label="item.name"
                    :value="item.name">
                  </el-option>
                </el-select>
              </el-form-item>
            </el-col>
          </el-row>
          <!--<el-row>
            <el-col >
              <div>
                <el-form-item label="备注:" prop="renark">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="userModule.renark" size="mini" style="margin-left: 26px;width: 160px"
                            placeholder="请输入备注信息..."></el-input>
                </el-form-item>
              </div>
            </el-col>

          </el-row>-->

          <!--弹出框底部的两个操作按钮-->
          <span slot="footer" class="dialog-footer">
            <el-button size="mini" @click="cancel()">取 消</el-button>
            <el-button size="mini" type="primary" @click="confirm('userForm')">确 定</el-button>
          </span>
        </el-dialog>
      </div>
    </el-form>
  </div>
</template>
<!--以上是数据显示域-->

<!--以下为逻辑处理域-->
<script>
  export default{
    inject:['reload'],/*注入relaod方法*/
    /*这里为常用的自定义变量*/
    data(){
      var validateName = (rule, value, callback) => {
        if (!this.userModule.name) {
          return callback(new Error('必填:名字'));
        }else {
          return callback();
        }
      };
      var validateUserName = (rule, value, callback) => {
        if (!this.userModule.username) {
          return callback(new Error('必填:用户名'));
        }else {
          return callback();
        }
      };
      var validatePhoneNumber = (rule, value, callback) => {
        if (!this.userModule.phoneNumber) {
          return callback(new Error('必填:手机号码'));
        }else{
          const reg = /^1[3|4|5|7|8][0-9]\d{8}$/
          if (reg.test(this.userModule.phoneNumber)) {
            return callback();
          } else {
            return callback(new Error('请输入正确的手机号'));
          }
        }
      };
      var validateRoleName = (rule, value, callback) => {
        if (!this.userModule.roleName) {
          return callback(new Error('必填:角色名称'));
        }else {
          return callback();
        }
      };
      var validateAddress = (rule, value, callback) => {
        if (!this.userModule.address) {
          return callback(new Error('必填:联系地址'));
        }else {
          return callback();
        }
      };
      var validateDepartment = (rule, value, callback) => {
        if (!this.userModule.department) {
          return callback(new Error('必选:部门'));
        }else {
          return callback();
        }
      };

      return{
        userModule:{
          name:'',
          phoneNumber:'',
          roleName:'',
          renark:'',
          address:'',
          username:'',
          roleNames:[
            /*{id:'1',name:'超级管理员'},*/
            {id:'2',name:'人力资源部'},
            {id:'3',name:'猎头人员'},
            {id:'4',name:'地推人员'},
            /*{id:'5',name:'门店人员'},*/
          ],
          department:'',
          departments:[
            {id:'1',name:'社招'},
            {id:'2',name:'校招'},
            {id:'3',name:'猎头'},
            {id:'4',name:'机构'},
            {id:'5',name:'地推一组'},
            {id:'6',name:'人才选聘部'},
          ]
        },
        totalCount: -1,
        currentPage: 1,
        HrUsers:[],
        dialogTitle:'',
        dialogVisible:false,

        rules: {
          name: [{required: true, validator: validateName, trigger: 'blur'}], /*姓名必填*/
          username: [{required: true, validator: validateUserName, trigger: 'blur'}], /*用户名必填*/
          phoneNumber: [{required: true, validator: validatePhoneNumber, trigger: 'blur'}], /*电话号码必填*/
          roleName: [{required: true, validator: validateRoleName, trigger: 'blur'}], /*角色名称必填*/
          address: [{required: true, validator: validateAddress, trigger: 'blur'}], /*必填学历*/
          department: [{required: true, validator: validateDepartment, trigger: 'blur'}], /*必填学历*/
        }
      }
    },

    /*自动访问的逻辑函数可以放这里*/
    mounted: function () {
      this.loadHrUsers();
    },
    methods:{

      /*当前页数*/
      currentChange(currentChange){
        this.currentPage = currentChange;
        this.loadHrUsers();
      },

      //显示添加账户的弹出框
      showAddTalentInformationView(){
        this.dialogTitle = "添加账号";
        this.dialogVisible = true;
      },

      //取消按钮
      cancel(){
        this.dialogVisible = false;
      },

      //加载有账户的信息的列表
      loadHrUsers(){
        var _this = this;
        this.getRequest("/login/selectRoot?userName="+this.$store.state.user.username+"&pageNumber=" + this.currentPage + "&pageSize=10").then(resp=> {
          if (resp && resp.status == 200) {
            var data = resp.data;
            _this.HrUsers = data.data.list;
            _this.totalCount = data.data.total;
          }
        })
      },

      //确认按钮,触发添加账户逻辑接口
      confirm(formName){
        var _this = this;
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.getRequest("/login/addRoot?name=" + this.userModule.name + "&userName=" + this.userModule.username + "&phoneNumber=" + this.userModule.phoneNumber + "&roleName=" + this.userModule.roleName + "&address=" + this.userModule.address+"&department="+this.userModule.department+"&renark="+this.userModule.renark).then(resp => {
              if (resp && resp.status == 200) {
                var data = resp.data;
                _this.HrUsers = data.data.list;
                this.$message({type: 'success', message: data.data});
                /*重新加载账户信息*/
                _this.loadHrUsers()
                this.dialogVisible = false;
                this.empty()
              }
            })
          } else {
            return false;
          }


        });

      },

      /*将添加账号的文本框清空*/
      empty(){
        this.userModule.name='',
        this.userModule.username='',
        this.userModule.phoneNumber='',
        this.userModule.roleName='',
        this.userModule.address=''
        this.userModule.department=''
        this.userModule.renark=''
      }

    }
  }
</script>

<!--这里为页面的CSS,用于修饰数据显示-->
<style>
  .el-dialog__body {
    padding-top: 0px;
    padding-bottom: 0px;
  }

  .slide-fade-enter-active {
    transition: all .8s ease;
  }

  .slide-fade-leave-active {
    transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }

  .slide-fade-enter, .slide-fade-leave-to {
    transform: translateX(10px);
    opacity: 0;
  }
</style>
