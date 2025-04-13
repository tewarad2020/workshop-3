<template>
  <v-container>
    <v-card class="pa-4" elevation="2">
      <v-card-title>
        🛒 ตะกร้าสินค้า
        <v-spacer></v-spacer>
        จำนวน {{ cart.length }} รายการ
      </v-card-title>
      <v-divider class="mb-4"></v-divider>

      <v-row v-if="cartItems.length > 0">
        <v-col
          v-for="item in cartItems"
          :key="item._id"
          cols="12"
        >
          <v-card class="mb-3">
            <v-row no-gutters>
              <v-col cols="3">
                <v-img :src="getProductDetailById(item.productId).image ?? ''" height="100px" cover></v-img>
              </v-col>

              <v-col cols="6">
                <v-card-title>{{ getProductDetailById(item.productId).name }}</v-card-title>
                <v-card-subtitle>
                  ราคา {{ getProductDetailById(item.productId).price }} บาท × {{ item.quantity }}
                  <br/>รวม {{ getProductDetailById(item.productId).price * item.quantity }} บาท
                </v-card-subtitle>
              </v-col>

              <v-col cols="3" class="d-flex flex-column align-center justify-center">
                <v-btn color="#ff5252" class="white--text" @click="deleteOrder(item._id)">
                  delete
                </v-btn>
              </v-col>
            </v-row>
          </v-card>
        </v-col>

        <v-col cols="12" class="text-right">
          <h3>รวมทั้งหมด: {{ totalPrice }} บาท</h3>
          <br/>
          <v-btn color="green white--text" @click="checkout">ชำระเงิน</v-btn>
        </v-col>
      </v-row>

      <v-row v-else>
        <v-col cols="12" class="text-center">
          <h3>ยังไม่มีสินค้าในตะกร้า</h3>
        </v-col>
      </v-row>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: "CartPage",
  data() {
    return {
      cart: [],
      products: []
    };
  },
  computed: {
    cartItems() {
      return Object.values(this.cart);
    },
    totalPrice() {
      return this.cartItems.reduce((sum, item) => {
        return sum + this.getProductDetailById(item.productId).price * item.quantity;
      }, 0);
    }
  },
  created() {
    const token = this.$cookies.get('token')
    if (!token) {
      this.$router.push('/login');
    } else {
      this.fetchProducts();
      this.fetchOrders();
    }
  },
  methods: {
    getProductDetailById(id) {
      return this.products.find(product => product._id === id);
    },
    fetchOrders() {
      this.axios.get(`${process.env.VUE_APP_API_URL}/orders`, {
        headers: {
          Authorization: `Bearer ${this.$cookies.get('token')}`
        }
      })
        .then(res => {
          const orders = res.data.data;
          const isPaidOrders = orders.filter(order => !order.isPaid);
          this.cart = isPaidOrders
        })
        .catch(error => {
          console.error('Error fetching orders:', error);
          this.$router.push('/login');
        });
    },
    deleteOrder(id) {
      this.axios.delete(`${process.env.VUE_APP_API_URL}/orders/${id}`, {
        headers: {
          Authorization: `Bearer ${this.$cookies.get('token')}`
        }
      })
        .then(res => {
          alert("ลบรายการสั่งซื้อเรียบร้อยแล้ว");
          this.$router.go();
        })
        .catch(error => {
          console.error('Error deleting order:', error);
          alert("ไม่สามารถลบรายการสั่งซื้อได้");
        });
    },
    checkout() {
      this.axios.put(`${process.env.VUE_APP_API_URL}/orders`, {
        orderIds: this.cartItems.map(item => item._id),
      }, {
        headers: {
          Authorization: `Bearer ${this.$cookies.get('token')}`
        }
      })
      .then(res => {
        alert("ขอบคุณสำหรับการสั่งซื้อ!");
        this.$router.go();
      })
      .catch(error => {
        console.error('Error pay the payment:', error);
        alert('ไม่สามารถชำระเงินได้');
      });
    },
    fetchProducts() {
      this.axios.get(`${process.env.VUE_APP_API_URL}/products`, {
        headers: {
          Authorization: `Bearer ${this.$cookies.get('token')}`
        }
      })
        .then(res => {
          this.products = res.data.data;
        })
        .catch(error => {
          console.error('Error fetching products:', error);
          this.$router.push('/login');
        });
    },
  }
};
</script>

<style scoped>
.v-card-title {
  font-weight: bold;
}
</style>
