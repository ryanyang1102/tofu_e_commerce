<template>
  <div class="login flex-center bg-light">
    <form class="form-signin" @submit.prevent="signin">
      <h1 class="h3 mb-3 font-weight-normal">請先登入</h1>
      <label for="inputEmail" class="sr-only">Email address</label>
      <input type="email" id="inputEmail" class="form-control" placeholder="Email address" v-model="user.username" required autofocus>
      <label for="inputPassword" class="sr-only">Password</label>
      <input type="password" id="inputPassword" class="form-control" placeholder="Password" v-model="user.password" required>
      <div class="checkbox mb-3">
        <label>
          <input type="checkbox" value="remember-me"> Remember me
        </label>
      </div>
      <button class="btn btn-lg btn-primary btn-block" type="submit">Sign in</button>
      <p class="mt-5 mb-3 text-muted">&copy; 2021-2022</p>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      user: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    signin () {
      const api = process.env.VUE_APP_APIPATH
      const url = `${api}/admin/signin`
      const vm = this
      vm.$http.post(url, vm.user).then((response) => {
        console.log(response.data)
        if (response.data.success) {
          // 將伺服器傳來的 token 及 expired 給讀出來
          const token = response.data.token
          const expired = response.data.expired
          // console.log(token, expired)
          // 寫入 cookie
          document.cookie = `hexToken=${token};expires=${new Date(expired)};`
          vm.$router.push('/admin/products_manage')
        };
      })
    }
  }
}
</script>
