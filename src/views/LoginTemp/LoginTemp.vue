<template>
  <t-layout theme-mode>
    <header class="login-header">
      <LogoAppleFilledIcon class="logo" />
      <div class="operations-container">
        <t-button
          shape="square"
          theme="default"
          variant="text"
          @click="navToGitHub"
        >
          <t-icon class="icon" name="logo-github" />
        </t-button>
        <t-button
          shape="square"
          theme="default"
          variant="text"
          @click="navToHelper"
        >
          <t-icon class="icon" name="tips" />
        </t-button>
        <t-button
          shape="square"
          theme="default"
          variant="text"
          @click="toggleSettingPanel"
        >
          <t-icon class="icon" name="usb" />
        </t-button>
      </div>
    </header>
  </t-layout>
  <t-layout>
    <t-aside id="t-layout__sider" theme-mode width="600px">
      <h1 style="font-size: 45px; margin-left: 68px; margin-top: 30%">
        登录到
      </h1>
      <h1 style="font-size: 45px; margin-left: 68px; margin-top: 0">
        Apple Store
      </h1>
      <t-form
        ref="form"
        :data="formData"
        :label-width="0"
        :rules="rules"
        :style="{ width: '65%', marginLeft: '68px', marginTop: '75px' }"
        @submit="onSubmit"
      >
        <t-form-item name="account">
          <t-input
            v-model="formData.account"
            clearable
            placeholder="请输入账户名"
            size="large"
          >
            <template #prefix-icon>
              <desktop-icon />
            </template>
          </t-input>
        </t-form-item>

        <t-form-item name="password">
          <t-input
            v-model="formData.password"
            clearable
            placeholder="请输入密码"
            size="large"
            type="password"
          >
            <template #prefix-icon>
              <lock-on-icon />
            </template>
            <template #suffix-icon>
              <t-icon
                :name="showPsw ? 'browse' : 'browse-off'"
                @click="showPsw = !showPsw"
              />
            </template>
          </t-input>
        </t-form-item>
        <t-form-item>
          <t-button
            block
            size="large"
            style="margin-top: 30px"
            theme="primary"
            type="submit"
            >登录
          </t-button>
        </t-form-item>
      </t-form>
    </t-aside>
    <t-content class="login-content">
      <h1
        style="
          margin-left: 40%;
          font-size: 60px;
          margin-top: 20%;
          font-family: 'Times New Roman';
        "
      >
        Say Hi
      </h1>
      <h1 style="margin-left: 32%; font-size: 60px">Hello World</h1>
    </t-content>
  </t-layout>
  <t-layout>
    <t-footer class="copyright" height="30px">
      Copyright @ 2021-2022 Tencent. All Rights Reserved
    </t-footer>
  </t-layout>
</template>

<script lang="jsx" setup>
import {
  DesktopIcon,
  LockOnIcon,
  LogoAppleFilledIcon,
} from "tdesign-icons-vue-next";
import { NotifyPlugin } from "tdesign-vue-next";
import { reactive, ref } from "vue";
import router from "../../router";

const headUrl = reactive({
  githubUrl: "https://github.com/AllianceTing",
  JuejinUrl: "https://juejin.cn/user/1596931233822798",
});
const value = ref(0);
const form = ref();
const showPsw = ref(false);
const INITIAL_DATA = {
  account: "username",
  password: "password",
};
const formData = ref({ ...INITIAL_DATA });
const rules = {
  account: [
    { required: true, message: "姓名必填", type: "error", trigger: "blur" },
    { required: true, message: "姓名必填", type: "error", trigger: "change" },
    { whitespace: true, message: "姓名不能为空" },
    { min: 3, message: "输入字数太少", type: "error", trigger: "blur" },
    {
      max: 18,
      message: "输入字数应在3到18之间",
      type: "error",
      trigger: "blur",
    },
  ],
  password: [
    { required: true, message: "密码必填", type: "error" },
    { min: 6, message: "输入字数太少", type: "error", trigger: "blur" },
    {
      max: 18,
      message: "输入字数应在6到18之间",
      type: "error",
      trigger: "blur",
    },
  ],
};

const SUCCESS = () => {
  NotifyPlugin.success({
    title: "Login Success",
    content: "Welcome TO TDesign-VNEXT WEBSITE",
    placement: "top-right",
  }).then(() => {
    router.push("/");
  });
};
const FAIL = () => {
  OnReset();
  NotifyPlugin.error({
    title: "Login error",
    content: "Welcome TO TDesign-VNEXT WEBSITE",
    placement: "top-right",
  }).then(() => {
    router.push({ name: "LoginTemp" });
  });
};
const onSubmit = async ({ validateResult }) => {
  if (validateResult === true) {
    try {
      (await login()) === true ? SUCCESS() : FAIL();
    } catch (e) {
      document.write(e.message);
    }
  }
};
const navToGitHub = () => {
  window.open(headUrl.githubUrl);
};
const navToHelper = () => {
  window.open(headUrl.JuejinUrl);
};
const login = () => {
  if (
    formData.value.account === "username" &&
    formData.value.password === "password"
  ) {
    return true;
  }
  return false;
};
const OnReset = () => {
  form.value.reset({ type: "empty" });
};
const toggleSettingPanel = () => {
  if (value.value === 0) {
    value.value = 1;
    document.getElementById("t-layout__sider").style.backgroundColor =
      "#181818";
    return document.documentElement.setAttribute("theme-mode", "dark");
  }
  if (value.value === 1) {
    value.value = 0;
    document.getElementById("t-layout__sider").style.backgroundColor =
      "#eeeeee";
    return document.documentElement.removeAttribute("theme-mode");
  }
};
</script>

<style lang="less" scoped>
.login-content {
}

.t-layout__sider {
  position: relative;
  transition: all 0.2s;
  background-color: #eeeeee;
}

.login-header {
  padding: 0 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;

  .login-aside {
    width: 400px;
  }

  .logo {
    width: 68px;
    height: 20px;
  }

  .operations-container {
    display: flex;
    align-items: center;

    .icon {
      height: 20px;
      width: 20px;
      padding: 6px;
      box-sizing: content-box;

      &:hover {
        cursor: pointer;
      }
    }
  }
}

.copyright {
  font-size: 14px;
  margin-left: 48px;
  margin-top: 20px;
}

.t-layout__sider {
  position: relative;
  transition: all;
  width: 232px;
}
</style>
