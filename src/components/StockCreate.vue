<template>
    <div>
      <h1>{{ isUpdateMode ? "Update Stock" : "Create Stock" }}</h1>
      <form @submit.prevent="submitStock">
        <div>
          <label for="symbol">Stock Symbol:</label>
          <input type="text" v-model="stock.symbol" id="symbol" required />
        </div>
        <div>
          <label for="name">Stock Name:</label>
          <input type="text" v-model="stock.name" id="name" required />
        </div>
        <div>
          <label for="shares">Shares:</label>
          <input type="number" v-model="stock.shares" id="shares" required />
        </div>
        <div>
          <label for="purchasePrice">Purchase Price:</label>
          <input type="number" v-model="stock.purchase_price" id="purchasePrice" required />
        </div>
        <button type="submit">{{ isUpdateMode ? "Update" : "Create" }}</button>
      </form>
    </div>
  </template>
  
  <script>
  import { APIService } from "@/http/APIService";
  const apiService = new APIService();
  
  export default {
    data() {
      return {
        stock: {
          symbol: "",
          name: "",
          shares: 0,
          purchase_price: 0
        },
        isUpdateMode: false
      };
    },
    methods: {
      submitStock() {
        if (this.isUpdateMode) {
          apiService.updateStock(this.stock).then(() => {
            this.$router.push("/stock-list");
          });
        } else {
          apiService.createStock(this.stock).then(() => {
            this.$router.push("/stock-list");
          });
        }
      }
    },
    mounted() {
      const stockId = this.$route.params.pk;
      if (stockId) {
        apiService.getStock(stockId).then((response) => {
          this.stock = response.data;
          this.isUpdateMode = true;
        });
      }
    }
  };
  </script>
  