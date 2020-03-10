<template>
  <div class="xxzx_cla">
    <!-- 查询 -->
    <div class="yjlb_cla">
      <!-- 输入 -->
      <div class="yjlbinput_cla">
        <div class="emailtheme_cla">
          <el-input
            v-model="username"
            placeholder="输入用户名称搜索"
            style="display:inline-block;width:300px;"
          ></el-input>
        </div>
      </div>
      <!-- button按钮 -->
      <div class="yjlbbutton_cla">
        <el-button type="primary" class="uFblack" @click="queryUser">查询</el-button>
        <el-button type="primary" class="uFblack" @click="dialogFormVisible = true">新增</el-button>
        <el-button type="primary" class="uFblack" @click="reset">重置</el-button>
      </div>
    </div>

    <!-- 查询结果列表 -->
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
      <!-- 单条录入面板 -->
      <el-dialog
        class="userDataInput"
        style="width:50%;padding-left: 25%;"
        title="用户录入"
        :visible.sync="dialogFormVisible"
      >
        <el-form
          :model="registeForm"
          ref="registeForm"
          :label-position="labelPosition"
          label-width="130px"
        >
          <el-form-item label="用户名" required>
            <el-input v-model="registerName" style="width:auto" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="电话">
            <el-input v-model="registerNumber" style="width:auto" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="邮箱">
            <el-input v-model="registerEmail" style="width:auto" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="部门/岗位">
            <el-input v-model="registerWork" style="width:auto" autocomplete="off"></el-input>
          </el-form-item>
          <!--           <el-form-item label="状态">
            <el-input v-model="registerState" style="width:auto" autocomplete="off"></el-input>
          </el-form-item>-->
          <el-form-item label="是否为管理员" required>
            <el-input v-model="registerIsadmin" style="width:auto" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="密码" required>
            <el-input v-model="registerPassword" style="width:auto" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="创建日期">
            <el-date-picker
              style="display:inline-block;width:200px;"
              v-model="yearValueInput"
              size="mini"
              type="year"
              placeholder="选择年"
            ></el-date-picker>
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
        style="width:50%;padding-left: 25%;"
        title="数据编辑"
        :visible.sync="dialogForm1Visible1"
      >
        <el-form
          :model="registeForm"
          ref="registeForm"
          :label-position="labelPosition"
          label-width="130px"
        >
          <el-form-item label="用户名" required>
            <el-input v-model="registerName" style="width:auto" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="电话">
            <el-input v-model="registerNumber" style="width:auto" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="邮箱">
            <el-input v-model="registerEmail" style="width:auto" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="部门/岗位">
            <el-input v-model="registerWork" style="width:auto" autocomplete="off"></el-input>
          </el-form-item>
          <!--           <el-form-item label="状态">
            <el-input v-model="registerState" style="width:auto" autocomplete="off"></el-input>
          </el-form-item>-->
          <el-form-item label="是否为管理员" required>
            <el-input v-model="registerIsadmin" style="width:auto" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="密码" required>
            <el-input v-model="registerPassword" style="width:auto" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="创建日期">
            <el-date-picker
              style="display:inline-block;width:200px;"
              v-model="yearValueInput"
              size="mini"
              type="date"
              placeholder="选择年"
            ></el-date-picker>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogForm1Visible1 = false">取 消</el-button>
          <el-button type="primary" @click="updateInfo">确 定</el-button>
        </div>
      </el-dialog>
      <!-- 永远为隐藏，使得导出时是所有数据而不是当前页数据 -->
      <el-table id="yj_tabel" :data="alldata" v-show="false">
        <el-table-column type="index" label="序号" width="60"></el-table-column>
        <el-table-column
          v-for="item in result_title"
          :key="item.id"
          :prop="item.data"
          :label="item.label"
          :width="item.width"
        ></el-table-column>
      </el-table>
    </div>
    <!-- 分页 -->
    <el-pagination
      background
      layout="prev, pager, next"
      :page-count="total"
      :current-page.sync="currentPage"
      class="resulttab_cla"
      style="position:absolute;top:75%;left:50%"
      @current-change="current_change"
    ></el-pagination>
  </div>
</template>

<script>
// import bus from "../common/bus.js";
// import commonFunc from "../common/commonFunction";
// import XLSX from "xlsx";
// import { reject } from "q";
export default {
  data() {
    return {
      tableData: [],
      registeForm: {},
      labelPosition: "left",
      emailtheme: "", //邮件主题
      username: "", //接受人
      sendtime: null, //发送时间范围
      result_title: [
        { id: 1, label: "用户名", data: "usename" },
        { id: 2, label: "电话", data: "tel", width: 200 },
        { id: 3, label: "邮箱", data: "email" },
        { id: 4, label: "部门/岗位", data: "engency" },
        { id: 5, label: "密码", data: "password" },
        { id: 6, label: "创建日期", data: "creatdata" }
      ], //列表头
      emailaddress: "", //邮箱地址
      emailpassword: "", //邮箱密码
      total: 0,
      pagesize: 8,
      currentPage: 1, //当前页号（默认值为1），用于更新后刷新列表
      paginations: 3, //分页数
      alldata: [], //导出的所有数据
      showrows: "7", //每页显示条数
      dialogFormVisible: false,
      dialogForm1Visible1: false,
      registerName: "",
      registerNumber: "",
      registerEmail: "",
      registerWork: "",
      registerState: "",
      yearValueInput: "",
      registerIsadmin: "",
      registerPassword: "",
      role: "",
      role_id: "",
      rolename: "",
      roledescribe: "",
      creatdate: "",
      st_no: "",
      proid: "",
      Username: "",
      email: "",
      work: "",
      state: "",
      Creatdate: "",
      password: "",
      isadmin: ""
    };
  },
  mounted() {
    this.getAll();
  },
  methods: {
    //查询结果翻页
    current_change(currentPage) {
      this.currentPage = currentPage;
    },
    getAll() {
      this.tableData = [];
      var self = this;
      let pointInfoUrl = "http://localhost:5000/";
      // let pointInfoUrl = " http://localhost:5000/";
      //   let parms = {};
      console.log(6);
      self.$axios.post(pointInfoUrl).then(res => {
        console.log(5);
        if (res.data.data.rowCount > 0) {
          self.tableData = res.data.data.rows;
          self.total = Math.round(self.tableData.length * (1 / 8));
          console.log(self.tableData.length);
        }
      });
    },
    queryUser() {
      this.tableData = [];
      var self = this;
      let parms = {
        key: "username",
        val: this.username.replace(/\s*/g, "")
      };
      let getByKVUrl = appUrl.serverUrl + "user1/getByKV"; //查询服务
      if (this.username == "") {
        this.getAll();
      } else {
        //根据条件查询
        self.$axios.post(getByKVUrl, parms).then(res => {
          if (res.data.data.rowCount > 0) {
            self.tableData = res.data.data.rows;
            self.currentPage = 1;
            self.total = Math.round(self.tableData.length * (1 / 8));
            console.log(self.tableData.length);
          }
        });
      }
    },
    formatJson(filterVal, jsonData) {
      return jsonData.map(v => filterVal.map(j => v[j]));
    },
    reset() {
      this.emailtheme = "";
      this.username = "";
      this.getAll();
    },
    //单条数据导入，提交表单
    submitRegister() {
      var self = this;
      // 获取注册时间
      let date = new Date().toLocaleString();
      // 判断是否输入的为空

      if (this.registerIsadmin === "1") {
        this.role = "guanliyuan" + new Date().valueOf();
        this.role_id = "guanliyuan" + new Date().valueOf();
        this.rolename = "管理员";
        this.roledescribe = "系统管理者，权限设置者，可以更改权限，删除用户等";
      } else {
        this.role = "putongyonghu" + new Date().valueOf();
        this.role_id = "putongyonghu" + new Date().valueOf();
        this.rolename = "普通用户";
        this.roledescribe = "只可访问系统，无法管理与设置权限";
      }
      let postData1 = {}; //保存数据对象
      let postData2 = {}; //保存数据对象
      // let postData3 = {};
      postData1.role = self.role;
      postData1.st_no = new Date().valueOf();
      postData1.proid = new Date().valueOf();
      postData1.username = self.registerName;
      postData1.email = self.registerEmail;
      postData1.work = self.registerWork;
      postData1.state = "激活";
      var d = new Date(self.yearValueInput);
      postData1.creatdate = d.getFullYear();
      postData1.password = self.registerPassword;
      postData1.telnum = self.registerNumber;
      postData1.isadmin = self.registerIsadmin;
      postData2.role_id = self.role_id;
      postData2.rolename = self.rolename;
      postData2.roledescribe = self.roledescribe;
      var f = new Date(self.yearValueInput);
      postData2.creatdate = f.getFullYear();
      let addroleUrl = appUrl.serverUrl + "role/addItem"; //注册服务
      let adduserUrl = appUrl.serverUrl + "user1/addItem"; //注册服务
      if (
        postData1.role == "" ||
        postData1.st_no == "" ||
        postData1.username == "" ||
        postData1.email == "" ||
        postData1.work == "" ||
        postData1.creatdate == "" ||
        postData1.telnum == "" ||
        postData1.isadmin == "" ||
        postData1.password == "" ||
        postData2.role_id == "" ||
        postData2.rolename == "" ||
        postData2.roledescribe == "" ||
        postData2.creatdate == ""
      ) {
        self.$message.error("信息录入失败，请完善用户信息");
      } else {
        self.$axios.post(addroleUrl, postData2).then(res => {
          if (res.data.data.rowCount > 0) {
            self.$message.success("成功录入");
          } else {
            self.$message.error("用户注册失败！");
          }
        });
        self.$axios.post(adduserUrl, postData1).then(res => {
          if (res.data.data.rowCount > 0) {
            self.$message.success("成功录入");
          } else {
            self.$message.error("录入失败！");
          }
        });
        self.dialogFormVisible = false;
        self.getAll();
      }
    },
    //数据修改
    handleEdit(index, row) {
      let self = this;
      this.dialogForm1Visible1 = true;
      this.proid = row.proid;
      this.role = row.role;
      this.st_no = row.st_no;
      this.registerName = row.username;
      this.registerEmail = row.email;
      this.registerWork = row.work;
      /* this.registerState = row.state; */
      this.yearValueInput = row.creatdate;
      this.registerNumber = row.telnum;
      this.registerPassword = row.password;
      this.registerIsadmin = row.isadmin;
    },
    updateInfo() {
      var self = this;
      var updateUserUrl = appUrl.serverUrl + "user1/updateUser";
      var myDate = new Date(this.yearValueInput);
      var Y = myDate.getFullYear();
      var M = myDate.getMonth() + 1;
      var D = myDate.getDate();
      var curDay = Y + "-" + M + "-" + D;
      let postData = {
        proid: this.proid,
        role: this.role,
        st_no: this.st_no,
        username: this.registerName,
        email: this.registerEmail,
        work: this.registerWork,
        /*         state: this.registerState,
         */ creatdate: curDay,
        telnum: this.registerNumber,
        password: this.registerPassword,
        isadmin: this.registerIsadmin
      };
      this.$axios.post(updateUserUrl, postData).then(res => {
        console.log(res);
        if (res.data.data.rowCount > 0) {
          self.$message.success("更新成功");
          self.getAll();
        } else {
          self.$message.error("更新失败");
        }
      });
    },
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
          deleteData.key = '"proid"';
          deleteData.val = "'" + row.proid + "'";
          let serverUrl = appUrl.serverUrl;
          let deletePointsUrl = appUrl.serverUrl + "user1/remove";
          this.$axios.post(deletePointsUrl, deleteData).then(res => {
            if (res.data.data.rowCount > 0) {
              let self = this;
              deleteData.key = '"role_id"';
              deleteData.val = "'" + row.role + "'";
              let serverUrl = appUrl.serverUrl;
              let deletePointsUrl = appUrl.serverUrl + "role/remove";
              self.$axios.post(deletePointsUrl, deleteData).then(res => {
                if (res.data.data.rowCount > 0) {
                  self.getAll();
                  self.$message({
                    type: "success",
                    message: "删除成功！"
                  });
                }
              });
            } else {
              this.$message({
                type: "error",
                message: "删除失败！"
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
    }
  }
};
</script>
<style scoped>
.uFblack {
  margin-right: 50px;
  width: 100px;
  background-color: #474747;
  border-color: #6b6b6b;
}
.xxzx_cla {
  height: 100%;
  background: #474747;
  position: absolute;
  width: 100%;
}
.yjlb_cla {
  display: block;
  border: solid 2px #a8a8a8;
  height: 20%;
  width: 100%;
  background: #6b6b6b;
  position: relative;
}
.emailtheme_cla {
  display: inline-block;
  left: 8%;
  top: 10%;
  position: absolute;
}
.yjlbinput_cla span {
  color: black;
}
.sendtime_cla {
  display: inline-block;
  position: absolute;
  top: 10%;
  right: 37%;
}
.inputtime_cla {
  display: inline-block;
  position: absolute;
  left: 100%;
  top: 1%;
}
.yjlbbutton_cla {
  display: inline-block;
  position: absolute;
  right: 35%;
  top: 10%;
}
/* 查询结果table */
.result_cla {
  display: block;
  width: 100%;
  /* height: 100%; */
  position: relative;
}
/* 发送邮箱设置 */
.sendemailset_cla {
  display: block;
  width: 95%;
  height: 22%;
  left: 2.5%;
  top: 68%;
  position: absolute;
  border-top: dashed 1px #959595;
}
.title_cla {
  display: inline-block;
  top: -75px;
  right: 15%;
  position: relative;
  color: chocolate;
  font-size: 18px;
}
.emailaddress_cla {
  display: inline-block;
  bottom: 22px;
  right: -12.5%;
  position: relative;
  color: black;
}
.emailpassword_cla {
  display: inline-block;
  bottom: -20px;
  right: 13%;
  position: relative;
  color: black;
}
.sendemailbutton_cla {
  display: inline-block;
  bottom: -70px;
  right: 31%;
  position: relative;
}
.resulttab_cla {
  top: -95px;
  display: inline-block;
  left: 34%;
  position: relative;
}
.useTable .el-table--border {
  height: 100%;
  background: #414141;
}
</style>
<style>
.userDataInput .el-form-item__label {
  color: #a3a9b7;
}
</style>