<template>
  <div>
    <!--整个人才信息模板容器-->
    <el-container>
      <!--内容头部-->
      <el-header style="padding: 0px;display:flex;justify-content:space-between;align-items: center">
        <div style="margin-left: 5px;margin-right: 20px;display: inline">
          <el-button type="primary" size="mini" icon="el-icon-plus" @click="showAddTalentInformationView('addForm')">
            新录入
          </el-button>
        </div>

        <div style="display: inline">

          <!--新录入人名字的搜索框-->
          <el-input
            placeholder="通过新录入人名字搜索人才信息,记得回车哦..."
            clearable
            @change="keywordsChange"
            style="width: 300px;margin: 0px;padding: 0px;"
            size="mini"
            :disabled="advanceSearchViewVisible"
            @keyup.enter.native="searchTalent"
            prefix-icon="el-icon-search"
            v-model.trim="keywords">
          </el-input>


          <el-select v-model.trim="department" style=";width: 160px" size="mini" placeholder="所有部门">
            <el-option
              v-for="item in TalentInformationModule.departments"
              :key="item.id"
              :label="item.name"
              :value="item.name">
            </el-option>
          </el-select>

          <el-select v-model.trim="job" style="width: 160px" size="mini" placeholder="所有岗位">
            <el-option
              v-for="item in TalentInformationModule.jobs"
              :key="item.id"
              :label="item.name"
              :value="item.name">
            </el-option>
          </el-select>


          <!--触发搜索函数的按钮-->
          <el-button type="primary" size="mini" style="margin-left: 5px" icon="el-icon-search" @click="searchTalent()">
            搜索
          </el-button>
          <!--触发搜索函数的按钮-->
          <el-button type="primary" size="mini" style="margin-left: 5px" icon="el-icon-back" @click="emptySearch()">
            重置
          </el-button>

        </div>

      </el-header>
      <!--内容头部结束-->

      <!--人才信息主体内容-->
      <el-main>
        <div>
          <el-row>
            <el-col :span="24">
              <el-table
                border
                stripe
                :data="TalentInformations"
                v-loading="tableLoading"
                size="mini"
                style="width: 100%">
                <el-table-column
                  prop="id"
                  align="left"
                  fixed
                  label="编号"
                  width="50">
                </el-table-column>
                <el-table-column
                  prop="name"
                  align="left"
                  fixed
                  label="姓名"
                  width="90">
                </el-table-column>

                <el-table-column
                  prop="sex"
                  align="left"
                  label="性别"
                  width="50">
                </el-table-column>

                <el-table-column
                  prop="age"
                  align="left"
                  label="年龄"
                  width="50">
                </el-table-column>

                <el-table-column
                  prop="noteWorker"
                  align="left"
                  label="录入人"
                  width="90">
                </el-table-column>

                <el-table-column
                  prop="phoneNumber"
                  align="left"
                  label="电话号码"
                  width="140">
                </el-table-column>

                <el-table-column
                  prop="wx"
                  align="left"
                  label="微信"
                  width="140">
                </el-table-column>

                <el-table-column
                  prop="department"
                  align="left"
                  label="应聘部门"
                  width="140">
                </el-table-column>

                <el-table-column
                  prop="job"
                  align="left"
                  label="应聘职位"
                  width="140">
                </el-table-column>

                <el-table-column
                  prop="transfer"
                  align="left"
                  label="可否工作调动"
                  width="140"
                  :formatter = "transferFormat">
                </el-table-column>

                <el-table-column
                  prop="income"
                  align="left"
                  label="目前收入"
                  width="140">
                </el-table-column>

                <el-table-column
                  prop="socialSecurityPayment"
                  align="left"
                  label="是否缴纳社保"
                  width="140"
                  :formatter = "socialSecurityPaymentFormat">
                </el-table-column>

                <el-table-column
                  prop="education"
                  align="left"
                  label="学历"
                  width="80">
                </el-table-column>

                <el-table-column
                  prop="university"
                  align="left"
                  label="毕业院校"
                  width="140">
                </el-table-column>

                <el-table-column
                  prop="majorName"
                  align="left"
                  label="专业名称"
                  width="140">
                </el-table-column>

                <el-table-column
                  prop="entryTime"
                  align="left"
                  label="录入日期"
                  width="140">
                </el-table-column>

                <el-table-column
                  prop="channelSources"
                  align="left"
                  label="渠道"
                  width="90">
                </el-table-column>

                <el-table-column
                  prop="statusName"
                  align="left"
                  label="人员状态"
                  width="90">
                </el-table-column>

                <el-table-column
                  prop="idNumber"
                  align="left"
                  label="身份证号"
                  width="140">
                </el-table-column>

                <el-table-column
                  prop="isMarry"
                  align="left"
                  label="婚否"
                  width="70">
                </el-table-column>

                <el-table-column
                  prop="politicalOutlook"
                  align="left"
                  label="政治面貌"
                  width="110">
                </el-table-column>

                <el-table-column
                  prop="famousRace"
                  align="left"
                  label="民族"
                  width="90">
                </el-table-column>

                <el-table-column
                  prop="nativePlace"
                  align="left"
                  label="籍贯"
                  width="90">
                </el-table-column>

                <el-table-column
                  prop="email"
                  align="left"
                  label="电子邮箱"
                  width="140">
                </el-table-column>

                <el-table-column
                  prop="birthDate"
                  align="left"
                  label="出生日期"
                  width="140">
                </el-table-column>

                <el-table-column
                  prop="address"
                  align="left"
                  label="联系地址"
                  width="140">
                </el-table-column>



                <!--<el-table-column
                  prop="workerId"
                  align="left"
                  label="工号"
                  width="90">
                </el-table-column>-->



                <el-table-column
                  fixed="right"
                  label="操作"
                  width="250">

                  <!--嵌套一个内模板包含两个操作按钮-->
                  <template slot-scope="scope">
                    <!--编辑按钮-->
                    <el-button @click="showEditTalentInformationView(scope.row)" style="padding: 3px 4px 3px 4px;margin: 2px"
                               size="mini">编辑
                    </el-button>
                    <!--加入候选人按钮-->
                    <el-button type="primary" style="padding: 3px 4px 3px 4px;margin: 2px" size="mini"
                               @click="addInterview(scope.row)">加入候选人
                    </el-button>

                    <!--添加备注按钮-->
                    <el-button type="primary" style="padding: 3px 4px 3px 4px;margin: 2px" size="mini"
                               @click="showAddRemarkView(scope.row)">添加备注
                    </el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div style="display: flex;justify-content: space-between;margin: 2px">

                <!--分页展示的组件()-->
              <el-pagination
                background
                :page-size="10"
                :current-page="currentPage"
                @current-change="currentChange"
                :page-sizes="[10, 15, 20]"
                layout="prev,total, pager, next,jumper"
                :total="totalCount">
              </el-pagination>
            </div>
            </el-col>
          </el-row>
        </div>
      </el-main>
    </el-container>
    <!--主体内容结束-->

    <!--添加备注弹出框-->
    <el-form :model="remarkModule"  ref="addRemarkForm"  style="margin: 0px;padding: 0px;" label-position="left">
      <div style="text-align: left">
        <el-dialog
          :title="addReamrkTitle"
          style="padding: 0px;"
          :close-on-click-modal="false"
          :visible.sync="addRemarkVisible"
          width="45%">
          <el-row>
            <el-col :span="24">
              <el-form-item label="备注一:" prop="remark1">
                <el-input type="textarea" :rows="5" v-model.trim="remarkModule.oldRemark1" size="mini" :readonly="true"></el-input>
                <el-input type="textarea" :rows="5" v-model.trim="remarkModule.remark1" placeholder="请输入备注内容..." size="mini"></el-input>
                <span>{{this.remarkModule.wordSize-this.remarkModule.remark1.length-this.remarkModule.oldRemark1.length}}/不超过{{this.remarkModule.wordSize}}字</span>
              </el-form-item>
            </el-col>
          </el-row>

          <el-row>
            <el-col :span="24">
              <el-form-item label="备注二:" prop="remark2">
                <el-input type="textarea" :rows="5" v-model.trim="remarkModule.oldRemark2" size="mini" :readonly="true"></el-input>
                <el-input type="textarea" :rows="5"v-model.trim="remarkModule.remark2" placeholder="请输入备注内容..." size="mini"></el-input>
                <span>{{this.remarkModule.wordSize-this.remarkModule.remark2.length-this.remarkModule.oldRemark2.length}}/不超过{{this.remarkModule.wordSize}}字</span>
              </el-form-item>
            </el-col>
          </el-row>

          <!--弹出框底部包含两个按钮-->
          <span slot="footer" class="dialog-footer">
            <el-button size="mini" @click="cancelRemark()">取 消</el-button>
            <el-button size="mini" type="primary" @click="saveRemark()">保存</el-button>
          </span>
        </el-dialog>
      </div>
    </el-form>

    <!--新录入弹出框部分-->
    <el-form :model="TalentInformationModule"  :rules="rules" ref="addForm" style="margin: 0px;padding: 0px;" label-position="left" >
      <div style="text-align: left">
        <!--弹出框主题内容-->
        <el-dialog
          :title="addFormTitle"
          style="padding: 0px;"
          :close-on-click-modal="false"
          :visible.sync="addFormVisible"
          width="50%">
          <el-row>
            <el-col :span="8">
              <div>
                <el-form-item label="姓名:" prop="name">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="TalentInformationModule.name" size="mini" style="margin-left: 26px; width: 180px"
                            placeholder="请输入员工姓名"></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="性别:" prop="sex">
                  <el-radio-group v-model.trim="TalentInformationModule.sex" style="margin-left: 26px;width: 180px;">
                    <el-radio label="男">男</el-radio>
                    <el-radio style="margin-left: 10px" label="女">女</el-radio>
                  </el-radio-group>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="年龄:" prop="age">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="TalentInformationModule.age" size="mini" style="margin-left: 26px; width: 180px"
                            placeholder="请输入年龄..."></el-input>
                </el-form-item>
              </div>
            </el-col>
          </el-row>

          <el-row>
            <el-col :span="8">
              <div>
                <el-form-item label="微信:" prop="wx">
                  <el-input  v-model.trim="TalentInformationModule.wx" size="mini" style="margin-left: 26px;width: 180px"
                             placeholder="微信..."></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <el-form-item label="应聘部门:" prop="department">
                <el-select v-model.trim="TalentInformationModule.department" style="margin-left: 26px;;width: 180px" size="mini" placeholder="请选择部门">
                  <el-option
                    v-for="item in TalentInformationModule.departments"
                    :key="item.id"
                    :label="item.name"
                    :value="item.name">
                  </el-option>
                </el-select>
              </el-form-item >
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="应聘职位:" prop="job">
                  <el-select v-model.trim="TalentInformationModule.job" style="width: 180px" size="mini" placeholder="请选择应聘职位">
                    <el-option
                      v-for="item in TalentInformationModule.jobs"
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
                <el-form-item label="婚姻状况:" prop="isMarry">
                  <el-radio-group v-model.trim="TalentInformationModule.isMarry">
                    <el-radio label="已婚">已婚</el-radio>
                    <el-radio style="margin-left: 15px" label="未婚">未婚</el-radio>
                    <el-radio style="margin-left: 15px" label="离异">离异</el-radio>
                  </el-radio-group>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="接受工作调动:" prop="workChange">
                  <el-radio-group v-model.trim="TalentInformationModule.workChange">
                    <el-radio label="能" >能</el-radio>
                    <el-radio style="margin-left: 15px" label="否" >否</el-radio>
                  </el-radio-group>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="民族:" prop="famousRace">
                  <el-select v-model.trim="TalentInformationModule.famousRace" style="margin-left: 26px;width: 180px" size="mini" placeholder="请选择民族">
                    <el-option
                      v-for="item in TalentInformationModule.famousRaces"
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
                <el-form-item label="手机号码:" prop="phoneNumber">
                  <el-input prefix-icon="el-icon-phone" v-model.trim="TalentInformationModule.phoneNumber" size="mini" style="width: 180px"
                            placeholder="请输入手机号码..."></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="社保缴纳:" prop="socialPay">
                  <el-radio-group v-model.trim="TalentInformationModule.socialPay">
                    <el-radio label="是" >是</el-radio>
                    <el-radio style="margin-left: 15px" label="否" >否</el-radio>
                  </el-radio-group>
                </el-form-item>
              </div>
            </el-col>
            <el-col :span="8">
              <div>
                <el-form-item label="目前收入:" prop="currentIncome">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="TalentInformationModule.currentIncome" size="mini" style="margin-left: 26px; width: 180px"
                            placeholder="目前收入"></el-input>
                </el-form-item>
              </div>
            </el-col>
          </el-row>

          <el-row>
            <el-col :span="8">
              <div>
                <el-form-item label="籍贯:" prop="nativePlace">
                  <el-input v-model.trim="TalentInformationModule.nativePlace" size="mini" style="margin-left: 26px;width: 180px" placeholder="员工籍贯"></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="电子邮箱:" prop="email">
                  <el-input prefix-icon="el-icon-message" v-model.trim="TalentInformationModule.email" size="mini" style="width: 180px"
                            placeholder="请输入电子邮箱..."></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="联系地址:" prop="address">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="TalentInformationModule.address" size="mini" style="width: 180px"
                            placeholder="联系地址..."></el-input>
                </el-form-item>
              </div>
            </el-col>
          </el-row>

          <el-row>
            <el-col :span="8">
              <div>
                <el-form-item label="毕业院校:" prop="university">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="TalentInformationModule.university" size="mini" style="width: 180px"
                            placeholder="请输入毕业院校.."></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="专业名称:" prop="majorName">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="TalentInformationModule.majorName" size="mini" style="width: 180px"
                            placeholder="请输入专业名称..."></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="政治面貌:" prop="politicalOutlook">
                  <el-select v-model.trim="TalentInformationModule.politicalOutlook" style="width: 180px" size="mini" placeholder="政治面貌">
                    <el-option
                      v-for="item in TalentInformationModule.politicalOutlooks"
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
                <el-form-item label="学历:" prop="education">
                  <el-select v-model.trim="TalentInformationModule.education" style="margin-left: 26px;width: 180px" size="mini" placeholder="最高学历">
                    <el-option
                      v-for="item in TalentInformationModule.educations"
                      :key="item.id"
                      :label="item.name"
                      :value="item.name">
                    </el-option>
                  </el-select>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="出生日期:" prop="birthDate">
                  <el-date-picker
                    v-model.trim="TalentInformationModule.birthDate"
                    size="mini"
                    value-format="yyyy-MM-dd"
                    style="width: 180px"
                    type="date"
                    placeholder="出生日期">
                  </el-date-picker>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="身份证号:" prop="idNumber">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="TalentInformationModule.idNumber" size="mini" style="width: 180px"
                            placeholder="请输入员工身份证号码..."></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="工号:" prop="workerId" v-if="false">
                  <el-input v-model.trim="TalentInformationModule.workerId" size="mini" style="margin-left: 26px;width: 180px"
                            placeholder="员工工号..."></el-input>
                </el-form-item>
              </div>
            </el-col>
          </el-row>

          <!--弹出框底部包含两个按钮-->
          <span slot="footer" class="dialog-footer">
            <el-button size="mini" @click="cancelAdd('addForm')">取 消</el-button>
            <el-button size="mini" type="primary" @click="confirm('addForm')">保存</el-button>
          </span>
        </el-dialog>
      </div>
    </el-form>

    <!--编辑弹出框部分-->
    <el-form :model="TalentInformationModule"  :rules="rules" ref="editForm" style="margin: 0px;padding: 0px;" label-position="left" >
      <div style="text-align: left">
        <!--弹出框主题内容-->
        <el-dialog
          :title="editFormTitle"
          style="padding: 0px;"
          :close-on-click-modal="false"
          :visible.sync="editFormVisible"
          width="50%">
          <el-row>
            <el-col :span="8">
              <div>
                <el-form-item label="姓名:" prop="name">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="TalentInformationModule.name" size="mini" style="margin-left: 26px; width: 180px"
                            placeholder="请输入员工姓名"></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="性别:" prop="sex">
                  <el-radio-group v-model.trim="TalentInformationModule.sex" style="margin-left: 26px;width: 180px;">
                    <el-radio label="男">男</el-radio>
                    <el-radio style="margin-left: 10px" label="女">女</el-radio>
                  </el-radio-group>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="年龄:" prop="age">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="TalentInformationModule.age" size="mini" style="margin-left: 26px; width: 180px"
                            placeholder="请输入年龄..."></el-input>
                </el-form-item>
              </div>
            </el-col>
          </el-row>

          <el-row>
            <el-col :span="8">
              <div>
                <el-form-item label="微信:" prop="wx">
                  <el-input  v-model.trim="TalentInformationModule.wx" size="mini" style="margin-left: 26px;width: 180px" :readonly="ifEdit"
                             placeholder="微信..."></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <el-form-item label="应聘部门:" prop="department">
                <el-select v-model.trim="TalentInformationModule.department" style="margin-left: 26px;;width: 180px" size="mini" placeholder="请选择部门">
                  <el-option
                    v-for="item in TalentInformationModule.departments"
                    :key="item.id"
                    :label="item.name"
                    :value="item.name">
                  </el-option>
                </el-select>
              </el-form-item >
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="应聘职位:" prop="job">
                  <el-select v-model.trim="TalentInformationModule.job" style="width: 180px" size="mini" placeholder="请选择应聘职位">
                    <el-option
                      v-for="item in TalentInformationModule.jobs"
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
                <el-form-item label="婚姻状况:" prop="isMarry">
                  <el-radio-group v-model.trim="TalentInformationModule.isMarry">
                    <el-radio label="已婚">已婚</el-radio>
                    <el-radio style="margin-left: 15px" label="未婚">未婚</el-radio>
                    <el-radio style="margin-left: 15px" label="离异">离异</el-radio>
                  </el-radio-group>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="接受工作调动:" prop="workChange">
                  <el-radio-group v-model.trim="TalentInformationModule.workChange">
                    <el-radio label="能">能</el-radio>
                    <el-radio style="margin-left: 15px" label="否">否</el-radio>
                  </el-radio-group>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="民族:" prop="famousRace">
                  <el-select v-model.trim="TalentInformationModule.famousRace" style="margin-left: 26px;width: 180px" size="mini" placeholder="请选择民族">
                    <el-option
                      v-for="item in TalentInformationModule.famousRaces"
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
                <el-form-item label="手机号码:" prop="phoneNumber">
                  <el-input prefix-icon="el-icon-phone" v-model.trim="TalentInformationModule.phoneNumber" size="mini" style="width: 180px"
                            placeholder="请输入手机号码..."></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="社保缴纳:" prop="socialPay">
                  <el-radio-group v-model.trim="TalentInformationModule.socialPay">
                    <el-radio label="是">是</el-radio>
                    <el-radio style="margin-left: 15px" label="否">否</el-radio>
                  </el-radio-group>
                </el-form-item>
              </div>
            </el-col>
            <el-col :span="8">
              <div>
                <el-form-item label="目前收入:" prop="currentIncome">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="TalentInformationModule.currentIncome" size="mini" style="margin-left: 26px; width: 180px"
                            placeholder="目前收入"></el-input>
                </el-form-item>
              </div>
            </el-col>
          </el-row>

          <el-row>
            <el-col :span="8">
              <div>
                <el-form-item label="籍贯:" prop="nativePlace">
                  <el-input v-model.trim="TalentInformationModule.nativePlace" size="mini" style="margin-left: 26px;width: 180px" placeholder="员工籍贯"></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="电子邮箱:" prop="email">
                  <el-input prefix-icon="el-icon-message" v-model.trim="TalentInformationModule.email" size="mini" style="width: 180px"
                            placeholder="请输入电子邮箱..."></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="联系地址:" prop="address">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="TalentInformationModule.address" size="mini" style="width: 180px"
                            placeholder="联系地址..."></el-input>
                </el-form-item>
              </div>
            </el-col>
          </el-row>

          <el-row>
            <el-col :span="8">
              <div>
                <el-form-item label="毕业院校:" prop="university">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="TalentInformationModule.university" size="mini" style="width: 180px"
                            placeholder="请输入毕业院校.."></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="专业名称:" prop="majorName">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="TalentInformationModule.majorName" size="mini" style="width: 180px"
                            placeholder="请输入专业名称..."></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="政治面貌:" prop="politicalOutlook">
                  <el-select v-model.trim="TalentInformationModule.politicalOutlook" style="width: 180px" size="mini" placeholder="政治面貌">
                    <el-option
                      v-for="item in TalentInformationModule.politicalOutlooks"
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
                <el-form-item label="学历:" prop="education">
                  <el-select v-model.trim="TalentInformationModule.education" style="margin-left: 26px;width: 180px" size="mini" placeholder="最高学历">
                    <el-option
                      v-for="item in TalentInformationModule.educations"
                      :key="item.id"
                      :label="item.name"
                      :value="item.name">
                    </el-option>
                  </el-select>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="出生日期:" prop="birthDate">
                  <el-date-picker
                    v-model.trim="TalentInformationModule.birthDate"
                    size="mini"
                    value-format="yyyy-MM-dd"
                    style="width: 180px"
                    type="date"
                    placeholder="出生日期">
                  </el-date-picker>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="身份证号:" prop="idNumber">
                  <el-input prefix-icon="el-icon-edit" v-model.trim="TalentInformationModule.idNumber" size="mini" style="width: 180px"
                            placeholder="请输入员工身份证号码..."></el-input>
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="8">
              <div>
                <el-form-item label="工号:" prop="workerId" v-if="false">
                  <el-input v-model.trim="TalentInformationModule.workerId" size="mini" style="margin-left: 26px;width: 180px"
                            placeholder="员工工号..."></el-input>
                </el-form-item>
              </div>
            </el-col>
          </el-row>

          <!--accept=".doc,.docx,application/msword,application/vnd.openxmlformats-officedocument.wordprocessingml.document"-->
          <!--这里为上传简历和下载简历按钮-->
          <div>
            <el-upload
              :show-file-list="false"
              action="/interViewer/uploadPhoto?type=1"
              :data="param"
              :on-success="resumeUploadSuccess"
              :on-error="resumeUploadError"
              :disabled="this.resumeUploadBtnText=='正在上传'"
              :before-upload="beforeResumeUpload"
              style="display: inline">
              <el-button style="padding: 3px 4px 3px 4px;margin: 2px" type="info" :loading="this.resumeUploadBtnText=='正在上传'" size="mini">
                上传简历
              </el-button>
            </el-upload>
            <el-button style="padding: 3px 4px 3px 4px;margin: 2px" type="info" size="mini" @click="downloadResume">
              下载简历
            </el-button>
          </div>
          <!--弹出框底部包含两个按钮-->
          <span slot="footer" class="dialog-footer">
            <el-button size="mini" @click="cancelEidt">取 消</el-button>
            <el-button size="mini" type="primary" @click="confirm('editForm')">保存</el-button>
          </span>
        </el-dialog>
      </div>
    </el-form>
  </div>
</template>
<!--以上是数据显示域-->

<!--以下为逻辑处理域-->
<script>
  export default {
    inject:['reload'],/*注入relaod方法*/
    props:['user'],
    /*这里为常用的自定义变量*/
    data(){
      var validateName = (rule, value, callback) => {
        if (!this.TalentInformationModule.name) {
          return callback(new Error('必填:名字'));
        }else {
          return callback();
        }
      };
      var validateWx = (rule, value, callback) => {
        if (!this.TalentInformationModule.wx) {
          return callback(new Error('必填:微信号'));
        }else {
          return callback();
        }
      };
      var validateWorkChange = (rule, value, callback) => {
        if (!this.TalentInformationModule.workChange) {
          return callback(new Error('必选:工作调动'));
        }else {
          return callback();
        }
      };
      var validateJob = (rule, value, callback) => {
        if (!this.TalentInformationModule.job) {
          return callback(new Error('必填:应聘职位'));
        }else{
          return callback();
        }
      };

      var validateAge = (rule, value, callback) => {
        if (!this.TalentInformationModule.age) {
          return callback(new Error('必填:年龄'));
        }else{
            if(this.TalentInformationModule.age < 18 || this.TalentInformationModule.age> 60 ){
              return callback(new Error('年龄要在18到60岁之间'));
            }else{
              return callback();
            }
        }
      };

      var validateIsMarry = (rule, value, callback) => {
        if (!this.TalentInformationModule.isMarry) {
          return callback(new Error('必填:婚姻状况'));
        }else{
          return callback();
        }
      };
      var validateDepartment = (rule, value, callback) => {
        if (!this.TalentInformationModule.department) {
          return callback(new Error('必填:应聘部门'));
        }else{
          return callback();
        }
      };
      var validateSex = (rule, value, callback) => {
        if (!this.TalentInformationModule.sex) {
          return callback(new Error('必选:性别'));
        }else{
          return callback();
        }
      };

      var validatePhoneNumber = (rule, value, callback) => {
        if (!this.TalentInformationModule.phoneNumber) {
          return callback(new Error('必填:手机号码'));
        }else{
          const reg = /^1[3|4|5|7|8][0-9]\d{8}$/
          console.log(reg.test(this.TalentInformationModule.phoneNumber));
          if (reg.test(this.TalentInformationModule.phoneNumber)) {
            return callback();
          } else {
            return callback(new Error('请输入正确的手机号'));
          }
        }
      };

      return{
        tableLoading: false,
        resumeUploadBtnText:'',
        functionButton:-1,
        ifEdit:false,
        editId:'',
        param:{
            id:'',
        },
        resumeUploadBtnTest: '上传简历',
        back:'',
        count:0,
        advanceSearchViewVisible: false,
        keywords: '',
        totalCount: -1,
        currentPage: 1,
        select:'',
        status:[
          {stat:1,name:'候选'},
          {stat:2,name:'待面试'},
          {stat:3,name:'合格'},
          {stat:4,name:'不合格'},
          {stat:5,name:'待定'},
        ],
        talentId:'',
        name:'',
        sex:'',
        politicalOutlook:'',
        famousRace:'',
        nativePlace:'',
        email:'',
        address:'',
        phoneNumber:'',
        wx:'',
        job:'',
        department:'',
        workerId: '',
        education:'',
        statusName:'',
        university:'',
        majorName:'',
        idNumber:'',
        isMarry:'',

        TalentInformations:[],/*列表数据展示接收对象*/

        TalentInformationModule: {
          id:{},
          workChange:'',
          socialPay:'',
          currentIncome:'',
          address:'',
          name: '',
          sex: '',
          job:'',
          jobs:[
            {id:'1',name:'美导'},
            {id:'2',name:'店长'},
            {id:'3',name:'队员'},
            {id:'4',name:'队长'},
            {id:'5',name:'专员'},
            {id:'6',name:'主管'},
            {id:'7',name:'经理'},
            {id:'8',name:'总监'},

          ],
          department:'',
          departments:[
            {id:'1',name:'江苏'},
            {id:'2',name:'上海'},
            {id:'3',name:'浙江'},
            {id:'4',name:'南京'},
            {id:'5',name:'精英特战队'},
            {id:'6',name:'公司职能'},
          ],
          noteWorker: '',
          channelSources: '',
          InformationStatus: '',
          idNumber: '',
          isMarry: '',
          birthDate:'',
          politicalOutlook:'',
          politicalOutlooks: [
            {id:'1',name:'中共党员'},{id:'2',name:'中共预备党员'},{id:'3',name:'共青团员'},{id:'4',name:'民革党员'},{id:'5',name:'民盟盟员'},
            {id:'6',name:'民建会员'},{id:'7',name:'民进会员'},{id:'8',name:'农工党党员'}, {id:'9',name:'致公党党员'},{id:'10',name:'九三学社社员'},
            {id:'11',name:'台盟盟员'},{id:'12',name:'无党派民主人士'},{id:'13',name:'普通公民'}],
          famousRace:'',
          famousRaces: [
            {id:'1',name:'汉族'},{id:'2',name:'蒙古族'},{id:'3',name:'回族'},{id:'4',name:'藏族'},{id:'5',name:'维吾尔族'},{id:'6',name:'苗族'},{id:'7',name:'彝族'},{id:'8',name:'壮族'},{id:'9',name:'布依族'},{id:'10',name:'朝鲜族'},
            {id:'11',name:'满族'},{id:'12',name:'侗族'},{id:'13',name:'瑶族'},{id:'14',name:'白族'},{id:'15',name:'土家族'},{id:'16',name:'哈尼族'},{id:'17',name:'哈萨克族'},{id:'18',name:'傣族'},{id:'19',name:'黎族'},{id:'20',name:'傈僳族'},
            {id:'21',name:'佤族'},{id:'22',name:'畲族'},{id:'23',name:'高山族'},{id:'24',name:'拉祜族'},{id:'25',name:'水族'},{id:'26',name:'东乡族'},{id:'27',name:'纳西族'},{id:'28',name:'景颇族'},{id:'29',name:'柯尔克孜族'},{id:'30',name:'土族'},
            {id:'31',name:'达斡尔族'},{id:'32',name:'仫佬族'},{id:'33',name:'羌族'},{id:'34',name:'布朗族'},{id:'35',name:'撒拉族'},{id:'36',name:'毛难族'},{id:'37',name:'仡佬族'},{id:'38',name:'锡伯族'},{id:'39',name:'阿昌族'},{id:'40',name:'普米族'},
            {id:'41',name:'塔吉克族'},{id:'42',name:'怒族'},{id:'43',name:'乌孜别克族'},{id:'44',name:'俄罗斯族'},{id:'45',name:'鄂温克族'},{id:'46',name:'崩龙族'},{id:'47',name:'保安族'},{id:'48',name:'裕固族'},{id:'49',name:'京族'},{id:'50',name:'塔塔尔族'},
            {id:'51',name:'独龙族'},{id:'52',name:'鄂伦春族'},{id:'53',name:'赫哲族'},{id:'54',name:'门巴族'},{id:'55',name:'珞巴族'},{id:'56',name:'基诺族'}
          ],
          education:'',
          educations: [{id:'1',name:'初中'},{id:'2',name:'高中'},{id:'3',name:'大专'},{id:'4',name:'本科'},{id:'5',name:'研究生'},{id:'6',name:'博士'},{id:'7',name:'其他'}],
          age:'',
          entryTime: ''
        },
        addFormTitle:'',
        addFormVisible:false,
        editFormTitle:'',
        editFormVisible:false,
        addReamrkTitle:'',
        addRemarkVisible:false,
        remarkModule:{
          remarkId:'',
          oldRemark1:'',
          oldRemark2:'',
          remark1:'',
          remark2:'',
          wordSize:1500,
        },
        TalentInformation:{
          name:'',
          sex:'',
          noteWorker:'',
          channelSources:'',
          InformationStatus:'',
          idNumber:'',
          isMarry:'',
          politicalOutlook:'',
          famousRace:'',
          nativePlace:'',
          email:'',
          address:'',
          phoneNumber:'',
          workerId:'',
          education:'',
          university:'',
          majorName:'',
          entryTime:''
        },
        loginForm: {
          username: '',
          password: ''
        },

        /*表单验证规则*/
        rules: {
          name: [{required:true, validator:validateName,trigger: 'blur'}],/*姓名必填*/
          job:[{required: true,validator: validateJob,trigger: 'blur'}],/*应聘职位必填*/
          isMarry: [{required: true, validator: validateIsMarry, trigger: 'blur'}],/*婚姻状况必填*/
         /* phoneNumber: [{required: true, validator: validatePhoneNumber,trigger: 'blur'}],电话号码必填*/
          age: [{required: true, validator: validateAge,trigger: 'blur'}],/*年龄必填*/
          department: [{required: true, validator: validateDepartment,trigger: 'blur'}],/*应聘部门必填*/
          sex: [{required: true, validator: validateSex,trigger: 'blur'}],/*性别必填*/
          wx: [{required: true, validator: validateWx,trigger: 'blur'}],/*微信号必填*/
          workChange: [{required: true, validator: validateWorkChange,trigger: 'blur'}],/*工作调动必填*/
        }
      }
    },

    /*自动访问的逻辑函数可以放这里*/
    mounted: function () {
      /*this.loadTalentInformations();*/
      this.searchTalent();
    },
    /*以下为自定义函数*/
    methods:{
      transferFormat(row) {
        if (row.transfer === 0) {
          return '否'
        } else if (row.transfer === 1) {
          return '能'
        }else{
          return '未知';
        }
      },

      socialSecurityPaymentFormat(row){
        if (row.socialSecurityPayment === 0) {
          return '否'
        } else if (row.socialSecurityPayment === 1) {
          return '是'
        }else{
          return '未知';
        }
      },

      /*显示添加备注弹出框*/
      showAddRemarkView(row){
        if(row.noteUserName===this.$store.state.user.username || 'admin'=== this.$store.state.user.username){
          this.addReamrkTitle = "添加备注";
          this.remarkModule.remarkId = row.id;
          /*加载上次备注信息*/
          this.loadOldRemark();
          this.addRemarkVisible = true;
        }else {
          this.$message({type: 'warning', message: '您没有操作权限!'});
        }

      },

      /*加载备注信息*/
      loadOldRemark(){
        var _this = this;
        this.getRequest("/talent/mationId?id="+this.remarkModule.remarkId).then(resp=> {
          if (resp) {
            //查询成功则赋值
            if (resp.status == 200) {
              var data = resp.data.data;
              this.remarkModule.oldRemark1 = data.remark1;
              this.remarkModule.oldRemark2 = data.remark2;
            }
          }
        })
      },

      /*取消添加备注*/
      cancelRemark(){
        this.addremarkEmpty();
        this.addRemarkVisible = false;
      },

      /*将添加备注的参数清空*/
      addremarkEmpty(){
        this.remarkModule.remark1 = '',
        this.remarkModule.remark2 = '',
        this.remarkModule.oldRemark1 = '',
        this.remarkModule.oldRemark2 = '',
        this.remarkModule.remarkId = ''
      },

      /*保存添加备注*/
      saveRemark(){
        var _this = this;
        /*判断是否获取到对应的行id*/
        if(this.remarkModule.remarkId ==''){
          /*没获取到给出提示*/
          this.$message({type: 'error', message: 'remarkId is empty'});
        }else{
          /*判断字数是否超标*/
          if(this.remarkModule.oldRemark1.length+this.remarkModule.remark1.length >= this.remarkModule.wordSize || this.remarkModule.oldRemark2.length+this.remarkModule.remark2.length >= this.remarkModule.wordSize){
            this.$message({type: 'error', message: '字数不能超过'+this.remarkModule.wordSize+'字'});
          }else{
            if(this.remarkModule.remark1 != ''|| this.remarkModule.remark2 != ''){
              this.postRequest("/talent/addRemark",
                { id: this.remarkModule.remarkId,
                  remark1: this.remarkModule.oldRemark1+this.remarkModule.remark1,
                  remark2: this.remarkModule.oldRemark2+this.remarkModule.remark2
                }
               ).then(resp=> {
                if (resp && resp.status == 200) {
                  var data = resp.data.data;
                  this.$message({type: 'success', message: data});
                  _this.loadOldRemark();
                  _this.remarkModule.remark1 = '',
                  _this.remarkModule.remark2 = ''
                }
              })
            }else {
              this.$message({type: 'error', message: '请输入备注后保存!'});
            }
          }

        }

      },

      /*新录入框取消按钮*/
      cancelAdd(){
        this.addFormVisible=false;
      },
      /*编辑框取消按钮*/
      cancelEidt(){
        this.editFormVisible=false;
      },
      /*当前页数*/
      currentChange(currentChange){
        this.currentPage = currentChange;
        this.searchTalent();
      },

      //加入候选人按钮接口
      addInterview(row){
        if(row.noteUserName===this.$store.state.user.username || 'admin'=== this.$store.state.user.username) {
          var _this = this;
          this.getRequest("/talent/addInterview?&id=" + row.id).then(resp => {
            if (resp && resp.status == 200) {
              var data = resp.data;
              _this.back = data.data;
              this.$message({type: 'success', message: this.back});
              _this.loadTalentInformations()
            }
          })
        }else {
          this.$message({type: 'warning', message: '您没有操作权限!'});
        }

      },
      //加入候选人按钮接口
      addRemark(row){
        var _this = this;
        this.getRequest("?&id="+row.id).then(resp=> {
          if (resp && resp.status == 200) {
            var data = resp.data;
            _this.back = data.data;
            this.$message({type: 'success', message: this.back});
            _this.loadTalentInformations()
          }
        })
      },

      /*条件转换*/
      conditionTransfer(){
        if(this.TalentInformationModule.workChange=='能'){
          this.TalentInformationModule.workChange = 1;
        }else if(this.TalentInformationModule.workChange=='否'){
          this.TalentInformationModule.workChange = 0;
        }
        if(this.TalentInformationModule.socialPay=='是'){
          this.TalentInformationModule.socialPay = 1;
        }else if(this.TalentInformationModule.socialPay=='否'){
          this.TalentInformationModule.socialPay = 0;
        }
      },

      //人才信息添加接口
      addTalentInformation(){
        var _this = this;
        _this.conditionTransfer();
        _this.getRequest("/talent/addMation?noteUserName="+this.$store.state.user.username
          +"&name="+this.TalentInformationModule.name
          +"&sex="+this.TalentInformationModule.sex
          +"&age=" +this.TalentInformationModule.age
          +"&politicalOutlook="+this.TalentInformationModule.politicalOutlook
          +"&famousRace="+this.TalentInformationModule.famousRace
          +"&nativePlace="+this.TalentInformationModule.nativePlace
          +"&transfer="+this.TalentInformationModule.workChange
          +"&income="+this.TalentInformationModule.currentIncome
          +"&socialSecurityPayment="+this.TalentInformationModule.socialPay
          +"&email="+this.TalentInformationModule.email
          +"&address="+this.TalentInformationModule.address
          +"&department="+this.TalentInformationModule.department
          +"&phoneNumber="+this.TalentInformationModule.phoneNumber
          +"&job="+this.TalentInformationModule.job
          +"&workerId=" +this.TalentInformationModule.workerId
          +"&education="+this.TalentInformationModule.education
          +"&university="+this.TalentInformationModule.university
          +"&majorName="+this.TalentInformationModule.majorName
          +"&idNumber="+this.TalentInformationModule.idNumber
          +"&isMarry="+this.TalentInformationModule.isMarry
          +"&wx="+this.TalentInformationModule.wx
          +"&birthDate="+this.TalentInformationModule.birthDate).then(resp=> {
          if (resp && resp.status == 200) {
            var data = resp.data;
            if(data.code==200){

              if(data.data.name!=null&&data.data.phoneNumber!=null){
                  /*如果返回信息为一条人才信息,则说明是当前账户为地推人员,进行判断是否进入编辑页面*/
                _this.infoRepeatDeal(data.data);
              }else{
                /*添加成功则提示成功信息*/
                this.$message({type: 'success', message: data.data});
              }
                /*当前账户为非地推人员,则提示信息重复*/
            }else if(data.code==400){
              this.$message({type: 'error', message: data.data});
            }
            //每次添加完信息重新加载人才数据
            _this.loadTalentInformations();
          }
        })
        this.addFormVisible=false;

      },

      /*重复信息处理*/
      infoRepeatDeal(data){
        var _this = this;
        var data = data;
        /*如果信息重复会返回重复信息,并且将信息赋值给编辑界面显示*/
        this.$confirm('该人才信息已在人才库,是否进入编辑页面?', '提示', {
          confirmButtonText: '是',
          cancelButtonText: '否',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '正在进入编辑页面!',
          });
          /*进入编辑页面*/
          this.addToEditView(data);
          this.functionButton=1;/*如果进入编辑页面则确定按钮应该调用编辑接口,这里赋值为1,仅做标记使用*/

        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消'
          });
        });
      },

      /*将重复人才信息赋值到编辑框*/
      addToEditView(data){
        var data = data;
        this.editFormTitle = "编辑"
        this.editId=data.id;
        this.TalentInformationModule.name=data.name;
        this.TalentInformationModule.sex=data.sex;
        this.TalentInformationModule.age=data.age;
        this.TalentInformationModule.politicalOutlook=data.politicalOutlook;
        this.TalentInformationModule.famousRace=data.famousRace;
        this.TalentInformationModule.nativePlace=data.nativePlace;
        this.TalentInformationModule.workChange=data.transfer;
        this.TalentInformationModule.currentIncome=data.income;
        this.TalentInformationModule.socialPay=data.socialSecurityPayment;
        this.TalentInformationModule.email=data.email;
        this.TalentInformationModule.address=data.address;
        this.TalentInformationModule.phoneNumber=data.phoneNumber;
        this.TalentInformationModule.department=data.department;
        this.TalentInformationModule.job=data.job;
        this.TalentInformationModule.wx=data.wx;
        this.TalentInformationModule.birthDate=data.birthDate;
        this.TalentInformationModule.workerId=data.workerId;
        this.TalentInformationModule.education=data.education;
        this.TalentInformationModule.university=data.university;
        this.TalentInformationModule.majorName=data.majorName;
        this.TalentInformationModule.idNumber=data.idNumber;
        this.TalentInformationModule.isMarry=data.isMarry;
        this.editFormVisible = true;

      },

      //人才信息编辑接口
      editTalentInformation(){
        var _this = this;
        _this.conditionTransfer();
        _this.getRequest("/talent/edit?noteUserName="+this.$store.state.user.username
          +"&id="+this.editId+"&name="+this.TalentInformationModule.name
          +"&sex="+this.TalentInformationModule.sex
          +"&age="+this.TalentInformationModule.age
          +"&politicalOutlook="+this.TalentInformationModule.politicalOutlook
          +"&famousRace="+this.TalentInformationModule.famousRace
          +"&nativePlace="+this.TalentInformationModule.nativePlace
          +"&transfer="+this.TalentInformationModule.workChange
          +"&income="+this.TalentInformationModule.currentIncome
          +"&socialSecurityPayment="+this.TalentInformationModule.socialPay
          +"&email="+this.TalentInformationModule.email
          +"&address="+this.TalentInformationModule.address
          +"&phoneNumber="+this.TalentInformationModule.phoneNumber
          +"&department="+this.TalentInformationModule.department
          +"&job="+this.TalentInformationModule.job
          +"&workerId="+this.TalentInformationModule.workerId
          +"&education="+this.TalentInformationModule.education
          +"&university="+this.TalentInformationModule.university
          +"&majorName="+this.TalentInformationModule.majorName
          +"&idNumber="+this.TalentInformationModule.idNumber
          +"&isMarry="+this.TalentInformationModule.isMarry
          +"&wx="+this.TalentInformationModule.wx
          +"&birthDate="+this.TalentInformationModule.birthDate).then(resp=> {
          if (resp && resp.status == 200) {
            var data = resp.data;
            if(data.data==='修改姓名次数超过限制,请勿在修改姓名'){
              this.$message({type: 'warning', message: data.data});
            }else if(data.data==='修改成功'){
              this.$message({type: 'success', message: data.data});
            }

            this.editFormVisible = false;
            _this.loadTalentInformations();
          }
        })

      },

      //编辑和新录入界面的确定接口
      confirm(formName){
        var _this= this;
        this.$refs[formName].validate((valid) => {
          if (valid) {
            //如果有editId进入编辑接口
            if(this.editId && this.functionButton===-1){
              _this.editTalentInformation();
              _this.reload()
              //没有则进入添加接口
            }else if(this.functionButton===-1){
              _this.addTalentInformation();/*否则正常进入添加接口*/
              _this.reload()
            }else if(this.functionButton===1){
              _this.editTalentInformation();
            }
          }else {
            return false;
          }

        });

      },

      //加载人才信息
      loadTalentInformations(){
        var _this = this;
        this.tableLoading = true;
        this.getRequest("/talent/information/list?userName="+this.$store.state.user.username+"&pageNumber=" + this.currentPage + "&pageSize=10").then(resp=> {
          this.tableLoading = false;
          if (resp && resp.status == 200) {
            var data = resp.data;
            _this.TalentInformations = data.data.list;
            _this.totalCount = data.data.total;
          }
        })
      },

      //简历上传成功
      resumeUploadSuccess(response){

        this.$message({type: success, message: "上传成功!"});

        this.loadTalentInformations();
        this.resumeUploadBtnTest = '上传简历';
      },
      //简历上传失败
      resumeUploadError(err, file, fileList){
        this.$message({type: 'error', message: "上传失败!"});
        this.resumeUploadBtnTest = '上传简历';
      },
      //简历上传之前
      beforeResumeUpload(file){
        this.resumeUploadBtnTest = '正在上传';
      },

      //下载简历
      downloadResume(){
        window.open("/upFile/downloadFile?id="+this.editId, "_parent");
      },

      //打开新录入人才信息界面
      showAddTalentInformationView(){
        this.addFormTitle = "新录入";
        this.functionButton=-1;
        this.empty();
        this.editId ='',
        this.addFormVisible = true;
      },

      //将本地变量置空
      empty(){
        this.TalentInformationModule.name='',
        this.TalentInformationModule.sex='',
        this.TalentInformationModule.age='',
        this.TalentInformationModule.politicalOutlook='',
        this.TalentInformationModule.famousRace='',
        this.TalentInformationModule.nativePlace='',
        this.TalentInformationModule.workChange='',
        this.TalentInformationModule.currentIncome='',
        this.TalentInformationModule.socialPay='',
        this.TalentInformationModule.email='',
        this.TalentInformationModule.address='',
        this.TalentInformationModule.phoneNumber='',
        this.TalentInformationModule.department='',
        this.TalentInformationModule.job='',
        this.TalentInformationModule.workerId='',
        this.TalentInformationModule.birthDate='',
        this.TalentInformationModule.wx='',
        this.TalentInformationModule.education='',
        this.TalentInformationModule.university='',
        this.TalentInformationModule.majorName='',
        this.TalentInformationModule.idNumber='',
        this.TalentInformationModule.isMarry=''
      },

      //打开编辑页面
      showEditTalentInformationView(row){
        if(row.noteUserName===this.$store.state.user.username || 'admin'=== this.$store.state.user.username){
          this.searchRoleByUsename();/*根据用户名查询当前用户角色*/
          this.editFormTitle = "编辑";
          this.ifEdit = false;
          this.editId = row.id;
          this.param.id=this.editId;
          this.showById();/*根据id查询人才信息数据,并赋值给编辑框里*/
          this.editFormVisible = true;
        }else {
          this.$message({type: 'warning', message: '您没有操作权限!'});
        }

      },

      //搜索框与列表结果动态查询
      keywordsChange(val){
        if (val == '') {
          this.searchTalent();
        }
      },

      /*通过用户名查询角色*/
      searchRoleByUsename(){
        var _this = this;
        this.getRequest("/talent/searchRoid?userName="+this.$store.state.user.username).then(resp=> {
          if (resp && resp.status == 200) {
            var data = resp.data.data;
            /*如果是地推就不允许编辑姓名手机号微信号*/
            if(data.roleId===4){
                console.log(data)
                _this.ifEdit = true;
            }
          }
        })
      },

      //搜索框通过名字模糊查询
      searchTalent(){
        var _this = this;
        if(this.keywords == '' && this.department == '' && this.job == ''){
          _this.loadTalentInformations()
        }else {
          this.getRequest(" /talent/search?userName="+this.$store.state.user.username+"&pageNumber=" + this.currentPage + "&pageSize=10&name="+this.keywords+"&department="+this.department+"&job="+this.job).then(resp=> {
            if (resp && resp.status == 200) {
              var data = resp.data;
              _this.TalentInformations = data.data.list;
             /* _this.keywords = '';
              _this.department = '';
              _this.job = '';*/
              _this.totalCount = data.data.total;
            }
          })
        }

      },

      /*清空搜索条件*/
      emptySearch(){
        var _this = this;
        _this.keywords = '';
        _this.department = '';
        _this.job = '';
      },

      //按照id查询人才信息
      showById(){
        var _this = this;
        this.getRequest("/talent/mationId?id="+this.editId).then(resp=> {
          if (resp) {
              //查询成功则赋值
            if (resp.status == 200) {
              var data = resp.data;
              _this.TalentInformationModule.name = data.data.name;
              _this.TalentInformationModule.sex = data.data.sex;
              _this.TalentInformationModule.age = data.data.age;
              _this.TalentInformationModule.politicalOutlook = data.data.politicalOutlook;
              _this.TalentInformationModule.nativePlace = data.data.nativePlace;
              if ( data.data.transfer===1){
                _this.TalentInformationModule.workChange ='能';
              }else if(data.data.transfer===0){
                _this.TalentInformationModule.workChange ='否';
              }
              _this.TalentInformationModule.currentIncome = data.data.income;
              if(data.data.socialSecurityPayment===1){
                _this.TalentInformationModule.socialPay='是';
              }else if( data.data.socialSecurityPayment===0){
                _this.TalentInformationModule.socialPay='否';
              }
              _this.TalentInformationModule.famousRace = data.data.famousRace;
              _this.TalentInformationModule.email = data.data.email;
              _this.TalentInformationModule.address = data.data.address;
              _this.TalentInformationModule.phoneNumber = data.data.phoneNumber;
              _this.TalentInformationModule.department = data.data.department;
              _this.TalentInformationModule.job = data.data.job;
              _this.TalentInformationModule.wx = data.data.wx;
              _this.TalentInformationModule.birthDate = data.data.birthDate;
              _this.TalentInformationModule.workerId = data.data.workerId;
              _this.TalentInformationModule.education = data.data.education;
              _this.TalentInformationModule.university = data.data.university;
              _this.TalentInformationModule.majorName = data.data.majorName;
              _this.TalentInformationModule.idNumber = data.data.idNumber;
              _this.TalentInformationModule.isMarry = data.data.isMarry;

              //查询失败则置空
            }else{
                this.empty();
            }
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
</style>
