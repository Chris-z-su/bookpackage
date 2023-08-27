<template>
    <div>
        <el-row>
            <el-col :span="4" class="serInput">
                <el-input v-model="title" placeholder="标题" size="medium"></el-input>
            </el-col>
            <el-col :span="4" class="serInput">
                <el-input v-model="author" placeholder="作者" size="medium"></el-input>
            </el-col>
            <el-col :span="4" class="serInput">
                <el-input v-model="content" placeholder="内容" size="medium"></el-input>
            </el-col>
            <el-col :span="4">
                <el-button @click="queryPoetry" size="medium">搜索</el-button>
            </el-col>
        </el-row>

        <el-row>
            <el-col :span="6" v-for="(item, index) in poetryList" :key="index">
                <div class="box">
                    <el-card class="box-card card-height" shadow="hover" style="margin: 10px; overflow-y: auto; overflow-x: hidden;">
                        <div slot="header" class="header">
                            <span>{{ item.title }}</span>
                        </div>
                        <p class="niandai_zuozhe">{{ item.author }}</p>
                        <div class="content">{{ item.content }}</div>
                    </el-card>
                </div>
            </el-col>
            <el-alert type="error" v-show="erroInfo != ''">{{ erroInfo }}</el-alert>
        </el-row>
    </div>
</template>

<script>
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name: "Poetry",
    props: [],
    data: function () {
        return {
            // 古诗词
            // 标题
            title: "",
            // 作者
            author: "",
            // 内容
            content: "",
            // 诗词数组
            poetryList: [],
            // 错误信息
            erroInfo: ""
        }
    },
    created() {
    },
    methods: {
        // 查询古诗词
        queryPoetry: function () {
            var that = this;
            // axios.post("http://127.0.0.1:8080/springboot_study/chinesepoetry/getChinesePoetry", {"title": this.title, "author": this.author, "content": this.content}).then(
            //     function(resp) {
            //         console.log(resp);
            //     },
            //     function(err) {
            //     }
            // ),
            this.$http({
                url: '/chinesepoetry/getChinesePoetry',
                method: 'post',
                data: {
                    "title": this.title,
                    "author": this.author,
                    "content": this.content
                },
                //为前后端都为true可以解决session不一致问题
                // withCredentials: true,
                // 这个参数代表发送跨域请求
                // changeOrigin: true
            })
                .then(
                    function (resp) {
                        // 处理成功清空
                        console.log(resp);
                        if (resp.data.code != 0) {
                            that.erroInfo = resp.data.msg;
                            return;
                        }
                        that.erroInfo = "";
                        that.poetryList = resp.data.data;
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

        }
    }
}
</script>

<style>
.serInput{
    margin-right: 2%; /* 设置底部间距 */
}
</style>