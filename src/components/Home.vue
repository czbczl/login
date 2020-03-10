<template>
  <div class="xxzx_cla">
    <!-- 查询 -->
    <div class="yjlb_cla">
      <!-- 输入 -->
      <div class="yjlbinput_cla">
        <div class="emailtheme_cla" style="float: left;">
          <el-input
            v-model="username"
            placeholder="输入用户名称搜索"
            style="display:inline-block width:60%;"
          ></el-input>
        </div>
        <!-- </div> -->
        <!-- button按钮 -->
        <div class="yjlbbutton_cla" style="float: left;">
          <el-button type="primary" class="uFblack" @click="queryUser">查询</el-button>
          <el-button type="primary" class="uFblack" @click="dialogFormVisible = true">新增</el-button>
          <el-button type="primary" class="uFblack" @click="reset">重置</el-button>
        </div>
      </div>
    </div>

    <!-- 查询结果列表 -->
    <!-- <div class="table1">  -->

    <div class="useTable result_cla">
    <el-table
        :data="tableData.slice((currentPage-1)*pagesize,currentPage*pagesize)"
        height="60%"
        border
      >
        <el-table-column label="用户名" prop="username" sortable></el-table-column>
        <el-table-column label="电话" prop="telnum" sortable></el-table-column>
        <el-table-column label="邮箱" prop="email" sortable></el-table-column>
        <el-table-column label="部门/岗位" prop="work" sortable></el-table-column>
        <!--         <el-table-column label="状态" prop="state" sortable></el-table-column>
        -->
        <el-table-column label="是否为管理员" prop="isadmin" sortable></el-table-column>
        <el-table-column label="密码" prop="password" sortable></el-table-column>
        <el-table-column label="创建日期" prop="creatdate" sortable></el-table-column>
        <el-table-column label="操作" width="320">
          <template slot-scope="scope">
            <div class="tabcol_cla2">
              <el-button type="primary" @click="handleEdit(scope.$index, scope.row)">修改</el-button>
              <el-button type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
            </div>
          </template>
        </el-table-column>
      </el-table>
      <!-- <el-table
       :data="tableData.slice((currentPage-1)*pagesize,currentPage*pagesize)"
        height="432px"
        border
        width="100%"
        :header-cell-style="{background:'#DDDDDD'}"
      >
        <el-table-column label="序号" type="index" :index="indexMethodx1" align="center"></el-table-column>
        <el-table-column label="用户名" prop="username" align="center"></el-table-column>
        <el-table-column label="中文名" prop="userrealname" align="center"></el-table-column>
        <el-table-column label="管理员" prop="isadmin" align="center"></el-table-column>
        <el-table-column label="所属部门" prop="management" align="center"></el-table-column>
        <el-table-column label="电话" prop="tel" align="center"></el-table-column>
        <el-table-column label="邮箱" prop="email" align="center"></el-table-column>
        <el-table-column label="创建日期" prop="createdate" align="center"></el-table-column>
        <el-table-column label="备注" prop="remark" width="200px" align="center"></el-table-column>
        <el-table-column label="操作" width="200px" align="center">
          <template slot-scope="scope">
            <div class="tabcol_cla2">
              <el-button type="primary" @click="handleEdit(scope.$index, scope.row)">修改</el-button>
              <el-button type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
            </div>
          </template>
        </el-table-column>
      </el-table> -->
      <div style="width:100%;height:10%">
        <!-- 导航栏 -->
        <el-pagination
          background
          layout="prev, pager, next"
          :page-count="total"
          :current-page.sync="currentPage"
          class="resulttab_cla"
          style="position:relative;"
          @current-change="userChange"
        ></el-pagination>
      </div>
      <!-- </div> -->
      <!-- 单条录入面板 -->
      <el-dialog
        class="userDataInput"
        style="width:100%;padding-left: 25%;"
        title="用户录入"
        :visible.sync="dialogFormVisible"
      >
        <el-form
          :model="registeForm"
          ref="registeForm"
          :label-position="labelPosition"
          label-width="130px"
          style="width:90%"
        >
          <el-form-item label="用户名" required>
            <el-input
              type="text"
              v-model="registerName"
              placeholder="请输入用户名"
              autocomplete="off"
              clearable
            ></el-input>
          </el-form-item>
          <el-form-item label="密码" required>
            <el-input
              type="password"
              v-model="registerPassword"
              placeholder="请输入密码（不少于6位）"
              autocomplete="new-password"
              show-password
            ></el-input>
          </el-form-item>
          <el-form-item label="确认密码" required>
            <el-input
              v-model="registerPasswordagain"
              placeholder="请确认密码"
              autocomplete="off"
              show-password
            ></el-input>
          </el-form-item>
          <el-form-item label="中文名" required>
            <el-input v-model="registerCHname" placeholder="请输入中文名" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="管理员" required>
            <el-select v-model="registerIsadmin" style="width:100%" placeholder="请选择">
              <el-option
                v-for="item in adminOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="所属部门" required>
            <el-input v-model="registermanagement" placeholder="请输入部门" autocomplete="off"></el-input>
          </el-form-item>

          <el-form-item label="电话" required>
            <el-input v-model="registerNumber" placeholder="请输入电话" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="邮箱" required>
            <el-input v-model="registerEmail" placeholder="请输入邮箱" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="备注">
            <el-input
              type="textarea"
              maxlength="200"
              show-word-limit
              v-model="remark"
              autocomplete="off"
            ></el-input>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible = false">取 消</el-button>
          <el-button type="primary" @click="submitRegister">确 定</el-button>
        </div>
      </el-dialog>

      <!-- 编辑修改面板 -->
      <el-dialog
        class="userDataInput"
        style="width:100%;padding-left: 25%;"
        title="数据编辑"
        :visible.sync="dialogFormVisible1"
      >
        <el-form
          :model="registeForm"
          ref="registeForm"
          :label-position="labelPosition"
          label-width="130px"
          style="width:90%"
        >
          <el-form-item label="用户名" required>
            <el-input v-model="xgregisterName" autocomplete="off" :readonly="true"></el-input>
          </el-form-item>
          <el-form-item label="密码" required>
            <el-input
              v-model="xgregisterPassword"
              placeholder="请输入密码（不少于6位）"
              autocomplete="off"
              show-password
            ></el-input>
          </el-form-item>
          <el-form-item label="确认密码" required>
            <el-input
              v-model="xgregisterPasswordagain"
              placeholder="请确认密码"
              autocomplete="off"
              show-password
            ></el-input>
          </el-form-item>
          <el-form-item label="中文名" required>
            <el-input v-model="xgregisterCHname" placeholder="请输入中文名" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="管理员" required>
            <el-select v-model="xgregisterIsadmin" style="width:100%" placeholder="请选择">
              <el-option
                v-for="item in adminOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="所属部门" required>
            <el-input v-model="xgregistermanagement" placeholder="请输入部门" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="电话" required>
            <el-input v-model="xgregisterNumber" placeholder="请输入电话" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="邮箱" required>
            <el-input v-model="xgregisterEmail" placeholder="请输入邮箱" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="备注">
            <el-input
              type="textarea"
              maxlength="200"
              show-word-limit
              v-model="xgremark"
              autocomplete="off"
            ></el-input>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible1 = false">取 消</el-button>
          <el-button type="primary" @click="updateInfo">确 定</el-button>
        </div>
      </el-dialog>
    </div>
  </div>
</template>
<script>
// import bus from "../common/bus.js";
// import commonFunc from "../common/commonFunction";
// import XLSX from "xlsx";
// import { reject } from "q";
import axios from "axios";
export default {
  data() {
    return {
      registerCHname: "",
      renyuan: "",
      username: "",
      Username: "",
      loading: true,
      labelPosition: "left",
      dialogFormVisible: false,
      dialogFormVisible1: false,
      tableData: [],
      registeForm: {},
      currentPage: 1, //当前页号（默认值为1），用于更新后刷新列表
      cesadmin: "",
      admin: "",
      showrows: "7", //每页显示条数
      total: 0,
      pagesize: 8,
      registerName: "",
      registerNumber: "",
      registerEmail: "",
      registermanagement: "",
      registerIsadmin: "否",
      registerPassword: "",
      registerPasswordagain: "",
      xgregisterName: "",
      xgregisterNumber: "",
      xgregisterEmail: "",
      xgregistermanagement: "",
      xgregisterIsadmin: "",
      xgregisterPassword: "",
      xgregisterPasswordagain: "",
      xgregisterCHname: "",
      xgremark: "",
      textarea: "",
      createdate: "",
      Password: "",
      project: "",
      tel: "",
      email: "",
      management: "",
      email: "",
      remark: "",
      isadmin: "",
      remark: "",
      tel: "",
      cesadmin: 8,
      userrealname: "",
      emailtheme: "",
      time: "",
      adminOptions: [
        {
          value: "是",
          label: "是"
        },
        {
          value: "否",
          label: "否"
        }
      ]
    };
  },

  mounted() {
    this.getAll();
  },
  methods: {
    // 序号
    indexMethodx1(index) {
      let counts_ = parseInt(this.pagesize);
      let currentpage_ = parseInt(this.currentPage) - 1;
      return index + 1 + counts_ * currentpage_;
    },
    //查询结果翻页
    userChange(currentPage) {
      this.currentPage = currentPage;
    },

    getAll() {
      console.log(11);
      this.tableData = [];
      var self = this;
      // let list = appUrl.serverUrl + "user/list";
      // let list = " http://47.105.166.208:8089/user/list";
      let list = " http://localhost:5000/";
      axios.get(list).then(res => {
        console.log(3);
        console.log(res);
        // self.tableData = res.data.data.rows[0].sarray_to_json;
        self.tableData = res.data;
        for (var i = 0; i < self.tableData.length; i++) {
          if (self.tableData[i].isadmin == 1) {
            self.tableData[i].isadmin = "是";
          } else {
            self.tableData[i].isadmin = "否";
          }
        }

        self.total = Math.ceil(self.tableData.length * (1 / 8));
        console.log(self.tableData.length);
      });
    },
    // 查询
    queryUser() {
      this.tableData = [];
      var self = this;
      let parms = {
        key: "username",
        val: this.username.replace(/\s*/g, "")
      };
      let getByKVUrl = appUrl.serverUrl + "user/getquery"; //查询服务
      if (this.username == "") {
        this.getAll();
      } else {
        //根据条件查询
        self.$axios.post(getByKVUrl, parms).then(res => {
          if (res.data.data.rowCount > 0) {
            self.tableData = res.data.data.rows;
            self.currentPage = 1;
            self.total = Math.ceil(self.tableData.length * (1 / 8));
          }
        });
      }
    },

    // 手机邮箱验证
    iPhoneAvailable(registerNumber) {
      var self = this;
      var Number = /^[1][3,4,5,7,8][0-9]{9}$/;
      var Availableemail = /^([a-zA-Z0-9]+[_|\_|\.]?)*[a-zA-Z0-9]+@([a-zA-Z0-9]+[_|\_|\.]?)*[a-zA-Z0-9]+\.[a-zA-Z]{2,3}$/;
      if (Number.test(registerNumber) || Availableemail.test) {
        this.$message({
          type: "error",
          message: "手机号不正确，请重新输入！"
        });
        return;
      } else if (self.registerEmail != Availableemail) {
        this.$message({
          type: "error",
          message: "邮箱不正确，请重新输入！"
        });
        return;
      } else {
      }
    },

    // 重置
    reset() {
      this.emailtheme = "";
      this.username = "";
      this.getAll();
    },
    // 修改
    handleEdit(index, row) {
      let self = this;
      this.dialogFormVisible1 = true;
      self.xgid = row.id;
      self.xgregisterName = row.username;
      self.xgst_no = row.st_no;
      self.xgregisterPassword = row.password;
      self.xgregistermanagement = row.management;
      self.xgregisterCHname = row.userrealname;
      self.xgregisterNumber = row.tel;
      self.xgregisterEmail = row.email;
      self.xgremark = row.remark;
      self.xgregisterIsadmin = row.isadmin;
      self.xgcreatedate = row.createdate;
      self.xgregisterPasswordagain = row.password;
    },
    updateInfo() {
      var self = this;
      var AvailableNumber = /^[1][3,4,5,7,8][0-9]{9}$/;
      var Availableemail = /^([a-zA-Z0-9]+[_|\_|\.]?)*[a-zA-Z0-9]+@([a-zA-Z0-9]+[_|\_|\.]?)*[a-zA-Z0-9]+\.[a-zA-Z]{2,3}$/;
      if (self.xgregisterPassword == "" || self.xgregisterPassword.length < 6) {
        alert("密码不能少于6位数,请重新输入");
        return;
      }
      if (self.xgregisterPassword != self.xgregisterPasswordagain) {
        alert("密码不一致，请重新输入");
        return;
      }
      if (self.xgregisterCHname == "") {
        alert("请输入中文名");
        return;
      }
      if (self.xgregistermanagement == "") {
        alert("请输入所属部门");
        return;
      }
      if (
        !AvailableNumber.test(self.xgregisterNumber) ||
        !Availableemail.test(self.xgregisterEmail)
      ) {
        alert("手机号或邮箱输入格式不正确!");
        return;
      } else {
        var updateUserUrl = appUrl.serverUrl + "user/updateUser";

        var cesadmin;
        if (self.xgregisterIsadmin == "是") {
          // console.log(self.registerIsadmin);

          cesadmin = "1";
        } else {
          cesadmin = "0";
        }

        var admin = cesadmin;
        let postData1 = {
          username: self.xgregisterName,
          st_no: self.xgst_no,
          password: self.xgregisterPassword,
          management: self.xgregistermanagement,
          userrealname: self.xgregisterCHname,
          tel: self.xgregisterNumber,
          email: self.xgregisterEmail,
          remark: self.xgremark,
          isadmin: admin,
          id: self.xgid,
          createdate: self.xgcreatedate
        };

        if (
          postData1.id == "" ||
          postData1.username == "" ||
          postData1.email == "" ||
          postData1.management == "" ||
          postData1.createdate == "" ||
          postData1.tel == "" ||
          postData1.isadmin == "" ||
          postData1.userrealname == "" ||
          postData1.password == ""
        ) {
          self.$message.error("信息录入失败，请完善用户信息");
        } else {
          self.$axios.post(updateUserUrl, postData1).then(res => {
            if (res.data.data.rowCount > 0) {
              self.getAll();
              self.$message.success("更新成功");
            } else {
              self.$message.error("更新失败");
            }
          });
          self.dialogFormVisible1 = false;
        }
      }
    },
    // 删除
    handleDelete(index, row) {
      let self = this;
      this.$confirm("此操作将永久删除该用户, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
        center: true
      })
        .then(() => {
          let deleteData = {};
          deleteData.key = '"username"';
          deleteData.val = "'" + row.username + "'";
          // let serverUrl = appUrl.serverUrl;
          let deleteUsersUrl = appUrl.serverUrl + "user/remove";
          this.$axios.post(deleteUsersUrl, deleteData).then(res => {
            if (res.data.data.rowCount > 0) {
              self.$message({ type: "success", message: "删除成功！" });
              self.getAll();
            } else {
              this.$message({
                type: "error",
                message: "数据删除失败！"
              });
            }
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          });
        });
    },

    // 注册
    submitRegister() {
      var self = this;
      // 获取时间
      let yy = new Date().getFullYear();
      let mm = new Date().getMonth() + 1;
      let dd = new Date().getDate();
      var curDay = yy + "-" + mm + "-" + dd;
      var AvailableNumber = /^[1][3,4,5,7,8][0-9]{9}$/;
      var Availableemail = /^([a-zA-Z0-9]+[_|\_|\.]?)*[a-zA-Z0-9]+@([a-zA-Z0-9]+[_|\_|\.]?)*[a-zA-Z0-9]+\.[a-zA-Z]{2,3}$/;
      if (self.registerName == "") {
        alert("请输入用户名");
        return;
      }
      if (self.registerPassword == "" || self.registerPassword.length < 6) {
        alert("密码不能少于6位数,请重新输入");
        return;
      }
      if (self.registerPassword != self.registerPasswordagain) {
        alert("密码不一致，请重新输入");
        return;
      }
      if (self.registerCHname == "") {
        alert("请输入中文名");
        return;
      }
      if (self.registermanagement == "") {
        alert("请输入所属部门");
        return;
      }
      if (
        !AvailableNumber.test(this.registerNumber) ||
        !Availableemail.test(this.registerEmail)
      ) {
        alert("手机号或邮箱输入格式不正确!");
        return;
      } else {
        var cesadmin;
        if (self.registerIsadmin == "是") {
          cesadmin = "1";
        } else {
          cesadmin = "0";
        }

        var admin = cesadmin;
        let postData1 = {}; //保存数据对象
        postData1.username = self.registerName;
        postData1.st_no = self.registerName;
        postData1.password = self.registerPassword;
        postData1.management = self.registermanagement;
        postData1.userrealname = self.registerCHname;
        postData1.tel = self.registerNumber;
        postData1.email = self.registerEmail;
        postData1.remark = self.remark;
        postData1.isadmin = admin;
        postData1.createdate = curDay;

        let parms = {
          //注册新用户前查询该用户名是否已注册
          key: "username",
          val: this.registerName
        };

        let adduserUrl = appUrl.serverUrl + "user/addItem"; //注册服务
        let getByKVUrl = appUrl.serverUrl + "user/getByKV"; //查询服务
        //在数据库中查询用户名是否存在
        this.$axios.post(getByKVUrl, parms).then(res => {
          if (res.data.data.rowCount > 0) {
            //表明数据库中可以查询到该用户名
            self.$message({
              message: "该用户名已存在，请重新设置用户名",
              type: "error"
            });
            return;
          } else {
            if (
              postData1.id == "" ||
              postData1.username == "" ||
              postData1.st_no == "" ||
              postData1.email == "" ||
              postData1.management == "" ||
              postData1.createdate == "" ||
              postData1.tel == "" ||
              postData1.isadmin == "" ||
              postData1.userrealname == "" ||
              postData1.password == ""
            ) {
              self.$message.error("信息录入失败，请完善用户信息");
            } else {
              self.$axios.post(adduserUrl, postData1).then(res => {
                // console.log(res);

                if (res.data.data.rowCount > 0) {
                  self.getAll();
                  self.$message.success("成功录入");
                } else {
                  self.$message.error("用户注册失败！");
                }
              });

              // self.dialogFormVisible = false;
            }
          }
        });
      }

      self.registerName = "";
      self.registerNumber = "";
      self.registermanagement = "";
      self.registerPassword = "";
      self.registerPasswordagain = "";
      self.registerCHname = "";
      self.registerEmail = "";
      self.remark = "";
      self.registerIsadmin = "否";
      self.dialogFormVisible = false;
    }
  }
};
</script>

<style scoped>
.uFblack {
  margin-left: 10px;
  width: 25.2%;
  background-color: #474747;
  border-color: #6b6b6b;
}
.yjlbinput_cla {
  width: 30%;
  /* line-height: 380%; */
  height: 100%;
  margin: auto;
}
.xxzx_cla {
  background: #474747;
  position: absolute;
  width: 100%;
  height: 93%;
  overflow-x: hidden;
  overflow-y: scroll;
}
/* .table1{
  height: 100%;
} */
.yjlb_cla {
  display: block;
  /* border-top: 1.5px solid #e4e7ed; */
  border-bottom: 1px solid #e4e7ed;
  height: %;
  width: 100%;
  background: #6b6b6b;
  position: relative;
}
.emailtheme_cla {
  width: 45%;
  display: inline-block;

  /* float: left; */
  margin: 3.8% auto;
}
.tabcol_cla2 .el-button {
  float: left;
  left: 18%;
  position: relative;
  text-align: center;
}

.yjlbbutton_cla {
  /* float: left; */
  display: inline-block;
  /* position: absolute; */
  margin: 3.8% -2.8%;
  width: 55%;
}
/* 查询结果table */
.result_cla {
  display: block;
  width: 95%;
  height: 84%;
  padding: 30px;
  /* position: relative; */
}
/* .usertabletitle{
  width: 95%;
  height: 84%;
} */
/* 分页 */
.resulttab_cla {
  display: inline-block;

  position: relative;
}
/* table 背景 */
.useTable .el-table--border {
  height: 70%;
  background: #414141;
}
/* .useTable .el-table--header {
  height: 39px;
 
} */

.useTable .el-table--body {
  width: 1249px;
}
</style>
<style>
.userDataInput .el-form-item__label {
  color: #a3a9b7;
  text-align: center;
}
/* el-table所有 */
.useTable.el-table__footer-wrapper,
.el-table__header-wrapper {
  overflow: hidden;
  height: 39px;
}
.useTable .el-dialog {
  margin-top: 5%;
  width: 26%;
  margin-left: 15%;
}
</style>
