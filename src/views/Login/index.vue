<template>
  <div class="login-container">
    <!-- 导航栏 -->
    <van-nav-bar class="page-nav-bar" title="登录" />
    <!-- /导航栏 -->

    <!-- 登录表单 -->
    <van-form ref="loginForm" @submit="onSubmit">
      <van-field
        v-model="user.mobile"
        name="mobie"
        :rules="userFormRules.mobile"
        type="number"
        maxlength="11"
        placeholder="请输入手机号"
      >
        <i slot="left-icon" class="toutiao toutiao-shouji"></i>
      </van-field>
      <van-field
        v-model="user.code"
        name="code"
        :rules="userFormRules.code"
        type="number"
        maxlength="6"
        placeholder="请输入验证码"
      >
        <i slot="left-icon" class="toutiao toutiao-yanzhengma"></i>
        <template #button>
          <van-button
            class="send-sms-btn"
            round
            size="small"
            type="default"
            native-type="button"
            >发送验证码</van-button
          >
        </template>
      </van-field>
      <div class="login-btn-wrap">
        <van-button class="login-btn" block type="info" native-type="submit">
          登录
        </van-button>
      </div>
    </van-form>
    <!-- /登录表单 -->
  </div>
</template>

<script>
import { login } from "@/api/user";
export default {
  name: "LoginPage",
  methods: {
    async onSubmit() {
      //获取表单数据
      const user = this.user;

      this.$toast.loading({
        message: "登录中。。。",
        forbidClick: true,
        duration: 0,
      });

      try {
        const res = await login(user);
        console.log("登录成功", res);
        this.$toast.success("登录成功");
      } catch (err) {
        if (err.response.status === 400) {
          this.$toast.fail("手机号或验证码错误");
        } else {
          this.$toast.fail("登录失败");
        }
      }
    },

   async onSendSms() {
      console.log("onsendsms");
      try{
        await this.$refs.loginForm.validate('mobile')
        console.log('验证通过');
      }catch(err){
       return console.log('验证失败',err);
      }
    },
  },
  data() {
    return {
      user: {
        mobile: "",
        code: "",
      },

      userFormRules: {
        mobile: [
          {
            required: true,
            message: "手机号不能为空",
          },
          {
            pattern: /^1[3|5|7|8]\d{9}$/,
            message: "手机号格式错误",
          },
        ],
        code: [
          {
            required: true,
            message: "验证码不能为空",
          },
          {
            pattern: /^\d{6}$/,
            message: "验证码格式错误",
          },
        ],
      },
    };
  },
};
</script>
<style scoped lang="less"></style>
