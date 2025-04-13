<template>
  <v-app-bar
      app
      color="green"
      dark
    >
      <div class="d-flex align-center" style="cursor: pointer;" @click="goToHome">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2 rounded-xl"
          contain
          src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTQjJ-0AVg4WjuAdKoJsyVoJ96AvrESnjhJ9ZcEg4D_QD5x8U-ZxV4XxjtvnYCdoL7lMFE&usqp=CAU"
          transition="scale-transition"
          width="40"
        />

        <span class="font-weight-bold">Chin Jung Shop</span>
      </div>

      <v-spacer></v-spacer>

      <div
        class="d-flex align-center mr-4"
      >
        <v-btn
          class="mr-4"
          text
          @click="goToHome"
        >
          <span>Home</span>
        </v-btn>

        <v-btn
          class="mr-4"
          text
          @click="() => {}"
        >
          <span>About</span>
        </v-btn>

        <v-btn
          class="mr-4"
          text
          v-if="isLogin"
          @click="goToShop"
        >
          <span>Shop</span>
        </v-btn>

        <v-btn
          class="mr-4"
          text
          v-if="isLogin"
          @click="goToCart"
        >
          <span>Cart</span>
          <span v-if="amount !== 0"> ({{ amount }}) </span>
        </v-btn>
      </div>

      <div
        v-if="!isLogin"
        @click="goToLogin()"
      >
          <v-img
            class="shrink mr-2"
            contain
            src="@/assets/login.png"
            transition="scale-transition"
            width="25"
          />
      </div>

      <div
        v-if="isLogin"
        @click="logout()"
      >
          <v-img
            class="shrink mr-2"
            contain
            src="@/assets/logout.png"
            transition="scale-transition"
            width="25"
          />
      </div>
    </v-app-bar>
</template>

<script>
  export default {
    data() {
      return {
        amount: 0,
        isLogin: false
      };
    },
    created() {
      const token = this.$cookies.get('token');
      if (token) {
        this.isLogin = true;
        this.fetchOders();
      } else {
        this.isLogin = false;
      }
    },
    methods: {
      goToLogin() {
        this.$router.push('/login');
      },
      goToHome() {
        if (this.$router.history.current.name === 'home') {
          this.$router.go();
        } else {
          this.$router.push('/');
        }
      },
      goToShop() {
        if (this.$router.history.current.name === 'shop') {
          this.$router.go();
        } else {
          this.$router.push('/shop');
        }
      },
      goToCart() {
        if (this.$router.history.current.name === 'cart') {
          this.$router.go();
        } else {
          this.$router.push('/cart');
        }
      },
      logout() {
        this.$cookies.remove('token');
        this.isLogin = false;
        this.$router.push('/login');
      },
      fetchOders() {
        this.axios.get(`${process.env.VUE_APP_API_URL}/orders`, {
          headers: {
            Authorization: `Bearer ${this.$cookies.get('token')}`
          }
        })
          .then(res => {
            const orders = res.data.data;
            const isPaidOrders = orders.filter(order => !order.isPaid);
            this.amount = isPaidOrders.length;
          })
          .catch(error => {
            console.error('Error fetching orders:', error);
            this.$router.push('/login');
          });
      },
    }
  };
</script>