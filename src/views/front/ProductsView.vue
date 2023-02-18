<template>
    產品列表
    <table>
        <thead>
            <tr></tr>
        </thead>
        <tbody>
            <tr v-for="product in products" :key="product.id">
                <td>{{ product.title }}</td>
                <td><img :src="product.imageUrl" width="150"></td>
                <td><router-link :to="`/product/${product.id}`">產品內容</router-link></td>
                <td><button type="button" class="btn btn-primary" @click="addToCart(product.id,1)">加入購物車</button></td>
            </tr>
        </tbody>
    </table>
</template>

<script>
import { RouterLink } from 'vue-router'

// 取得環境變數 除了這樣寫 const url = import.meta.env.VITE_PATH;
// 也可以使用解構的方式把環境變數取出來
const { VITE_API_URL, VITE_API_PATH } = import.meta.env
export default {
  data () {
    return {
      products: []
    }
  },
  methods: {
    getProducts () {
      // 用插件的方式使用 axios
      this.$http.get(`${VITE_API_URL}/api/${VITE_API_PATH}/products/all`)
        .then(res => {
          console.log(res)
          this.products = res.data.products
          console.log(this.products)
        })
    },
    addToCart (id) {
      const data = {
        product_id: id,
        qty: 1
      }
      this.$http.post(`${VITE_API_URL}/api/${VITE_API_PATH}/cart`, { data })
        .then(res => {
          console.log(res)
        })
    }
  },
  components: {
    RouterLink
  },
  mounted () {
    this.getProducts()
  }
}
</script>
