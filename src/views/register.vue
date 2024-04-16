<template>
    <div class="background">
        <div class="container">
            <div class="login-form">
                <h1 >毕业设计系统注册</h1>
                <el-form class="rgs-form" label-width="120px">
                        <el-form-item label="账号" class="input">
                            <el-input v-model="ruleForm.username" autocomplete="off" placeholder="账号"  />
                        </el-form-item>
                        <el-form-item label="密码" class="input">
                            <el-input type="password" v-model="ruleForm.password" autocomplete="off" show-password/>
                        </el-form-item>
                        <el-form-item label="重复密码" class="input">
                            <el-input type="password" v-model="ruleForm.repetitionPassword" autocomplete="off" show-password/>
                        </el-form-item>
                        <el-form-item label="教师姓名" class="input" v-if="tableName=='jiaoshi'">
                            <el-input v-model="ruleForm.jiaoshiName" autocomplete="off" placeholder="教师姓名"  />
                        </el-form-item>
                        <el-form-item label="手机号" class="input" v-if="tableName=='jiaoshi'">
                            <el-input v-model="ruleForm.jiaoshiPhone" autocomplete="off" placeholder="手机号"  />
                        </el-form-item>
                        <el-form-item label="身份证号" class="input" v-if="tableName=='jiaoshi'">
                            <el-input v-model="ruleForm.jiaoshiIdNumber" autocomplete="off" placeholder="身份证号"  />
                        </el-form-item>
                        <el-form-item label="电子邮箱" class="input" v-if="tableName=='jiaoshi'">
                            <el-input v-model="ruleForm.jiaoshiEmail" autocomplete="off" placeholder="电子邮箱"  />
                        </el-form-item>
                        <el-form-item label="学生姓名" class="input" v-if="tableName=='yonghu'">
                            <el-input v-model="ruleForm.yonghuName" autocomplete="off" placeholder="学生姓名"  />
                        </el-form-item>
                        <el-form-item label="手机号" class="input" v-if="tableName=='yonghu'">
                            <el-input v-model="ruleForm.yonghuPhone" autocomplete="off" placeholder="手机号"  />
                        </el-form-item>
                        <el-form-item label="身份证号" class="input" v-if="tableName=='yonghu'">
                            <el-input v-model="ruleForm.yonghuIdNumber" autocomplete="off" placeholder="身份证号"  />
                        </el-form-item>
                        <el-form-item label="电子邮箱" class="input" v-if="tableName=='yonghu'">
                            <el-input v-model="ruleForm.yonghuEmail" autocomplete="off" placeholder="电子邮箱"  />
                        </el-form-item>
                        <div style="display: flex;flex-wrap: wrap;width: 100%;justify-content: center;">
                            <el-button class="btn" type="primary" @click="login()">注册</el-button>
                            <el-button class="btn close" type="primary" @click="close()">取消</el-button>
                        </div>
                </el-form>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                ruleForm: {
                },
                tableName:"",
                rules: {},
                sexTypesOptions : [],
            };
        },
        mounted(){
            let table = this.$storage.get("loginTable");
            this.tableName = table;

            //级联表的下拉框查询

        },
        methods: {
            // 获取uuid
            getUUID () {
                return new Date().getTime();
            },
            close(){
                this.$router.push({ path: "/login" });
            },
            // 注册
            login() {

                            if((!this.ruleForm.username)){
                                this.$message.error('账号不能为空');
                                return
                            }
                            if((!this.ruleForm.password)){
                                this.$message.error('密码不能为空');
                                return
                            }
                            if((!this.ruleForm.repetitionPassword)){
                                this.$message.error('重复密码不能为空');
                                return
                            }
                            if(this.ruleForm.repetitionPassword != this.ruleForm.password){
                                this.$message.error('密码和重复密码不一致');
                                return
                            }
                            if((!this.ruleForm.jiaoshiName)&& 'jiaoshi'==this.tableName){
                                this.$message.error('教师姓名不能为空');
                                return
                            }
                             if('jiaoshi' == this.tableName && this.ruleForm.jiaoshiPhone&&(!this.$validate.isMobile(this.ruleForm.jiaoshiPhone))){
                                 this.$message.error('手机应输入手机格式');
                                 return
                             }
                            if((!this.ruleForm.jiaoshiIdNumber)&& 'jiaoshi'==this.tableName){
                                this.$message.error('身份证号不能为空');
                                return
                            }
                            if('jiaoshi' == this.tableName && this.ruleForm.jiaoshiEmail&&(!this.$validate.isEmail(this.ruleForm.jiaoshiEmail))){
                                this.$message.error("邮箱应输入邮件格式");
                                return
                            }
                            if((!this.ruleForm.yonghuName)&& 'yonghu'==this.tableName){
                                this.$message.error('学生姓名不能为空');
                                return
                            }
                             if('yonghu' == this.tableName && this.ruleForm.yonghuPhone&&(!this.$validate.isMobile(this.ruleForm.yonghuPhone))){
                                 this.$message.error('手机应输入手机格式');
                                 return
                             }
                            if((!this.ruleForm.yonghuIdNumber)&& 'yonghu'==this.tableName){
                                this.$message.error('身份证号不能为空');
                                return
                            }
                            if('yonghu' == this.tableName && this.ruleForm.yonghuEmail&&(!this.$validate.isEmail(this.ruleForm.yonghuEmail))){
                                this.$message.error("邮箱应输入邮件格式");
                                return
                            }
                this.$http({
                    url: `${this.tableName}/register`,
                    method: "post",
                    data:this.ruleForm
                }).then(({ data }) => {
                    if (data && data.code === 0) {
                    this.$message({
                        message: "注册成功,请登录后在个人中心页面补充个人数据",
                        type: "success",
                        duration: 1500,
                        onClose: () => {
                        this.$router.replace({ path: "/login" });
                }
                });
                } else {
                    this.$message.error(data.msg);
                }
            });
            }
        }
    };
</script>
<style lang="scss" scoped>
   .background {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100vh;
        background: url('/biyeshejixitong/img/back-img-bg.jpg') no-repeat center center;
        background-size: cover;
    }

    .container {
        padding: 2rem;
        background-color: rgba(255, 255, 255, 0.85);
        border-radius: 8px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        width: auto;
    }

    .login-form {
        width: 100%;
        max-width: 500px;
    }

    h1 {
        text-align: center;
        color: #333;
        margin-bottom: 20px;
    }

    .rgs-form {
        .el-form-item {
            margin-bottom: 1rem;
        }

        .el-input__inner {
            border-radius: 5px;
        }

        .el-button {
            margin-top: 10px;
            width: 100%;
            border-radius: 5px;
        }
    }

    .btn {
        background-color: #409EFF;
        color: white;
        &:hover {
            background-color: darken(#409EFF, 10%);
        }
    }

    .close {
        background-color: white;
        color: #409EFF;
        &:hover {
            background-color: #f2f2f2;
        }
    }
</style>
