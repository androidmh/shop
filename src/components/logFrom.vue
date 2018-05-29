<template>
  <div class="login-form">
    <div class="g-form">
      <div class="g-form-line">
        <span class="g-form-label">用户名：</span>
        <div class="g-form-input">
          <input type="text"
                 v-model="usernameModel" placeholder="请输入用户名">
        </div>
        <span class="g-form-error">{{ userErrors.errorText }}</span>
      </div>
      <div class="g-form-line">
        <span class="g-form-label">密码：</span>
        <div class="g-form-input">
          <input type="password"
                 v-model="passwordModel" placeholder="请输入密码">
        </div>
        <span class="g-form-error">{{ passwordErrors.errorText }}</span>
      </div>
      <div class="g-form-line">
        <div class="g-form-btn">
          <a class="button" @click="onLogin">登录</a>
        </div>
      </div>
      <p>{{ errorText }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      usernameModel: '',
      passwordModel: '',
      errorText: '',
      userFlag: false,
      psdFlag: false,
      userFlags: {
        get: function () {
          return this.userFlag
        },
        set: function () {
          this.userFlag = true
        }
      },
      psdFlags: {
        get: function () {
          return this.psdFlag
        },
        set: function () {
          this.psdFlag = true
        }
      }
    }
  },
  computed: {
    userErrors () {
      let errorText, status
      if (!(/@/g.test(this.usernameModel))) {
        status = false
        errorText = '不包含@'
      } else {
        status = true
        errorText = '已通过'
      }
      if (!this.userFlags.get()) {
        errorText = ''
        this.userFlags.set()
      }
      return {
        status,
        errorText
      }
    },
    passwordErrors () {
      let errorText, status
      if (!(/^\w{1,6}$/g.test(this.passwordModel))) {
        status = false
        errorText = '请输入1-6位密码'
      } else {
        status = true
        errorText = '已通过'
      }
      if (!this.psdFlags.get()) {
        errorText = ''
        this.psdFlags.set()
      }
      return {
        status,
        errorText
      }
    }
  },
  methods: {
    onLogin () {
      if (!this.userErrors.status || !this.passwordErrors.status) {
        this.errorText = '部分选项未通过'
      } else {
        this.errorText = ''
        this.$http.get('api/login')
          .then((data) => {
            this.$emit('has-log', data.body)
          }, (error) => {
            console.log(error)
          })
      }
    }
  }
}
</script>

<style scoped>

</style>
