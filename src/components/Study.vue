<template>
    <div>
        <el-row>
            <el-col :span="4" style="margin-right: 10px;">
                <el-input v-model="content" placeholder="内容" size="medium" @keyup.enter.native="queryStudy"></el-input>
            </el-col>
            <el-col :span="4" style="margin-right: 10px;">
                <el-button @click="queryStudy" size="medium">搜索</el-button>
            </el-col>
            <el-col :span="4">
                <el-button @click="updateTiku" size="medium">更新题库</el-button>
            </el-col>
        </el-row>

        <el-row>
            <el-col v-for="(item, index) in studyList" :key="index">
                <div style="width: 60%; margin: 10px;">
                    <div>题目：<span>{{ item.question }}</span></div>
                    <div>答案：<span>{{ item.answer }}</span></div>
                </div>
            </el-col>
            <el-alert type="error" v-show="erroInfo != ''">{{ erroInfo }}</el-alert>
        </el-row>
    </div>
</template>

<script>
import axios from 'axios'
export default{
    // eslint-disable-next-line vue/multi-word-component-names
    name: "Study",
    props: [],
    data: function () {
        return {
            content: "",
            studyList: [],
            // 错误信息
            erroInfo: ""
        }
    },
    created() {
    },
    methods: {
        // 查询题库
        queryStudy: function () {
            var that = this;
            if (that.content == null || that.content == '') {
                this.$message({
                showClose: true,
                message: '查询内容不能为空！',
                type: 'error'
                });
                return;
            }
            this.$http({
                url: '/study/queryStudy',
                method: 'post',
                data: {
                    "content": this.content
                }
            })
            .then(
                function (resp) {
                    console.log(resp);
                    if (resp.data.code != 0) {
                        that.erroInfo = resp.data.msg;
                        return;
                    }
                    that.erroInfo = "";
                    that.studyList = resp.data.data.data;
                }
            )
            .catch(
                function (error) {
                    // 处理错误情况
                    console.log(error);
                }
            )
            .then(
                function () {
                    // 总是会执行
                }
            )
        },
        // 更新题库
        updateTiku: function () {
            // var that = this;
            // axios.jsonp('http://49.235.90.76:5000', {
            //     params: {},
            //     jsonp: 'handleCallback'
            // }).then((res) => {
            //     console.log(res); 
            // })

            axios.jsonp = (url) => {
                if(!url){
                    console.error('请传入一个url参数')
                    return;
                }
                return new Promise((resolve, reject) => {
                    console.log(reject)
                    window.jsonCallBack =(result) => {
                        resolve(result)
                    }
                    var JSONP=document.createElement("script");
                    JSONP.type="text/javascript";
                    JSONP.src=`${url}&callback=jsonCallBack`;
                    document.getElementsByTagName("head")[0].appendChild(JSONP);
                    setTimeout(() => {
                        document.getElementsByTagName("head")[0].removeChild(JSONP)
                    },500)
                })
            }
            axios.jsonp('http://49.235.90.76:5000/set_session_jsonp?callback=demo').then(function (response) {  
                console.log(response);
                }).catch(function (error) {
                console.log(error);
            });

        }
        

    }
}


</script>

<style>
</style>
