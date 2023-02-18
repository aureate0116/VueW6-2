<template>
    前台頁面
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link> |
    <router-link to="/products">products</router-link> |
    <router-link to="/cart">cart</router-link> |
    <router-link to="/login">登入</router-link> |
    <router-link to="/admin/products">後台產品列表</router-link>
    <hr>
    <router-view></router-view>
</template>

<script>
// import { RouterView } from 'vue-router'

// export default {
//   components: {
//     RouterView
//   }
// }
</script>
