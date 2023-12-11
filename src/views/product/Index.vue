<template>
  <div>
    <div id="page-wrap">
      <div class="grid-wrap">
      <ProductItem 
      v-for="product in products"
      :key="product.id"

      :product="product"
      />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import ProductItem from '../../components/ProductItem.vue'

export default {
  components : {
    ProductItem
  },
data(){{
  return{
    products:[]
  }
}
},
async created(){
  // kita tunggu prosesnya sampai selesai dengan await
  const result = await axios.get('http://localhost:8000/api/products')
  // kita akan menggunakan this dan diarahkan ke products,dan array yang kosong kita akan coba isikan dengan result.data
  this.products = result.data
}
}

</script>
<!-- style ini hanya bisa digunakan di index -->
<style scoped>
.grid-wrap {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    margin-top: 16px;
  }

</style>