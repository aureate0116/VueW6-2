<template>
購物車
<table class="table align-middle">
            <thead>
              <tr>
                <th></th>
                <th>品名</th>
                <th style="width: 150px">數量/單位</th>
                <th>小計</th>
              </tr>
            </thead>
            <tbody>
              <template v-if="cart.carts">
                <tr v-for="item in cart.carts" :key="item.id">
                  <td>
                    <button type="button" class="btn btn-outline-danger btn-sm" @click="deleteItem(item)"
                    :disabled ="item.id === loadingItem">
                      <!-- <i class="fas fa-spinner fa-pulse"></i> -->
                      x
                    </button>
                  </td>
                  <td>
                    {{ item.product.title }}
                    <!-- <div class="text-success">
                      已套用優惠券
                    </div> -->
                  </td>
                  <td>
                    <div class="input-group input-group-sm">
                      <div class="input-group mb-3">
                        <!-- <input min="1" type="number" class="form-control"> -->
                        <select name="" id="" class="form-select" v-model="item.qty" :disabled ="item.id === loadingItem"
                        @change="updateCartItem(item)">
                          <option :value="i" v-for="i in 20" :key="`${i}122`">{{i}}</option>
                        </select>
                        <span class="input-group-text" id="basic-addon2">{{ item.product.unit }}</span>
                      </div>
                    </div>
                  </td>
                  <td class="text-end">$ {{item.total}}
                  </td>
                </tr>
              </template>
            </tbody>
            <tfoot>
              <tr>
                <td colspan="3" class="text-end">總計</td>
                <td class="text-end">$ {{ cart.total   }}</td>
              </tr>

            </tfoot>
          </table>
</template>

<script>

const { VITE_API_URL, VITE_API_PATH } = import.meta.env

export default {
  data () {
    return {
      cart: {}
    }
  },
  methods: {
    getCarts () {
      this.$http.get(`${VITE_API_URL}/api/${VITE_API_PATH}/cart`)
        .then(res => {
          console.log('購物車:', res)
          this.cart = res.data.data
        })
        .catch(err => {
          console.log(err)
        })
    },
    updateCartItem (item) { // 購物車內有兩個id :  購物車id  & 產品id
      const data = {
        product_id: item.product.id, // 產品id
        qty: item.qty
      }
      // console.log("更新項目: ",data, item.id);
      this.$http.put(`${VITE_API_URL}/api/${VITE_API_PATH}/cart/${item.id}`, { data }) // 要帶購物車id
        .then(res => {
          // console.log("購物車:", res.data);
          this.cart = res.data.data
          this.getCarts()
        })
    },
    deleteItem (item) {
      this.$http.delete(`${VITE_API_URL}/api/${VITE_API_PATH}/cart/${item.id}`)
        .then(res => {
          this.getCarts()
        })
    },
    clearCart () {
      this.$http.delete(`${VITE_API_URL}/api/${VITE_API_PATH}/carts`)
        .then(res => {
          this.getCarts()
        })
    }
  },
  mounted () {
    this.getCarts()
  }
}
</script>
