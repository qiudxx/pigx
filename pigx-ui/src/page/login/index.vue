<template>
  <div class="login-container pull-height" @keyup.enter.native="handleLogin">
    <div class="login-weaper">
      <div class="login-left animated fadeInLeft">
        <div class="login-info">
          <h2 class="login-info-title">{{website.info.title}}</h2>
          <ul class="login-info-list">
            <li class="login-info-item" v-for="(item,index) in website.info.list" :key="index">
              <i class="el-icon-check"></i>
              &nbsp;{{item}}
            </li>
          </ul>
        </div>
      </div>
      <div class="login-border animated fadeInRight">
        <div class="login-main">
          <h4 class="login-title">
            <el-select
              class="login-select animated fadeIn"
              v-model="active"
              @change="handleCommand"
              placeholder="点击请选择租户"
              size="mini">
              <el-option label="租户1 用户登录" value="1"></el-option>
              <el-option label="租户2 用户登录" value="2"></el-option>
            </el-select>
          </h4>
          <userLogin v-if="activeName==='user'"></userLogin>
          <codeLogin v-else-if="activeName==='code'"></codeLogin>
          <thirdLogin v-else-if="activeName==='third'"></thirdLogin>
        </div>
        <div class="login-menu">
          <a href="#" @click.stop="activeName='user'">账号密码</a>
          <a href="#" @click.stop="activeName='code'">手机号登录</a>
          <a href="#" @click.stop="activeName='third'">第三方登录</a>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import userLogin from "./userlogin";
import codeLogin from "./codelogin";
import thirdLogin from "./thirdlogin";
import topColor from "../index/top/top-color";
import color from "@/mixins/color";
import { setStore, getStore } from "@/util/store";
import { mapGetters } from "vuex";
import { validatenull } from "@/util/validate";
export default {
  name: "login",
  mixins: [color()],
  components: {
    topColor,
    userLogin,
    codeLogin,
    thirdLogin
  },
  data() {
    return {
      active: "",
      activeName: "user"
    };
  },
  watch: {
    $route() {
      const params = this.$route.query;
      this.socialForm.state = params.state;
      this.socialForm.code = params.code;
      if (!validatenull(this.socialForm.state)) {
        const loading = this.$loading({
          lock: true,
          text: `登录中,请稍后。。。`,
          spinner: "el-icon-loading"
        });
        setTimeout(() => {
          loading.close();
        }, 2000);
        this.handleSocialLogin();
      }
    }
  },
  created() {
    this.active = getStore({ name: "tenantId" });
  },
  mounted() {},
  computed: {
    ...mapGetters(["website"])
  },
  props: [],
  methods: {
    handleCommand(command) {
      setStore({ name: "tenantId", content: command });
    }
  }
};
</script>

<style lang="scss">
  .login-container {
    position: relative;
    width: 100%;
    height: 100%;
    margin: 0 auto;
  }
  .login-weaper {
    position: absolute;
    top: 22%;
    left: 0;
    width: 100%;
  }
  .login-container::before {
    z-index: -1024;
    content: "";
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-image: url("/img/login.png");
    background-size: cover;
  }
  .login-left {
    float: left;
    width: 50%;
    position: relative;
  }
  .login-logo {
    position: absolute;
    top: 0;
    left: 0;
    padding-top: 50px;
    font-size: 24px;
    color: #333;
  }
  .login-info {
    padding-left: 140px;
  }
  .login-info-title {
    line-height: 90px;
    color: rgb(52, 58, 64);
  }
  .login-info-item {
    font-size: 14px;
    line-height: 2.5;
    color: rgb(33, 37, 41);
  }
  .login-border {
    width: 50%;
    float: left;
    box-sizing: border-box;
  }
  .login-main {
    margin: 0 auto;
    width: 50%;
    box-sizing: border-box;
  }
  .login-main > h3 {
    margin-bottom: 20px;
  }
  .login-main > p {
    color: #76838f;
  }
  .login-title {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 40px;
    font-weight: 500;
    font-size: 22px;
    text-align: center;
    letter-spacing: 4px;
  }
  .login-select {
    input {
      color: #333;
      font-size: 18px;
      font-weight: 400;
      border: none;
      text-align: center;
    }
  }
  .login-menu {
    width: 100%;
    text-align: center;
    a {
      color: rgb(153, 153, 153);
      font-size: 12px;
      margin: 0px 8px;
    }
  }
  .login-index {
    margin-top: 40px !important;
    width: 180px;
    height: 48px;
    text-align: center;
    border-radius: 50px !important;
    background: transparent !important;
    color: #333 !important;
    font-size: 16px !important;
    border: 1px solid rgb(152, 22, 244);
  }
  .login-submit {
    display: block !important;
    margin: 40px auto 10px auto !important;
    width: 200px;
    height: 48px;
    font-size: 14px !important;
    text-align: center;
    border-radius: 50px !important;
    border: 0px;
    box-shadow: rgba(152, 22, 244, 0.19) 0px 5px 10px 2px;
  }
  .login-form {
    margin: 10px 0;
    i {
      color: #999;
    }
    .el-form-item__content {
      width: 100%;
    }
    .el-form-item {
      margin-bottom: 12px;
    }
    .el-input {
      input {
        padding-bottom: 10px;
        text-indent: 5px;
        background: transparent;
        border: none;
        border-radius: 0;
        color: #333;
        border-bottom: 1px solid rgb(235, 237, 242);
      }
      .el-input__prefix {
        i {
          padding: 0 5px;
          font-size: 16px !important;
        }
      }
    }
  }
  .login-code {
    display: flex;
    align-items: center;
    justify-content: space-around;
    margin: 0 0 0 10px;
  }
  .login-code-img {
    margin-top: 2px;
    width: 100px;
    height: 38px;
    background-color: #fdfdfd;
    border: 1px solid #f0f0f0;
    color: #333;
    font-size: 14px;
    font-weight: bold;
    letter-spacing: 5px;
    line-height: 38px;
    text-indent: 5px;
    text-align: center;
  }
</style>

