<script setup lang="ts">
import myAxios from "../plugins/myAxios.ts";
import {showFailToast,showSuccessToast} from "vant/lib/vant.es";
import {ref} from "vue";
import {useRoute, useRouter} from "vue-router";

const route = useRoute();
const router = useRouter();

const userAccount = ref('');
const userPassword = ref('');
const checkPassword = ref('');
const planetCode = ref('');

const onSubmit = async () => {
  // console.log("用户登录");
  const res = await myAxios.post("/user/register", {
    userAccount: userAccount.value,
    userPassword: userPassword.value,
    checkPassword: checkPassword.value,
    planetCode: planetCode.value,
  });
  if (res.code == 0 && res.data != null) {
    showSuccessToast("注册成功");
    //跳转到之前的页面
    // const redirectUrl = route.query?.redirect as string ?? '/';
    // window.location.href = redirectUrl;
    router.replace('/user/login');
    // console.log("登录成功");
  } else {
    showFailToast("注册失败");
    // console.log("登录失败");
  }
};

//跳转到登录页
const toLogin = () => {
  router.push({
    path: '/user/login',
  })
}
</script>

<template>
  <van-form @submit="onSubmit">
    <div class="myImg">
      <van-image
          width="100"
          height="100"
          src="https://web-study-123.oss-cn-hangzhou.aliyuncs.com/%E5%90%8C%E4%BC%B4%E7%AE%A1%E7%90%86.png"
      />
    </div>
    <div class="myText">
      <strong style="color: dodgerblue; margin-bottom: 20px">云交友</strong>
    </div>
    <van-cell-group inset>
      <van-field
          v-model="userAccount"
          name="userAccount"
          label="账号"
          placeholder="请输入账号"
          :rules="[{ required: true, message: '请填写用户名' }]"
      />
      <van-field
          v-model="userPassword"
          type="password"
          name="userPassword"
          label="密码"
          placeholder="请输入密码"
          :rules="[{ required: true, message: '请填写密码' }]"
      />
      <van-field
          v-model="checkPassword"
          type="password"
          name="checkPassword"
          label="确认密码"
          placeholder="请输入密码"
          :rules="[{ required: true, message: '请填写密码' }]"
      />
      <van-field
          v-model="planetCode"
          type="password"
          name="planetCode"
          label="注册编号"
          placeholder="请输入编号"
          :rules="[{ required: true, message: '请填写编号' }]"
      />
    </van-cell-group>
    <div style="margin: 16px;">
      <van-button round block type="primary" native-type="submit">
        提交
      </van-button>
    </div>
    <div style="margin: 16px;">
      <van-button round block type="primary" @click="toLogin">
        登录
      </van-button>
    </div>
  </van-form>
</template>

<style scoped>
.myImg{
  display: flex;
  justify-content: center;
}
.myText{
  display: flex;
  justify-content: center;
}
</style>