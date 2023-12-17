<template>
  <div class="login-container">
    <div class="login-content">
      <el-form
        label-position="left"
        label-width="100px"
        :model="formLabelAlign"
        style="max-width: 460px"
      >
        <el-form-item label="用户名">
          <el-input v-model="formLabelAlign.userName" />
        </el-form-item>
        <el-form-item label="密码">
          <el-input v-model="formLabelAlign.password" />
        </el-form-item>
        <el-form-item label="验证码">
          <el-input v-model="formLabelAlign.code" @keyup.enter="submit" />
          <img class="code-img" @click="resetCode" :src="codeUrl" alt="" />
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script lang="ts" setup>
const codeUrl = ref("/proxyapi/user/captcha");

const formLabelAlign = reactive({
  userName: "",
  password: "",
  code: ""
});

function resetCode() {
  codeUrl.value = "/proxyapi/user/captcha" + "?t=" + new Date().getTime();
}
function submit() {
  request
    .post("/user/create", {
      userName: formLabelAlign.userName,
      password: formLabelAlign.password,
      code: formLabelAlign.code
    })
    .then((res) => {
      console.log(res);
    });
}
</script>
<script lang="ts">
export default {
  name: "LoginPage"
};
</script>
<style scoped lang="scss">
.login-container {
  position: relative;
  width: 100%;
  height: 100%;
  .login-content {
    width: fit-content;
    height: fit-content;
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    right: 0;
    margin: auto;
    .el-input {
      flex: 1;
    }
    .code-img {
      width: 100px;
      height: 30px;
      /* 设置图片的垂直居中 */
      margin: 0 auto;
    }
  }
}
</style>
