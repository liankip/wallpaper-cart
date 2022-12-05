<script>
export default {
  data() {
    return {
      items: [],
      quantity: 1,
      total: 0,
    }
  },
  methods: {
    increment() {
      this.quantity++
    },

    async daftarCheckout() {
      this.items = JSON.parse(localStorage.getItem('produk'))
    },

    formatNumber(num) {
      return "Rp. " + num.toString().replace(/(\d)(?=(\d{3})+(?:\.\d+)?$)/g, "$1.")
    },

    async totalCheckout() {
      let total = 0
      this.items.forEach((item) => {
        total += item.harga * item.quantity
      })
      this.total = total
    },

    async hapusCheckout(id) {
      let selectedArray = this.items.splice(id, 1)
      this.items = selectedArray
      localStorage.setItem('produk', JSON.stringify(selectedArray))
    },

    async updateProduct(item) {
      const listProduk = JSON.parse(localStorage.getItem('produk')) || [];
      for (let i = 0; i < listProduk.length; i++) {
        if (item.id === listProduk[i].id) {
          listProduk[i].quantity++;
        }
      }
      localStorage.setItem('produk', JSON.stringify(listProduk))
    }
  },
  mounted() {
    this.daftarCheckout()
    this.totalCheckout()
  }
}
</script>

<template>
  <div class="flex justify-center my-6">
    <div class="flex flex-col w-full p-8 text-gray-800 bg-white shadow-lg pin-r pin-y md:w-4/5 lg:w-4/5">
      <div class="flex-1">
        <table class="w-full text-sm lg:text-base" cellspacing="0">
          <thead>
          <tr class="h-12 uppercase">
            <th class="hidden md:table-cell"></th>
            <th class="hidden md:table-cell"></th>
            <th class="text-left">Produk</th>
            <th class="lg:text-right text-left pl-5 lg:pl-0">
              <span class="lg:hidden" title="Quantity">Qtd</span>
              <span class="hidden lg:inline">Quantity</span>
            </th>
            <th class="hidden text-right md:table-cell">Harga</th>
            <th class="text-right">Total Harga</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="(item, index) in items" :key="index">
            <td>
              <input type="checkbox">
            </td>
            <td class="hidden pb-4 md:table-cell">
              <a href="#">
                <img :src="item.file_foto" class="w-20 rounded"
                     alt="Thumbnail">
              </a>
            </td>
            <td>
              <p class="mb-2 md:ml-4">{{ item.nama }}</p>
              <button @click="hapusCheckout(item.id)" type="button" class="text-red-700 md:ml-4">
                <small>(Hapus)</small>
              </button>
            </td>
            <td class="justify-center md:justify-end md:flex mt-6">
              <div class="w-20 h-10">
                <div class="relative flex flex-row w-full h-8">
                  <input type="number" :value="item.quantity" @click="updateProduct(item)"
                         class="w-full font-semibold text-center text-gray-700 bg-gray-200 outline-none focus:outline-none hover:text-black focus:text-black"/>
                </div>
              </div>
            </td>
            <td class="hidden text-right md:table-cell">
              <span class="text-sm lg:text-base font-medium">
                {{ formatNumber(item.harga) }}
              </span>
            </td>
            <td class="text-right">
              <span class="text-sm lg:text-base font-medium">
                {{ formatNumber(item.harga * item.quantity) }}
              </span>
            </td>
          </tr>
          </tbody>
        </table>
        <hr class="pb-6 mt-6">
        <div class="my-4 mt-6 -mx-2 lg:flex">
          <div class="lg:px-2 lg:w-full">
            <div class="p-4">
              <div class="flex justify-between border-b">
                <div class="lg:px-4 lg:py-2 m-2 text-lg lg:text-xl font-bold text-center text-gray-800">
                  Subtotal
                </div>
                <div class="lg:px-4 lg:py-2 m-2 lg:text-lg font-bold text-center text-gray-900">
                  {{ formatNumber(total) }}
                </div>
              </div>
              <div class="flex justify-between border-b">
                <div class="lg:px-4 lg:py-2 m-2 text-lg lg:text-xl font-bold text-center text-gray-800">
                  Biaya Pemesanan
                </div>
                <div class="lg:px-4 lg:py-2 m-2 lg:text-lg font-bold text-center text-gray-900">
                  Rp. 14.000
                </div>
              </div>
              <div class="flex justify-between pt-4 border-b">
                <div class="lg:px-4 lg:py-2 m-2 text-lg lg:text-xl font-bold text-center text-gray-800">
                  Total
                </div>
                <div class="lg:px-4 lg:py-2 m-2 lg:text-lg font-bold text-center text-gray-900">
                  {{ formatNumber(total + 14000) }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
</style>
