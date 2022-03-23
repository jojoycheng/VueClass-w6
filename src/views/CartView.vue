<template>
  <h2>購物車</h2>
    <table class="table align-middle">
      <thead>
          <tr>
              <th>圖片</th>
              <th>商品名稱</th>
              <th>價格</th>
              <th></th>
          </tr>
      </thead>
      <tbody>
          <!-- 渲染產品 -->
          <tr v-for="item in products" :key="item.id">
              <td style="width: 200px">
                  <!-- 插入背景圖 -->
                  <div style="height: 100px; background-size: cover; background-position: center"
                      :style="{backgroundImage: `url(${item.imageUrl})`}"></div>
              </td>
              <td>
                  {{ item.title }}
              </td>
              <td>
                  <div class="h5" v-if="item.price === item.orgin_price">{{ item.origin_price }} 元</div>
                  <div v-else>
                      <del class="h6">原價 {{ item.origin_price }} 元</del>
                      <div class="h5">現在只要 {{ item.price }} 元</div>
                  </div>
              </td>
              <td>
                  <div class="btn-group btn-group-sm">
                      <!-- 查看單一產品資料 -->
                      <button type="button" class="btn btn-outline-secondary" @click="openProductModal(item.id)"
                          :disabled="loadingItem === item.id || !item.is_enabled">
                          查看更多
                      </button>
                      <!-- 將產品加入購物車 -->
                      <!-- 點擊後切換按鈕狀態，加上過場動畫，增進使用者回饋，避免連續點擊按鈕 -->
                      <button type="button" class="btn btn-danger" @click="addToCart(item.id)"
                          :disabled="loadingItem === item.id || !item.is_enabled">
                          <i class="fas fa-spinner fa-pulse"
                              v-if="loadingItem === item.id"></i>
                          加到購物車
                      </button>
                  </div>
              </td>
          </tr>
      </tbody>
  </table>
</template>

<script>
import emitter from '@/libs/emitter'
export default {
  data () {
    return {
      products: [],
      loadingItem: ''
    }
  },
  methods: {
    getProducts () {
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/products/all`
      this.axios.get(url).then(res => {
        this.products = res.data.products
      }).catch(err => {
        alert(err.data.message)
      })
    },
    addToCart (id, qty = 1) {
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`
      this.loadingItem = id
      const cart = {
        product_id: id,
        qty
      }
      this.axios.post(url, { data: cart }).then(res => {
        alert(res.data.message)
        this.loadingItem = ''
        emitter.emit('get-cart')
      }).catch(err => {
        alert(err.data.message)
      })
    }
  },
  mounted () {
    this.getProducts()
  }
}
</script>
