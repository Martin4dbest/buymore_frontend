<template>
  <div id="wrapper">
    <!-- Navbar -->
    <nav class="navbar is-blue">
      <div class="navbar-brand">
        <router-link to="/" class="navbar-item">
          <img src="@/assets/dorbasslogo.png" alt="Dorbass Logo" class="dorbass-logo">
          <strong>Dorbass</strong>
        </router-link>

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
          <router-link to="/Grains-Cereals" class="navbar-item">Grains and Cereals</router-link>
          <router-link to="/Vegetables-Fruits" class="navbar-item">Vegetables and Fruits</router-link>
          <router-link to="/others" class="navbar-item">Others</router-link>

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

    <!-- Hero Section for Home Page -->
    <section v-if="isHomePage" class="hero is-primary is-bold is-rounded mt-4">
      <div class="hero-body">
        <div class="container has-text-centered">
          <h1 class="title">
            Welcome to DORBASS
          </h1>
          <h1 class="subtitle">
            OPTIMUM GLOBAL
          </h1>
          <h2 class="subtitle">
            Shop at your comfort
          </h2>
        </div>
      </div>
    </section>

    <!-- Content Section -->
    <div class="is-loading-bar has-text-centered" v-bind:class="{'is-loading': $store.state.isLoading }">
      <div class="lds-dual-ring"></div>
    </div>

    <section class="section">
      <router-view />
    </section>

    <!-- Footer -->
    <footer class="footer">
      <p class="has-text-centered">Copyright (c) 2024</p>
    </footer>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      showMobileMenu: false,
      cart: {
        items: []
      }
    };
  },
  computed: {
    cartTotalLength() {
      return this.cart.items.reduce((total, item) => total + item.quantity, 0);
    },
    isHomePage() {
      // Check if current route is the home page
      return this.$route.path === '/';
    }
  },
  beforeCreate() {
    this.$store.commit('initializeStore');

    const token = this.$store.state.token;

    if (token) {
      axios.defaults.headers.common['Authorization'] = "Token " + token;
    } else {
      axios.defaults.headers.common['Authorization'] = "";
    }
  },
  mounted() {
    this.cart = this.$store.state.cart;
  }
};
</script>

<style lang="scss">
@import '../node_modules/bulma';
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

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

/* Buymore Logo styling */
.buymore-logo {
  height: 2rem; /* Adjust the height of the logo */
  border-radius: 90%; /* Make the logo round */
  overflow: hidden; /* Ensure content doesn't overflow */
}


/* Additional styling */
.hero.is-rounded {
  border-radius: 12px; /* Adjust the border-radius for rounded corners */
  margin-top: 20px; /* Add some top margin for space between navbar and hero */
}
</style>
