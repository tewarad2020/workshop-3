<template>
  <v-container>
    <v-row>
      <v-col
        v-for="product in products"
        :key="product._id"
        cols="12"
        sm="6"
        md="3"
      >
        <v-card ref="productCard">
          <v-img
            :src="product.image"
            height="200px"
          ></v-img>

          <v-card-title class="headline">{{ product.name }}</v-card-title>

          <v-card-subtitle>{{ product.price }} บาท <span class="red--text"> (คงเหลือ {{ product.quantity }} ชิ้น)</span></v-card-subtitle>

          <v-card-actions
            class="flex justify-space-between"
          >
            <div
              class="w-2/5 flex items-center justify-center"
            >
              <v-btn 
                color="primary" 
                small 
                @click="decreateItem(product._id)"
              >
                -
              </v-btn>

              <span 
                v-if="orderDetail[product._id]"
                class="mx-2"
              >
                {{ orderDetail[product._id].count }}
              </span>
              
              <v-btn 
                color="primary" 
                small 
                @click="increateItem(product._id)"
              >
                +
              </v-btn>
            </div>
            
            <v-btn 
              color="primary" 
              @click="addToCart(product)"
              :disabled="orderDetail[product._id]?.count === 0"
            >
              เพิ่มลงตะกร้า
            </v-btn>
          </v-card-actions>

        </v-card>
      </v-col>

      <v-col
        cols="12"
        sm="6"
        md="3"
        style="cursor: pointer;"
        @click="dialog = true"
        v-if="isAdmin"
      >
        <v-card
          class="d-flex justify-center align-center"
          :height="productHeight"
        >
        <div
          style="width: 40%; height: 40%;"
        >
          <v-img
            src="@/assets/add.png"
            width="100%"
            height="auto"
          ></v-img>
        </div>
        </v-card>
      </v-col>
    </v-row>

    <v-dialog v-model="dialog" max-width="600px">
      <v-card>
        <v-card-title>
          <span class="text-h5">เพิ่มสินค้าใหม่</span>
        </v-card-title>

        <v-card-text>
          <v-form ref="form" lazy-validation>
            <v-text-field
              label="ชื่อสินค้า"
              v-model="newProduct.name"
              required
            ></v-text-field>

            <v-text-field
              label="ลิงก์รูปภาพ"
              v-model="newProduct.image"
              required
            ></v-text-field>

            <v-text-field
              label="ราคา"
              v-model="newProduct.price"
              type="number"
              required
            ></v-text-field>

            <v-text-field
              label="จำนวนสินค้า"
              v-model="newProduct.quantity"
              type="number"
              required
            ></v-text-field>
          </v-form>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click="dialog = false">ยกเลิก</v-btn>
          <v-btn color="primary" @click="addNewProduct">บันทึก</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
import { jwtDecode } from "jwt-decode";

export default {
  name: "ProductList",
  data() {
    return {
      userData: {},
      products: [],
      orderDetail: {},
      productHeight: 0,
      isAdmin: false,
      dialog: false,
      newProduct: {
        name: '',
        image: '',
        price: null,
        quantity: null,
      }
    };
  },
  created() {
    const token = this.$cookies.get('token')
    if (!token) {
      this.$router.push('/login');
    } else {
      if (token) {
        const decoded = jwtDecode(token);
        this.userData = decoded;
      }

      if (this.userData.role === 'admin') {
        this.isAdmin = true;
      } else {
        this.isAdmin = false;
      }
      this.fetchProducts();
    }
  },
  mounted() {
    this.setProductHeight();
    window.addEventListener('resize', this.setProductHeight);
  },
  methods: {
    fetchProducts() {
      this.axios.get(`${process.env.VUE_APP_API_URL}/products`, {
        headers: {
          Authorization: `Bearer ${this.$cookies.get('token')}`
        }
      })
        .then(res => {
          this.products = res.data.data;
          this.products.forEach(product => {
            this.$set(this.orderDetail, product._id, {
              product: product,
              count: 0 
            });
          });
          this.setProductHeight();
        })
        .catch(error => {
          console.error('Error fetching products:', error);
          this.$router.push('/login');
        });
    },
    decreateItem(id) {
      if (this.orderDetail[id].count > 0) {
        this.orderDetail[id].count--;
      }
    },
    increateItem(id) {
      if (this.orderDetail[id].count < this.orderDetail[id].product.quantity) {
        this.orderDetail[id].count++;
      }
    },
    addToCart(product) {
      this.axios.post(`${process.env.VUE_APP_API_URL}/products/${product._id}/orders`, {
        quantity: this.orderDetail[product._id].count,
      }, {
        headers: {
          Authorization: `Bearer ${this.$cookies.get('token')}`
        }
      })
      .then(res => {
        alert('เพิ่มลงตะกร้าเรียบร้อยแล้ว');
        this.$router.go();
      })
      .catch(error => {
        console.error('Error adding to cart:', error);
        alert('ไม่สามารถเพิ่มลงตะกร้าได้');
      });
    },
    setProductHeight() {
      this.$nextTick(() => {
        const productCard = this.$refs.productCard?.[0] || this.$refs.productCard;
        if (productCard) {

          this.productHeight = productCard.$el.offsetHeight;
        }
      });
    },
    addNewProduct() {
      if (!this.newProduct.name || !this.newProduct.image || !this.newProduct.price || !this.newProduct.quantity) {
        alert('กรุณากรอกข้อมูลให้ครบถ้วน');
        return;
      }

      this.axios.post(`${process.env.VUE_APP_API_URL}/products`, this.newProduct, {
        headers: {
          Authorization: `Bearer ${this.$cookies.get('token')}`
        }
      })
      .then(() => {
        alert('เพิ่มสินค้าเรียบร้อยแล้ว');
        this.$router.go();
      })
      .catch(err => {
        console.error('Error adding product:', err);
        alert('เกิดข้อผิดพลาดในการเพิ่มสินค้า');
      });
    }
  }
};
</script>
