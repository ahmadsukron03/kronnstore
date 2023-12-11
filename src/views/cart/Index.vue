/* eslint-disable */
<template>
  <div>
    <div id="page-wrap">
      <h1>Shopping Cart</h1>
      <!-- evnet ini akan mengambil nilai kedua pada 
      click="$emit('remove-item', item.code)" 
      pada saat remove-item ini berhasil melalui emit maka
      akan memanggil removeFromCart-->
        <ItemCart 
        v-for="item in cartItems"
        :key="item.id"
        :item="item"
        
      v-on:remove-item="removeFromCart($event)"
      />
      <h3 id="total-price">Total: Rp{{ totalprice }}</h3>
      <button id="checkout-button">Checkout </button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import ItemCart from '../../components/ItemCart'


export default {
  components: {
    ItemCart
  },
  data() {
    return {
      // kita definisikan sebagai data array kosong
      cartItems: []
    }
  },
  methods: {
    // membuat request delete
    async removeFromCart(product){
      await axios.delete(
        `http://localhost:8000/api/orders/delete/user/1/product/${product}`
        )
        // melakukan mapping dari array cartItems
        let IndexCart = this.cartItems.map(function(item){
          return item.code
          // memilih salah satu data dari cartItems berdasarkan pencocokan code dari item code nya
        }).indexOf(product)
        // dan kita akan hapus index berdasarkan cartnya dan hapus 1 saja
        this.cartItems.splice(IndexCart, 1)
    }
  },
  computed: {
    totalprice() {
      // dari masing2 array dari card items kita akan mendapatkan satu per satu dari data array nya
      return this.cartItems.reduce(
        // menjumlahkan data dari sum sebelumnya dengan number yang merujuk pada item.price
        (sum, item) => sum + Number(item.price),
        0
      )
    }
  },


  async created(){
    // api yang kita miliki : localhost:8000/api/orders/user/1
    const result = await axios.get('http://localhost:8000/api/orders/user/1')
    let data = Object.assign({},
    ...(result.data.map(
      result => ({
        // kita dapatkan satu data index dan kita masukkan ke cart_items
        cart_items : result.products
      })
    ))
    )
    // kita akan masukkan dari cart items kita isikan data.cart_items
    this.cartItems = data.cart_items
  }
}
</script>

<style scoped>
h1 {
  border-bottom: 1px solid #41B883;
  margin: 0;
  margin-top: 16px;
  padding: 16px;
}

#total-price {
  padding: 16px;
  text-align: right;
}

#checkout-button {
  width: 100%;
}

</style>