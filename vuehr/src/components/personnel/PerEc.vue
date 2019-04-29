<template>
  <div>
    <!--整个面试信息模板容器-->
    <el-container>
      <!--内容头部-->
      <el-header style="padding: 0px;display:flex;justify-content:space-between;align-items: center">
        <div style="display: inline">
          <!--候选人名字的搜索框-->
          <el-input
            placeholder="通过候选人名字搜索,记得回车哦..."
            clearable
            @change="keywordsChange"
            style="width: 250px;margin: 0px;padding: 0px;"
            size="mini"
            :disabled="advanceSearchViewVisible"
            @keyup.enter.native="searchInterviewer"
            prefix-icon="el-icon-search"
            v-model.trim="keywords">
          </el-input>
          <!--触发搜索函数的按钮-->
          <el-button type="primary" size="mini" style="margin-left: 5px" icon="el-icon-search" @click="searchInterviewer()">
            搜索
          </el-button>
          <!--重置按钮-->
          <el-button type="primary" size="mini" style="margin-left: 5px" icon="el-icon-back" @click="emptySearch()">
            重置
          </el-button>
        </div>
      </el-header>
      <!--内容头部结束-->

      <!--面试信息主体内容-->
      <el-main>
        <div>
          <el-row>
            <el-col :span="20">
              <el-table
                :data="InterviewInformations"
                v-loading="tableLoading"
                border
                stripe
                size="mini"
                style="width: 100%">

                <el-table-column
                  prop="id"
                  align="left"
                  fixed
                  label="编号"
                  width="60">
                </el-table-column>

                <el-table-column
                  prop="name"
                  align="left"
                  label="姓名"
                  width="130">
                </el-table-column>

                <el-table-column
                  prop="channelSources"
                  align="left"
                  label="渠道"
                  width="130">
                </el-table-column>

                <el-table-column
                  prop="noteWorker"
                  align="left"
                  label="录入人"
                  width="130">
                </el-table-column>

                <el-table-column
                  prop="interviewer"
                  align="left"
                  label="面试人"
                  width="130">
                </el-table-column>

                <el-table-column
                  prop="time"
                  align="left"
                  label="面试时间"
                  width="140">
                </el-table-column>

                <el-table-column
                  prop="interviewWork"
                  align="left"
                  label="面试岗位"
                  width="140">
                </el-table-column>

                <el-table-column
                  prop="statusName"
                  align="left"
                  label="面试状态"
                  width="140">
                </el-table-column>

                <el-table-column
                  prop="order"
                  align="left"
                  label="电话预约"
                  width="140">
                </el-table-column>

                <el-table-column
                  fixed="right"

                  label="操作"
                  width="200">

                  <!--嵌套一个内模板包含两个操作按钮-->
                  <template slot-scope="scope">

                    <!--预约面试按钮-->
                    <el-button type="primary" @click="showEditInterviewerView(scope.row)"
                               size="mini">预约面试
                    </el-button>
                    <!--style="padding: 3px 4px 3px 4px;margin: 2px"-->
                    <!--更多操作按钮-->
                    <el-button type="primary" @click="showOperationView(scope.row)"
                               size="mini">更多操作
                    </el-button>
                    <!-- style="padding: 3px 4px 3px 4px;margin: 2px"-->
                  </template>
                </el-table-column>
              </el-table>
            </el-col>
          </el-row>
          <div style="display: flex;justify-content: space-between;margin: 2px">
            <el-pagination
              background
              :page-size="10"
              :current-page="currentPage"
              @current-change="currentChange"
              layout="prev,total, pager, next,jumper"
              :total="totalCount">
            </el-pagination>
          </div>
        </div>
      </el-main>
      <!--主体内容结束-->
    </el-container>

    <!--更多操作的弹出框-->
    <el-form style="margin: 0px;padding: 0px;" ref="operationForm">
      <div  style="text-align: left">
        <!--弹出框主体内容-->
          <el-dialog
            :title="operationTitle"
            style="padding: 0px;"
            width="30%"
            :close-on-click-modal="false"
            :visible.sync="operationVisible">
            <div>
              <el-upload
                :show-file-list="false"
                accept=".jpg,.png,application/msword,application/vnd.openxmlformats-officedocument.wordprocessingml.document"
                action="/interViewer/uploadPhoto?type=0"
                :data="param"
                :on-success="resumeUploadSuccess"
                :on-error="resumeUploadError"
                :disabled="this.resumeUploadBtnText=='正在上传'"
                :before-upload="beforeResumeUpload"
                style="display: inline">
                <el-button style="padding: 3px 4px 3px 4px;margin: 2px" type="info" :loading="this.resumeUploadBtnText=='正在上传'"
                           size="mini">上传面试结果
                </el-button>
              </el-upload>
              <el-button style="padding: 3px 4px 3px 4px;margin: 2px" type="info"
                         size="mini" @click="downloadResume">浏览面试结果
              </el-button>
              状态修改：
              <el-select v-model.trim="select" placeholder="请选择" size="mini" style="width: 120px;">
                <el-option v-for="(item,index) in status" :key="index"  :label="item.name" :value="item.stat">{{ item.name }}</el-option>
              </el-select>
            </div>
            <span slot="footer" class="dialog-footer" >

              <!--弹出框底部包含两个按钮-->
            <el-button size="mini" @click="cancelOperate()">取 消</el-button>
            <el-button size="mini" type="primary" @click="changeStatus()">确 定</el-button>
          </span>
          </el-dialog>
      </div>
    </el-form>

    <!--预约面试的弹出框-->
    <el-form style="margin: 0px;padding: 0px;" :model="addInterviewModule" :rules="rules"  ref="addInterviewForm">
      <div style="text-align: left">
        <!--弹出框主题内容-->
        <el-dialog
          :title="dialogTitle"
          style="padding: 0px;"
          width="55%"
          :close-on-click-modal="false"
          :visible.sync="dialogVisible">
          <el-row>
            <el-col :span="8">
              <el-form-item label="选择面试人:" prop="interviewer">
                <!--<el-select v-model.trim="addInterviewModule.interviewer" placeholder="请选择" size="mini" style="width: 120px;">
                  <el-option  v-for="(item,index) in addInterviewModule.interviewers" :key="index" :value="item.name">{{ item.name }}</el-option>
                </el-select>-->
                <el-input prefix-icon="el-icon-edit" v-model.trim="addInterviewModule.interviewer" size="mini" style="margin-left: 26px; width: 180px"
                          placeholder="请输入面试老师"></el-input>
              </el-form-item>
            </el-col>

            <el-col  :span="8">
              <el-form-item label="面试时间:" prop="intervieTime" size="mini">
                <el-date-picker
                  v-model.trim="addInterviewModule.intervieTime"
                  type="datetime"
                  format="yyyy-MM-dd HH:mm"
                  value-format="yyyy-MM-dd HH:mm"
                  placeholder="选择日期时间">
                </el-date-picker>
              </el-form-item>
            </el-col>
            <el-col :span="8">
                <el-form-item label="是否已经电话约定:" prop="appoint" size="mini">
                  <el-radio-group v-model.trim="addInterviewModule.appoint">
                    <el-radio :label="item.value" :key="item.value" v-for="(item,index) in addInterviewModule.ifAppoint">{{item.label}}</el-radio>
                  </el-radio-group>
                </el-form-item>
            </el-col>
          </el-row>

          <!--弹出框底部包含两个按钮-->
          <span slot="footer" class="dialog-footer" >
            <el-button size="mini" @click="cancelEidt()">取 消</el-button>
            <el-button size="mini" type="primary" @click="orderInterview('addInterviewForm')">确 定</el-button>
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
        var validateInterviewer = (rule, value, callback) => {
          if (!this.addInterviewModule.interviewer) {
            return callback(new Error('必选:面试老师'));
          }else {
            return callback();
          }
        };
        var validateIntervieTime = (rule, value, callback) => {
          if (!this.addInterviewModule.intervieTime) {
            return callback(new Error('必选:面试时间'));
          }else {
            return callback();
          }
        };
        var validateAppoint = (rule, value, callback) => {
          if (this.addInterviewModule.appoint==="") {
            return callback(new Error('必选:是否电话预约'));
          }else {
            return callback();
          }
        };
          return {
            addInterviewModule:{
              interviewer:'',
              interviewers:[],
              intervieTime:'',
              appoint:'',
              ifAppoint: [
                { label: "是", value: 1 },
                { label: "否", value: 0 }
              ],
            },
            resumeUploadBtnText:'',
            totalCount: -1,
            currentPage: 1,
            advanceSearchViewVisible: false,
            keywords: '',
            totalCount: -1,
            currentPage: 1,
            keywords:'',
            operationTitle:'',
            operationVisible:false,
            id:'',
            operateId:'',
            param:{
              id:'',
            },
            value: '',
            InterviewInformations:[],
            select:'',
            status:[
              {stat:3,name:'已入职'},
              {stat:5,name:'合格'},
              {stat:6,name:'不合格'},
              {stat:7,name:'待定'},
            ],
            dialogTitle:'',

            dialogVisible: false,
            tableLoading: false,
            totalCount: -1,
            rules: {
              interviewer: [{required: true, validator:validateInterviewer, trigger: 'blur'}],
              intervieTime:[{required: true, validator:validateIntervieTime, trigger: 'blur'}],
              appoint:[{required: true, validator:validateAppoint, trigger: 'blur'}]
            }

          }
      },

    /*自动访问的逻辑函数可以放这里*/
      mounted: function () {
          //点击导航栏开始加载面试信息
        this.searchInterviewer();
      },

    /*以下为自定义函数*/
      methods:{
        /*当前页数*/
        currentChange(currentChange){
          this.currentPage = currentChange;
          this.searchInterviewer();
        },
        //搜索框与列表结果动态查询
        keywordsChange(val){
          if (val == '') {
            this.searchInterviewer();
          }
        },

        emptySearch(){
          this.keywords="";
        },
        //搜索框按照面试人名字模糊查询
        searchInterviewer(){
          var _this = this;
          if(this.keywords==''){
            _this.loadInterviewInfo();
          }else{
            _this.getRequest("/interViewer/search?userName="+this.$store.state.user.username+"&pageNumber=" + this.currentPage + "&pageSize=10&name="+this.keywords).then(resp=> {
              if (resp && resp.status == 200) {
                var data = resp.data;
                _this.InterviewInformations = data.data.list;
                _this.totalCount = data.data.total;
              }
            })
          }

        },
        //更多操作取消
        cancelOperate(){
          this.operationVisible=false;
        },
        //打开更多操作页面
        showOperationView(row){
          if(row.userName===this.$store.state.user.username || 'admin'=== this.$store.state.user.username){
            this.operationTitle = "更多操作";
            this.param.id= row.id;

            this.operationVisible = true;
          }else{
            this.$message({type: 'warning', message: '您没有操作权限!'});
          }

        },
        //面试结果上传成功
        resumeUploadSuccess(response, file, fileList){
          if (response) {
            this.$message({type: response.status, message: response.msg});
          }
          this.loadInterviewInfo();
          this.resumeUploadBtnTest = '上传面试结果';
        },
        //面试结果上传失败
        resumeUploadError(err, file, fileList){
          this.$message({type: 'error', message: "上传失败!"});
          this.resumeUploadBtnTest = '上传面试结果';
        },
        //面试结果上传之前
        beforeResumeUpload(file,row){
          this.resumeUploadBtnTest = '正在上传';
        },

        //下载面试结果
        downloadResume(){
          window.open("/interViewer/downPhoto?id="+this.param.id, "_parent");
        },

        //更改候选人状态
        changeStatus(){
          var _this = this;
          this.getRequest("/interViewer/updateStatus?id="+this.param.id+"&status="+this.select).then(resp=> {
            if (resp && resp.status == 200) {
              var data = resp.data;
              this.$message({type: 'success', message: data.data});
              this.loadInterviewInfo();
            }
          })
          this.operationVisible=false;
          _this.reload()/*刷新当前页面*/
        },

        //预约面试接口
        orderInterview(formName){
          var _this= this;
          this.$refs[formName].validate((valid) => {
            if (valid) {
              _this.getRequest("/interViewer/order?id="+this.id+"&time="+this.addInterviewModule.intervieTime+"&interviewer="+this.addInterviewModule.interviewer+"&isOrder="+this.addInterviewModule.appoint).then(resp=> {
                if (resp && resp.status == 200) {
                  var data = resp.data;
                  this.$message({type: 'success', message: data.data});
                  /*每次预约完重新加载列表*/
                  _this.loadInterviewInfo();
                  _this.dialogVisible=false;
                  _this.empty()/*每次预约操作完成后将文本框置空*/
                }
              })
            }else {
              return false;
            }

          });
        },

        /*将文本框置空*/
        empty(){
            this.addInterviewModule.intervieTime='',
            this.addInterviewModule.interviewer='',
            this.addInterviewModule.appoint='',
            this.id=''
        },

        //打开预约页面
        showEditInterviewerView(row){
          if(row.userName===this.$store.state.user.username || 'admin'=== this.$store.state.user.username){
            this.dialogTitle = "预约面试";
            this.id = row.id;

            //打开页面之前加载面试老师信息
            //this.showInterviewer();
            this.dialogVisible = true;
          }else{
            this.$message({type: 'warning', message: '您没有操作权限!'});
          }

        },

        //查询面试老师
        showInterviewer(){
          var _this = this;
          this.getRequest("/interViewer/teacher").then(resp=> {
            if (resp && resp.status == 200) {
              var data = resp.data;
              _this.addInterviewModule.interviewers = data.data.list;
            }
          })
        },

        //取消按钮
        cancelEidt(){
          this.dialogVisible = false;
        },

        //加载面试信息
        loadInterviewInfo(){
          var _this = this;
          this.tableLoading = true;
          this.getRequest("/interViewer/hr?userName="+this.$store.state.user.username+"&pageNumber=" + this.currentPage + "&pageSize=10").then(resp=> {
            this.tableLoading = false;
            if (resp && resp.status == 200) {
              var data = resp.data;
              _this.InterviewInformations = data.data.list;
              _this.totalCount = data.data.total;
            }
          })
        },
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
  .select{
    margin-left: 1px;
    width: 4px;
    height: 2px;
  }
</style>
