<template>
  <div class="login-box">
    <el-form
    class="login-form"
    label-position="top" 
    label-width="80px" 
    :model="formdata">
      <span>用户登录</span>
      <el-form-item label="用户名1">
        <el-input  v-model="formdata.username"></el-input>
      </el-form-item>
      <el-form-item label="密码1">
        <el-input v-model="formdata.password"></el-input>
      </el-form-item>
      <el-button class="login-btn" type="primary" @click="handleLogin">登录</el-button>
    </el-form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      formdata: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    // handleLogin(){
    //   this.$http.post('login',this.formdata).then( res => {
    //     // console.log(res);
    //     const {
    //       data,
    //       meta:{msg,status}
    //     } = res.data;
    //     if(status === 200){
    //       this.$router.push({name:'home'})
    //       this.$message.success(msg)
    //     }else{
    //       this.$message.warning(msg)
    //     }
    //   })
    // }
    async handleLogin(){
      const res = await this.$http.post('login',this.formdata)
        // console.log(res);
        const {
          data,
          meta:{msg,status}
        } = res.data;
        if(status === 200){
          localStorage.setItem('token',data.token)
          this.$router.push({name:'home'})
          this.$message.success(msg)
        }else{
          this.$message.warning(msg)
        }
      }
    
  }
}
</script>

<style >
/* 如果是 常规 的，执行 npm install stylus-loader css-loader style-loader --save-dev 安装依赖就行。
如果是 less 的，执行 npm install less less-loader --save-dev 安装依赖就行。  npm install less-loader@5.0.0 --save-dev
如果是 sass 的，执行 npm install sass sass-loader --save-dev 安装依赖就行。 */
.login-box{
    height: 100%;
    background-color: #324152;
    display: flex;
    justify-content: center;
    align-items: center;
}
.login-box .login-form{
    width: 400px;
    border-radius: 5px;
    background-color: #fff;
    padding: 30px;
}
.login-box .login-form span{
    font-size: 20px;
    font-weight: bold;
    display: inline-block;
    margin-bottom: 10px;
}
.login-box .login-btn{
    width: 100%;
}
</style>
