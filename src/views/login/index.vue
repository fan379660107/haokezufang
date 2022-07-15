<template>
  <div>
    <!-- 头部导航栏 -->
    <van-nav-bar
      class="navbar"
      title="账号登录"
      left-arrow
      @click-left="goback"
    />
    <!-- form表单 -->
    <van-form @submit="login" class="form">
      <van-field
        v-model="username"
        name="username"
        placeholder="请输入账号"
        :rules="[{ required: true, message: '请输入账号' }]"
      />
      <van-field
        v-model="password"
        type="password"
        name="password"
        placeholder="请输入密码"
        :rules="[{ required: true, message: '请输入密码' }]"
      />
      <div style="margin: 16px">
        <van-button block type="info" native-type="submit" class="login-btn"
          >登录</van-button
        >
      </div>
    </van-form>
    <div class="zhuce">
      <van-button @click="zhuceFn" class="zhuce-btn"
        >还没有账号，去注册~
      </van-button>
    </div>
  </div>
</template>

<script>
import { login } from '@/api/user'
export default {
  name: 'Login',
  data() {
    return {
      username: '',
      password: ''
    }
  },
  methods: {
    goback() {
      this.$router.back()
    },
    async login() {
      try {
        const res = await login(this.username, this.password)
        // 储存token
        this.$store.commit('setUser', res.data.body)
        // 跳转页面
        this.$router.push('/profile')
        this.$toast.success('登录成功')
        // console.log(res)
      } catch (err) {
        this.$toast.fail('登入失败！请检查账号密码是否正确')
      }
    },
    zhuceFn() {}
  }
}
</script>

<style lang="less" scoped>
.navbar {
  :deep(.van-nav-bar__content) {
    background-color: #21b97a;
    .van-nav-bar__title {
      color: #fff;
      font-size: 18px;
    }
    .van-icon {
      color: #fff;
      font-size: 16px;
      margin-left: 4px;
    }
  }
}

.form {
  :deep(.van-field__control) {
    height: 60px;
    box-sizing: border-box;
    padding: 2px 0;
    font-size: 17px;
  }
}
.login-btn {
  background-color: #1cb676;
  font-size: 18px;
}
.zhuce {
  width: 100%;
  height: 30px;
  .zhuce-btn {
    font-size: 14px;
    color: #66667f;
    margin-left: 115px;
  }
}
</style>
