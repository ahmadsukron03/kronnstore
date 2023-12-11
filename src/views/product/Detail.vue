<template>
  <div>
    <div id="page-wrap" v-if="product">
      <h4 v-if="notif" class="notif">Item added succesfuly</h4>
      <div id="img-wrap">
        <img :src="`http://localhost:8000${product.imageUrl}`" alt="">
      </div>
      <div id="product-details">
        <h1>{{ product.name }}</h1>
        <h3 id="price">Rp{{ product.price }}</h3>
        <p>Average rating: {{ product.averageRating }}</p>
        <!-- dimana ketika kita click kita akan memanggil method add to cart -->
        <button id="add-to-cart" @click="addToCart(product.code)">Add to cart</button>
        <p>{{ product.description }}</p>
      </div>
    </div>

    <NotFound v-else />
  </div>
</template>

<script>
import axios from 'axios'
import NotFound from '../errors/404'

export default {
  components: {
    NotFound,
  },
  data() {
    return {
      // buat data tsb sebagai object kosong
      product: {},
      notif:false
    }
  },
  methods: {
    // kita tambahkan argumen pada method
    async addToCart(product) {
      await axios.post('http://localhost:8000/api/orders/update/user/1', {
        // data yang akan kita kirimkan kedalam server :,body request yang ada didalam postman
        product: product
      })
      this.notif = true
        
    },
  },
  // mounted kita ubah menjadi created
  async created() {
    // kita dapatkan dari $route
    // kita mendefinisikan id pada productItem
    const code = this.$route.params.id
    // kita tunggu prosesnya sampai selesai dengan await
    const result = await axios.get(
      // kita akan mendapatkan data berdasarkan dari code id
      `http://localhost:8000/api/products/${code}`
    )
    // kita akan menggunakan this dan diarahkan ke products,dan object yang kosong kita akan coba isikan dengan result.data
    this.product = result.data
  }
}
</script>

<style scoped>
#page-wrap {
  margin-top: 16px;
  padding: 16px;
  max-width: 600px;
}

#img-wrap {
  text-align: center;
}

img {
  width: 400px;
}

#product-details {
  padding: 16px;
  position: relative;
}

#add-to-cart {
  width: 100%;
}

#price {
  position: absolute;
  top: 24px;
  right: 16px;
}

.notif{
  text-align: center;
  color: white;
  background-color: #41B883;
  padding: 3%;
  border-radius: 8px;
}
</style>