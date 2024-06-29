<template>
  <div id="wrapper">
    <nav class="navbar is-blue">
      <div class="navbar-brand">
        <router-link to="/" class="navbar-item"><strong>Buymore</strong></router-link>

        <a class="navbar-burger" aria-label="menu" aria-expanded="false" data-target="navbar-menu" @click="showMobileMenu = !showMobileMenu">
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>

      <div class="navbar-menu" id="navbar-menu" :class="{'is-active': showMobileMenu}">
        <div class="navbar-start">
          <div class="navbar-item">
            <form method="get" action="/search">
              <div class="field has-addons">
                <div class="control">
                  <input type="text" class="input" placeholder="What are you looking for?" name="query">
                </div>

                <div class="control">
                  <button class="button is-success">
                    <span class="icon">
                      <i class="fas fa-search"></i>
                    </span>
                  </button>
                </div>
              </div>
            </form>
          </div>
        </div>
        <div class="navbar-end">
          <router-link to="/fresh-produce" class="navbar-item">Fresh Produce</router-link>
          <router-link to="/meat" class="navbar-item">Meat</router-link>
          <router-link to="/fishery" class="navbar-item">Fishery</router-link>
          <router-link to="/bakery" class="navbar-item">Bakery</router-link>
          <router-link to="/beverages" class="navbar-item">Beverages</router-link>
          <router-link to="/snacks" class="navbar-item">Snacks</router-link>

          <div class="navbar-item">
            <div class="buttons">
              <template v-if="$store.state.isAuthenticated">
                <router-link to="/my-account" class="button is-light">My account</router-link>
              </template>

              <template v-else>
                <router-link to="/log-in" class="button is-light">Log in</router-link>
              </template>

              <router-link to="/cart" class="button is-success">
                <span class="icon"><i class="fas fa-shopping-cart"></i></span>
                <span>Cart ({{ cartTotalLength }})</span>
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </nav>

    <div class="is-loading-bar has-text-centered" v-bind:class="{'is-loading': $store.state.isLoading }">
      <div class="lds-dual-ring"></div>
    </div>

    <section class="section">
      <router-view />
    </section>

    <footer class="footer">
      <p class="has-text-centered">Copyright (c) 2024</p>
    </footer>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      showMobileMenu: false,
      cart: {
        items: []
      }
    }
  },
  beforeCreate() {
    this.$store.commit('initializeStore')

    const token = this.$store.state.token

    if (token) {
        axios.defaults.headers.common['Authorization'] = "Token " + token
    } else {
        axios.defaults.headers.common['Authorization'] = ""
    }
  },
  mounted() {
    this.cart = this.$store.state.cart
  },
  computed: {
    cartTotalLength() {
      return this.cart.items.reduce((total, item) => total + item.quantity, 0);
    }
  }
}
</script>

<style lang="scss">
@import '../node_modules/bulma';
.lds-dual-ring {
  display: inline-block;
  width: 80px;
  height: 80px;
}
.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 6px solid #ccc;
  border-color: #ccc transparent #ccc transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.is-loading-bar {
  height: 0;
  overflow: hidden;

  -webkit-transition: all 0.3s;
  transition: all 0.3s;

  &.is-loading {
    height: 80px;
  }
}

/* Custom navbar color */
.navbar.is-blue {
  background-color: #1E3A8A; /* Dark blue color */
  color: #FFD700; /* Yellow color */
}

.navbar.is-blue .navbar-item,
.navbar.is-blue .navbar-burger {
  color: #FFD700; /* Yellow color */
}

/* Hover effect */
.navbar.is-blue .navbar-item:hover,
.navbar.is-blue .navbar-burger:hover {
  background-color: #2B6CB0; /* Slightly lighter blue for hover */
  color: #FFD700; /* Maintain yellow color for text */
}

/* Remove focus effects */
.navbar.is-blue .navbar-item:focus,
.navbar.is-blue .navbar-burger:focus {
  background-color: #1E3A8A; /* Same as the background color to remove focus effect */
  color: #FFD700; /* Same as the text color to remove focus effect */
}
</style>
