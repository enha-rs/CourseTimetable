<template>
  <div>
    <div v-show="courseProvision_visible">
      <div class="search-first-row">
        <el-input v-model="schoolInput" placeholder="请输入学校名称"></el-input>
        <el-input v-model="collegeInput" placeholder="请输入学院名称"></el-input>
        <el-input v-model="majorInput" placeholder="请输入专业名称"></el-input>
        <el-input v-model="classNameInput" placeholder="请输入班级名称"></el-input>
      </div>
      <div class="search-second-row">
        <el-input v-model="courseInput" placeholder="请输入课程名称"></el-input>
        <el-input v-model="teacherInput" placeholder="请输入教师名称"></el-input>
        <!-- </div> -->
        <div><el-button type="danger">查询</el-button></div>
        <el-checkbox v-model="checked">仅显示当前学期的作业安排</el-checkbox>
      </div>
      <div class="opration">
        <el-button type="primary" icon="el-icon-plus" plain @click="showChange"
          >添加</el-button
        >
        <el-button
          type="success"
          icon="el-icon-edit"
          plain
          @click="modifyCourse"
          >修改</el-button
        >
        <el-button
          type="danger"
          icon="el-icon-delete"
          plain
          @click="deleteRecord"
          >删除</el-button
        >
      </div>
      <el-table
        ref="multipleTable"
        :data="search"
        tooltip-effect="dark"
        style="width: 100%"
        border
        :default-sort="{ prop: 'updateTime', order: 'descending' }"
        @selection-change="handleSelectionChange"
      >
        <el-table-column type="selection" width="55"> </el-table-column>
        <el-table-column type="index" label="序号"> </el-table-column>
        <el-table-column prop="term" label="学期" width="120" sortable>
          <template slot-scope="scope">{{ scope.row.term }}</template>
        </el-table-column>
        <el-table-column
          prop="courseName"
          label="课程名称"
          width="120"
          sortable
        >
        </el-table-column>
        <el-table-column
          prop="school"
          label="学校"
          show-overflow-tooltip
          :formatter="formatter"
        >
        </el-table-column>
        <el-table-column prop="college" label="学院名称" width="120">
        </el-table-column>
        <el-table-column prop="major" label="专业" width="120">
        </el-table-column>
        <el-table-column prop="className" label="班级名称" width="120">
        </el-table-column>
        <el-table-column prop="teacherName" label="教师姓名" width="120">
        </el-table-column>
        <el-table-column prop="homework" label="作业表" width="120">
        </el-table-column>
        <el-table-column prop="regenerator" label="更新者" width="120">
        </el-table-column>
        <el-table-column
          prop="updateTime"
          label="更新时间"
          width="120"
          sortable
        >
        </el-table-column>
      </el-table>
      <div class="totalCount">共 {{ totalRow }} 条</div>
    </div>
    <AddCourse v-show="dialog_visible"  :courseSelected="multipleSelection" :isModify='isModify' :dialog_visible='dialog_visible'/>
  </div>
</template>

<script>
import AddCourse from "./AddCourse.vue";
import pubsub from "pubsub-js";

export default {
  name: "CourseProvision",
  components: {
    AddCourse,
  },

  data() {
    return {
      input: "",
      isModify: false,
      checked: true,
      dialog_visible: false,
      courseProvision_visible: true,
      schoolInput: '',
      collegeInput: '',
      majorInput: '',
      classNameInput: '',
      courseInput: '',
      teacherInput: '',
      tableData: [
        {
          term: "2021年秋",
          courseName: "C语言程序设计",
          school: "广东工业大学",
          college: "计算机学院",
          major: "计算机科学与技术",
          className: "计科1班",
          teacherName: "林志颖",
          homework: "C语言题库2021年秋",
          regenerator: "苏定方",
          updateTime: "2021-07-25",
        },
        {
          term: "2021年春",
          courseName: "JAVA程序设计",
          school: "广东工业大学",
          college: "计算机学院",
          major: "信息安全",
          className: "信安4班",
          teacherName: "林志颖",
          homework: "JAVA语言题库2021年春",
          regenerator: "苏号",
          updateTime: "2021-02-25",
        },
        {
          term: "2021年春",
          courseName: "C语言程序数据结构",
          school: "广东工业大学",
          college: "计算机学院",
          major: "软件工程",
          className: "软工卓越班",
          teacherName: "林志颖",
          homework: "C语言题库2021年春",
          regenerator: "苏定方",
          updateTime: "2021-03-25",
        },
        {
          term: "2020年秋",
          courseName: "Python语言程序设计",
          school: "广东工业大学",
          college: "计算机学院",
          major: "网络工程",
          className: "网工1班",
          teacherName: "林志颖",
          homework: "Python语言题库2020年秋",
          regenerator: "苏定方",
          updateTime: "2020-07-25",
        },
      ],
      multipleSelection: [],
      // tableDatakey: 0,
    };
  },
  // computed: {
  //   isModifyChange: function() {
  //     return this.isModify;
  //   }
  // },
  methods: {
    // 修改课程
    modifyCourse() {
      if (this.multipleSelection.length === 0) {
        alert("请选择需要修改的课程");
      } else if (this.multipleSelection.length > 1) {
        alert("一次只能选择一个课程修改");
      } else {
        this.isModify = true;
        this.courseProvision_visible = false;
        this.dialog_visible = true;
      }
    },
    toggleSelection(rows) {
      if (rows) {
        rows.forEach((row) => {
          this.$refs.multipleTable.toggleRowSelection(row);
        });
      } else {
        this.$refs.multipleTable.clearSelection();
      }
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
      // console.log(this.multipleSelection);
    },
    formatter(row) {
      return row.school;
    },
    // showDialog(visible) {
    //   this.dialog_visible = visible;
    // },
    showChange() {
      this.courseProvision_visible = false;
      this.dialog_visible = true;
    },
    // 删除课程
    deleteRecord() {
      let _this = this;
      // 1、获取选中的数组元素
      // 2、删除原来数据数组与1中获得的元素相同的元素
      // this.tableDataKey = Math.random();
      let newdataTable = this.tableData.filter(
        (item) => !_this.multipleSelection.includes(item)
      );
      // console.log(
      //   newdataTable
      // );
      this.tableData = newdataTable;
      // this.tableData.filter((item) => !_this.multipleSelection.includes(item));
      // this.$set(this.tableData, this.tableData.filter((item) => _this.multipleSelection.includes(item)), null);
    },
  },

  computed: {
    totalRow() {
      return this.tableData.length;
    },
    // 查询课程  --- 模糊匹配
    search: function() {
      let _this = this;
      // 当没有输入任何查询条件时
      if(!this.schoolInput && !this.collegeInput && !this.majorInput && !this.classNameInput && !this.teacherInput && !this.collegeInput) {
          return this.tableData;
      }
      // 查询条件为学校时
      return this.tableData.filter(function(v) {
        return v.school.includes(_this.schoolInput);
      })
      
    },
  },

  mounted() {
    let _this = this;
    pubsub.subscribe("addNewCourse", function (msgName, data) {
      // console.log('这是传过来的数据：' + data.school + data.term);
      let tableDataObj = {};

      tableDataObj.school = data.school;
      tableDataObj.term = data.term;
      tableDataObj.courseName = data.courseName;
      tableDataObj.teacherName = data.teacher;

      for (let i = 0; i < data.college.length; i++) {
        tableDataObj.college = data.college[i];
        for (let j = 0; j < data.major.length; j++) {
          tableDataObj.major = data.major[j];
          for (let a = 0; a < data.classNameSelected.length; a++) {
            tableDataObj.className = data.classNameSelected[a].className;
            for (let b = 0; b < data.homeworkSelected.length; b++) {
              tableDataObj.homework = data.homeworkSelected[b].homework;
            }
          }
        }
      }

      // console.log(tableDataObj);
      _this.tableData.unshift(tableDataObj);
      _this.dialog_visible = false;
      _this.courseProvision_visible = true;
    });

    pubsub.subscribe("showCourseProvision", function (msgName, data) {
      if (data) {
        _this.dialog_visible = false;
        _this.courseProvision_visible = true;
      }
    });
  },

  // watch: {
  //   tableData: {
  //     deep: true,
  //     function(newValue) {
  //       this.tableData = newValue;
  //     },
  //   },
  // },
};
</script>

<style scoped>
.search-first-row,
.search-second-row {
  display: flex;
  margin-bottom: 10px;
}
.search-first-row > *,
.search-second-row > * {
  margin-right: 10px;
  width: 310px;
}

.opration {
  margin-bottom: 10px;
}
/* 总共多少条数据的样式 */
.totalCount {
  padding-top: 20px;
}
</style>