<template>
  <div>
    <!-- 
      按照 新旧约 newOrOld(新旧约，旧约：0  新约：1)、
      卷 volumeSN(shortName(简拼)、fullName(全拼))、章 chapterSN、节 verseSN
      查询内容
    -->

    <el-row id="poetry_title">
      <el-col :span="4">
        <el-select v-model="newOrOldSN" filterable placeholder="新旧约" @change="queryVolumeSNList">
          <el-option v-for="item in newOrOldSNList" :key="item.value" :label="item.label" :value="item.value">
          </el-option>
        </el-select>
      </el-col>
      <el-col :span="4">
        <el-select v-model="volumeSN" filterable placeholder="卷" @change="queryChapterSNList">
          <el-option v-for="(item, index) in volumeSNList" :key="index" :label="item.label" :value="item.value">
          </el-option>
        </el-select>
      </el-col>
      <el-col :span="4">
        <el-select v-model="chapterSN" filterable placeholder="章" @change="queryVerseSNList">
          <el-option v-for="(item, index) in chapterSNList" :key="index" :label="item.label" :value="item.value">
          </el-option>
        </el-select>
      </el-col>
      <el-col :span="4">
        <el-select v-model="verseSN" filterable placeholder="节" @change="queryBibleList">
          <el-option v-for="(item, index) in verseSNList" :key="index" :label="item.label" :value="item.value">
          </el-option>
        </el-select>
      </el-col>
    </el-row>

    <el-row style="margin: 10px 0px;">
      <el-col :span="4" style="margin-right: 10px;">
        <el-input v-model="bibleLection" placeholder="经文" size="small" @change="handleReset"
          @keyup.enter.native="queryBibleInfo"></el-input>
      </el-col>
      <el-col :span="4">
        <el-button :plain="true" @click="queryBibleInfo" size="small">搜索</el-button>
      </el-col>
      <!-- <el-col :span="4">
        <el-button size="small" @click="handleReset">重置</el-button>
      </el-col> -->
    </el-row>

    <el-row>
      <el-col v-for="(item, index) in bibleList" :key="index">
        <div style="margin: 10px 0px;">
          <span>{{ item.value }}. {{ item.label }}</span>
        </div>
      </el-col>
    </el-row>

    <el-row>
      <el-col v-for="(item, index) in bibleLectionList" :key="index">
        <div style="width: 60%; margin: 10px;">
          <span>{{ item.lection }}</span>
          <el-divider content-position="right">{{ item.newOrOldSN }} {{ item.volumeSN }} {{ item.chapterSN }} {{
            item.verseSN }}</el-divider>
        </div>
      </el-col>
    </el-row>

    <el-pagination background layout="total, sizes, prev, pager, next, jumper" :page-sizes="[20, 50, 100, 200]"
      :page-size="pageSize" :current-page="pageNum" :total="total" @size-change="handleSizeChange"
      @current-change="handleCurrentChange">
    </el-pagination>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Bible",
  props: [],
  data: function () {
    return {
      // 分页
      pageNum: 1,    //当前页
      pageSize: 20,   //每页的容量
      total: 0,   //列表总数
      loading: false, //列表的数据加载loading状态

      // 圣经
      // 新旧约，旧约：0  新约：1
      newOrOldSN: "",
      newOrOldSNList: [],
      // 卷
      shortName: "", // 简拼
      fullName: "", // 全拼
      volumeSN: "",
      volumeSNList: [],
      // 章
      chapterSN: "",
      chapterSNList: [],
      // 节
      verseSN: "",
      verseSNList: [],
      // 经文列表
      bibleList: [],
      // 经文内容
      bibleLection: "",
      // 搜索列表
      bibleLectionList: [],
    }
  },
  created() {
    this.queryNewOrOldSNList();
    this.queryVolumeSNList();
    // this.queryChapterSNList();
    // this.queryVerseSNList();
  },
  methods: {
    // 查询新旧约列表
    queryNewOrOldSNList: function () {
      var that = this;

      // that.newOrOldSNList = [];
      // that.volumeSNList = [];
      // that.chapterSNList = [];
      // that.verseSNList = [];

      this.$http.post("/bible/queryNewOrOldSNList", {}).then(
        function (resp) {
          // console.log(resp);
          that.newOrOldSNList = resp.data.data;
        },
        function (err) {
          console.log(err);
        }
      )
    },
    // 查询卷书列表
    queryVolumeSNList: function () {
      // console.log(this.newOrOldSN);
      var that = this;

      that.volumeSN = null;
      that.volumeSNList = null;
      that.chapterSN = null;
      that.chapterSNList = null;
      that.verseSN = null;
      that.verseSNList = null;
      that.bibleList = null;
      // that.bibleLectionList = null;

      this.$http.post("/bible/queryVolumeSNList",
        {
          "newOrOldSN": this.newOrOldSN
        }
      ).then(
        function (resp) {
          // console.log(resp);
          that.volumeSNList = resp.data.data;
        },
        function (err) {
          console.log(err);
        }
      );
    },
    // 查询章列表
    queryChapterSNList: function () {
      var that = this;

      // that.newOrOldSNList = [];
      // that.volumeSN = null;
      // that.volumeSNList = null;
      that.chapterSN = null;
      that.chapterSNList = null;
      that.verseSN = null;
      that.verseSNList = null;
      that.bibleList = null;
      // that.bibleLectionList = null;

      this.$http.post("/bible/queryChapterSNList",
        {
          "newOrOldSN": this.newOrOldSN,
          "volumeSN": this.volumeSN
        }
      ).then(
        function (resp) {
          // console.log(resp);
          that.chapterSNList = resp.data.data;
        },
        function (err) {
          console.log(err);
        }
      );
    },
    // 查询节列表
    queryVerseSNList: function () {
      var that = this;

      // that.newOrOldSNList = [];
      // that.volumeSN = null;
      // that.volumeSNList = null;
      // that.chapterSN = null;
      // that.chapterSNList = null;
      that.verseSN = null;
      that.verseSNList = null;
      that.bibleList = null;
      that.bibleLectionList = null;

      this.$http.post("/bible/queryVerseSNList",
        {
          "newOrOldSN": this.newOrOldSN,
          "volumeSN": this.volumeSN,
          "chapterSN": this.chapterSN
        }
      ).then(
        function (resp) {
          // console.log(resp);
          that.verseSNList = resp.data.data;
        },
        function (err) {
          console.log(err);
        }
      );

      that.queryBibleList();
    },
    // 获取经文内容
    queryBibleList: function () {
      var that = this;
      this.$http.post("/bible/queryBibleList",
        {
          "newOrOldSN": this.newOrOldSN,
          "volumeSN": this.volumeSN,
          "chapterSN": this.chapterSN,
          "verseSN": this.verseSN
        }
      ).then(
        function (resp) {
          console.log(resp);
          that.bibleList = resp.data.data;
          that.clearPageInfo();
        },
        function (err) {
          console.log(err);
        }
      );
    },
    // 查询经文内容，全文搜索，模糊查询
    queryBibleInfo: function () {
      var that = this;
      if (this.bibleLection == null || this.bibleLection == '') {
        this.$message({
          showClose: true,
          message: '经文内容不能为空！',
          type: 'error'
        });
        return;
      }
      this.bibleList = null;
      this.$http.post("/bible/queryBibleInfo",
        {
          "bibleLection": this.bibleLection,
          "pageNum": this.pageNum,
          "pageSize": this.pageSize
        }
      ).then(
        function (resp) {
          console.log(resp);
          if (resp.data.data == null || resp.data.data.length == 0) {
            that.$message({
              showClose: true,
              message: '查询失败，请重新尝试！',
              type: 'error'
            });
            return;
          }
          that.$message({
            message: '查询成功！',
            type: 'success'
          });
          that.bibleLectionList = resp.data.data.data;
          that.total = parseInt(resp.data.data.total);
          that.pageNum = parseInt(resp.data.data.pageNum);
        },
        function (err) {
          console.log(err);
        }
      );
    },

    //切换当前页的容量
    handleSizeChange(val) {
      this.pageSize = val;
      this.queryBibleInfo();
    },

    //翻页
    handleCurrentChange(val) {
      this.pageNum = val;
      this.queryBibleInfo();
    },

    // 重置
    handleReset() {
      this.pageNum = 1;
      this.pageSize = 20;
      // this.queryBibleInfo();
    },

    // 清除分页数据
    clearPageInfo() {
      this.handleReset();
      this.total = 0;
    }
  }
}
</script>

<style></style>