<script>
import axios from 'axios';

export default {
  data() {
    return {
      items: []
    }
  },
  methods: {
    async getProduct() {
      await axios.get("http://localhost:3000/product").then(response => {
        this.items = response.data
      })
    },

    formatNumber(num) {
      return "Rp. " + num.toString().replace(/(\d)(?=(\d{3})+(?:\.\d+)?$)/g, "$1.")
    },

    async beliProduct(item) {
      const produk = JSON.parse(localStorage.getItem('produk')) || [];
      const objProduk = {id: item.id, nama: item.nama, harga: item.harga, quantity: 1, file_foto: item.file_foto}
      const produkExist = produk.find(({id}) => id === item.id);

      if (produkExist) {
        // Jika localstorage produk ada maka akan menambah produk baru dan membatasi produk yang sudah ada
        Object.assign(produkExist, objProduk)
      } else {
        // Jika localstorage produk tidak ada maka akan membuat baru
        produk.push(objProduk)
      }

      localStorage.setItem('produk', JSON.stringify(produk))
    }
  },
  async mounted() {
    await this.getProduct()
  }
}
</script>

<template>
  <div class="flex flex-wrap -mx-1 lg:-mx-4">
    <div class="flex flex-wrap -mx-1 lg:-mx-4">
      <div class="my-1 px-1 w-full md:w-1/2 lg:my-4 lg:px-4 lg:w-1/3" v-for="(item, index) in items" :key="index">
        <article class="overflow-hidden rounded-lg shadow-lg bg-white ">
          <div class="relative pb-2/3">
            <img class="w-full h-48 block object-cover" alt="{{ item.nama }}" :src="item.file_foto">
          </div>
          <div class="px-4">
            <div class="p-6">
              <div v-if="item.baru === true" class="flex">
                  <span
                      class="inline-block bg-cyan-300 text-white text-xs px-2 rounded-full uppercase font-semibold tracking-wide">New</span>
              </div>
              <h4 class="font-semibold text-lg leading-tight truncate">{{ item.nama }}</h4>
              <div class="mt-1">
                {{ formatNumber(item.harga) }}
                <span class="text-gray-600 text-sm"> / Roll</span>
              </div>
              <div class="mt-2 flex items-center">
                <svg v-for="i in 5" :key="i" :class="i <= item.rating ? 'text-cyan-300' : 'text-gray-300'"
                     xmlns="http://www.w3.org/2000/svg" fill="none" stroke="currentColor" stroke-width="1.5"
                     class="w-4 h-4 fill-current" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round"
                        d="M11.48 3.499a.562.562 0 0 1 1.04 0l2.125 5.111a.563.563 0 0 0 .475.345l5.518.442c.499.04.701.663.321.988l-4.204 3.602a.563.563 0 0 0-.182.557l1.285 5.385a.562.562 0 0 1-.84.61l-4.725-2.885a.563.563 0 0 0-.586 0L6.982 20.54a.562.562 0 0 1-.84-.61l1.285-5.386a.562.562 0 0 0-.182-.557l-4.204-3.602a.563.563 0 0 1 .321-.988l5.518-.442a.563.563 0 0 0 .475-.345L11.48 3.5z"/>
                </svg>
                <span class="ml-2 text-gray-600 text-sm">{{ item.review }} Reviews</span>
              </div>
              <div class="mt-3">
                <button type="button" @click="beliProduct(item)"
                        class="text-white bg-cyan-300 hover:bg-sky-500 focus:ring-4 focus:outline-none focus:ring-blue-200 dark:focus:ring-blue-900 font-medium rounded-lg text-sm px-5 py-2.5 inline-flex justify-center w-full text-center">
                  Tambah
                </button>
              </div>
            </div>
          </div>
        </article>
      </div>
    </div>
  </div>
</template>
