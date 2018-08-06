<template>
    <el-row type="flex" justify="center">
        <el-form ref="loginForm" :model="user" :rules="loginRules" status-icon label-width="80px">
            <el-form-item label="用户名"  prop="name">
                <el-input v-model="user.name"  prop="name"></el-input>
            </el-form-item>
            <el-form-item label="密码"  prop="pass">
                <button class="show-pwd" @click="showPwd">111</button>
                <el-input v-model="user.pass"  :type='pwdType'></el-input>
            </el-form-item>
            <el-form-item>
                    <el-button type="primary" icon="el-icon-upload" @click="login">登录</el-button>
            </el-form-item>
        </el-form>
    </el-row>
</template>

<script>
export default {
  data() {
    const validateUserName = (rule, value, callback) => {
      if (value.trim().length < 1) {
        alert("1");
        callback(new Error("用户名"));
      } else {
        callback();
      }
    };
    const validatePassword = (rule, value, callback) => {
      if (value.trim().length < 1) {
        callback(new Error("密码"));
      } else {
        callback();
      }
    };
    return {
      user: {
        name: "",
        password: ""
      },
      loginRules: {
        name: [
          {
            required: true,
            message: "用户名不能为空",
            trigger: "blur",
            validator: validateUserName
          }
        ],
        pass: [
          {
            required: true,
            message: "密码不能为空",
            trigger: "blur",
            validator: validatePassword
          }
        ]
      },
      pwdType: "password",
      loading: false,
    };
  },
  methods: {
    login() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true
          if (this.user.name === "admin" && this.user.pass === "123") {
            this.$notify({
              type: "success",
              message: "欢迎你," + this.user.name + "!",
              duration: 3000
            });
            this.$router.replace("/");
          } else {
            this.$message({
              type: "error",
              message: "用户名或密码错误",
              showClose: true
            });
          }
        } else {
          return false;
        }
      });
    },
    showPwd() {
      if (this.pwdType === "password") {
        this.pwdType = "";
      } else {
        this.pwdType = "password";
      }
    }
  }
};
</script>