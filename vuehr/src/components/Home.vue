<template>
  <div>
    <!--主体数据展示部分-->
    <el-container class="home-container">
      <!--头部信息-->
      <el-header class="home-header">
        <span class="home_title">
          花间堂人力资源系统</span>
        <div style="display: flex;align-items: center;margin-right: 7px">
          <el-badge style="margin-right: 30px" :is-dot="this.$store.state.nfDot" v-if="false">
            <i class="fa fa-bell-o" @click="goChat" style="cursor: pointer"></i>
          </el-badge>
          <el-dropdown @command="handleCommand">
            <span class="el-dropdown-link home_userinfo" style="display: flex;align-items: center; margin-right: 30px">
              {{user.name}}
              <!--头像部分暂时隐藏-->
              <!--<i><img v-if="user.userface!=''" :src="user.userface"
                      style="width: 40px;height: 40px;margin-right: 5px;margin-left: 5px;border-radius: 40px"/></i>-->
            </span>
          <el-dropdown-menu slot="dropdown"style="margin-right: 10px">
            <el-dropdown-item>
              <!--触发修改密码的弹出框,可修改密码-->
              <div @click="changPersonPasswordView()">
              修改密码
              </div>
            </el-dropdown-item>
            <el-dropdown-item>
              <div @click="showPersonalView()">
              个人信息
              </div>
            </el-dropdown-item>
            <el-dropdown-item command="logout" divided>注销</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </div>
      </el-header>
      <!--导航栏部分-->
      <el-container>
        <el-aside width="180px" class="home-aside">
          <div style="display: flex;justify-content: flex-start;width: 180px;text-align: left;">
            <el-menu style="background: #ececec;width: 180px;" unique-opened router>
              <template v-for="(item,index) in this.routes" v-if="!item.hidden">
                <el-submenu :key="index" :index="index+''">
                  <template slot="title">
                    <i :class="item.iconCls" style="color: #20a0ff;width: 14px;"></i>
                    <span slot="title">{{item.name}}</span>
                  </template>

                  <el-menu-item width="180px"
                                style="padding-left: 30px;padding-right:0px;margin-left: 0px;width: 170px;text-align: left"
                                v-for="child in item.children"
                                :index="child.path"
                                :key="child.path"
                                v-if="isRoterAlive"
                                @click="aotoFlush()">{{child.name}}

                  </el-menu-item>

                </el-submenu>
              </template>
            </el-menu>
          </div>
        </el-aside>
        <el-main>
          <el-breadcrumb separator-class="el-icon-arrow-right">
            <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item v-text="this.$router.currentRoute.name"></el-breadcrumb-item>
          </el-breadcrumb>
          <keep-alive>
            <router-view v-if="this.$route.meta.keepAlive"></router-view>
          </keep-alive>
          <router-view v-if="!this.$route.meta.keepAlive"></router-view>
        </el-main>
      </el-container>

      <!--个人信息页面-->
      <el-form :model="personalInfoModule" status-icon  ref="personalInfoForm"  label-position="left" label-width="100px" style="margin: 0px;padding: 0px;" >
        <div style="text-align: left">
          <el-dialog
            :title="personalInfoTitle"
            style="padding: 0px;"
            :close-on-click-modal="false"
            :visible.sync="personalInfoVisible"
            width="20%">
            <el-row>
              <el-col>
                <el-form-item  label="姓名:" prop="name">
                  <el-input type="text" v-model.trim="personalInfoModule.name" size="mini" auto-complete="off" style="width: 80%" :readonly="true"
                            ></el-input>
                </el-form-item>
              </el-col>
            </el-row>
            <el-row>
              <el-col>
                <el-form-item  label="用户名:" prop="userName">
                  <el-input type="text" v-model.trim="personalInfoModule.userName" size="mini" auto-complete="off" style="width: 80%" :readonly="true"
                          ></el-input>
                </el-form-item>
              </el-col>
            </el-row>
            <el-row>
              <el-col>
                <el-form-item  label="部门:" prop="department">
                  <el-input type="text" v-model.trim="personalInfoModule.department" size="mini" auto-complete="off" style="width: 80%" :readonly="true"
                  ></el-input>
                </el-form-item>
              </el-col>
            </el-row>
            <el-row>
              <el-col>
                <el-form-item  label="联系方式:" prop="phoneNumber">
                  <el-input type="text" v-model.trim="personalInfoModule.phoneNumber" size="mini" auto-complete="off" style="width: 80%" :readonly="true"
                  ></el-input>
                </el-form-item>
              </el-col>
            </el-row>
            <el-row>
              <el-col>
                <el-form-item  label="联系地址:" prop="address">
                  <el-input type="text" v-model.trim="personalInfoModule.address" size="mini" auto-complete="off" style="width: 80%" :readonly="true"
                  ></el-input>
                </el-form-item>
              </el-col>
            </el-row>
          </el-dialog>
        </div>
      </el-form>

      <!--修改密码弹框-->
      <el-form :model="resetForm" status-icon :rules="resetFormRules" ref="updatePassForm"  label-position="left" label-width="100px" style="margin: 0px;padding: 0px;" >
        <div style="text-align: left">
          <el-dialog
            :title="dialogTitle"
            style="padding: 0px;"
            :close-on-click-modal="false"
            :visible.sync="dialogVisible"
            width="20%">
            <el-row width>
              <el-col :span="24">
                <el-form-item  label="原始密码:" prop="oldPassword">
                  <el-input type="password" v-model.trim="resetForm.oldPassword" size="mini" auto-complete="off" style="width: 80%"
                            placeholder="原始密码"></el-input>
                </el-form-item>

                <el-form-item label="新密码:" prop="newpwd">
                  <el-input type="password" v-model.trim="resetForm.newpwd" size="mini" auto-complete="off" style="width: 80%"
                            placeholder="新密码"></el-input>
                </el-form-item>

                <el-form-item label="确认密码:" prop="renewpwd">
                  <el-input type="password" v-model.trim="resetForm.renewpwd" size="mini" auto-complete="off" style="width: 80%"
                            placeholder="新密码"></el-input>
                </el-form-item>
              </el-col>
            </el-row>
            <span slot="footer" class="dialog-footer">
                <el-button size="mini" @click="cancelEidt">取 消</el-button>
                <el-button size="mini" type="primary" @click="updatePass('updatePassForm')">确 定</el-button>
            </span>
          </el-dialog>
        </div>
      </el-form>
    </el-container>
  </div>
</template>
<script>
  export default{
    mounted: function () {
//      this.devMsg();
      /*自动触发的函数*/
      this.loadNF();
    },

    /*自定义函数*/
    methods: {
      /*自动刷新当前页面*/
      aotoFlush(){
        this.reload()
      },
      /*取消按钮*/
      cancelEidt(){
        this.dialogVisible=false;
      },

      //修改密码接口
      updatePass(formName){
        var _this = this;
        this.$refs[formName].validate((valid) => {
            if (valid) {
              this.getRequest("/login/updatePassword?userName="+this.$store.state.user.username+"&oldPassWord="+this.resetForm.oldPassword+"&newPassWord="+this.resetForm.newpwd).then(resp=> {
                if (resp && resp.status == 200) {
                  var data = resp.data;
                  this.$message({type: 'success', message: data.data});
                }
              })
              this.dialogVisible=false;
              this.empty();
            }else {
              return false;
            }
        });

      },

      //将修改密码框置空
      empty(){
          this.resetForm.oldPassword='',
          this.resetForm.newpwd='',
          this.resetForm.renewpwd=''
      },

      //打开修改密码弹框
      changPersonPasswordView(){
        this.dialogTitle = "修改密码";
        this.dialogVisible = true;
      },

      //个人信息
      showPersonalView(){
        this.getRequest("/talent/searchRoid?userName="+this.$store.state.user.username).then(resp=> {
          if (resp && resp.status == 200) {
            var data = resp.data.data;
            this.personalInfoModule.name = data.name;
            this.personalInfoModule.userName = data.userName;
            this.personalInfoModule.department = data.department;
            this.personalInfoModule.phoneNumber = data.phoneNumber;
            this.personalInfoModule.address = data.address;
          }
        })
        this.personalInfoTitle = '个人信息';
        this.personalInfoVisible = true;
      },
      loadNF(){
        var _this = this;
        this.getRequest("/chat/sysmsgs").then(resp=> {
          var isDot = false;
          resp.data.forEach(msg=> {
            if (msg.state == 0) {
              isDot = true;
            }
          })
          _this.$store.commit('toggleNFDot', isDot);
        })
      },
      goChat(){
        this.$router.push({path: '/chat'});
      },
      devMsg(){
        this.$alert('为了确保所有的小伙伴都能看到完整的数据演示，数据库只开放了查询权限和部分字段的更新权限，其他权限都不具备，完整权限的演示需要大家在自己本地部署后，换一个正常的数据库用户后即可查看，这点请大家悉知!', '友情提示', {
          confirmButtonText: '确定',
          callback: action => {
            this.$notify({
              title: '重要重要!',
              type: 'warning',
              message: '小伙伴们需要注意的是，目前只有权限管理模块完工了，因此这个项目中你无法看到所有的功能，除了权限管理相关的模块。权限管理相关的模块主要有两个，分别是 [系统管理->基础信息设置->权限组] 可以管理角色和资源的关系， [系统管理->操作员管理] 可以管理用户和角色的关系。',
              duration: 0
            });
          }
        });
      },
      handleCommand(cmd){
        var _this = this;
        if (cmd == 'logout') {
          this.$confirm('注销登录, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            _this.getRequest("/logout");
            _this.$store.commit('logout');
            _this.$router.replace({path: '/'});
          }).catch(() => {
            _this.$message({
              type: 'info',
              message: '取消'
            });
          });
        }
      },

      reload(){
        this.isRoterAlive=false
        this.$nextTick(function(){
            this.isRoterAlive=true
        })
      }


    },
    provide(){
      return{
          reload:this.reload
      }
    },
    data(){
      var validateOldPassword = (rule, value, callback) => {
        if (!value) {
          callback(new Error('请输入原始密码'));
        } else {
          callback();
        }
      };
      var validateNewpwd = (rule, value, callback) => {
        if (!value) {
          callback(new Error('请输入新密码'));
        } else if (value.toString().length < 6 || value.toString().length > 12) {
          callback(new Error('密码长度为6 - 12个字符'))
        } else {
          callback();
        }
      };
      var validateRenewpwd = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value !== this.resetForm.newpwd) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
      return {
        personalInfoModule:{
          name:'',
          userName:'',
          department:'',
          address:'',
          phoneNumber:'',
        },
        //重置密码
        resetForm: {
          oldPassword:'',
          newpwd: '',
          renewpwd: '',
        },
        resetFormRules: {
          oldPassword:[{required: true, validator: validateOldPassword, trigger: 'blur' }],
          newpwd: [{ required: true, validator: validateNewpwd, trigger: 'blur' }],
          renewpwd: [{ required: true, validator: validateRenewpwd, trigger: 'blur' }]
        },
        isRoterAlive:true,
        userName:'',
        isDot: false,
        dialogTitle:'',
        dialogVisible:false,
        personalInfoTitle:'',
        personalInfoVisible:false,
      }
    },
    computed: {
      user(){
        return this.$store.state.user;
      },
      routes(){
        return this.$store.state.routes
      }
    }
  }
</script>
<style>
  .home-container {
    height: 100%;
    position: absolute;
    top: 0px;
    left: 0px;
    width: 100%;
  }

  .home-header {
    /*background-color: #20a0ff;*/
    background-color: #0000FF;

    color: #333;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: space-between;
    box-sizing: content-box;
    padding: 0px;
  }

  .home-aside {
    background-color: #ECECEC;
  }

  .home-main {
    background-color: #fff;
    color: #000;
    text-align: center;
    margin: 0px;
    padding: 0px;;
  }

  .home_title {
    color: #fff;
    font-size: 22px;
    display: inline;
    margin-left: 8px;
  }

  .home_userinfo {
    color: #fff;
    cursor: pointer;
  }

  .home_userinfoContainer {
    display: inline;
    margin-right: 20px;
  }

  .el-submenu .el-menu-item {
    width: 180px;
    min-width: 175px;
  }
</style>
