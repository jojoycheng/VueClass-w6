<template>
  <h2>產品列表</h2>
  <div class="container">
    <!-- row 決定內層數量 -->
    <div class="row row-cols-1 row-cols-lg-3 g-3">
      <!-- 內層不定義寬度 -->
      <div class="col" v-for="product in products" :key="product.id">
        <div class="card h-100" >
          <img class="card-img-top" alt="..." />
          <div class="card-body">
            <h5 class="card-title">{{ product.title}}</h5>
            <p class="card-text">
              {{ product.description }}
            </p>
            <router-link :to="`/product/${product.id}`" class="btn btn-primary">Go somewhere</router-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      products: []
    }
  },
  methods: {
    getProducts () {
      this.axios
        .get(
          `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/products/all`
        )
        .then(res => {
          this.products = res.data.products
        })
    }
  },
  mounted () {
    this.getProducts()
  }
}
</script>
