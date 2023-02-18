<template>
    後台頁面
    <router-link to="/admin/products">產品列表管理</router-link> |
    <router-link to="/admin/orders">訂單</router-link> |
    <router-link to="/">回前台</router-link> |
    <a href="#" @click.prevent="logout">登出</a>
    <hr>
    <router-view></router-view>
</template>

<script>
// import { RouterView } from 'vue-router'
const { VITE_API_URL } = import.meta.env

export default {
//   components: {
//     RouterView
//   }
  methods: {
    logout () {
      document.cookie = `userToken= ;expires=${new Date()}; path=/`
      this.$router.push('/')
    },
    checkAdmin () {
      // 取得token
      const token = document.cookie.replace(/(?:(?:^|.*;\s*)userToken\s*=\s*([^;]*).*$)|^.*$/, '$1')
      this.$http.defaults.headers.common.Authorization = token
      this.$http.post(`${VITE_API_URL}/api/user/check`)
        .then(res => {
          // this.getProduct()
          console.log(res)
          if (!res.data.success) {
            this.$router.push('/login')
          }
        }).catch(err => {
          alert(err.response.data.message)
          // console.log(err.response.data.message)
          this.$router.push('/login')
        })
    }
  },
  mounted () {
    this.checkAdmin()
  }
}

</script>
