<template>
  <div class="login-wrapper">
    <div class="login-header flex-row">
      <img src="../../assets/header-icon.png" alt="" />
      <h4>汕头市濠江区政企直通车平台</h4>
    </div>

    <div class="login-content flex-row">
      <img src="../../assets/bg.png" alt="" />

      <div class="form-wrapper">
        <div class="form-header flex-row">
          <img src="../../assets/user-icon.png" alt="" />

          <div class="form-text">
            <p>您好！</p>
            <p style="margin-bottom: 0px">登录账号，提交企业登记诉求</p>
          </div>
        </div>

        <el-form
          ref="loginForm"
          size="medium"
          :model="loginForm"
          :rules="loginRules"
          class="login-form"
          auto-complete="on"
          label-position="left"
        >
          <el-form-item prop="username">
            <el-input
              size="large"
              ref="username"
              v-model="loginForm.username"
              placeholder="用户名"
              name="username"
              type="text"
              tabindex="1"
              auto-complete="on"
            />
          </el-form-item>

          <el-form-item prop="password">
            <el-input
              size="large"
              :key="passwordType"
              ref="password"
              v-model="loginForm.password"
              :type="passwordType"
              placeholder="密码"
              name="password"
              tabindex="2"
              auto-complete="on"
              @keyup.enter.native="handleLogin"
            />
          </el-form-item>

          <el-form-item prop="captcha">
            <div class="flex-row">
              <el-input
                size="large"
                v-model="loginForm.captcha"
                placeholder="验证码"
                style="width: 240px"
                @keyup.enter.native="handleLogin"
              >
              </el-input>
              <span class="show-captcha">
                  <img :src="captchaPath" @click="getCaptcha()" alt="" style="height: 40px;" />
                </span>
            </div>
          </el-form-item>
          <div style="text-align: center;margin-bottom: 30px">
            <el-button
              size="medium"
              :loading="loading"
              type="primary"
              @click.native.prevent="handleLogin"
              >登录
            </el-button>
          </div>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
import { getUUID } from "@/utils";
import { getAction } from "@/api/manage";

export default {
  name: "Login",
  data() {
    const validateUserId = (rule, value, callback) => {
      if (value.length == 0) {
        callback(new Error("请输入用户名"));
      } else {
        callback();
      }
    };
    const validatePassword = (rule, value, callback) => {
      if (value.length == 0) {
        callback(new Error("请输入密码"));
      } else {
        callback();
      }
    };
    const validateCaptcha = (rule, value, callback) => {
      if (value.length == 0) {
        callback(new Error("请输入验证码"));
      } else {
        callback();
      }
    };
    return {
      captchaPath: "",
      loginForm: {
        username: "admin",
        password: "1",
        uuid: "",
        captcha: "",
      },
      loginRules: {
        username: [
          { required: true, trigger: "blur", validator: validateUserId },
        ],
        password: [
          { required: true, trigger: "blur", validator: validatePassword },
        ],
        captcha: [
          { required: true, trigger: "blur", validator: validateCaptcha },
        ],
      },
      loading: false,
      passwordType: "password",
      redirect: undefined,
    };
  },
  watch: {
    $route: {
      handler: function (route) {
        this.redirect = route.query && route.query.redirect;
      },
      immediate: true,
    },
  },
  created() {
    this.getCaptcha();
  },
  methods: {
    showPwd() {
      if (this.passwordType === "password") {
        this.passwordType = "";
      } else {
        this.passwordType = "password";
      }
      this.$nextTick(() => {
        this.$refs.password.focus();
      });
    },
    handleLogin() {
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          this.loading = true;
          this.$store
            .dispatch("user/login", this.loginForm)
            .then(() => {
              this.$router.push({ path: this.redirect || "/" });
              // this.$router.push({path: '/'})
              this.loading = false;
            })
            .catch(() => {
              this.loading = false;
            });
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    // 获取验证码
    getCaptcha() {
      this.loginForm.uuid = getUUID();
      this.captchaPath = `${process.env.VUE_APP_BASE_API}/sys/captcha.jpg?uuid=${this.loginForm.uuid}`;
    },
  },
};
</script>
<style lang="scss">
    /* 修复input 背景不协调 和光标变色 */
    /* Detail see https://github.com/PanJiaChen/vue-element-admin/pull/927 */

    $bg: #283443;
    $light_gray: #fff;
    $cursor: #fff;

    @supports (-webkit-mask: none) and (not (cater-color: $cursor)) {
        .login-wrapper .el-input input {
            // color: $cursor;
        }
    }

    /* reset element-ui css */
    .login-wrapper {
        .el-input {
            display: inline-block;
            height: 47px;
            // width: 85%;
        }
    }
</style>
<style lang="scss" scoped>
.login-wrapper {
  .flex-row {
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: flex-start;
  }
  .login-header {
    align-items: center;
    height: 80px;
    width: 100%;
    background: #ffffff;
    box-shadow: 0px 0px 7px 0px rgba(17, 37, 52, 0.45);
    img {
      margin-left: 100px;
      margin-right: 18px;
    }
    h4 {
      font-size: 24px;
      font-family: Source Han Sans CN;
      font-weight: 800;
      color: #112534;
    }
  }
  .login-content {
    width: 100%;
    padding: 30px;
    // height: 725px;
    background: #edf7ff;
    justify-content: space-around;
    align-items: center;
    .form-wrapper {
      width: 460px;
      height: 400px;
      background: #ffffff;
      box-shadow: 0px 0px 7px 0px rgba(66, 147, 244, 0.15);
      padding: 30px;
      .form-header {
        border-bottom: 1px solid #c4c4c4;
        padding-bottom: 26px;
        margin-bottom: 30px;
        img {
          margin-right: 22px;
        }
        p {
          margin: 0;
          font-size: 14px;
          font-family: Source Han Sans CN;
          font-weight: 500;
          color: #383838;
          margin-bottom: 13px;
        }
      }
    }
  }
}
</style>